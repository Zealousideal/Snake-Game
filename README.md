This is a simple Snake Game implimented in Rust. The explaination of the code is given here.

## Dependencies

```toml
[dependencies]
rand = "0.8.5"
piston_window = "0.132.0"
```

-   The `rand` crate is used o generate random positions of food for the snake
-   The `piston_window` is a game engine, that help render the graphics and make the game in every way, possible and runnable.

## The `draw.rs` file

This file contains the code that is executed every time the window is drawn

-   The `to_coord` function returns an explicit `f64` casted value of any coordinate
-   The `to_coord_u32` function casts the value given by the `to_coord` function to `u32` for better precision.
-   The `draw_block` function draws a block(later defined in the `snake.rs`). It takes the color of the box, its cordinates, its size, a mutable `Context` reference which helps to draw 2D graphics and a Graphics buffer as input.
-   The `draw_rectangle` function is almost the same as the `draw_block` function but it differs as it can be used to create a specific sized block or a rectangle.

## The `snake.rs` file

-   A constant `SNAKE_COLOR` is defined which will give the color of the snake. It is set to green here.
-   An enum `Direction` is defined which will basically is a character set of the directions that will be used here.
-   The `impl` of `Direction` has a function to match the case wherein the opposite directions can be measured (and in further) discarded. For eg. If the snake is moving up and then is instructe to directly ove down, it will clash with its own head and thus this action can't be allowed. Similar 4 options are measured here and in further will be discarded.
-   A `struct` of `Block` is created which will represent the basic structure of a block.
-   Another `struct Snake` is created which will be the snake of our game. It has the parameters
    -   `direction` of type `enum Direction` to have the current direction of the snake that it is moving in.
    -   `body` of type `LinkedList` as it will be easy to impliment and a perfect way to do impliment the body of the snake.
    -   `tail` of type `Option<Block>` for the tail of the snake as we will have to check and use the tail many times. One for example to move, to increase the size of the snake when it eath the food, etc.

### Implimentations on `struct Snake`

-   The `new` function is used to create a new instance of the `struct Snake`. The size of the snake at first is set to 4 blocks. The initial direction is set to `Right`.
-   The `draw` function is used to draw the snake. It uses `draw_block` defined in the `draw.rs` file to do so.
-   The `head_position` function returns a tuple of `(i32, i32)` which is used to pin point the coordinated of the head of the snake. This is useful in various scenarios when we decide how the game will be over and all.
-   The `move_forward` function first checks whether the direction that the snake is moving and is going to move next is valid.
    -   If it is, it gets the position of the head of the snake through the `head_position()`method.
    -   It then demos a new_block to give the next direction. As the coordinates of the origin in a screen is on the top lect corner and the (in-general) negative y is the positive vertical axis, the Directions in the new_block are given so.
    -   The `new_block` is then pushed in front of the linked list body of the snake.
    -   As the size of the snake after the movement and before should remain same, since it is not eating something, so the last block of the linked list body is poped out.
    -   The `tail` of the linked list is assigned to the removed block.
-   The `head_direction` function is used to return the current direction of the snake.
-   The `next_head` function returns the next coordinates, in which the head of the snake is going to be in.
-   The `restore_tail` function will be used when the snake will eat th food and its size will increase. It essentially clones a block and pushes it back to the linked list body of the snake.
-   The `overlap_tail` function returns a bool value based on whether any part of the snake is overlapping with any other part of it. It can happen hen the nsake is making a loop with its body. It also handles the ambigious case where the snake makes a perfect loop with its head and tail in one block, overlapping.

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
-

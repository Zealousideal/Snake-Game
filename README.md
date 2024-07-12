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

## The `game.rs` file

-   The functions `draw_block` and `draw_rectangle` from the `draw.rs` file are imported.
-   The color of the food, the border colours an the particular color at game over is defined in the `FOOD_COLOR`, `BORDER_COLOR` and `GAMEOVER_COLOR` constants respectively.
-   The `MOVING_PERIOD` and the `RESTART_TIME` is defined which basically is the fps and the interval is defined.
-   A `struct Game` is made which has the basic qualities of a session of the snake game.

### Implimentations of `struct Game`:

-   The `new` function is used to create a new instance of the `struct Gamr`. The new snake will spawn in coordinates `(2, 2)`, there will be food on screen at coordinates `(6,4)`.
-   The `key_pressed` function checks which directional key is pressed in the keyboard and applies the direction according to it.
    -   It also makes sure if any key other than the direction keys are pressed, then nothing happens.
    -   It also checks whether and opposte directions are tried her with the `snake::Direction::opposite` method and makes sure nothing happens in that situation.
    -   The direction of the snake i then updated using the `update_snake` method
-   The `draw` method draws the window for the game and impliments the window to show when the game-over happens. Here we show a red window, fullscreen with opacity = 0.5.
-   The `update` function if the game is over makes sure the game restarts. It also adds food to the screen if food is not there. Also, it updates the waiting time of the game.
-   The `check_eating` function is used when the snake actually eats the food. It checks whether the coordinates of the head of the snake and the food match. It it does, it sets the `food_exists` to `false` and adds a block tp the length of the snake by the `snake::restore_tail()` method.
-   The `check_if_snake_alive` function returns a boolean value if the snake is still alive when it takes the next step. It also checks whether the snake is not going off the boundaries.
-   The `add_food` function adds a bit of food to the window. It generates random coordinates of the food using the `thread_rnd` function. Upon generating the new food, it sets the `food_exists` to `true`.
-   The `update_snake` method checks if the snake is alove through the `check_if_snake_alive` method. If it is, it allows the snake to move forward in the direction it was moving with the `snake::move_forward()` method. Else, if the snake is not alive, it initiates game over by setting the `game_over` to true.
-   The last function is the `restart` function. It contains the default parameters and setup as we have in the beginning of the game. It looks much similar to the `game::new()` file but the purpose of the creation of this function is that if we call the `game::new()` function it will create a new window which we don't want.

## The `main.rs` file and combining it all together!

-   The `draw, game, snake` files are brought into scope.
-   The background color of the window is defined as the `BACK_COLOR`.
-   The width and the height of the game window is set.
-   A new window is created using thr `PistonWindow` and its specifications are set by the `PistonWindow::WindowSettings`. A new feature is also set that the window will close on the press of the `esc` key, and any potential errors are handled by the `unwrap` function.
-   A new `Game` instance is created with the set width and height.
-   An event is set such that if any keyboard key is pressed, it is redirected to the `game::key_pressed` function so that the snake can be operated.
-   The window is drawn with a `BACK_COLOR` and by using the `game::draw()` function.
-   The event is continiously updated through the `game::update()` function.

## Future Updates

-   To impliment `lives` into this game.
-   To modify the UI to look like a terrain in the background.
-   To add better graphics to the snake.
-   Before the gam starts initialize a pop up for the game to start.
-   Show a score at the end of each game.

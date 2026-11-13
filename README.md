# desmos-slide-logic

A playable sliding based movement game in Desmos, with collidable walls, bounce walls, and a trail effect on the player. The whole game is made in Desmos.

![Game play](Screenshot%202026-06-02%20103948.png)

**[Play it](https://www.desmos.com/calculator/nqc6uxhfls)**

## How to play

1. Open the link above.
2. (if needed you might need to click and drag, and/or zoom in or out so the red lines (boundaries) are at your screens edge or near it).
3. start the ticker (press the play button on the thing at the top of the math section)
4. Press Ctrl+Alt+P, then Tab. This locks the controls onto the player. Once it's working you should be able to use arrow keys (tap) to slide the player in that direction.

If you click somewhere or type something, press Ctrl+Alt+P then Tab again to re-lock onto the player.

## Features

- Bounce walls: send the player in the opposite direction when hit. 
- Collisions: the player stops when they hit a wall allowing you to move again.
- Trail effect: a simple trail effect that follows the player

## How it works (a bit technical)

Desmos has no scripting language, so all of the game logic is math: And one of the core ways it does logic is through ternary piecewise statements, so `true if condition else false` becomes `{condition:a,b}` (which means a if true, otherwise b) in Desmos this is used by the tile detection, and out of bounds detection.

- the input system: uses a node (point) which can be moved you lock on to it via (ctrl+alt+p then tab), and then you can move it to change it's values (x_1, y_1), which are then read by the physics loop and since it rests at (0,4) the y_1 is translated down by 4 for accurate actions. 
- Physics loop: The whole gameplay loop runs through a ticker that runs an action[^1], which moves the player and checks for reset conditions every frame.
- Sliding movement: The player's position is `(x₂, y₂)` and velocity is `(v_x, v_y)`. Each tick, the velocity is added to the position and the velocity is only recalculated off of inputs once it is stopped.
- Collisions: Before moving, the game checks the tile the player is about to enter against lists of wall positions (ran by ticker). translating each position into a complex number (`x + iy`), which lets one equality compare both coordinates, and `max(...)` over the list answers wether there is a wall if so, the move is stopped and velocity is reset.
- Bounce walls invert the velocity (`v → -v`), and thus the player rebounds.
- Bounds: Leaving the play area resets the players pos, like resetting the level.
- Hardest part: The main challenge was the collision logic, and most of the other stuff is built off it.

[^1]: a action looks like `a -> b`, which assigns the varriable a to the value of b. 

## Why no code here:

The game runs inside Desmos, which is the game engine, so the graph is hosted on their servers. This repository can't be run locally. The live graph above is the demo.

## Credits

Made solo by robopugo (no10123) for the Stardance Hack Club challenge.

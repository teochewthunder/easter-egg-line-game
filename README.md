# Easter Egg Line Game (in progress)

This is a game written using an older version of VueJS. The objective is to select eggs from the grid in an unbroken line (not neccessarily a *straight* line, but horizontally and vertically), which will in turn fill up colors on a rainbow. Once the rainbow is completed or the user runs out of moves, the game ends and points are tallied.

## HTML 
This utilizes a number of placeholders for data display. 
- visually, a title panel.
- display for `score` and `movesRemaining`.
   - and either:
       - a rainbow display that is a row of 7 divs, each with a colored div within. Each of these represent one color of a rainbow.
       - a grid of 10 by 10 squares, each containing an egg of a random color and style (to be populated by JavaScript).
   - or:
       - an instructions panel.
- buttons at the footer for gameplay.

## CSS
Only covering the stuff that's related to gameplay.
- Grid: The main grid is styled using `grid`. There are 10 divs within, styled using `col`. And within these, each has 10 divs styled using `square`. This gives us a 10 x 10 square grid.
- Eggs: Styled using `egg` AND `style1` or `style2` or `style3` AND a color. The `after` pseudoselector provides a 3D effect.
- Rainbow Stripes: Outer container is styled using `stripe` and a class derived from "outline_" and a color. Inner fill is another div styled using a color.

## JavaScript (TBA)
### Data
- `started`: ABoolean that determines if a game is in progress. It is set to `false` on a button click or when an end-game scenario is achieved. It is also used to show or hide some buttons.
- `paused`: A Boolean that pauses the game, disabling certain methods from running and some buttons from being displayed.
- `max`: An integer that is set at 100. It determines the maximum number of "points" required to fill each rainbow stripe.
- `moves`: An integer which determines the number of moves the player starts with.
- `remainingMoves`: A value that starts out the same as `moves`, but gets decremented with each move. Once it reaches 0, an end-game scenario is achieved. It also determines the `score` after the game is over.
- `pointsPerEgg`: A value that adds to the `score` depending on how many long an egg line is.
- `score`: The number of points accumulated. It starts at 0, gets incremented during the gam and at the end.
- grid: An array that the grid is bound to. It is supposed to be a 10 x 10 two-dimensional array. The second dimension contains an object.
   - `style`: "style1", "style1" or "style3".
   - `color`: "red, "orange", "yellow", "green", "blue", "indigo" or "violet".
- `selection`: An array that contains arrays pointing to the first and second dimension of `grid`.
- `rainbow`: An object that contains seven properties, all named after the colors of a rainbow. All these properties start at 0 and end at `max`.

### Methods
- `reset()`: This is run whenever a game is started (or restarted). The grid is reset using `resetEggs()`, data is reset to default values and `handleImpossibleScenario()` is called just to ensure that the game is playable.
- `resetEggs()`: This clears the entire grid of values, then calls `fillMissingEggs()` to populate the grid.
- `handleImpossibleScenario()`: This runs `isPossibleToMove()` and prmpes a warning message every time it is `false`, until the method returns `true`.

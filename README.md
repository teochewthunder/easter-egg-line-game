# Easter Egg Line Game

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

## JavaScript
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
- `handleImpossibleScenario()`: This runs `isPossibleToMove()` and pops up a warning message every time it is `false`, until the method returns `true`.
- `getMarginTop()`: 
    - This has a parameter, `top`. 
    - It uses `top` and the `max` property to determine the `margin-top` property of the div inside each "stripe", thus visually simulating some kind of "meter" effect. 
- `getMiddleHeight()`: 
    - This has a parameter, 'section`. 
    - This just helps to show either the instructions panel or the grid. Due to the `transition` property beng already set in the CSS, this will manifest as an animation.
- `getButtonVisibility()`: 
    - This has a parameter, 'section`. Not the best name, but this is simple functionality, anyway. 
    - The `display` property is set to either `block` or `none` depending on the values of `section` and the properties `started` and `paused`.
- `getEggClass()`: 
    - It has parameters `c` and `s`.
    - Uses these values to point to the `grid` array's `style` and `color` properties to set the element's `class` attribute. 
    - It also checks if the combination of `c` and `s` appear in the `selection` array (using the `isInSelection()` method) and adds the `outline` class if so.
- `isInSelection()`: 
    - It has parameters `c` and `s`.
    - Checks if the combination of `c` and `s` appear in the `selection` array. Returns a Boolean.
- `isLinkableAtPos()`: 
    - It has parameters `c` and `s`. 
    - If there is only one element in the `selection` array, the method automatically returns `true`. 
    - Otherwise, it uses the arguments to decide if either the first or last element, has any horizontal or vertical neighbors in the `grid` array with the same `color` property. It returns 0 or 1 depending on whether the first or the last element is found to be such, and `null` if neither.
- `setStarted()`: 
    - Has parameter `started`.
    - Sets the `started` property to that value (either `true` or `false`). 
    - If `true, also call `reset()`. 
    - If `false`, also set the `paused` property to `false`.
- `selectSquare()`: 
    - It has parameters `c` and `s`. 
    - If the `paused` property is `true`, exit early with no action. 
    - If no elements are n the `selection` array, push the values into `selection` and exit. 
    - Otherwise, use `isLinkableAtPos()` to determine if there is a suitable position to add these values into the `selection` array. 
        - If `null` and this does not already exist in the array, reset `selection` to an empty array. 
        - If 0, use `unshift()` to insert at the beginning. 
        - If 1, use `push()` to insert at the end.
- `fillMissingEggs()`: Checks if there are any elements in `grid` that have empty `style` and `color` properties, and fills them up using `getRandomPair()`. Note that `arr` is used to store the value of `grid` and work in, so that we can avoid superfluous re-rendering.
- `getRandomPair()`: Returns an array containing random values for `style` and `color`.
- `confirm()`:
    - Decrements the `remainingMoves` property.
    - Set `paused` to `true`.
    - All elements in `selection` should have their equivalent in `grid` have the CSS class `fadeout` added. This will cause the selected eggs to fade to nothing. 
    - Also set `style` and `color` properties of these elements to empty.
    - Meanwhile, use `setTimeout()` with a 1 second delay.
        - Increment `score` accordingly.
        - Make sure the appropriate color of `rainbow` gets incremented. But cannot exceeed `max` property value.
        - Traverse `grid` and ensure that all eggs above an empty space "drop down".
        - Use a nested `setTimeout()` with a half second delay.
            - call `fillMissingEggs()`.
            - set the `paused` property to the result of calling `isEndGame()`.
- `isEndGame()`: 
    - Basically checks if `remainingMoves` is 0, or if all properties of the `rainbow` object are at `max` value. If either is true, it is an end-game scenario.
    - If so, use `setInterval()` with a fast duration.
        - Increment `score` by the `pointsPerMove` property value, and decrement `remainingMoves`.
        - Once `remainingMoves` is at 0, stop running. Concatenate "GAME OVER" to `score`. Again, this is lazy but whatever.
    - Otherwise, just call `handleImpossibleScenario()`.
- `isPossibleToMove()`: Checks the entire grid. Once any element is found where there is either a vertical or horizontal neighbor with the same `color` property, return `true`. Otherwise, return `false`.

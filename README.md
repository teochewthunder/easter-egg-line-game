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
### Methods

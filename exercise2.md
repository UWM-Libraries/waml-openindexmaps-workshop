# Exercise 2: Create a grid index map from scratch

In this scenario we will create an index map from scratch
using a grid of 15-minute quads for the state of Colorado.
We'll use the OpenStreetMap basemap for reference.

- Start a new project in QGIS and add the OSM Basemap using the QuickMapServices plugin.

## 1. Create the grid

A 15-minute quad is 15/60 or 1/4 of a degree, so we need to create a rectangular grid where each cell is 0.25 degrees wide and tall.  To cover all of Colorado, we'll need to figure out the latitude and longitude bounds of the state.

- Zoom in to Colorado (shift-drag will zoom into the rectangle you select)
- Click the Project CRS ![EPSG:3857](/image/project-crs.png) and change it to EPSG:4326 "WGS 84"
- Move your cursor to the four corners of the state and note the values shown in the Coordinate display ![Coordinate](/image/ex2-coordinate.png).
- Open the "Create Grid" processing tool
- Set the following parameters:
  - Grid type > Rectangle (Polygon)
  - Grid extent > click the down arrow on the far right and select > Draw on Map Canvas, then drag a rectangle covering Colorado
  - Fine tune the Grid extent coordinates by rounding to the nearest whole number ("-109, -102, 37, 41")
  - Set the horizontal and vertical spacing to 0.25 degrees
  - Leave the default values for the other fields
- Click "Run"

## 2. Label the cells

Let's suppose we have a Colorado map series that uses these quads, and each map is number A1, A2, A3 across the top row, and A1, B1, C1 down the left column, like this:

![grid labels](/image/ex2-grid-labels.png)

Although we could just add a new column and type in the value for every cell, that would be very tedious and prone to error.  With some experimentation, we can figure out a formula for calculating the code for each cell.

Use the Identify tool ![identify tool](/image/identify-tool.png) to look at the attributes for one of the grid cells.  Notice that it includes values for the left, top, right, and bottom coordinates of the cell.

- Open the Properties... dialog box
- Click the "Labels" button on the left of the panel to switch to label styling
- Change "No labels" to "Single labels"
- Set Value = `top` and click "Apply"

QGIS dynamic label expressions can help us to figure out the formula in an iterative manner.  The complete formula is below, but here is the sequence used to figure it out. 

- Click the expression button ![Expression](/image/ex2-expression.png) to the right of the Value field.
- Change the expression as indicated below.  After typing to change the `top` expression, click "OK" and then click the "Apply" button.
  - `"top"*4` (to get whole numbers)
  - `165-"top"*4` (so that the top row is 1, and the numbers increase southwards)
  - `char(64 + 165-"top"*4)` (this converts the numbers to letters -- char(65) is A, char(66) is B, etc.)
  - `char(64 + 165-"top"*4) || ("left"*4)` (this appends numbers based on the longitude)
  - `char(64 + 165-"top"*4) || (437 + "left"*4)` (so that the left column is 1, and increases to the east)

## 3. Add columns to the attribute table

According to the standard, map sheet labels should be entered into a field (column) called "label".  We can copy the code from the previous step and use it to enter values into a new column.

![labelStandard](/image/ex2-labelStandard.png)

- Copy the value expression for the labels
- Open the Attribute Table.
- Open the Field Calculator ![field calculator button](/image/field-calculator.png)
- Make sure the "Create a new field" box is checked
- Output field name = "label"
- Output field type = "Text (string)"
- Paste the expression into the large area on the left of the dialog
- Check the output preview beneath
- Click "OK" -- the adds the column, but now we are in edit mode
- Click the pencil icon to toggle editing off (save when prompted)

Now the label is another field that could be used when we run "Refactor Fields".

When in edit mode, other fields could be added and edited while viewing the attribute table.

----

Next: [Optional Exercise](exercise-optional)

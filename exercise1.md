# Exercise 1: Create a polygon index map from an existing shapefile

TODO: Data Introduction

If you have not already downloaded the data for this workshop, do so now:

[Download Data](/index.md/#download-the-data-for-this-workshop)

Be sure to extract the contents of the .zip file to your computer.

## 1. Add the data to QGIS

- Start a new project in QGIS (Project menu > New)
- Add the /exercise1/cuba_62k_gdx.shp by dragging it onto QGIS

Let's verify that the data is in the correct location by adding a basemap, and set our map to use the CRS of the basemap.

- Web > QuickMapServices > OSM > OSM Standard`
- Right-click the "OSM Standard" layer name > Layer CRS > Set Project CRS from Layer.
Check the lower right corner of QGIS to confirm the Project CRS is set to `EPSG:3857`

## 2. Add some style

To improve the visibility of both the index map and the basemap, let's change the style of the index map:

- Open the Layer Styling panel ![processing toolbox button](/image/layer-styling-button.png)
- Select the "cuba_62k_gdx" layer in the Layers panel
- In the Layer Styling panel click "Simple fill"
- Set the "Fill color" to purple
- Under Layer Rendering, set the Opacity to 50%
- Set the "Stroke color" to dark purple

## 3. Explore the data

Let's explore the values in the table, to get an idea of how we might want to convert the fields into OpenIndexMaps properties.

- Right-click the "cuba_62k_gdx" layer > Open Attribute Table

Notice the values found in the different fields.

This example shapefile is a slightly modified extraction from the American Geogrpaphical Society Library's (AGSL) Geodex system,
which uses it's own schema that pre-dates the OpenIndexMap schema.

### Existing Fields:
- OBJECTID : uniquely identifies each tile
- RECORD : the sheet number
- LOCATION : the sheet title
- DATE : the publication date for the sheet
- SERIES_TIT : a title for the series (note two distinct series in our example)
- PUBLISHER : the publisher for the sheet (again note the two options for this example)
- SCALE : the scale of the sheet. In a regular series like this one, it will be the same for all sheets.
- PRODUCTION : indicating if the map is colored, 2-color, or blueprint
- CATLOC : The AGSL call number
- TOWNS : unique to this set, a list of towns on the sheet.
- HOLDINGS : a boolean if the item is held by the library or not
- ONLINE : a link to the item in the digital collections
- SCAN_NUM : a unique id the AGSL uses to identify scans
- X1 : westernmost extent
- X2 : easternmost extent
- Y1 : northernmost extent
- Y2 : southernmost extent

Note that some of the data is the same for each sheet in the set. 
See the note about [set- and flight-level metadata](https://openindexmaps.org/specification/1.0.0#set--and-flight-level-metadata)
in the schema documentation.

QGIS has a processing tool called "Refactor fields" that will let us rename, delete, and manipulate the values of these fields.

## 4. Refactor Fields

In the processing toolbox, search for "refactor fields" and open the tool.
The settings below will output a copy of the index map with just four fields: title, label, note, and downloadUrl.
In this case, we are putting the size into a note so that it will display in the GBL interface.

**Be sure to set all the types to string and set the Length to 255!** The schema specifies no limit to the length of the string in any element,
but in this case, setting a reasonably high limit only impacts the layer created in memory and not the output GeoJSON.

!["before" view of Refactor Fields window](/image/ex1-refactor-fields-before.png)

*Before view*

### Field Mapping:
- OBJECTID -> Remove Field (select and click 'Delete Selected Field')
- RECORD -> label
- LOCATION -> title
- DATE -> datePub
- SERIES_TIT -> edition or omit (note two distinct series in our example)
- PUBLISHER -> publisher (again note the two options for this example)
- SCALE -> scale
- PRODUCTION -> color
- CATLOC -> instCallNo
- TOWNS -> location*
- HOLDINGS -> available
- ONLINE -> digHold
- SCAN_NUM -> recId
- X1 -> west
- X2 -> east
- Y1 -> north
- Y2 -> south

When deciding which fields to use for `north`, `south`, `east`, and `west` ensure that you look closely how the input data is formatted.
Remember that negative longitudes indicate the western hemisphere and negative latitudes represent the southern hemisphere.

The source expressions can be typed in directly, but if you click on the epsilon (&epsilon;), QGIS will open an expression editor that provides a full list of available functions, along with syntax highlighting, error checking, and a preview of the output based on the first record.

Pay close attention to quotes in expressions.  Double quotes (sometimes optional) indicate a field name, whereas single quotes indicate a literal text string.

/* For the `location` field, we need to write an expression to ensure that we format our simple comma delimited string of locations as a valid JSON array:
![expression dialog for location field](/image/ex1-expression-dialog.png)

Type this exactly or copy-paste into the expression editor.
```
if(length("TOWNS") > 0,concat('["',replace("TOWNS",',','","'),'"]'),NULL)
```

The expression above checks the length of the existing TOWNS field.
If the length is greater than 0, or in other words, there are towns listed, it formats the list as a valid JSON string by adding square brackets and double quotes to the list.
If the length is not greater than 0, or in other words, it is Null, then it simply keeps the field null.

!["after" view of Refactor Fields window](/image/ex1-refactor-fields-after.png)

*After view. Yours may look different if you made different choices!*

- Leave the default setting "Create temporary layer", which will let you see the output without cluttering up your drive with files.

- Click "Run" and leave the "Refactor fields" dialog open in case you need to make any corrections and run it again.  (You can also get back to via the Processing menu > History)

You should have a new layer called "Refactored" on your map.
Use the identify tool or look at the attribute table to make sure the results look right.
If you need to make any corrections, remove the "Refactored" layer, 
then go back to the "Parameters" tab of the dialog,
make your changes and run it again.

## 5. Save as GeoJSON

Once we're satisfied with the output, we are ready to save it as a GeoJSON file.

- Right-click the "Refactored" layer > Make Permanent...
  - Set Format = GeoJSON
  - Always click the `...` button to specify where you want the file saved!
  - Under Layer Options, set RFC7946 = YES (this will force it into WGS84, and set decimal precision at 7 digits)

Now you should have an index map saved as a GeoJSON file!  If you open it in a text editor, you'll see that it starts something like this:

```
{
"type": "FeatureCollection",
"name": "cuba_62k_1913_gdx",
"features": [
```

You could also copy or drag the GeoJSON into [geojsonlint.com](http://geojsonlint.com/), which will check for any parsing errors and also render the GeoJSON in a map.

Notice that only set-level metadata is the "name", which we be whatever you named the file.
It would be a good idea to provide a bit more information, which could be added manually.
At this time, there are no standard properties for the index map as a whole,
but that is something that OpenIndexMaps may develop in the future.
Here is an example of what Cornell University does in their CUGIR portal:

```
{
"type": "FeatureCollection",
"name": "cugir-009099-index",
"title": "Index of 2-meter DEM, Tompkins County NY, 2008",
"websiteUrl": "https://cugir.library.cornell.edu/catalog/cugir-009099",
```


For the cuba example, you could make two separate index maps for the *Carta Militar De La Republica De Cuba 1:62,500* set and the *Military Map of Cuba 1:62,500* set.

## A word of warning

Due to over-enthusastic date-detection in GDAL (the format translator software that QGIS uses under the hood), if you try to edit an existing GeoJSON file that has date strings, they might get silently mangled into a different date string format.

If you plan on making further edits to your index map, I would suggest first saving it to another format such as geopackage, edit that, then generate geojson from there.

----

Next: [Exercise 2](exercise2)

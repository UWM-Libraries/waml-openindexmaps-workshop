# Creating GeoJSON for OpenIndexMaps
A workshop held at Geo4LibCamp, February 3, 2020, 1:30-4:30pm
by Keith Jenkins, GIS Librarian at Cornell University
<https://kgjenkins.github.io/openindexmaps-workshop/>

In this workshop, we'll learn how to use QGIS to create OpenIndexMaps GeoJSON files that can be used in GeoBlacklight to provide access to a series of maps or other datasets.

Download the data for this workshop here:
- <https://github.com/kgjenkins/openindexmaps-workshop/archive/v0.1.zip>

We'll start with a bit of background information about:
- Index Maps
- OpenIndexMaps
- GeoJSON
- QGIS

Then we'll work through various scenarios:
- [Exercise 1](#ex1): Create a polygon index map from an existing shapefile
- [Exercise 2](#ex2): Create a point index map from a CSV with x/y coordinates
- [Exercise 3](#ex3): Create a polygon index map from a CSV containing bounds
- [Exercise 4](#ex4): Create a grid index map from scratch


# Index Maps

An index map provides a map-based guide to finding individual maps or datasets in a series based upon their location.  They can be grid-like, with each rectangle representing a separate topographic map in a series, for example:

![grid index map](https://kgjenkins.github.io/openindexmaps-workshop/image/index-map-grid.png)

Or they can be point-based, with each point in the index referring to the approximate center of an aerial photo:

![point index map](https://kgjenkins.github.io/openindexmaps-workshop/image/index-map-points.png)

Or they can even be lines, each representing a LiDAR collection flight line:

![line index map](https://kgjenkins.github.io/openindexmaps-workshop/image/index-map-lines.png)


# OpenIndexMaps

OpenIndexMaps provides a specification for encoding an index map in the GeoJSON format.  It provides details and access information about each individual mapsheet or sub-dataset, which can be represented either as points, lines, or polygons.

- [OpenIndexMaps.org](https://openindexmaps.org/) has the basic specification created in 2018

The GeoJSON files are shared with others via the OpenIndexMaps organization on GitHub, where several institutions have repositories of index maps:

- [github.com/OpenIndexMaps](https://github.com/OpenIndexMaps) has repositories of index maps by institution

Further work is currently underway to standarize more elements of an index map, including both the data provided for each map or sub-dataset, as well as information about the collection as a whole.

- [GIS Index Map Creation Requirements and Recommendations](https://docs.google.com/document/d/1GS1_4JmgUkZcehiG1qEyQB3e6mRQ7jdGC7rpyesZqIw/edit) is a draft document being developed by a group led by Tom Brittnacher (UCSB)


# GeoJSON

GeoJSON is a geospatial data format designed for use on the web.  Like other JSON formats, it is a lightweight data-interchange format that is easy for a computer to parse, but is also relatively human-readable.  JSON is based on JavaScript Object Notation, but is also used across most other programming languages.

GeoJSON provides a standard way to add geospatial location information to JSON.  It was originally released in 2008 as a community-developed specification, and was quickly adopted by many geospatial projects.  It was later revised to become an official IETF specification ([RFC 7946](https://tools.ietf.org/html/rfc7946)) in 2016.  One of the major changes was that, whereas the 2008 allowed arbitrary coordinate systems, the 2016 standard mandates WGS84 (lon, lat) coordinates.

Example of a point in GeoJSON:
```
{
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [ -122.17, 37.428 ]
  },
  "properties": {
    "name": "Stanford University",
    "founded": 1885
  }
}
```

Example of a polygon in GeoJSON:
```
{
  "type": "Feature",
  "properties": {
    "name": "Mitchell Earth Sciences Building",
    "this is a *really long* property name!": "but just because \"you can\" doesn't mean you should"
  },
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [ -122.172949, 37.426643 ],
        [ -122.173051, 37.426349 ],
        [ -122.172398, 37.426208 ],
        [ -122.172302, 37.426498 ],
        [ -122.172949, 37.426643 ]
      ]
    ]
  }
}
```

Things to know about GeoJSON:
- Coordinates
  - All coordinates are in WGS84 (EPSG:4326) in longitude, latitude order
  - 7 digits past the decimal point (0.0000001) is about 1cm precision
- Geometries
  - Geometries can be points, lines, or polygons (or any combination even in the same file)
  - Geometries may also be multipoints, multilines, or multipolygons (think Hawaii)
  - Polygons may have holes, holes with islands, holes with islands with holes, etc.
- Properties
  - Property names can be very long (whereas shapefiles are limited to 10 characters)
  - Property names can contain spaces and non-alphanumeric characters (although perhaps not recommended)
  - The order of properties is not significant
  - Property values are either text strings or numbers (real or integer)
  - Double quotes in strings must be escaped as `\"`
  - Single quotes can also be escaped, but it is not required
- not recommended for large datasets (>20MB) but should be fine for most index maps


# QGIS

QGIS is a free, open-source desktop application for mapping and geospatial analysis.  It is a volunteer-driven project with contributers from around the world.  It reads and writes nearly every possible GIS data format, and has many features that are particularly useful for creating OpenIndexMaps GeoJSON files.

A quick tour of QGIS:
- Identify tool  ![EPSG:3857](https://kgjenkins.github.io/openindexmaps-workshop/image/identify-tool.png) lets you look at a feature's attributes
- Layer Styling panel ![processing toolbox button](https://kgjenkins.github.io/openindexmaps-workshop/image/layer-styling-button.png)
- Selection tools are useful for running processing tools on subsets, or saving subsets as new layers
  - ![selection tools 1](https://kgjenkins.github.io/openindexmaps-workshop/image/selection-tools1.png) and ![selection tools 2](https://kgjenkins.github.io/indexmaps-workshop/image/selection-tools2.png)
- Processing toolbox ![processing toolbox button](https://kgjenkins.github.io/openindexmaps-workshop/image/processing-button.png) contains hundreds of tools, including tools from other open-source GIS programs like GDAL and GRASS
- Over 600 plugins written by QGIS users provide a wide range of additional functionality
## CRS (coordinate reference system)

QGIS, like most open-source GIS programs, uses standard [EPSG codes](http://epsg.io/) for coordinate systems and map projections.

# TODO check that EPSG url

  - EPSG codes uniquely identify a CRS. For example:
    - EPSG:4326 = WGS 84 (standard latitude/longitude)
    - EPSG:3857 = WGS 84 / Pseudo-Mercator
    - EPSG:32610 = WGS 84 / UTM zone 10N
  - Hover over a layer name to see its CRS code
  - If a CRS is missing or incorrect, right-click the layer name > Set CRS > Set Layer CRS...a
  - But if the CRS is correct, and you want to transform the existing coordinates to another CRS:
    - Use the Reproject Layer tool
    - Or just specify a new CRS when saving to a new file (right-click layer name > export)
  - Project (map) CRS is identified in lower-right corner ![EPSG:3857](https://kgjenkins.github.io/openindexmaps-workshop/image/project-crs.png)
    - Click it to change the Project CRS
    - Or right-click a layer name > Set CRS > Set Project CRS from Layer

## QuickMapServices

QuickMapServices is one of the most popular plugins.  It provides access to basemaps from OpenStreetMaps, Google, Bing, Esri, and more.  Basemaps are useful for adding context to your maps, or to make sure that your data is showing up in the right place.

To install the plugin:

1. Plugins menu > Manage and Install Plugins...
2. Click the "All" tab at left
3. Type in the search box "quickm" (you don't have to type the whole name)
4. Click the name "QuickMapServices" in the results
5. Click "Install Plugin" (it only takes a few seconds)

Each plugin works a bit differently -- some add menus items, some add tool buttons.  QuickMapServices appears under the "Web" menu.  It comes with a few basemaps, but you'll want to add the full set of available basemaps:

6. Web menu > QuickMapServices > Settings
7. Click the "More services" tab
8. Click "Get contributed pack"


<a name="ex1"></a>
# Exercise 1: Create a polygon index map from an existing shapefile

Sometimes data publishers already have index maps for sets of tiled data, like elevation or LiDAR tiles.  These indexes are often in the form of shapefiles.  In this example, we'll convert a shapefile index of elevation tiles for Erie County, New York, already downloaded from <ftp://ftp.gis.ny.gov/elevation/DEM/>

If you have not already downloaded the data for this workshop, do so now:
- <https://github.com/kgjenkins/openindexmaps-workshop/archive/v0.1.zip>

Be sure to extract the contents of the .zip file to your computer.

1. Start a new project in QGIS (Project menu > New)
2. Add the /exercise1/County_Erie2008_DEM_Index.shp by dragging it onto QGIS

Let's verify that the data is in the correct location by adding a basemap, and set our map to use the CRS of the basemap.

3. Web > QuickMapServices > OSM > OSM Standard
4. Right-click the "OSM Standard" layer name > Set CRS > Set Project CRS from Layer

To improve the visibility of both the index map and the basemap, let's change the style of the index map:

5. Open the Layer Styling panel ![processing toolbox button](https://kgjenkins.github.io/openindexmaps-workshop/image/layer-styling-button.png)
6. Select the "County_Erie2008_DEM_Index" layer
7. Click "Simple fill"
8. Set the "Fill color" to purple with about 50% opacity
9. Set the "Stroke color" to dark purple

Another option would be to set the "Fill style" to "No Brush".  You may also want to adjust the "Stroke width".

10. Right-click the "County_Erie2008_DEM_Index" layer > Open Attribute Table

Explore the values in the table, to get an idea of how we might want to convert the fields into OpenIndexMaps properties.

| field | observation |
|:- |:- |
| FILENAME | uniquely identifies each tile, useful as title or label |
| SIZE | size in bytes, which we could convert to a more readable KB and put into a note |
| COLLECTION | always the same |
| YEAR | always the same |
| PARTIAL | indicates tiles with incomplete coverage, could go into a note |
| DEM_COLLEC | always the same |
| TILE_DATE | always the same |
| NOTES | empty |
| DIRECT_DL | download url |
| Shape_Leng | irrelevant |
| Shape_Area | irrelevant |


- refactor fields to edit columns and names

![refactor fields dialog](https://kgjenkins.github.io/openindexmaps-workshop/image/ex1-refactor-fields.png)

- export as geojson
- RFC7946, WGTS84
- beware of date-like fields (ogr2ogr has new option, though!)


<a name="ex2"></a>
# Exercise 2: Create a point index map from a CSV with x/y coordinates
- example: NYS aerial photos
- Create points layer from table
- show labels
- Note about data source manager > delimited text
- refactor fields
![refactor fields dialog](https://kgjenkins.github.io/openindexmaps-workshop/image/ex2-refactor-fields.png)
- export as geojson


<a name="ex3"></a>
# Exercise 3: Create a polygon index map from a CSV containing bounds
- example: nationalmap NED1
- convert bounds from text format to polygons
- QGIS virtual layers, SQL


<a name="ex4"></a>
# Create a grid index map from scratch
- Create grid in QGIS
- formula for grid labels
  - label expression like `char(45-bottom+65)||(left+81)`
  - once the formula looks good, calculate new 'label' column using the expression
- edit form for names - can hide columns (layer properties > attributes form > drag and drop designer)




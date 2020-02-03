# Creating GeoJSON for OpenIndexMaps
A workshop held at Geo4LibCamp, February 3, 2020, 1:30-4:30pm
by Keith Jenkins, GIS Librarian at Cornell University
<https://kgjenkins.github.io/openindexmaps-workshop/>

In this workshop, we'll learn how to use QGIS to create OpenIndexMaps GeoJSON files that can be used in GeoBlacklight (GBL) to provide access to a series of maps or other datasets.

Download the data for this workshop here:
- <https://github.com/kgjenkins/openindexmaps-workshop/archive/v0.1.zip>

We'll start with a bit of background information about:
- Index Maps
- OpenIndexMaps
- GeoJSON
- QGIS

Then we'll work through various scenarios:
- [Exercise 1](exercise1): Create a polygon index map from an existing shapefile
- [Exercise 2](exercise2): Create a point index map from a CSV with x/y coordinates
- [Exercise 3](exercise3): Create a grid index map from scratch
- [Exercise 4](exercise4): Create a polygon index map using virtual layer magic



# Index Maps

An index map provides a map-based guide to finding individual maps or datasets in a series based upon their location.  They can be grid-like, with each rectangle representing a separate topographic map in a series, for example:

![grid index map](https://kgjenkins.github.io/openindexmaps-workshop/image/index-map-grid.png)

Or they can be point-based, with each point in the index referring to the approximate center of an aerial photo:

![point index map](https://kgjenkins.github.io/openindexmaps-workshop/image/index-map-points.png)

Or they can even be lines, each representing a LiDAR collection flight line:

![line index map](https://kgjenkins.github.io/openindexmaps-workshop/image/index-map-lines.png)



# OpenIndexMaps

OpenIndexMaps provides a specification for encoding an index map in the GeoJSON format.  It provides details and access information about each individual mapsheet or sub-dataset, which can be represented either as points, lines, or polygons.

- [OpenIndexMaps.org](https://openindexmaps.org/) has the basic specification created in 2018.  It defines common properties that are assigned a specific meaning.

Currently, GeoBlacklight recognizes those properties defined by OpenIndexMaps.  Any other properties will be ignored and not displayed in the GBL interface.  (This may change in the future.)  Note also that GBL uses the "label" property to provide a mouseover tooltip on the map.

GeoJSON files are shared with others via the OpenIndexMaps organization on GitHub, where several institutions have repositories of index maps:

- [github.com/OpenIndexMaps](https://github.com/OpenIndexMaps) has repositories of index maps by institution

Note that many of the files found in those repos may include other local properties that are not defined by, but are still allowed by the specification.  Further work is currently underway to standarize more elements of an index map, including both the data provided for each map or sub-dataset, as well as information about the collection as a whole.

- [GIS Index Map Creation Requirements and Recommendations](https://docs.google.com/document/d/1GS1_4JmgUkZcehiG1qEyQB3e6mRQ7jdGC7rpyesZqIw/edit) is a draft document currently being developed by a group led by Tom Brittnacher (UCSB)


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
- Online tools
  - [GeoJSONLint](http://geojsonlint.com/) validates and previews GeoJSON
  - [GeoJSON.io](http://geojson.io/) validates, also edits and converts formats
- GeoJSON is not recommended for large datasets (>10MB) but should be fine for most index maps



# QGIS

QGIS is a free, open-source desktop application for mapping and geospatial analysis.  It is a volunteer-driven project with contributers from around the world.  It reads and writes nearly every possible GIS data format, and has many features that are particularly useful for creating OpenIndexMaps GeoJSON files.

A quick tour of QGIS:

- Identify tool  ![EPSG:3857](https://kgjenkins.github.io/openindexmaps-workshop/image/identify-tool.png) lets you look at a feature's attributes
- Layer Styling panel ![processing toolbox button](https://kgjenkins.github.io/openindexmaps-workshop/image/layer-styling-button.png)
- Selection tools are useful for running processing tools on subsets, or saving subsets as new layers
  - ![selection tools 1](https://kgjenkins.github.io/openindexmaps-workshop/image/selection-tools1.png) and ![selection tools 2](https://kgjenkins.github.io/openindexmaps-workshop/image/selection-tools2.png)
- Processing toolbox ![processing toolbox button](https://kgjenkins.github.io/openindexmaps-workshop/image/processing-button.png) contains hundreds of tools, including tools from other open-source GIS programs like GDAL and GRASS
- Over 600 plugins written by QGIS users provide a wide range of additional functionality


## CRS (coordinate reference system)

QGIS, like most open-source GIS programs, uses standard [EPSG codes](https://epsg.io/) for coordinate systems and map projections.

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


Now that we have all that background information, let's start creating index maps with [Exercise 1](exercise1)

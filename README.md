# Creating GeoJSON for OpenIndexMaps
a workshop held at Geo4LibCamp, February 3, 2020, 1:30-4:30pm

In this workshop, we'll learn how to use QGIS to create OpenIndexMaps GeoJSON files that can be used in GeoBlacklight to provide access to a series of maps or other datasets.

We'll start with a bit of background information about:
- Index Maps
- OpenIndexMaps
- GeoJSON
- QGIS

Then we'll work through various scenarios:
- Create a polygon index map from an existing shapefile
- Create a point index map from a CSV containing x/y coordinates
- Create a polygon index map from a CSV containing bounding coordinates
- Create a grid index map from scratch


# Index Maps

An index map provides a map-based guide to finding individual maps or datasets in a series based upon their location.  They can be grid-like, with each rectangle representing a separate topographic map in a series, for example:

![grid index map](https://kgjenkins.github.io/indexmaps-workshop/image/index-map-grid.png)

Or they can be point-based, with each point in the index referring to the approximate center of an aerial photo:

![point index map](https://kgjenkins.github.io/indexmaps-workshop/image/index-map-points.png)

Or they can even be lines, each representing a LiDAR collection flight line:

![line index map](https://kgjenkins.github.io/indexmaps-workshop/image/index-map-lines.png)


# OpenIndexMaps

OpenIndexMaps provides a specification for encoding an index map in the GeoJSON format.  It provides details and access information about each individual mapsheet or sub-dataset, which can be represented either as points, lines, or polygons.

- [OpenIndexMaps.org](https://openindexmaps.org/) has the basic specification created in 2018

The GeoJSON files are shared with others via the OpenIndexMaps organization on GitHub, where several institutions have repositories of index maps:

- [github.com/OpenIndexMaps](https://github.com/OpenIndexMaps) has repositories of index maps by institution

Further work is currently underway to standarize more elements of an index map, including both the data provided for each map or sub-dataset, as well as information about the collection as a whole.

- [GIS Index Map Creation Requirements and Recommendations](https://docs.google.com/document/d/1GS1_4JmgUkZcehiG1qEyQB3e6mRQ7jdGC7rpyesZqIw/edit) is a draft document being developed by a group led by Tom Brittnacher (UCSB)


# GeoJSON

GeoJSON is a geospatial data format designed for use on the web.  Like other JSON formats, it is a lightweight data-interchange format that is easy for a computer to parse, but is also relatively human-readable.  JSON is based on JavaScript Object Notation, but is also used across most other programming languages.

GeoJSON provides a standard way to add geospatial location information to JSON.  It was originally released in 2008 as a community-developed specification, and was quickly adopted by many geospatial projects.  It was later revised to become an official IETF specification ([RFC 7946](https://tools.ietf.org/html/rfc7946)) in 2016.

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
- Geometries can be points, lines, or polygons (or any combination in the same file)
- All coordinates are in WGS84 (EPSG:4326) (longitude, latitude order)
- 7 digits past the decimal point (0.0000001) is about 1cm precision
- GeoJSON supports property names of any length (whereas shapefiles are limited to 10 characters)
- Property order is not significant
- Property values are either text strings or numbers
- Double quotes in strings must be escaped as `\"`
- Single quotes may be escaped (or not)
- not recommended for large datasets (>20MB) but should be fine for most index maps


# QGIS

QGIS is a free, open-source desktop application for mapping and geospatial analysis.  It is a volunteer-driven project with contributers from around the world.  It reads and writes nearly every possible GIS data format, and has many features that are particularly useful for creating OpenIndexMaps GeoJSON files.

A quick tour of QGIS:
- CRS (coordinate reference system)
  - Project (map) CRS is identified in lower-right corner ![EPSG:3857](https://kgjenkins.github.io/indexmaps-workshop/image/project-crs.png)
- Identify tool  ![EPSG:3857](https://kgjenkins.github.io/indexmaps-workshop/image/identify-tool.png)
- Selection tools
![selection tools 1](https://kgjenkins.github.io/indexmaps-workshop/image/selection-tools1.png)
![selection tools 2](https://kgjenkins.github.io/indexmaps-workshop/image/selection-tools2.png)
- Styles (including color, size, transparency)
- Processing toolbox
- Plugins (especially QuickMapServices)

TODO expand each of those...

TODO Install the QuickMapServices plugin


# Create polygon index map from an existing shapefile
- example: NYS elevation indexes ftp://ftp.gis.ny.gov/elevation/DEM/
- QGIS symbology (transparency, outlines only, etc.)
- show labels
- refactor fields to edit columns and names

![refactor fields](https://kgjenkins.github.io/indexmaps-workshop/image/refactor-fields.png)

- export as geojson
- RFC7946, WGTS84
- beware of date-like fields (ogr2ogr has new option, though!)

# Create point index map from CSV
- example: NYS aerial photos
- lat/lon to point
- refactor fields
- export as geojson

# Create index map from CSV needing some manipulation
- example: nationalmap NED1
- convert bounds from text format to polygons
- QGIS virtual layers, SQL

# Create index map from new grid
- Create grid in QGIS
- formula for grid labels
  - label expression like `char(45-bottom+65)||(left+81)`
  - once the formula looks good, calculate new 'label' column using the expression
- edit form for names - can hide columns (layer properties > attributes form > drag and drop designer)




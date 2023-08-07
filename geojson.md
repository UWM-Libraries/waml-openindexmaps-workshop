# GeoJSON

GeoJSON is a geospatial data format designed for use on the web.
Like other formats based on JSON (JavaScript Object Notation),
it is a lightweight data-interchange format that is easy for a computer to parse,
but is also relatively human-readable. 

GeoJSON provides a standard way to store geospatial location information.
It was originally released in 2008 as a community-developed specification,
and was quickly adopted by many geospatial projects.
It was later revised to become an official IETF specification ([RFC 7946](https://tools.ietf.org/html/rfc7946))
in 2016.
One of the major changes was that, whereas the 2008 allowed arbitrary coordinate systems,
the 2016 standard mandates WGS84 (lon, lat) coordinates.

Example of a point in GeoJSON:
```
{
  "type": "Feature",
  "properties": {
    "name": "Walter C. Koerner Library",
    "address": "1958 Main Mall, Vancouver, BC V6T 1Z2, Canada"
  },
  "geometry": {
    "type": "Point",
    "coordinates": [
      -123.25509914782164,
      49.26669325071529
    ]
  }
}
```

Example of a polygon in GeoJSON:
```
{
  "type": "Feature",
  "properties": {
    "name": "Walter C. Koerner Library",
    "address": "1958 Main Mall, Vancouver, BC V6T 1Z2, Canada" 
  },
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [-123.255467, 49.266867],
        [-123.2550320, 49.266320],
        [-123.254822, 49.266392],
        [-123.255256, 49.266948],
        [-123.255467, 49.266867]
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
- GeoJSON is not recommended for very large datasets

----

Next: [QGIS](qgis)


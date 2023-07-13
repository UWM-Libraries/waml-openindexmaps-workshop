# GeoJSON

GeoJSON is a geospatial data format designed for use on the web.  Like other JSON formats, it is a lightweight data-interchange format that is easy for a computer to parse, but is also relatively human-readable.  JSON is based on JavaScript Object Notation, but is also used across most other programming languages.

GeoJSON provides a standard way to add geospatial location information to JSON.  It was originally released in 2008 as a community-developed specification, and was quickly adopted by many geospatial projects.  It was later revised to become an official IETF specification ([RFC 7946](https://tools.ietf.org/html/rfc7946)) in 2016.  One of the major changes was that, whereas the 2008 allowed arbitrary coordinate systems, the 2016 standard mandates WGS84 (lon, lat) coordinates.

`todo: replace with newer example`

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

`todo: replace with newer example`

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

----

Next: [QGIS](qgis)


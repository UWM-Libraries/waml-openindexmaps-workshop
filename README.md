# indexmaps-workshop
Geo4LibCamp 2020 workshop on creating OpenIndexMaps

QGIS
- review of QGIS features/tools for those new to QGIS
- dealing with CRS
- using selection and identify tools
- styling, transparency
- plugins
- processing toolbox

GeoJSON - what it is and why
- long attribute names, etc.
- History of GeoJSON
- RFC7946 - WGS84 only, 7 digit precision

Create polygon index map from an existing shapefile
- example: NYS elevation indexes
- QGIS symbology (transparency, outlines only, etc.)
- show labels
- refactor fields to edit columns and names
- export as geojson
- RFC7946, WGTS84
- beware of date-like fields (ogr2ogr has new option, though!)

Create point index map from CSV
- example: NYS aerial photos
- lat/lon to point
- refactor fields
- export as geojson

Create index map from CSV needing some manipulation
- example: nationalmap NED1
- convert bounds from text format to polygons
- QGIS virtual layers, SQL

Create index map from new grid
- Create grid in QGIS
- formula for grid labels
  - label expression like `char(45-bottom+65)||(left+81)`
  - once the formula looks good, calculate new 'label' column using the expression
- edit form for names - can hide columns (layer properties > attributes form > drag and drop designer)


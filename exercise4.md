# Exercise 4: Create a polygon index map using virtual layer magic

QGIS has many powerful tools for manipulating data.  Virtual Layers are a way of generating new dynamic layers that are the result of using spatial SQL on existing layers.  Virtual Layers do not need a spatial database like PostGIS or SpatiaLite, but can be used on any layer, including shapefiles or geojson.

In this exercise, we will query the NationalMap to get a CSV of all the 1 arc-second DEM tiles for the entire United States, and then turn it into an index map.  These tiles are 1-degree squares, and there are nearly 3400 of them.


## 1. Download the tile info

I'll demo this in the workshop, but the CSV has already been downloaded for you.  But this same technique could be used to make index maps of other National Map datasets.

- Go to https://viewer.nationalmap.gov/basic/
- On the map, zoom out so that all of US (including AK and HI) is visible
- Under "Elevation Products (3DEP)", select "1 arc-second DEM"
- Set the File Format to IMG
- Click "Find Products" (wait for results to display)
- Click the "Save as CSV" button



## 2. Create the tile index

- Start a new QGIS project and add the /exercise4/ned3397_20200201_195900.csv file

- Rename the layer to 'csv' (to make for simpler SQL below)

If you open the attribute table, you'll see how the min/max x/y values are all jammed into a single field.  This is not ideal, but we can extract each value using text-parsing functions in the SQL of QGIS Virtual Layers!

- Open the Data Source Manager > Virtual Layer

- Enter the following query:
```
-- first we extract all the coordinates out of the boundingBox field
with bbox as (
  select
    *,
    regexp_substr("boundingBox", 'minX:(-?\d+(\.\d+)?)') as minX,
    regexp_substr("boundingBox", 'minY:(-?\d+(\.\d+)?)') as minY,
    regexp_substr("boundingBox", 'maxX:(-?\d+(\.\d+)?)') as maxX,
    regexp_substr("boundingBox", 'maxY:(-?\d+(\.\d+)?)') as maxY
  from csv
)
-- then we assemble the attribute table and polygon geometry
select
  title,
  regexp_substr( "title", '(n\d+[ew]\d+)') as label,
  'true' as available,
  downloadUrl AS downloadUrl,
  metaUrl AS websiteUrl,
  (prettyFileSize || ' - updated ' || publicationDate) as note,
  prettyFileSize AS size,
  lastUpdated AS updated,
  st_envelope(
    geom_from_wkt('LINESTRING('||minX||' '||minY||', '||maxX||' '||maxY||')')
  ) as geometry
from bbox
where downloadUrl not like '%rockyftp%'
```

Note 1: "downloadURL AS downloadUrl" is necessary to get the capitalization correct for OpenIndexMaps.
Note 2: We omit records for the old "rockyftp" server, which are for old duplicate tiles.

- Under the "Geometry" section of the dialog, set the following:
    - Geometry column = geometry
    - Type = polygon
    - CRS = EPSG:4269

- Click "Add", then "Close"

- Right-click the virtual layer > Export > Save Features As...
   Save to a geojson file "ned1-index.geojson"
     - Set CRS = EPSG:4326 (this theoretically isn't needed when RFC7946=YES,
       but will help avoid export errors)
     - Set Layer Options > RFC7946 = YES (to follow the newer geojson standard)


# OpenIndexMaps

OpenIndexMaps provides a specification for encoding an index map in the GeoJSON format.  It provides details and access information about each individual mapsheet or sub-dataset, which can be represented either as points, lines, or polygons.

- [OpenIndexMaps.org](https://openindexmaps.org/) has the basic specification created in 2018.  It defines common properties that are assigned a specific meaning.

Currently, GeoBlacklight recognizes those properties defined by OpenIndexMaps.  Any other properties will be ignored and not displayed in the GBL interface.  (This may change in the future.)  Note also that GBL uses the "label" property to provide a mouseover tooltip on the map.

GeoJSON files are shared with others via the OpenIndexMaps organization on GitHub, where several institutions have repositories of index maps:

- [github.com/OpenIndexMaps](https://github.com/OpenIndexMaps) has repositories of index maps by institution

Note that many of the files found in those repos may include other local properties that are not defined by, but are still allowed by the specification.  Further work is currently underway to standarize more elements of an index map, including both the data provided for each map or sub-dataset, as well as information about the collection as a whole.

- [GIS Index Map Creation Requirements and Recommendations](https://docs.google.com/document/d/1GS1_4JmgUkZcehiG1qEyQB3e6mRQ7jdGC7rpyesZqIw/edit) is a draft document currently being developed by a group led by Tom Brittnacher (UCSB)

----

Next: [GeoJSON](geojson)

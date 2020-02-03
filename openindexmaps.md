# OpenIndexMaps

OpenIndexMaps is a community standard for encoding an index map in the GeoJSON format.   provides details and access information about each individual mapsheet or sub-dataset, which can be represented either as points, lines, or polygons.

- [OpenIndexMaps.org](https://openindexmaps.org/) has the basic specification created in 2018.

It describes a set of common properties that can be used to describe each item (map or sub-dataset) in an index map: available, recordIdentifier, downloadUrl, websiteUrl, thumbnailUrl, iiifUrl, label, title, and note.

Currently, GeoBlacklight recognizes just those properties defined by OpenIndexMaps.  Any other properties will be ignored and not displayed in the GBL interface.  (This may change in the future.)  Note also that GBL makes special use of the "label" property to provide a mouseover tooltip on the map.  GBL will also display the thumbnailUrl image along with the other OpenIndexMaps property values when a feature is clicked.

![label used as tooltip](https://kgjenkins.github.io/openindexmaps-workshop/image/label-tooltip.png)

GeoJSON files are shared with others via the OpenIndexMaps organization on GitHub, where several institutions have repositories of index maps:

- [github.com/OpenIndexMaps](https://github.com/OpenIndexMaps) has repositories of index maps by institution

Note that many of the files found in those repos may include other local properties that are not defined (but are still allowed) by the specification.  Further work is currently underway to standarize more elements of an index map, including both the data provided for each map or sub-dataset, as well as information about the collection as a whole.

- [GIS Index Map Creation Requirements and Recommendations](https://docs.google.com/document/d/1GS1_4JmgUkZcehiG1qEyQB3e6mRQ7jdGC7rpyesZqIw/edit) is a draft document currently being developed by a group led by Tom Brittnacher (UCSB)

----

Next: [GeoJSON](geojson)

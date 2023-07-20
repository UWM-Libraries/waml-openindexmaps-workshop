# OpenIndexMaps

OpenIndexMaps is a community standard for encoding an index map with details and access information about each individual map sheet or air photo frame.  The standard file format is the GeoJSON, which represents an index map as either points, lines, or polygons.

- [OpenIndexMaps.org](https://openindexmaps.org/specification/1.0.0) shows the latest specification finalized in 2021.

It describes a set of common properties that can be used to describe each item (map or air photo) in an index map.  The key is for each index map to use the same element names (also known as fields or column headers in an attribute table) and values (the terms entered into the cells in that column).  This commonality allows participating institutions to understand and interpret each others' index maps.

## Index maps in GeoBlacklight

Additionally, index maps created using the community standard are recognized by GeoBlacklight software.  While not all desired functionality has been implemented in GeoBlacklight yet, institutions can utilize the power of online index maps as an interactive finding aid.

GBL will make special use of the "label" property to provide a mouseover tooltip on the map:

![label used as tooltip](https://kgjenkins.github.io/openindexmaps-workshop/image/label-tooltip.png)

GBL will also display the thumbnailUrl image along with the other OpenIndexMaps property values when a feature is clicked:

![GeoBlacklight index map click](https://kgjenkins.github.io/openindexmaps-workshop/image/gbl-click.png)


## Sharing Index Maps

GeoJSON files are shared with others via the OpenIndexMaps organization on GitHub, where several institutions have repositories of index maps:

- [github.com/OpenIndexMaps](https://github.com/OpenIndexMaps) has repositories of index maps by institution

Note that many of the files found in those repos may include other local properties that are not defined (but are still allowed) by the specification.  Further work is currently underway to standarize more elements of an index map, including both the data provided for each map or sub-dataset, as well as information about the collection as a whole.

- [GIS Index Map Creation Requirements and Recommendations](https://docs.google.com/document/d/1GS1_4JmgUkZcehiG1qEyQB3e6mRQ7jdGC7rpyesZqIw/edit) is a draft document currently being developed by a group led by Tom Brittnacher (UCSB)

----

Next: [GeoJSON](geojson)

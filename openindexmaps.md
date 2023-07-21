# OpenIndexMaps

OpenIndexMaps is a community standard for encoding an index map with details and access information about each individual map sheet or air photo frame.  The standard file format is the GeoJSON, which represents an index map as either points, lines, or polygons.

- [OpenIndexMaps.org](https://openindexmaps.org/specification/1.0.0) shows the latest specification finalized in 2021.

It describes a set of common properties that can be used to describe each item (map or air photo) in an index map.  The key is for each index map to use the same element names (also known as fields or column headers in an attribute table) and values (the terms entered into the cells in that column).  This commonality allows participating institutions to understand and interpret each other's index maps.

## Index maps in GeoBlacklight

Additionally, index maps created using the community standard are recognized by GeoBlacklight software.  Institutions can utilize the power of online index maps as an interactive finding aid.

For example, GeoBlacklight will use the "label" property to provide a mouseover tooltip on the map:

![label used as tooltip](https://kgjenkins.github.io/openindexmaps-workshop/image/label-tooltip.png)

GeoBlacklight will also display the thumbnailUrl image along with the other OpenIndexMaps property values when a feature is clicked:

![GeoBlacklight index map click](https://kgjenkins.github.io/openindexmaps-workshop/image/gbl-click.png)

Not all functionality has been incorporated into GeoBlacklight yet.  For example, only the top polygon of overlapping polygons in an index map can be selected by a cursor, although overlapping polygons are suggested in the standard when multiple editions of a single map sheet are available.  

## Sharing Index Maps

The community also wanted a place to share these standard index maps so that other institutions can benefit from their work.  Therefore, the OpenIndexMaps GitHub site was created where several institutions have repositories of index maps:

- [github.com/OpenIndexMaps](https://github.com/OpenIndexMaps) has repositories of index maps by institution

Note that many of the files found in those repos may include other local properties that are not defined (but are still allowed) by the standard.  Also, some files predate the current version of the standard.
  Further work on the standard is still needed, including recommendations for entering information about the collection as a whole.  

----

Next: [GeoJSON](geojson)

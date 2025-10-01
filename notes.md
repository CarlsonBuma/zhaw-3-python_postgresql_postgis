# EPSG Codes - Spatial Reference
 - LV03: 21781
 - LV95: 2056
 - WGS84: 4326

## Example Projection of Data
"""SELECT
        osm_id,
        shop,
        st_transform(way, 4326) as geom
    FROM
        planet_osm_point
    WHERE
        shop = 'car_repair'
;"""

## Datatypes

Raster data
 - Subdivision of an area into elements of the same size 
 - consist of pixels, rows and columns, coordinate origin at a point
 - digital aerial photos, Satellite images, digital images, Scanned cards

Vector data
 - Points, lines, areas (polygons)
 - Vector data is an abstraction of reality

Factual data / attribute data
 - Non-geographic information on raster and vector data
 - Vector data set with associated attribute table

## GIS dataformats 
Are standardizedformatsthat are used tocollect, edit, organize, analyze and present geographic data (.shp, .kml, .wkt, .geojson, .geotiff).

Shapefile consists of least 3 individuel files, eg.:
 - lines.dbf, attribute data in dBASE format
 - lines.shp, is used to store the geometry data
 - lines.shx, serves as index of the geometry for linking the attribute data

# KML (Keyhole Markup Language) - .geojson / .geotiff
is a markup language for describing geodata. It became known through its use in the Google Earth program. KML follows the XML syntax. KML documents can contain geodata in both vector and raster form.

 - POINT(30 10)
 - LINESTRING(30 10, 10 30, 40 40)
 - Polygon((30 10, 10 30, 40 40, 50 50, 20 40), (...))


 ## Vector Data
  > Vector Data Editing see file_week3 S.57
# EPSG Codes - Spatial Reference
## Spatial Data Outputs GEOM
table.way : geometry(TYPESTRING, INT)
 - TypeStrings:
    - POINT(30 10)
    - LINESTRING(30 10, 10 30, 40 40)
    - Polygon((30 10, 10 30, 40 40, 50 50, 20 40), (...))

### Example Projection of Data
sql = """SELECT
            p.osm_id,
            p."addr:street",
            p."addr:housenumber",
            p."addr:postcode",
            p."addr:city",
            st_transform(p.way, 4326) as geom  --Geodata Output
         FROM
            planet_osm_polygon as p
         WHERE
            p."addr:street" = 'Stadthausstrasse'
            AND p.building IS NOT NULL
            AND p."addr:housenumber" IS NOT NULL
            AND p."addr:postcode" IS NOT NULL
            AND p."addr:city" IS NOT NULL
     ;"""

### Vizualization
 - LV03: 21781
 - LV95: 2056
 - WGS84: 4326
 
#### Projektion definieren (WGS84)
if gdf.crs is None:
    gdf.set_crs(epsg=4326, inplace=True)
else:
    pass

#### Latitude und Longitude für die Zentrierung der Karte ermitteln
centroids = gdf.geometry.centroid
lon = centroids.x.mean()
lat = centroids.y.mean()

#### Initialisieren der Map
m = folium.Map(
            location=[lat, lon], 
            zoom_start=10,

            ## Maptype 
            tiles='ESRIWorldImagery'
        ) 

#### Map settings
folium.Choropleth(
    geo_data=gdf,
    name='map',
    fill_color='greenyellow'
    popup=folium.GeoJsonPopup(fields=['name'])
).add_to(m)

### Plot map
folium.LayerControl().add_to(m)
m


# Datatypes
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




 ## Vector Data
  > Vector Data Editing see file_week3 S.57
# solar-radiation
Islands of Kauai, Oahu, Maui, Molokai and Hawaii
- The inspiration for this map comes from [Solar Insolation Ranges](https://hub.arcgis.com/datasets/HiStateGIS::solar-insolation-ranges) on ArcGIS Hub.

# Data
 `$ mapshaper Solar_Insolation_Ranges.shp -info`
 
 ```
[info]
Layer 1 *
Layer name: Solar_Insolation_Ranges
Records: 94
Geometry
  Type: polygon
  Bounds: -159.78812850652284 18.9106914560001 -154.80661999141634 22.23247182615665
  Proj.4: +proj=longlat +datum=WGS84
Attribute data
  Field       First value
  agency      ''
  comments    ''
  createdate  ''
  createdby   ''
  deliveryda  '2016-09-13T00:00:00.000Z'
  featureuid  '{F6578AF1-F36E-44A6-96C7-5EDEE164972A}'
  modifiedby  ''
  modifiedda  ''
  objectid      1
  publishdat  '1899-12-30T00:00:00.000Z'
  severity    ''
  solar_cal   '400-450'
  sourceid    ''
  st_areasha  306.31794582565857
  st_lengths    0
  ```
  
  Using mapshaper to clean up the data and convert to GeoJSON
  `$ mapshaper Solar_Insolation_Ranges.shp -filter-fields deliveryda,featureuid,publishdat,solar_cal,st_areasha,st_lengths -simplify dp 20% -o format=geojson solar-ranges.json`

```
[simplify] Repaired 3 intersections 
[o] Wrote solar-ranges.json
```
### Metadata
Metadata available for `solar-ranges.json` can be found within this [PDF](solrad.pdf "solar radiation").  Basically, the attribute `solar_cal` stands for *_estimated solar calories per square centimeter per day_*.

- wrangling data - the value *solar_cal* is a set of string ranges.

`ogr2ogr -f CSV ranges.csv Solar_Insolation_Ranges/Solar_Insolation_Ranges.shp -sql "select distinct solar_cal  from Solar_Insolation_Ranges order by solar_cal"`

Because this _CSV_ only has 9 distinct values, we just added a column and number 0-8 to create a numeric value with which to use with the map.  then to join the _CSV_ to the _solar-ranges.json_  we used the following _mapshaper_ script.
`
$ mapshaper solar-ranges.json -join ranges.csv keys=solar_cal,solar_cal -o force  format=geojson solar-ranges.json
`

# Hawaii GIS Data

Several layers from these sources will be downloaded to create a vector tilesets for the basemap.

- Climate [data](http://geodata.hawaii.gov/arcgis/rest/services/Climate/MapServer/) from Hawaii's geoportal.  [
- Hawaii GIS Data](http://planning.hawaii.gov/gis/download-gis-data/)
- [Solar Geography](http://solar.geography.hawaii.edu/)

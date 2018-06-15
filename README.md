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
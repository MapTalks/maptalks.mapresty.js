# maptalks.mapresty

[![CircleCI](https://circleci.com/gh/maptalks/maptalks.mapresty.js.svg?style=svg)](https://circleci.com/gh/maptalks/maptalks.mapresty.js)
[![Appveyor](https://ci.appveyor.com/api/projects/status/github/maptalks/maptalks.mapresty.js?branch=master&svg=true)](https://ci.appveyor.com/project/maptalks/maptalks-mapresty-js)

A MapResty's client for maptalks.js.

## Examples

### Usage
#### In Browser
```html
<script type="text/javascript" src="path/to/maptalks.js"></script>
<script type="text/javascript" src="path/to/maptalks.mapresty.js"></script>
<script type="text/javascript">
var featureQuery = new maptalks.FeatureQuery({
    //...
});
</script>
```

#### ES Modules
```js
import * as maptalks from 'maptalks';
//plugin's classes should be imported directly like
import { FeatureQuery, SpatialFilter } from 'maptalks.mapresty';

const filter = new SpatialFilter(
    geometry, 
    SpatialFilter['RELATION_INTERSECT'], 
    maptalks.CRS.BD09LL
);

const featureQuery = new FeatureQuery({
    spatialFilter : filter,
    //...
});
```

### Feature Query
```javascript
var featureQuery = new maptalks.FeatureQuery({
    'host': engineHost,//host of mapresty
    'port': enginePort,//port
    'mapdb': mapdb     //spatial db name
});

var spatialFilter = new maptalks.SpatialFilter(
    geometry, //geometry, can be a maptalks.Geometry or a geojson object

    //RELATION_CONTAIN, RELATION_DISJOINT, RELATION_OVERLAP, RELATION_TOUCH,
    //RELATION_WITHIN, RELATION_INTERECTNOTCONTAIN, RELATION_CONTAINCENTER, RELATION_CENTERWITHIN
    SpatialFilter['RELATION_INTERSECT'], 
    maptalks.CRS.BD09LL //CRS of spatial filter
);

//query data
featureQuery.query({
    'page' : 0,
    'count': 100,
    'layer': ['layer1', 'layer2'],
    'queryFilter': {
        'returnGeometry': true,
        'spatialFilter' : spatialFilter, // spatialFilter

        //maptalks.CRS.GCJ02, maptalks.CRS.EPSG4490, maptalks.CRS.EPSG3857, maptalks.CRS.EPSG4326
        'resultCRS': maptalks.CRS.BD09LL,
        'condition': cond,      //properties condition
        'resultFields': ['*']   //fields to return，['*'] means all the fields
    },
    //additional url parameters
    'urlParameters' : 'token=xxxx'
},function(err, data) {
    if (err) {
        alert('error when querying data', err);
        return;
    }
    //....
});

//count data
featureQuery.count({
    'layer': ['layer1', 'layer2'],
    'queryFilter': {
        'spatialFilter' : spatialFilter, // spatialFilter
        'condition': cond,      //properties condition
    },
    //additional url parameters
    'urlParameters' : 'token=xxxx'
},function(err, count) {
    if (err) {
        alert('error when querying data', err);
        return;
    }
    //....
});
```

### Spatial Query
```javascript
var topo = new maptalks.TopoQuery({
    'host' : engineHost,
    'port' : enginePort
});

//query if source is spatial related with targets in the given relation
topo.relate({
    'source' : sourceGeo,     //source
    'targets' : [targetGeo],  //geometries to query relation
    'relation'   : SpatialFilter['RELATION_INTERSECT'], //relation
    'urlParameters' : 'token=xxxx'
}, function (err, data) {
    if (err) {
        alert('error when querying relation', err);
        return;
    }
    //...
});

//query buffer of geometries
topo.buffer({
    'geometries' : [geometry],
    'distance'   : 100,      //buffer distance, in meters
    'urlParameters' : 'token=xxxx'
},function(err, data) {
    if (err) {
        alert('error when querying buffer', err);
        return;
    }
    //....
});
```

Original Repo: https://github.com/mapbox/shp-write

Updated to fix custom projection.

## Usage

    <script src=".../js/write-shp.js"></script>

## Caveats

- Requires a capable fancy modern browser with [Typed Arrays](http://caniuse.com/#feat=typedarrays)
  support
- Geometries: Point, LineString, Polygon, MultiLineString, MultiPolygon
- Tabular-style properties export with Shapefile's field name length limit
- Uses jsZip for ZIP files, but [compression is buggy](https://github.com/Stuk/jszip/issues/53) so it uses STORE instead of DEFLATE.

## Minimal Example

```js
// a GeoJSON bridge for features
const zipData = shpwrite.zip(
  {
    type: "FeatureCollection",
    features: [
      {
        type: "Feature",
        geometry: {
          type: "Point",
          coordinates: [0, 0],
        },
        properties: {
          name: "Foo",
        },
      },
      {
        type: "Feature",
        geometry: {
          type: "Point",
          coordinates: [0, 10],
        },
        properties: {
          name: "Bar",
        },
      },
    ],
  }
);

```

## Options Example

```js
const options = {
  folder: "my_internal_shapes_folder",
  filename: "my_zip_filename",
  outputType: "blob",
  compression: "DEFLATE",
  types: {
    point: "mypoints",
    polygon: "mypolygons",
    polyline: "mylines",
  },
};

// a GeoJSON bridge for features
const zipData = shpwrite.zip(
  {
    type: "FeatureCollection",
    features: [
      {
        type: "Feature",
        geometry: {
          type: "Point",
          coordinates: [0, 0],
        },
        properties: {
          name: "Foo",
        },
      },
      {
        type: "Feature",
        geometry: {
          type: "Point",
          coordinates: [0, 10],
        },
        properties: {
          name: "Bar",
        },
      },
    ],
  },
  options
);
```

## Custom .prj file
To pass a custom [WKT string](http://www.opengeospatial.org/standards/wkt-crs) in the .prj file to define a different projection the prj option can be used:

```js
var options = {
    prj: 'PROJCS["Amersfoort / RD New",GEOGCS["Amersfoort",DATUM["D_Amersfoort",SPHEROID["Bessel_1841",6377397.155,299.1528128]],PRIMEM["Greenwich",0],UNIT["Degree",0.017453292519943295]],PROJECTION["Stereographic_North_Pole"],PARAMETER["standard_parallel_1",52.15616055555555],PARAMETER["central_meridian",5.38763888888889],PARAMETER["scale_factor",0.9999079],PARAMETER["false_easting",155000],PARAMETER["false_northing",463000],UNIT["Meter",1]]'
}
```

Another example for set custom projection

```js
 const options = {
                folder: 'shp',
                filename: "my_zip_filename",
                outputType: "blob",
                types: {
                    point: "points",
                    polygon: "polygons",
                    polyline: "lines",
                },
                prj: 'PROJCS["WGS_1984_UTM_Zone_50S",GEOGCS["GCS_WGS_1984",DATUM["D_WGS_1984",SPHEROID["WGS_1984",6378137.0,298.257223563]],PRIMEM["Greenwich",0.0],UNIT["Degree",0.0174532925199433]],PROJECTION["Transverse_Mercator"],PARAMETER["False_Easting",500000.0],PARAMETER["False_Northing",10000000.0],PARAMETER["Central_Meridian",117.0],PARAMETER["Scale_Factor",0.9996],PARAMETER["Latitude_Of_Origin",0.0],UNIT["Meter",1.0]]',
            };
```
For the value of the PRJ can copy from https://epsg.io/ and search for a projection system or the desired EPSG code. Then select Esri WKT to copy the code.

Example: https://epsg.io/32750


## API
### `write(data, geometrytype, geometries, callback)`

Given data, an array of objects for each row of data, geometry, the OGC standard
geometry type (like `POINT`), geometries, a list of geometries as bare coordinate
arrays, generate a shapfile and call the callback with `err` and an object with

```js
{
    shp: DataView(),
    shx: DataView(),
    dbf: DataView()
}
```

### `zip(geojson, [options])`

Generate a ArrayBuffer of a zipped shapefile, dbf, and prj, from a GeoJSON
object.

### `download(geojson, [options])`

Given a [GeoJSON](http://geojson.org/) FeatureCollection as an object,
converts convertible features into Shapefiles and triggers a download. 

The additional `options` parameter is passed to the underlying `zip` call. 

This is now marked as deprecated because it applies to browsers only and the
user should instead rely on an external library for this functionality like
`file-saver` or `downloadjs`

## Other Implementations

- https://code.google.com/p/pyshp/

## Reference

- http://www.esri.com/library/whitepapers/pdfs/shapefile.pdf

## Contributors

- Nick Baugh <niftylettuce@gmail.com>


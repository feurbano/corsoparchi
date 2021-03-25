<p align="center"> <img src="materiale/loghi.png" width="315" height="100" /></p>

# WORK IN PROGRESS

#### Lezione 7
## Dati spaziali

Autore: Ferdinando Urbano  

---

1. Oggetti spaziali in PostGIS
2. Visualizzazione dei dati spaziali in PgAdmin
3. Creare un punto a partire dalle coordinate
4. Sistemi di riferimento e coordinate
x. Calcolare l'area di un poligono
5. Trovare in quale comune ricade un punto
6. Riproiettare le coordinate di un punto
7. Visualizzare una view in QGIS


Until some years ago, the spatial information produced by GPS sensors was managed and analyzed using dedicated software (GIS) in file-based data formats (e.g. shapefile). Nowadays, the most advanced approaches in data management consider the spatial component of objects (e.g. a set of locations, or even better, a moving animal) as one of its many attributes: thus, while understanding the spatial nature of your data is essential to proper analysis, from a software perspective spatial is (less and less) not special. Spatial databases are (at the moment) the best technical tool to implement this perspective in the ecology studies. They integrate spatial data types (vector and raster) together with standard data types that store the objects' other (non-spatial) associated attributes, with particular reference to time. Spatial data types can be manipulated by SQL through additional commands and functions for the spatial domain. This essentially allows you to build a GIS using the existing capabilities of relational databases. Moreover, while dedicated GIS software is usually focused on analyses and data visualization, providing a rich set of spatial operations, few are optimized for managing large spatial data sets (in particular, vector data) and complex data structures. Spatial databases, in turn, allow both advanced management and spatial operations that can be efficiently undertaken on a large set of elements. This combination of features is becoming essential, as with animal movement data sets the challenge is now on the extraction of synthetic information from very large data sets rather than on the extrapolation of new information (e.g. kernel home ranges from VHF data) from limited data sets with complex algorithms.  
There has also been an effort to [standardize](http://www.opengeospatial.org/) many aspects of spatial systems, which made data exchange between different platforms somewhat more comfortable and spatial database the option of choice for data management, leaving data visualization to more specific software tools. In fact, PostgreSQL/PostGIS offers no tool for spatial data visualization, but this can be done by a number of client applications, in particular GIS desktop software like ESRI Arc* or QGIS.

An ordinary database has strings, numbers, and dates. A spatial database adds additional (spatial) types for representing geographic features. These spatial data types abstract and encapsulate spatial structures such as boundary and dimension. In many respects, spatial data types (vector) can be understood simply as shapes: typically points, curves, surfaces and collections of them, in 2 or 3 dimensions. **[Raster Data](http://postgis.net/docs/manual-dev/RT_reference.html)** are discussed in a **[dedicated lesson](https://github.com/feurbano/data_management_2018/blob/master/sections/section_2/l2.13_raster.md)**.

For manipulating data during a query, an ordinary database provides functions such as concatenating strings, performing hash operations on strings, doing mathematics on numbers, and extracting information from dates. A spatial database provides a complete set of functions for analyzing geometric components, determining spatial relationships, and manipulating geometries. These spatial functions serve as the building block for any spatial project. The majority of all spatial functions can be grouped into one of the following five categories:

*  Conversion: Functions that convert between geometries and external data formats.
*  Management: Functions that manage information about spatial tables and PostGIS administration.
*  Retrieval: Functions that retrieve properties and measurements of a Geometry.
*  Comparison: Functions that compare two geometries with respect to their spatial relation.
*  Generation: Functions that generate new geometries from others.

The list of possible functions is very large, but a common set of functions is defined by the **[OGC SFSQL](http://www.opengeospatial.org/standards/sfs)**.

In the Open Source world (and in the world in general...), one of the richest implementations of the spatial SQL standards is provided by the **[PostGIS](http://postgis.net/)** extension for PostgreSQL - and that was one strong motivation for choosing this particular RDBMS for dealing with tracking data.

As we have seen before, RDBMS allow for storing and searching large amounts of data: to optimize access times, they make use of indexes which are often in the form of **[B-trees](http://en.wikipedia.org/wiki/B-tree)**. Spatial data require a different kind of indexes for efficient searching: spatial indexes are generally computed around the concept of *bounding box*. A bounding box is the smallest rectangle - parallel to the coordinate axes - capable of containing a given feature. Bounding boxes are used because answering the question "is A inside B?"  is very computationally intensive for polygons but very fast in the case of rectangles. Even the most complex polygons and linestrings can be represented by a simple bounding box.

Indexes have to perform quickly in order to be useful. So instead of providing exact results, spatial indexes provide approximate results very fast. The question "what lines are inside this polygon?"  will be instead interpreted by a spatial index as "what lines have bounding boxes that are contained inside this polygon’s bounding box?". Once the potential results are limited by the use of the index, more computation intense process (but now on few options!) are performed to find the exact answer.

## <a name="c_1.17"></a>1.17 Create a point from coordinates

If you visualize how the coordinates of a spatial objects are stored in the database, you see that the database representation is not meant for being readable:

``` sql
SELECT geom FROM main.gps_data_animals WHERE geom IS NOT NULL limit 5;
```

The geometry object is just a sequence of coordinates. To view a human-readable format you can use the function `ST_ASTEXT` or `ST_ASEWKT`:

``` sql
SELECT ST_ASTEXT(geom), ST_ASEWKT(geom) FROM main.gps_data_animals WHERE geom IS NOT NULL limit 5;
```

A point geometry can be created directly from pair of coordinates with `ST_MakePoint`:

```sql
SELECT ST_MakePoint(11.001,46.001) AS point;
```

Here you create the point object and then visualize it in a textual representation:

```sql
SELECT ST_AsText(ST_MakePoint(11.001,46.001)) AS point;
```

In a similar way, you can create a 3D object:

```sql
SELECT ST_AsText(ST_MakePoint(11.001,46.001, 100)) AS point;
```

There are many available spatial functions available in PostGIS: look them up in the **[PostGIS reference documentation](http://postgis.net/docs/manual-dev/reference.html)** to get a grasp of what kind of tools PostGIS will offer you.

In conformance with the standard, PostGIS offers a way to track and report on the geometry types available in a given database:

```sql
SELECT * FROM geometry_columns;
```

The previous query informs us that inside our database there are a number of tables with a geometry column, and that each of these columns is named `geom` and contains 2-dimensional data and they only accept a specific data type. Furthermore, they also carry information about the reference coordinate system in use, via the **[SRID](https://en.wikipedia.org/wiki/SRID)** parameter.

## <a name="c_1.18"></a>1.18 Reference systems and projections

The earth is approximately spheric whereas maps are two-dimensional. Projections are used to give such a two-dimensional representation of the earth. Many different projection systems exist, each using different mathematical formulas to estimate the earth on a flat surface. Some commonly used projections are the [(Transverse) Mercator projection](https://map-projections.net/compare.php?p1=mercator-84&p2=miller&w=0), the [Robinson projection](https://map-projections.net/compare.php?p1=robinson&p2=schjerning-1&w=0) and the [Lambert Conformal Conic](https://map-projections.net/compare.php?p1=lambert-conformal-conic&p2=mercator-84&w=0). Projections always give a certain distortion of the shape, size, distance and/or angle between different features on the earth's surface. For instance the Mercator projection, from which a variant (Spherical Normal equatorial Mercator projection) is used in google maps, shows large overestimation of the surface area towards the poles. At this website ([The True Size](https://thetruesize.com)) you can compare the size of a country at different latitudes using the Mercator projection. Here you can find some more interesting links to explore map projections and map distortion.

* [Map distortion](http://www.gis.osu.edu/misc/map-projections/)
* [Map characteristics](http://bl.ocks.org/syntagmatic/raw/ba569633d51ebec6ec6e/)
* [Compare projections](https://map-projections.net/imglist.php)

Coordinates alone do not allow to understand where on earth spatial objects (points, lines, polygons) are located. In addition a corresponding **geographical reference system** needs to be identified. A geographical reference system uses an ellipsoid and a datum including a reference zero X and Y axis, in order to assign coordinates to certain locations. Some commonly used geographical reference systems are the [World Geodetic System](http://spatialreference.org/ref/epsg/4326/) (WGS84, EPSG:4326), [the Projected coordinate system for Europe](http://spatialreference.org/ref/epsg/3035/) (ETRS89, EPSG:3035) and the [Universal Transverse Mercator coordinate system](https://gisgeography.com/utm-universal-transverse-mercator-projection/) (UTM).
There are two types of geographical reference systems, **global or spherical reference systems** and **projected reference systems** often defined more locally (e.g., country, continental). A typical example of a global reference system is EPSG:4326, which uses as geodetic datum and ellipsoid WGS84 and as zero reference axes the prime meridian at Greenwich (longitude) and the equator (latitude). Since this is a spherical reference system the measurement unit is in degrees (e.g., [we are here](https://goo.gl/maps/7WyJ7bYBp892): longitude = 11°08'10.7"E, latitude = 46°11'30.5"N). An example of a projected reference system is the Universal Transverse Mercator (UTM) coordinate system. In UTM the earth is devided into a grid, where each grid cell is projected using a standard set of map projections with a central meridian for each six-degree wide UTM zone. In UTM the measurement unit is in meters.

All spatial reference systems available in a postgresql-postgis spatial database can be called using:

```sql
SELECT * FROM spatial_ref_sys;
```

Each reference system has a specific spatial reference identifier (SRID). For instance, the World Geodetic System (SRID = 4326), the Projected coordinate system for Europe (SRID = 3035), UTM for North-Italy (SRID = 32632).

```sql
SELECT * FROM spatial_ref_sys WHERE srid in (4326, 3035, 32632);
```

The reference system of a spatial objects can be set as follows:
```sql
SELECT ST_SetSRID(ST_MakePoint(11.136293,46.191794),4326);
```

When using real world spatial data obtained from various sources, you will likely encounter different coordinate systems. One of the tasks that you will need to accomplish will be to re-project the data into a common SRID, in order to be able to do any useful work.

If you feed in geometries with differing SRIDs you will just get an error:
```sql
SELECT ST_Equals(
        ST_GeomFromText('POINT(0 0)', 4326),
        ST_GeomFromText('POINT(0 0)', 32632)
);
```

Once the SRID code is set you can transform it into another reference system:
```sql
SELECT ST_Transform(ST_SetSRID(ST_MakePoint(11.136293,46.191794),4326),32632);
```

If you compare, the units are clearly different (WGS84 = degrees; UTM = meters):
```sql
SELECT  ST_AsText(ST_SetSRID(ST_MakePoint(11.136293,46.191794),4326)) wgs84,
	ST_AsText(ST_Transform(ST_SetSRID(ST_MakePoint(11.136293,46.191794),4326),32632)) utm32;
```

**Take home message:**
Coordinates only do not identify a position on earth and the same position has different values according to the reference system.
When performing spatial operations (such as intersection, union, distance between points from two layers) always make sure the coordinate reference systems are the same.

In our database, we are not storing planar Euclidean coordinates, but use latitude and longitude to identify a point on the ellipsoid expressed by the geodetic datum WGS\_1984 - the one used globally by GPS systems.

##### EXERCISE
* Visualize the coordinates of your points in both WGS84 and UTM32 (SRID 32632)

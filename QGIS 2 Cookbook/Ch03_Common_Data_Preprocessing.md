
# Chapter 3: Common Data Preprocessing
<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [Chapter 3: Common Data Preprocessing](#chapter-3-common-data-preprocessing)
  * [3.1 Introduction](#31-introduction)
  * [3.2 Converting points to lines to polygons and back – QGIS](#32-converting-points-to-lines-to-polygons-and-back-qgis)
    * [Getting ready](#getting-ready)
    * [How to do it](#how-to-do-it)
    * [How it works](#how-it-works)
    * [There's more](#theres-more)
  * [3.3 Converting points to lines to polygons and back – SpatiaLite](#33-converting-points-to-lines-to-polygons-and-back-spatialite)
    * [Getting ready](#getting-ready-1)
    * [How to do it](#how-to-do-it-1)
    * [How it works](#how-it-works-1)
    * [There's more](#theres-more-1)
    * [See more](#see-more)
  * [3.4 Converting points to lines to polygons and back – PostGIS](#34-converting-points-to-lines-to-polygons-and-back-postgis)
    * [Getting ready](#getting-ready-2)
    * [How to do it](#how-to-do-it-2)
    * [How it works](#how-it-works-2)
    * [There's more](#theres-more-2)
    * [See more](#see-more-1)
  * [3.5 Cropping rasters](#35-cropping-rasters)
    * [Getting ready](#getting-ready-3)
    * [How to do it](#how-to-do-it-3)
    * [How it works](#how-it-works-3)
    * [There's more](#theres-more-3)
    * [See more](#see-more-2)
  * [3.6 Clipping vectors](#36-clipping-vectors)
    * [Getting ready](#getting-ready-4)
    * [How to do it](#how-to-do-it-4)
    * [How it works](#how-it-works-4)
    * [There's more](#theres-more-4)
    * [See more](#see-more-3)
  * [3.7 Extracting vectors](#37-extracting-vectors)
    * [Getting ready](#getting-ready-5)
    * [How to do it](#how-to-do-it-5)
    * [How it works](#how-it-works-5)
    * [There's more](#theres-more-5)
    * [See more](#see-more-4)
  * [3.8 Converting rasters to vectors](#38-converting-rasters-to-vectors)
    * [Getting ready](#getting-ready-6)
    * [How to do it](#how-to-do-it-6)
    * [How it works](#how-it-works-6)
    * [There's more](#theres-more-6)
    * [See more](#see-more-5)
  * [3.9 Converting vectors to rasters](#39-converting-vectors-to-rasters)
    * [Getting ready](#getting-ready-7)
    * [How to do it](#how-to-do-it-7)
    * [How it works](#how-it-works-7)
    * [There's more](#theres-more-7)
    * [See more](#see-more-6)
  * [3.10 Building DateTime strings](#310-building-datetime-strings)
    * [Getting ready](#getting-ready-8)
    * [How to do it](#how-to-do-it-8)
    * [How it works](#how-it-works-8)
    * [There's more](#theres-more-8)
    * [See more](#see-more-7)
  * [3.11 Geotagging photos](#311-geotagging-photos)
    * [Getting ready](#getting-ready-9)
    * [How to do it](#how-to-do-it-9)
    * [How it works](#how-it-works-9)
    * [There's more](#theres-more-9)
    * [See more](#see-more-8)

<!-- tocstop -->

In this chapter, we will cover the following recipes:
* Converting points to lines to polygons and back – QGIS
* Converting points to lines to polygons and back – SpatiaLite
* Converting points to lines to polygons and back – PostGIS
* Cropping rasters
* Clipping vectors
* Extracting vectors
* Converting rasters to vectors
* Converting vectors to rasters
* Building DateTime strings
* Geotagging photos

## 3.1 Introduction

When working with other people's data, it is often not the exact format that you need for a particular use.
This chapter is all about taking the data that you do have and converting it to what you actually need.
It covers converting between different types of vectors (points, lines, and polygons), between vectors and polygons, and cutting out only the parts that you need.
Taking data from how you get it and converting it to the format and layout that you need in order to work with is often called 'data preprocessing'.


## 3.2 Converting points to lines to polygons and back – QGIS

Sometimes your data is vector formatted (point, line, or polygon), but it is not the right kind   of vector for a particular type of analysis.
Or perhaps you need to split a vector in a particular way to facilitate some analysis or cartography.
Thankfully, all vector formats are related, lines are two or more connected points, polygons are lines whose first and last point are the same, multipolygons are two or more polygons for the same record, and rings are nested polygons where the inner polygon outlines an area to be excluded.
This recipe covers how to convert between the different vector types using built-in QGIS methods.


### Getting ready

To convert points to lines or polygons, you will need a shapefile with an ID column that has a single value shared between the points of the same line or polygon.
In the following example, we will use census_wake_2000_points.shp.

You will also need to install and activate the Points2One plugin.
Refer to the following website for how install plugins, http://docs.qgis.org/2.8/en/docs/user_manual/plugins/plugins.html.


### How to do it
### How it works
### There's more

You can also split or combine multipolygons with the Singleparts to Multiparts and Multiparts to Singleparts commands.
When things get really tricky, you may need to switch to editing the shapes by hand or custom scripts.
A good example of this is when you want a polygon with a hole in the middle.
If you do go the route of editing by hand, make sure to turn on snapping so that your lines are automatically snapped to existing points.
The official documentation on snapping can be found at http://docs.qgis.org/2.8/en/docs/user_manual/working_with_vector/editing_geometry_attributes.html#setting-the-snapping-tolerance-and-search-radius.
The Editing and Advanced Editing toolbars and additional editing related plugins offer the ability to manipulate particularly tricky geometries, one at a time, if you need to.


## 3.3 Converting points to lines to polygons and back – SpatiaLite

The goal of this recipe is identical to the previous recipe, but it covers how to perform the process with data in a SpatiaLite database.
You will to turn points into lines and lines into polygons.
Not all methods are available; for those that are not available, you can use the previous recipe.
It will also work on a database layer; it just doesn't save the results to the database.
So, the results will need to be imported to the database after completion.


### Getting ready

You need to load a vector layer of points with a numeric ID indicating order, and an identifier of unique lines or polygons that is shared between points of the same geometry.
For example, you can use census_wake_2000_points loaded into SpatiaLite with the geometry field called geom.


### How to do it

Using DB Manager Plugin (comes with QGIS and is in the Database menu), the QspatiaLite plugin, or an alternate SpatiaLite SQL application (command line or GUI), the following SQL examples will perform the conversions between vector types.


* __Points to lines__

1. Create a table with points grouped by common ID:
```
--Create table grouping points with shared stfid into lines
CREATE Table census_pts2lines AS
SELECT stfid,MakeLine(geom) as geom
FROM census_wake_2000_points
GROUP BY stfid;
```
The following screenshot shows what the screen will look like:
Common Data Preprocessing Steps

2. Register the new table as spatial:
```
--Register the new table's geometry so QGIS knows its a spatial layer
SELECT
RecoverGeometryColumn('census_pts2lines','geom',3358,'LINES TRING',2);
```
Some SQL interfaces can run multiple SQL statements in a row, separated by a semicolon.
However, there are also many interfaces that can only perform one query at a time.
Generally, run one query at a time unless you know your software supports multiple queries; otherwise, this may fail or silently only run the first query.


* __Lines to polygons__

1. Create a table with lines grouped by common ID:
```
--Create table grouping lines with shared stfid into polygons
CREATE Table census_line2poly AS
SELECT stfid,ST_Polygonize(geom) as geom
FROM census_pts2lines
GROUP BY stfid;
```

2. Register the new table as spatial:
```
--Register the new table's geometry so QGIS knows its a spatial layer
SELECT
RecoverGeometryColumn('census_line2poly','geom',3358,'POLYGON',2);
```

> Double dashes (--) is the SQL character for a comment line.
It is used to include descriptive text that is ignored in a query.


### How it works

### There's more

### See more

* SpatiaLite does have functions to dump specific points by first, last, or ID, one at a time. Refer to the index of functions (Reference Guide) online for details at https://www.gaia-gis.it/fossil/libspatialite/index
* The Converting points to lines to polygons and back – QGIS recipe in this chapter for nondatabase methods


## 3.4 Converting points to lines to polygons and back – PostGIS

The goal of this recipe is identical to the previous two recipes, but it covers how to perform the process with data in a PostGIS database.
You will use it to turn points into lines, and lines into polygons.

Not all methods are available; for those not available, you can use the previous recipe.
It will also work on a database layer; it just doesn't save the results to the database.
So, the results will need to be imported to the database after completion.


### Getting ready
### How to do it
### How it works
### There's more
### See more

* For more details on the dump functions of PostGIS (ST_Dump, ST_DumpPoints, and ST_DumpRings), refer to the PostGIS manual at http://postgis.net/docs/manual-2.1/reference.html
* Refer to the Converting points to lines to polygons and back – QGIS recipe in this chapter for the non-database methods


## 3.5 Cropping rasters
### Getting ready

### How to do it

The easiest way to do this is to use a polygon mask layer. The vector mask can be a rectangle, but it doesn't have to be. The outline of a single polygon works best, though.

> An alternate method would be to determine the bounding box (bbox) coordinates of the extent that you want with the Capture Coordinate tool or to draw the rectangle directly on the map.

1.  Go to Raster | Extraction | Clipper.
2.  Set Input file (raster) as elev_state_500m.tif.
3.  Set Output file using the Select button to pick a directory, and name the output elev_wake_500m.tif.
4.  Set No data value to -9999.

> Why -9999?
Setting No data value to something impossible makes it more obvious later to other users.
The value 0 is a really bad choice as data can legitimately have a value of zero.
As some raster formats only support numbers and, in particular, integers, a large negative number is a common choice.

5.  Now change Clipping mode to Mask Layer and select county_wake.shp as Mask Layer:


### How it works

### There's more

You'll notice that with this particular example, an issue that arises with converting rasters to nonrectangular shapes; the edges are jagged as compared to the vector.
If you need it to be really smooth, there are a few options.
You can decrease the pixel size, splitting current pixels into multiple pixels using the -tr option.
As with other Raster tools, you can use the pencil icon to override the GDAL command-line options to add features not included in the interface.
In this case, the -tr option inline with the rest of the already formatted command:
```python
gdalwarp -tr 100 100
```
This would make each pixel 100 units instead of the current 500 x 500.

>Some important options to remember when saving TIFF files with GDAL are number type (Integer versus Float) and compression.
Both of these can greatly impact the final file size.
Refer to the Converting Vectors to Rasters recipe later in this chapter for an example.
Also, if you have a multicore CPU, add -multi to take advantage of your CPU cores for faster processing of most raster operations.


### See more

* For a full list of the gdalwarp options refer to http://gdal.org/gdalwarp.html

## 3.6 Clipping vectors
### Getting ready

### How to do it
### How it works
### There's more
### See more

## 3.7 Extracting vectors
### Getting ready

### How to do it
### How it works
### There's more
### See more

## 3.8 Converting rasters to vectors
### Getting ready

### How to do it
### How it works
### There's more
### See more

## 3.9 Converting vectors to rasters

### Getting ready

### How to do it

In order to be useful in analysis, here's a checklist:

1. Is the vector data in the same projection as the rest of the raster analysis data? If not, reproject it first. Check the following URL for help, https://docs.qgis.org/2.8/en/docs/training_manual/vector_analysis/reproject_transform.html.
2. Clip the vector data to the analysis extent. You may need to convert a raster into a polygon mask to clip it (refer to the clipping vectors recipe earlier in this chapter).

> You can only pick numeric fields as raster formats only store a single number per cell.
In order to keep the attribute that you want, you may need to use the field calculator to create a new field that reclassifies categories of text into a numeric scheme (for example, 1 = water, 2 = land, and so on) before performing the conversion.
If you copy a unique ID as the attribute, there are some tools later that let you rejoin the original attribute table as a value attribute table (refer to the GRASS functions in Processing Toolbox).


3. Load geology_wake2000reclass.shp and elev_wake_500m.tif.
4. Open Properties of elev_wake_500m.tif:
1. In the Metadata section, scroll down to Dimensions. You will want to match either the dimensions or resolution so that your new raster will match the existing elevation data pixels.
2. Note that Dimensions are X: 134, Y: 124 and Bands: 1. The resolution is 499.637,-498.342 pixel size.
3. Close the dialog.

5.  Now, open the conversion dialog by navigating to Raster | Conversion | Rasterize and follow these steps:
1. Input the file as geology_wake2000reclass.
2. Name your output geology_wake.tif (you will get a warning to set the size or resolution).
3. Set the raster size: Width to 134 and Height to 124.
4. Click on OK to run the process.

The following screenshot shows how the screen will look:


### How it works

### There's more

### See more

## 3.10 Building DateTime strings
### Getting ready

### How to do it
### How it works
### There's more
### See more

* If using database layers, this can all be performed with SQL in SpatiaLite or PostGIS.
The ISO date time standard is 8601, and can be found at http://en.wikipedia.org/wiki/ISO_8601.



## 3.11 Geotagging photos

Newer cameras and phones with built-in GPS can be wonderful tools for data collection, as they help keep track of exactly where and when a picture was taken.
However, not all cameras have a built-in GPS.
You can add geotags afterwards, either with a GPS log from a separate GPS unit or just using a reference map and your memory or notes.



### Getting ready

For this recipe, you'll need a some photos and either a GPS log (*.gpx), reference vector, reference raster, or coordinates.
We've provided centerofcalifornia.jpg in the geotag folder, and the coordinates are in the image itself but also included as a point in centerofcalifornia.shp.

You will also need the Geotag photos plugin, which requires the exiftool program to be installed on your system.
If exiftool didn't come with your install, you can easily get it from the Web at http://www.sno.phy.queensu.ca/~phil/exiftool/ or at package repositories (Linux).



### How to do it

### How it works

### There's more

### See more

* The OpenStreetMap wiki lists other free and paid options out there at http://wiki.openstreetmap.org/wiki/Geotagging_Source_Photos



```python

```

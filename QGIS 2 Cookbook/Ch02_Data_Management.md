
# Chapter 2: Data Management
<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [Chapter 2: Data Management](#chapter-2-data-management)
  * [2.1 Introduction](#21-introduction)
  * [2.2 Joining layer data](#22-joining-layer-data)
    * [Getting ready](#getting-ready)
    * [How to do it](#how-to-do-it)
    * [How it works](#how-it-works)
    * [There's more](#theres-more)
  * [2.3 Cleaning up the attribute table](#23-cleaning-up-the-attribute-table)
    * [Getting ready](#getting-ready-1)
    * [How to do it](#how-to-do-it-1)
    * [How it works](#how-it-works-1)
    * [There's more](#theres-more-1)
  * [2.4 Configuring relations](#24-configuring-relations)
    * [Getting ready](#getting-ready-2)
    * [How to do it](#how-to-do-it-2)
    * [How it works](#how-it-works-2)
    * [There's more](#theres-more-2)
  * [2.5 Joining tables in databases](#25-joining-tables-in-databases)
    * [Getting ready](#getting-ready-3)
    * [How to do it](#how-to-do-it-3)
    * [How it works](#how-it-works-3)
    * [There's more](#theres-more-3)
    * [See Also](#see-also)
  * [2.6 Creating views in SpatiaLite](#26-creating-views-in-spatialite)
    * [Getting ready](#getting-ready-4)
    * [How to do it](#how-to-do-it-4)
    * [How it works](#how-it-works-4)
    * [There's more](#theres-more-4)
    * [See Also](#see-also-1)
  * [2.7 Creating views in PostGIS](#27-creating-views-in-postgis)
    * [Getting ready](#getting-ready-5)
    * [How to do it](#how-to-do-it-5)
    * [How it works](#how-it-works-5)
    * [There's more](#theres-more-5)
  * [2.8 Creating spatial indexes](#28-creating-spatial-indexes)
    * [Getting ready](#getting-ready-6)
    * [How to do it](#how-to-do-it-6)
    * [How it works](#how-it-works-6)
    * [There's more](#theres-more-6)
    * [See Also](#see-also-2)
  * [2.9 Georeferencing rasters](#29-georeferencing-rasters)
    * [Getting ready](#getting-ready-7)
    * [How to do it](#how-to-do-it-7)
    * [How it works](#how-it-works-7)
    * [There's more](#theres-more-7)
    * [See Also](#see-also-3)
  * [2.10 Georeferencing vector layers](#210-georeferencing-vector-layers)
    * [Getting ready](#getting-ready-8)
    * [How to do it](#how-to-do-it-8)
    * [How it works](#how-it-works-8)
    * [There's more](#theres-more-8)
    * [See Also](#see-also-4)
  * [2.11 Creating raster overviews (pyramids)](#211-creating-raster-overviews-pyramids)
    * [Getting ready](#getting-ready-9)
    * [How to do it](#how-to-do-it-9)
    * [How it works](#how-it-works-9)
    * [There's more](#theres-more-9)
    * [See Also](#see-also-5)
  * [2.12 Building virtual rasters (catalogs)](#212-building-virtual-rasters-catalogs)
    * [Getting ready](#getting-ready-10)
    * [How to do it](#how-to-do-it-10)
    * [How it works](#how-it-works-10)
    * [There's more](#theres-more-10)
    * [See more](#see-more)

<!-- tocstop -->
<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [Chapter 2: Data Management](#chapter-2-data-management)
  * [2.1 Introduction](#21-introduction)
  * [2.2 Joining layer data](#22-joining-layer-data)
    * [Getting ready](#getting-ready)
    * [How to do it](#how-to-do-it)
    * [How it works](#how-it-works)
    * [There's more](#theres-more)
  * [2.3 Cleaning up the attribute table](#23-cleaning-up-the-attribute-table)
    * [Getting ready](#getting-ready-1)
    * [How to do it](#how-to-do-it-1)
    * [How it works](#how-it-works-1)
    * [There's more](#theres-more-1)
  * [2.4 Configuring relations](#24-configuring-relations)
    * [Getting ready](#getting-ready-2)
    * [How to do it](#how-to-do-it-2)
    * [How it works](#how-it-works-2)
    * [There's more](#theres-more-2)
  * [2.5 Joining tables in databases](#25-joining-tables-in-databases)
    * [Getting ready](#getting-ready-3)
    * [How to do it](#how-to-do-it-3)
    * [How it works](#how-it-works-3)
    * [There's more](#theres-more-3)
    * [See Also](#see-also)
  * [2.6 Creating views in SpatiaLite](#26-creating-views-in-spatialite)
    * [Getting ready](#getting-ready-4)
    * [How to do it](#how-to-do-it-4)
    * [How it works](#how-it-works-4)
    * [There's more](#theres-more-4)
    * [See Also](#see-also-1)
  * [2.7 Creating views in PostGIS](#27-creating-views-in-postgis)
    * [Getting ready](#getting-ready-5)
    * [How to do it](#how-to-do-it-5)
    * [How it works](#how-it-works-5)
    * [There's more](#theres-more-5)
  * [2.8 Creating spatial indexes](#28-creating-spatial-indexes)
    * [Getting ready](#getting-ready-6)
    * [How to do it](#how-to-do-it-6)
    * [How it works](#how-it-works-6)
    * [There's more](#theres-more-6)
    * [See Also](#see-also-2)
  * [2.9 Georeferencing rasters](#29-georeferencing-rasters)
    * [Getting ready](#getting-ready-7)
    * [How to do it](#how-to-do-it-7)
    * [How it works](#how-it-works-7)
    * [There's more](#theres-more-7)
    * [See Also](#see-also-3)
  * [2.10 Georeferencing vector layers](#210-georeferencing-vector-layers)
    * [Getting ready](#getting-ready-8)
    * [How to do it](#how-to-do-it-8)
    * [How it works](#how-it-works-8)
    * [There's more](#theres-more-8)
    * [See Also](#see-also-4)
  * [2.11 Creating raster overviews (pyramids)](#211-creating-raster-overviews-pyramids)
    * [Getting ready](#getting-ready-9)
    * [How to do it](#how-to-do-it-9)
    * [How it works](#how-it-works-9)
    * [There's more](#theres-more-9)
    * [See Also](#see-also-5)
  * [2.12 Building virtual rasters (catalogs)](#212-building-virtual-rasters-catalogs)
    * [Getting ready](#getting-ready-10)
    * [How to do it](#how-to-do-it-10)
    * [How it works](#how-it-works-10)
    * [There's more](#theres-more-10)
    * [See more](#see-more)

<!-- tocstop -->


In this chapter, we will cover the following recipes:

* Joining layer data
* Cleaning up the attribute table
* Configuring relations
* Joining tables in databases f Creating views in SpatiaLite f Creating views in PostGIS
* Creating spatial indexes
* Georeferencing rasters
* Georeferencing vector layers
* Creating raster overviews (pyramids)
* Building virtual rasters (catalogs)


## 2.1 Introduction

One of the reasons to use QGIS is its many features that enable management and analysis preparation of spatial data in a visual manner.
This chapter focuses on common operations that users need to perform to get data ready for other uses, such as analysis, cartography, or input into other programs.

In this chapter, you will find recipes to manage vector as well as raster data.
These recipes cover the handling of data from both file and database sources.


## 2.2 Joining layer data

We often get data in different formats and information spread over multiple fi s.
Therefore, one important skill to know is how to join attribute data from different layers.
Joining data is a way to combine data from multiple tables based on common values, such as IDs or categories.

This exercise shows you how to use the join functionality in Layer Properties to join geographic census tract data to tabular population data and how to save the results to a new fi  .


### Getting ready
### How to do it
### How it works

Joins can be used to join vector layers and tabular layers from many different file and database sources, including (but not limited to) Shapefile PostGIS, CSV, Excel sheets, and more.

When two layers are joined, the attributes of Join layer are appended to the original layer's attribute table.
If you want, you can use the Choose which fields are joined option to select which of the fields from the population layer should be joined to the census tracts.
Otherwise, by default, all fields will be added.
The number of features in the original layer is not changed.
Whenever there is a match between the values in the join and the target field, the new attribute values will be filled; otherwise, there will be NULL values in the new columns.

By default, the names of the new columns are constructed from join layer name with underscore followed by join layer column name.
For example, the STATE column of census_wake2000_pop becomes census_wake2000_pop_STATE.
You can change this default behavior by enabling the Custom field name prefix option, as shown in the previous screenshot.
With these settings, the STATE column becomes pop_STATE, which is considerably shorter and, thus, easier to handle.


### There's more

The join that you've created now only exists in memory.
None of the original files have been altered.
However, it's possible to create a new file from the joined layers.
To do this, just use Save as … from the Layer menu or Context menu.
You can choose between a variety of data formats, including the ESRI shapefile, Mapinfo MIF, or GML.

Shapefiles are a very common choice as they are still the de facto standard GIS data exchange format, but if you are familiar with GIS data formats, you will have noticed that the names of the joined columns are too long for the 10 character-name length limit of the shapefile format.
QGIS ensures that all columns in the exported shapefiles have unique names even after the names have been shortened to only 10 characters.
To do this, QGIS adds incrementing numbers to the end of, otherwise, duplicate column names.
If you save the join from this example as a shapefile, you will see that the column names are altered to census_w_1,  census_w_2, and so on.
Of course, these names are less than optimal to continue working with the data.
As described in How it works... in this recipe, the names for the joined columns are a combination of joined layer name and column name.

Therefore, we can use the following trick if we want to create a shapefile from the join: we can shorten the layer name.
Just rename the layer in the layer list.
You can even have a completely empty layer name! If you change the joined layer name to an empty string, the joined column names will be _STATE, _COUNTY, and so on instead of census_wake2000_pop_STATE and census_wake2000_pop_COUNTY.
In any case, it is good practice to document your data and provide a description of the attribute table columns in the metadata.

In any case, it is very likely that you will want to clean up the attribute table of the new dataset, and this is exactly what we are going to do in the next exercise.


## 2.3 Cleaning up the attribute table

### Getting ready

### How to do it

### How it works

> The GDAL/OGR version that is used by QGIS is either part of the QGIS package (as in the case of the Windows installers) or QGIS uses the GDAL library existing in your system (on Linux and Mac).
To get access to specific drivers that are not supported by the provided GDAL/OGR version, it is possible to compile custom versions of GDAL/OGR, but the details of doing this are out of the scope of this cookbook.


### There's more

## 2.4 Configuring relations

__In the Joining layer data recipe, we discussed that joins only append additional columns to existing features (1:1 or n:1 relationships).__
Using joins, it is, therefore, not possible to model 1:n relationships, such as "one zip code area containing n schools".
These kinds of relationships can instead be modeled using relations.
This recipe introduces the concept of relations and shows how you can put them to use.


### Getting ready

### How to do it

### How it works

### There's more

## 2.5 Joining tables in databases

If you use a database (SpatiaLite or PostGIS) to store your data, vector and nonspatial, then you also have the option of using the database and SQL to perform tables joins.
The primary advantages of this method include being able to filter data before loading in the map, perform multitable joins (three or more), and have full control over the details of the join via queries.



### Getting ready

### How to do it

1.  Open the DB Manager plugin that comes with QGIS. You can find this in the
Database menu.

2.  Select your database from the tree on the left-hand side, use cookbook.db in
SpatiaLite (which was created in Chapter 1, Data Input and Output).
> If you don't see this database listed, use Add SpatiaLite Layer (the icon or the menu item), or right-click on SpatiaLite in the Browser window to make a new connection and add it to an existing database.

3.  Now, open the SQL window (the second icon from the left in top toolbar of the plugin window).
4.  Put in the following SQL code to query and JOIN the tables:
```sql
SELECT *
FROM census_wake2000 Sas a
JOIN census_wake2000_pop AS b
ON a.stfid = b.stfid;
```


### How it works

### There's more

### See Also

* Refer to the Creating views in SpatiaLite and Creating views in PostGIS sections in this chapter to learn how to make views of the query results.
* For more general information on writing SQL queries refer to http://sqlzoo.net/
* Refer to Chapter 1, Data Input and Output, about using the cookbook.db database


## 2.6 Creating views in SpatiaLite

In a database, view is a stored query.
Every time you open it, the query is run and fresh results are generated.
To use views as layers in QGIS takes a couple of steps.

### Getting ready

For this recipe, you'll need a query that returns results containing a geometry.
The example that we'll use is the query from the Joining tables in databases recipe (the previous recipe) where attributes were joined 1:1 between the census polygons and the population CSV.
The QSpatiaLite plugin is recommended for this recipe.

### How to do it

### How it works

### There's more

### See Also

Read more about writable-view at https://www.gaia-gis.it/fossil/libspatialite/wiki?name=writable-view.
This recipe is extremely similar to the next one on PostGIS and demonstrates how interchangeable the two can be if you are aware of the slight differences.


## 2.7 Creating views in PostGIS

### Getting ready

### How to do it

### How it works

### There's more

* Finer details from the PostGIS manual can be read at http://postgis.refractions.net/docs/using_postgis_dbmanagement.html#Manual_Register_Spatial_Column.
This recipe is extremely similar to the previous one and demonstrates how interchangeable these two can be if you are aware of the slight differences.
* Read more about Materialized Views at http://www.postgresql.org/docs/9.3/static/rules-materializedviews.html


## 2.8 Creating spatial indexes

Spatial indexes are methods to speed up queries of geometries.
This includes speeding up the display of database layers in QGIS when you zoom in close (it has no effect on viewing entire layers).
This recipe applies to SpatiaLite and PostGIS databases.
In the event that you've made a new table or you have imported some data and didn't create a spatial index, it's usually a good idea to add this.

> You can also create a spatial index for shapefile layers.
Take a look at Layer Properties | General for the Create Spatial Index button.
This will create a .qix file that works with QGIS, Mapserver, GDAL/OGR, and other open source applications.
Refer to https://en.wikipedia.org/wiki/Shapefile.

### Getting ready

### How to do it

### How it works

### There's more

### See Also

* Information on SpatiaLite spatial index implementation can be found at https://www.gaia-gis.it/fossil/libspatialite/wiki?name=SpatialIndex
* More details on using spatial indexes can be found at https://www.gaia-gis.it/fossil/libspatialite/wiki?name=SpatialIndex
* Information about PostGIS implementation is at http://postgis.net/docs/manual-2.0/using_postgis_dbmanagement.html#gist_indexes
* You can also check out Chapter 10, Maintenance, Optimization, and Performance Tuning, of PostGIS Cookbook by Packt Publishing,


## 2.9 Georeferencing rasters

Sometimes, you have a paper map, an image of a map from the Internet, or even a raster file with projection data included.
When working with these types of data, the first thing you'll need to do is reference them to existing spatial data so that they will work with your other data and GIS tools.
This recipe will walk you through the process to reference your raster (image) data, called georeferencing.


### Getting ready

### How to do it

### How it works

### There's more

### See Also

* When performing georeferencing in a setting where you need it to be very accurate (science and surveying), you should read up on the different transformations and what RMSE values are good for your type of data. Refer to the general GIS or Remote Sensing textbooks for more information.
* For full details of all the features of the QGIS georeferencer, refer to the online manual at http://docs.qgis.org/2.8/en/docs/user_manual/plugins/plugins_georeferencer.html.
* The QGIS documentation has some basic information about how to pick transformation type at http://docs.qgis.org/2.8/en/docs/user_manual/plugins/plugins_georeferencer.html#available-transformation-algorithms.


## 2.10 Georeferencing vector layers

For various reasons, sometimes you have a vector layer that lacks projection information.
This is often the case with CAD layers that were created only in local coordinates.
When it is possible, try to track down the original projection information.
As a last resort, you can attempt to warp the vector layer to match a known reference layer with the recipe described here.


### Getting ready

### How to do it

### How it works

### There's more

### See Also

* This method is very similar to the Georeferencing Rasters recipe and many of the same tips apply to both
* If you want to see how we got the CAD file into an SHP to begin with, look at Importing DXF/DWG files in the Chapter 1, Data Input and Output
* See the Using Rule Based Rendering recipe in Chapter 10, Cartography Tips, for tips on how to visualize the resulting CAD import better by applying attribute based rule filtering
* Too lazy to do the math? You can also just use GvSig to do the math and make a world file; refer to http://foss4gis.blogspot.com/2011/05/computing-and-applying-affine.html
* If you want to do the math yourself see http://press.underdiverwaterman.com/rotating-a-point-grid-in-qgis/


## 2.11 Creating raster overviews (pyramids)

Overviews, or pyramids, and resampling are all about making raster layers load faster when zooming and panning in your map canvas, by reducing the amount of data loaded when not zoomed in all the way.

### Getting ready

### How to do it

### How it works

### There's more

### See Also

* This is the same effect as using the GDAL gdaladdo command; refer to http://gdal.org/gdaladdo.html
* More details from the QGIS documentation can be found at https://docs.qgis.org/2.8/en/docs/user_manual/working_with_raster/raster_properties.html


## 2.12 Building virtual rasters (catalogs)

When you have a lot of rasters (instead of one big raster) that are all part of the same dataset (typically adjacent to each other), you don't want to load each file individually and then style it.
It's much easier to load one file and treat it as one layer.
This recipe lets you do this without actually creating a single monstrous raster, which can be difficult to work with.


### Getting ready

You will need two or more raster files that have adjacent extents or only overlap partially around the edges and are in the same projection.
Ideally, the files should be of the same type, such as all elevations, all air photos, and so on.
For this recipe, the elevation rasters from the OSGeo EDU (North Carolina) dataset will work.


### How to do it

1. (Optional) Load the elevation rasters to your current map.
2. Go to Raster Menu | Miscellanous | Build Virtual Raster (Catalog).
3. Check the Use visible raster layers checkbox or choose SELECT, browse to the example data, and select all four.
4. SELECT and name an output file using the .vrt extension.
5. (Optional) Check the Load into canvas when finished checkbox if you want to see the results immediately:


> GDAL command line equivalent: <command> <output.vrt> <list of inputs... space between each...>
For example, gdalbuildvrt elevlid.vrt elevlid_D782_6.tif elevlid_D783_6m.tif elevlid_D792_6m.tif elevlid_D793_6m.tif.

### How it works

### There's more

Using a VRT is all about saving time.
When you have hundreds of raster files for one particular dataset, you can combine them into a single file.
However, this file could be gigantic in size and somewhat impossible to work with.
This is a quick way to be able use the files as a seamless background layer.
If you need to perform analysis, you'll likely need to either combine the layers or loop over them individually.

You could also generate Tile Index (also in the Miscellaneous menu), which makes a shapefi of the outlines of the rasters and puts the ID and path of the raster in the attribute table.
This would allow you to fi  e out which image you want to load for a given map without having to load them all.

Finally, if you really want to make all of the files a single large file, use the context menu (right-click on the loaded VRT layer and choose Save As).
If you have overlaps, more complicated situations, or want to merge without loading the files, first use the Merge tool (also in the Miscellaneous menu).
This can be tricky if your files overlap, you'll need to decide how to handle the double data.



### See more

* For another example, please refer to http://manual.linfiniti.com/en/rasters/data_manipulation.html#basic-fa-create-a-virtual-raster
* GDAL's gdalbuildvrt is the underlying tool; it's documentation can be found at http://gdal.org/gdalbuildvrt.html


```python

```

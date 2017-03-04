
# Chapter 1: Data Input and Output
<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [Chapter 1: Data Input and Output](#chapter-1-data-input-and-output)
  * [1.1 Introduction](#11-introduction)
  * [1.2 Finding geospatial data on your computer](#12-finding-geospatial-data-on-your-computer)
  * [1.3 Describing data sources](#13-describing-data-sources)
  * [1.4 Importing data from text files](#14-importing-data-from-text-files)
  * [1.5 Importing KML/KMZ files](#15-importing-kmlkmz-files)
  * [1.6 Importing DXF/DWG files](#16-importing-dxfdwg-files)
  * [1.7 Opening a NetCDF file](#17-opening-a-netcdf-file)
  * [1.8 Saving a vector layer](#18-saving-a-vector-layer)
  * [1.9 Saving a raster layer](#19-saving-a-raster-layer)
  * [1.10 Reprojecting a layer](#110-reprojecting-a-layer)
  * [1.11 Batch format conversion](#111-batch-format-conversion)
  * [1.12 Batch reprojection](#112-batch-reprojection)
  * [1.13 Loading vector layers into SpatiaLite](#113-loading-vector-layers-into-spatialite)
  * [1.14 Loading vector layers into PostGIS](#114-loading-vector-layers-into-postgis)
    * [1.14.1 Getting ready](#1141-getting-ready)
    * [1.14.2 How to do it](#1142-how-to-do-it)
    * [1.14.3 How it works](#1143-how-it-works)
    * [1.14.4 There's more](#1144-theres-more)
    * [1.14.5 See also](#1145-see-also)

<!-- tocstop -->

In this chapter, we will cover the following recipes:
* Finding geospatial data on your computer
* Describing data sources
* Importing data from text files
* Importing KML/KMZ files
* Importing DXF/DWG files
* Opening a NetCDF fie
* Saving a vector layer
* Saving a raster layer
* Reprojecting a layer
* Batch format conversion
* Batch reprojection
* Loading vector layers into SpatiaLite
* Loading vector layers into PostGIS

## 1.1 Introduction


```python

```

## 1.2 Finding geospatial data on your computer

## 1.3 Describing data sources

* See also
This is a more complex way to retrieve properties as you can call the tool by adjusting the parameters with more details to get additional information.
To know more, check the gdalinfo help page at http://www.gdal.org/gdalinfo.html


## 1.4 Importing data from text files

* See also
To know more about the WKT format, you can go to http://en.wikipedia.org/wiki/Well-known_text

## 1.5 Importing KML/KMZ files

## 1.6 Importing DXF/DWG files

* __Opening DWG files__

DWG is a closed format of Autodesk.
This means that the specifiation of the format is not available.
For this reason, QGIS, like other open source applications, does not support DWG fies.
To open a DWG fie in QGIS, you need to convert it.
Converting it to a DXF fie is a good option as this will let you open your fie in QGIS without any problem.
There are many tools to do this.
The Teigha converter can be found at http://opendesign.com/guestfiles/TeighaFileConverter (obsolete) and is a popular and reliable option.
(update: https://www.opendesign.com/guestfiles/teigha_file_converter)

Another option is using the free service offered by Autodesk, called Autocad 360, which can be found at https://www.autocad360.com/.


## 1.7 Opening a NetCDF file

The NetCDF data is a data format, which is designed to be used with array-oriented scientifi data, and it is frequently used for climate or ocean data, among others.
This recipe shows you how to open a NetCDF fie in QGIS.

* __How to do it__

NetCDF fies are raster fies, and they can be opened using the Add raster layer menu.
Select NGMT NetCDF Grid for CDF as the fie format in the fie selection dialog that you will see, and select the rx5dayETCCDI_yr_MIROC5_rcp45_r2i1p1_2006-2100.nc fie from the example dataset.
Click on OK.

* __How it works__

The proposed NetCDF fie contains a single variable, which is opened as a regular raster layer.

* __There's more__

A NetCDF fie can contain contain multiple layers.
In this case, QGIS will prompt you to select the one that you want to add from the ones contained in the specifid fie.
When only one layer is available, it is opened directly, as in the previously described example.

___The NetCDF Browser plugin___
Another way of opening NetCDF fies is using the NetCDF Browser plugin.
Select the Manage and install plugins...
menu to open the plugin manager.
Go to the Not installed section and type netcdf in the search fild to fiter the list of available plugins.
Select the NetCDF Browser plugin and click on Install plugin to install it.
Close the plugin manager.
The plugin is now installed, and you can open it by selecting NetCDF Browser in the Plugins menu:


Select the NetCDF fie in the upper field.
The other filds will be updated with the content of the selected file.
Select a layer from the available ones and click on Add to add the layer to your QGIS project.


## 1.8 Saving a vector layer

## 1.9 Saving a raster layer

## 1.10 Reprojecting a layer

Layers may be in a CRS other than the one that is best for a given task.
Although QGIS supports on-the-fly reprojection when rendering, other tasks, such as performing spatial analysis, may require using a given CRS or having all input layers in the same one.
This recipe shows you how to reproject a vector layer.

## 1.11 Batch format conversion

## 1.12 Batch reprojection


```python

```

## 1.13 Loading vector layers into SpatiaLite

SpatiaLite is a single file relational database that is built on top of the well-known SQLite database.
It can store many layers of various types, including nonspatial tables.
Interfaces to the format also allow the ability to run spatial queries of various kinds.
It's a highly-flexible and portable format that is great for everyday use, especially when working on standalone projects or with only one user at a time.
SpatiaLite works in a similar manner to PostGIS without the need to configure or run a database server.


* __Getting ready__


Pick a vector layer and load it up in QGIS.
This step is optional, as you can pick the source layer from the filesystem in a later dialog.




* __How to do it__


1.  Create a SpatiaLite database if you don't already have one and name it cookbook.db. The easiest way to do this is with the Browser tab, as shown in the following screenshot:

2.  Then, pick one of the following methods to import your data. The first option is faster, but the second option gives you more control over the import settings:


* __Import method 1—the fast method__

1. In the QGIS Browser tab, find the layer that you want to copy to the database.

2. Drag and drop this layer on the Spatialite DB entry.

> If you have a lot of files listed, this will be quite difficult as the browser doesn't scroll during the drag operation.
You can optionally open a second browser window and drag the layer across.
Also, note that this defaults to multi-type geometry.
If you need to control the options, use the next method.


* __Import method 2—the standard method__

1. Open DB Manager from the Database menu.

2. Expand the Spatialite item to list your databases. Expand the database that you want to connect to.

3. Click on the following import layer icon:

4. A dialog will pop up, providing you with import options.
> SQL databases are usually case insensitive, so you can use all lower case characters.
Also, never use spaces or special characters in table names; this can just lead to headaches later. An occasional underscore is okay.
5.  Select the layer to import from the drop-down list.

6.  Fill in a name for the new table.

7.  In most cases, the only thing left to do is check the Create spatial index checkbox.

8.  If this works, great. Now, you can load the layer to the map and verify that it's identical to the input.

> This method is more similar to traditional database import and very similar to the PostGIS recipe next in this chapter.




* __How it works__


QGIS converts your geometry to a format that is compatible with SpatiaLite and inserts it,  along with the attribute table.
Afterwards, it updates the metadata tables in SpatiaLite to register the geometry column and build the spatial index on it.
These two postprocesses make the database table appear as a spatial layer to QGIS and speed up the loading of data from the table when panning and zooming.


* __There’s more__

The import dialog contained a few other features that are often useful.
You can reproject data as part of the import process if you want, or you can specify the projection if QGIS didn't detect it properly.
You can also name the geometry column something different than the default, geom; for example, utmz10n83 (this is normally not recommended).
You can specify the character encoding of the text in the event that it's not handled correctly.

You can even use the dialog to append data to an existing table; for example, you have multiple counties with the same data structure that come as two separate files, but you want them all in one layer.

If, for some reason, the layer didn't import the way that you want, delete it and redo the import.
If you delete layers, make sure to learn how to vacuum the database to recover the now empty space in the file and shrink its total size (this is not automatic).

>Look for the Vacuum option as a button in many graphical tools.
If you don't see it, no worries, just run the SQL, VACUUM;.

What happens if this fails? Databases can be really picky sometimes.
Here are some common issues and solutions:

* It could be character encoding (accents, non-Latin languages), which requires that you specify the encoding.
* It could be picky about mixing multilayers with regular layers.
Multilayers is when you have several separate geometries that are part of one record.
For example, Hawaii is actually many islands.
So, if you only have one row representing Hawaii, you need to cram all the island polygons into one geometry fi  ld.
However, if you mix this with North Dakota, which is just a polygon, the import will fail.
If you have this problem, you'll need to perform the import on the command-line using ogr2ogr and its newish feature, -nlt PROMOTE_TO_MULTI, which converts all single items to multi-items to fi  this.
* Depending on your original source, you may have a mix of points, lines, and polygons.
You'll either need to convert this to a Geometry Collection, or you need to split each type of geometry into a separate layer.
Geometry Collections are currently poorly- supported in many GIS viewers, so this is only recommended for advanced users.


## 1.14 Loading vector layers into PostGIS

__PostGIS is the spatial add-on to the popular PostgreSQL database.__
It's a server-style database with authentication, permissions, schemas, and handling of simultaneous users.
When you want to store large amounts of vector data and query them efficiently, especially in a multicomputer networked environment, consider PostGIS.
This works fine for small data too, but many users find its configuration too much work when SpatiaLite may be better suited.


### 1.14.1 Getting ready

Pick a vector layer and load it in QGIS.
You will also need to have a working copy of Postgres/ PostGIS running, a PostGIS database created, and an account that allows table creation.

> BostonGIS maintains a decent tutorial on installation for Windows, and getting a PostGIS set up for everyone.
You can find this at http://www.bostongis.com/?content_name=postgis_tut01#316.

You should configure QGIS to be aware of your database and its connection parameters by creating a new database item in the PostGIS load dialog or by right-clicking on PostGIS in the Browser tab and selecting New Connection:

You can find more information about PostGIS at http://docs.qgis.org/2.8/en/docs/user_manual/working_with_vector/supported_data.html#postgis-layers.


### 1.14.2 How to do it

Now that you can connect to a PostGIS database, you are ready to try importing data:

1.  Open DB Manager from the Database menu.

2.  Expand the PostGIS item to list your databases.
Expand the database that you want to connect to, and you should be prompted to authenticate (if you haven't saved your password in the settings).

3.  Expand the list and select the Public schema.
> In general, unless you are performing advanced work and understand how Postgres schemas work, place your layers in the Public schema.
This is the default that everyone expects.
4.  Click on the following import layer icon:

5.  A dialog will pop up, providing you with import options.
> SQL databases are usually case insensitive, so you can use all lowercase.
Also, never use spaces or special characters in table names; this can just lead to headaches later.
An occasional underscore is okay.
6.  Select the layer to import from the drop-down list.

7.  Fill in a name for the new table.

8.  Check whether schema is set to public.

9.  In most cases, the only thing left to do is check the Create spatial index checkbox:



### 1.14.3 How it works

QGIS converts your geometries to a format that is compatible with PostGIS, and inserts it, along with importing the attributes.
Afterwards, it updates the metadata views in PostGIS   to register the geometry column and build the spatial index on it.
These two post-processes make the database table appear as a spatial layer to QGIS and speed up the loading of data from the table when panning and zooming.



### 1.14.4 There's more

The options presented in the dialog are not all the options that are available.
If you need more control or advanced options present, you'll likely be looking at the command-line tools: __shp2pgsql__ (a graphical plugin for pgadmin3 is available on some platforms) and __ogr2ogr__.
The __shp2pgsql__ tool generally only handles shapefiles.
If you have other formats, __ogr2ogr__ can handle everything that QGIS is capable of loading.
You can also use these tools to develop batch import scripts.

To import large or complicated CSV or text files, you sometimes will need to use the pgadmin3 or psql command-line interface to Postgres.

Need even more control? Then, consider scripting.
OGR and Postgres both have very capable Python libraries.

Another option is using the OpenGeo Suite plugin, which has some additional options, such as allowing importing multiple layers into a single table or into one table per layer.
To learn more about this, including how to install it, refer to http://qgis.boundlessgeo.com/static/docs/intro.html.

What happens if this fails? Databases can be really picky sometimes:

* It could be character encoding (accents, non-Latin languages), which requires specifying the encoding.

* It could be picky about mixing multilayers with regular layers.
Multilayers is when you have several separate geometries that are part of one record.
For example, Hawaii is actually many islands.
So, if you only have one row representing Hawaii, you need  to cram all the island polygons into one geometry field.
However, if you mix this with North Dakota that is just a polygon, the import will fail.
If you have this problem, you'll need to perform the import on the command-line using ogr2ogr and its new feature, -nlt PROMOTE_TO_MULTI, which converts all single items to multi-items, to fix this.

* Depending on your original source, you may have a mix of points, lines, and polygons.
You'll either need to convert this to a Geometry Collection, or you need to split each type of geometry into a separate layer.
Geometry Collections are currently poorly supported in many GIS viewers, so this is only recommended for advanced users.



### 1.14.5 See also

For more information on PostGIS installation and setup, refer to http://postgis.net/install.

For a more in-depth text on using PostGIS, there are many books available, including Packt Publishing's PostGIS Cookbook.

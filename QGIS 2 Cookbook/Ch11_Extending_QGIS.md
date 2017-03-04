
# Chapter 11: Extending QGIS
<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [Chapter 11: Extending QGIS](#chapter-11-extending-qgis)
  * [11.1 Introduction](#111-introduction)
  * [11.2 Defining custom projections](#112-defining-custom-projections)
    * [Getting ready](#getting-ready)
    * [How to do it](#how-to-do-it)
    * [How it works](#how-it-works)
    * [There's more](#theres-more)
    * [See also](#see-also)
  * [11.3 Working near the dateline](#113-working-near-the-dateline)
    * [Getting ready](#getting-ready-1)
    * [How to do it](#how-to-do-it-1)
    * [How it works](#how-it-works-1)
    * [See also](#see-also-1)
  * [11.4 Working offline](#114-working-offline)
    * [Getting ready](#getting-ready-2)
    * [How to do it](#how-to-do-it-2)
    * [How it works](#how-it-works-2)
    * [There's more](#theres-more-1)
    * [See also](#see-also-2)
  * [11.5 Using the QspatiaLite plugin](#115-using-the-qspatialite-plugin)
    * [Getting ready](#getting-ready-3)
    * [How to do it](#how-to-do-it-3)
    * [How it works](#how-it-works-3)
    * [There's more](#theres-more-2)
    * [See also](#see-also-3)
  * [11.6 Adding plugins with Python dependencies](#116-adding-plugins-with-python-dependencies)
    * [Getting ready](#getting-ready-4)
    * [How to do it](#how-to-do-it-4)
    * [How it works](#how-it-works-4)
    * [There's more](#theres-more-3)
  * [11.7 Using the Python console](#117-using-the-python-console)
    * [Getting ready](#getting-ready-5)
    * [How to do it](#how-to-do-it-5)
    * [How it works](#how-it-works-5)
    * [There's more](#theres-more-4)
    * [See also](#see-also-4)
  * [11.8 Writing Processing algorithms](#118-writing-processing-algorithms)
    * [Getting ready](#getting-ready-6)
    * [How to do it](#how-to-do-it-6)
    * [How it works](#how-it-works-6)
    * [There's more](#theres-more-5)
    * [See also](#see-also-5)
  * [11.9 Writing QGIS plugins](#119-writing-qgis-plugins)
    * [Getting ready](#getting-ready-7)
    * [How to do it](#how-to-do-it-7)
    * [How it works](#how-it-works-7)
    * [There's more](#theres-more-6)
  * [11.10 Using external tools](#1110-using-external-tools)
    * [Getting ready](#getting-ready-8)
    * [How to do it](#how-to-do-it-8)
    * [How it works](#how-it-works-8)
    * [There's more](#theres-more-7)

<!-- tocstop -->

In this chapter, we will cover the following topics:

* Defining custom projections
* Working near the dateline
* Working offline
* Using the QSpatiaLite plugin
* Adding plugins with Python dependencies
* Using the Python console
* Writing Processing algorithms
* Writing QGIS plugins
* Using external tools


## 11.1 Introduction

QGIS can do many things on its own.
However, as with all software, there are limits to its default abilities.
The great news is there are many ways to extend QGIS to do even more through built-in customization options, existing add-on plugins, creating new analysis algorithms, creating your own plugins, and using external software that compliments QGIS.
This chapter covers just a few of the common customizations and plugins that haven't been mentioned in other chapters, and how you can get started with making your own add-ons to share with others.


## 11.2 Defining custom projections

Map projections stump just about everybody at some point in their GIS career, if not more often.
If you're lucky, you just stick to the common ones that are known by everyone and your life is simple.
Sometimes though, for a particular location or a custom map, you just need something a little different that isn't in the already vast QGIS projections database.
__(Often, these are also referred to as Coordinate Reference System (CRS) or Spatial Reference System (SRS).)__

I'm not going to cover what the difference is between a Projection, Projected Coordinate System, and a Coordinate system.
From a practical perspective in QGIS, you can pick the one that matches your data or your intended output.
There's lots of little caveats that come with this, but a book or class is a much better place to get a handle on it.



### Getting ready

For this recipe, we'll be using a custom graticule, a grid of lines every 10 degrees (10d_graticule.json.geojson), and the Natural Earth 1:10 million coastline (ne_10m_coastline.shp).

### How to do it

1. Determine what projection your data is currently in. In this case, we're starting with EPSG:4236, which is also known as Lat/Lon WGS84.
2. Determine what projection you want to make a map in. In this example, we'll be making an Oblique Stereographic projection centered on Ireland.
3. Search the existing QGIS projection list for a match or similar projection. If you open the Projection dialog and type Stereographic, this is a good start.
4. If you find a similar projection and just want to customize it, highlight the proj4 string and copy the information. NAD83(CSRS) / Prince Edward Isl. Stereographic (NAD83) is a similar enough projection.
If you don't find anything in the QGIS projection database, search the Web for a proj4 string for the projection that you want to use. Sometimes, you'll find Projection WKT.
With a little work, you can figure out which proj4 slot each of the WKT parameters corresponds to using the documentation at https://github.com/OSGeo/proj.4/wiki/GenParms.
A good place to research projections is provided at the end of this recipe.

5. Under Settings, open the Custom CRS option.
6. Click on the + symbol to add a new definition.
7. Put in a name and paste in your projection string, modifying it in this case with coordinates that center on Ireland.
Change the values for the lat_0 and lon_0 parameters to match the following example.
This particular type of projection only takes one reference point.
For projections with multiple standard parallels and meridians, you will see the number after the underscore increment:
```
+proj=sterea +lat_0=53.5 +lon_0=-7.8 +k=0.999912
+x_0=400000 +y_0=800000 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0
+units=m +no_defs
```
The following screenshot shows what the screen will look like:

8. Now, click on another projection in the list of custom projections.
There's currently a quirk where if you don't toggle off to another projection, then it doesn't save when you click on OK.
9. Now, go to the map, open the projection manager and apply your new projection with OTF on to check whether it's right.
You'll find your new projection in the third section, User Defined Coordinate Systems:


### How it works

Projection information (in this case, a proj4 string) encodes the parameters that are needed by the computer to pick the correct math formula (projection type) and variables (various parameters, such as parallels and the center line) to convert the data into the desired flat map from whatever it currently is.
This library of information includes approximations for the shape of the earth and differing manners to squash this into a flat visual.

You can really alter most of the parameters to change your map appearance, but generally, stick to known definitions so that your map matches other maps that are made the same way.



### There's more


QGIS only allows forward/backward transformation projections.
Cartographic forward-only projections (for example, Natural Earth, Winkel Tripel (III), and Van der Grinten) aren't in the projection list currently; this is because these reprojections are not a pure math formula, but an approximate mapping from one to the other, and the inverse doesn't always exist.
You can get around this by reprojecting your data with the ogr2ogr and gdal_transform command line  to the desired projection, and then loading it into QGIS with Projection-on-the-fl  disabled.
While the proj4 strings exist for these projections, QGIS will reject them if you try to enter them.
> If you disable Projection-on-the-fly, make sure that all layers are in the same projection; otherwise, they won't line up.
Also, perform all analysis steps before converting to a projection that is intended for cartography, as the units of measurement may become messy.

Geometries that cross the outer edge of projections don't always cut off nicely.
You will often see this as an unexpected polygon band across your map.
The easiest thing to do in this case is to remove data that is outside your intended mapping region.
You can use a clip function or simply select what you want to keep and Save Selection As a new layer.

There are other common projection description formats (prj, WKT, and proj4) out there.
Luckily, several websites help you translate.
There are a couple of good websites to look up the existing Proj4 style projection information available at http://spatialreference.org and http://epsg.io.



### See also


* Need more information on how to pick an appropriate projection for the type of  map you are making? Refer to the USGS classic map projections poster available at http://egsc.usgs.gov/isb/pubs/MapProjections/projections.html.
(correct url?) https://egsc.usgs.gov/isb//pubs/MapProjections/projections.html
Much of this is also used in the Wikipedia article on the topic available at http://en.wikipedia.org/wiki/Map_projection.
The https://www.mapthematics.com/ProjectionsList.php link also has a great list of projections, including unusual ones with pictures.


```python

```

## 11.3 Working near the dateline

If you read the previous recipe about custom projections, you might have noticed the note about data that crosses the edge of projections and how it doesn't usually render properly.
When working on data near -180 or 180 degrees longitude, you are going to have this issue.
Maps showing far Eastern Russia, Fiji, New Zealand, and the South Pacific, to name a few places, will often contend with this issue.

The required solution really depends on what you're trying to do.
If you just need a map of such areas, pick a locally suitable projection.
If you have existing data from other sources,  it may be cut along the edge and you might need to stitch it back together.
As for worldwide maps, sometimes you have to trim .01 degrees of the edge of your data so that it doesn't display oddly.



### Getting ready

To follow this recipe, you will need the honolulu-flights.shp layer and the SpatiaLite database new-zealand.sqlite from the sample data.

### How to do it

Load the honolulu-flights.shp layer in QGIS. This layer represents great circles flight lines from the Honolulu airport. As you can see, it is displayed in a very strange manner,  as some flight lines cross the dateline meridian:

To display this layer correctly, we can select a suitable projection for your map. To do this, perform the following steps:

1. Open the Project Properties dialog by clicking on the Ctrl + Shift + P keyboard shortcut or navigating to Project | Project Properties.

2. Go to the CRS tab and activate the Enable 'on the fly' CRS transformation checkbox.

3. Select projection suitable for location.
In our case, this is WGS84/PDC Mercator (EPSG:3832).
> Note that for different locations, different projections should be used.

4. Click on the OK button to save changes and close the dialog.

__Now, our data is displayed correctly:__
Another option is to clip data to the dateline meridian. This can be done with the ogr2ogr
tool from the GDAL toolset.

__To do this, perform the following steps:__
1. Open the OSGeo4W command prompt if you are a Windows user, or the terminal window if you are a Linux or Mac OS user.
2. Change the directory to the folder where honolulu-flights.shp is located, for example, the following directory:
```cmd
cd c:\data
```
3. Enter the following command in the command prompt. Note that we first specify the output file and then the input file:
```py
ogr2ogr -wrapdateline honolulu-flights-wrapped.shp honolulu-flights.shp
```
4. After loading the newly-created honolulu-flights-wrapped.shp layer into QGIS,
you will now see that flights wrapped on the dateline meridian are displayed correctly:

5. Now load the nz-coastlines-and-islands layer from the new-zealand.
sqlite database. You should see two sets of polygons far from each other.
This is New Zealand and Chatham islands, which should be located nearby:

__To display them correctly, we can use the ST_Shift_Longitude function available in the SpatiaLite.__
To do this, perform the following steps:
1. Open the DB Manager plugin by clicking on its button on the toolbar or navigating to Database | DB Manager | DB Manager.
2. Expand the SpatiaLite group in the connections tree on the left-hand side of the dialog, and select the new-zealand database.
3. Open SQL Window and run the following query:
```sql
UPDATE "nz-coastlines-and-islands" SET Geometry=ST_Shift_Longitude(Geometry)
```
4. Remove the nz-coastlines-and-islands layer from QGIS and add it again. Now, New Zealand and Chatham islands will be displayed correctly, as follows:
It is worth mentioning that the ST_Shift_Longitude function is also available in PostGIS.

### How it works

When we enable 'on the fly' CRS transformation, every geometry is reprojected to the given CRS (in our case Pacific centered) so that coordinates now have the same sign.

The ogr2ogr tool with the -wrapdateline option splits all geometries that cross the dateline and write them to the output file.
Geometries that do not cross the dateline are copied without changes.

The ST_Shift_Longitude function translates negative longitudes by 360 and as a result, all the data will be in the range of 0-360 degrees and displayed correctly.



### See also

* http://docs.qgis.org/2.2/en/docs/user_manual/working_with_vector/supported_data.html#vector-layers-crossing-180-degrees-longitude

## 11.4 Working offline

The Internet is an awesome resource, but sometimes you just don't have access to it.
For field work, in places with intermittent services, on an airplane, or even in a meeting room, you just might not be able to access all the stuff that you need.
By stuff, we're referring to documentation (user and developer), but more importantly database layers (for example, PostGIS) and web service-based layers (for example, WMS, WFS, the OpenLayers plugin, and so on).

This recipe is about caching local copies of the files that you need on your computer before you leave for an unconnected place.



### Getting ready

For this recipe, you will need to open a PostGIS database or WFS and enable the Offline Editing plugin that ships with QGIS.

### How to do it

1. Load your layer from PostGIS or WFS.
2. Make sure to activate the Offline Editing option.
3. In the Database menu, you should see Offline Editing, choose Convert to offline project:
4. Choose the local file to use to store the data.
5. Then, select all of the layers to convert (vector layers only):
6. You can now use your PostGIS layers without a network connection to the online database.
Go ahead and try to make some edits.
(We recommend that you do this on a copy of database or table until you know what you're doing.)
7. When you come back to your network, you can now send your changes to the database by choosing the Synchronize option:



### How it works

The basics are straightforward, a copy of you data is saved into a SpatiaLite database locally on your computer.
The project file records the change, and you are good to go as SpatiaLite can do anything any other vector data source in QGIS can do.

Be careful when working in a multiuser environment, this does not handle dealing with editing conflicts if multiple users have been modifying the same dataset independently.

### There's more

Also, there are all sorts of ways to create offline caches of Raster datasets (network files or web services) including gdalwmscaching or mbtiles.
If you plan to need to work away from the Internet for periods of time, plan ahead, and test solutions before actually needing to go offline.
No amount of plugins makes up for good planning.

Please remember to check the legality of caching web services (for example, Google and Bing) before doing so.
OpenStreetMap is a reliable source of tiles for offline usage without restrictions.



### See also

* GDAL WMS Cache options are available at http://www.gdal.org/frmt_wms.html
* A discussion of mbtiles usage is available at http://blogs.terrorware.com/geoff/2012/11/17/offline-map-tiles-in-qgis/
* A recent plugin to help you set this up is available at https://plugins.qgis.org/plugins/MBTiles2img/

## 11.5 Using the QspatiaLite plugin

Sometimes, you may not need to load a whole layer into QGIS, but only some subset of it, or perform some calculations on the fly.
In such situations, the ability to run complex SQL queries and display their results in QGIS will be very useful.

This recipe shows you how to execute SQL code with the QspatiaLite plugin and load data in QGIS.

### Getting ready


To follow this recipe, you need to create connection to the cookbook.db database created in the Loading vector layers into SpatiaLite recipe in Chapter 1, Data Input and Output.
Alternatively, you can use your own SpatiaLite database, but be aware that you might have to alter some of the following SQL statements to match your tables.

Additionally, install the QspatiaLite plugin from Plugin Manager.

### How to do it

Make sure that you created a connection to cookbook.db. Start the QspatiaLite plugin by navigating to Database | SpatiaLite | QspatiaLite:

To execute the SQL query, perform the following steps:
1. Select database you want to use from the combobox in the top-left corner of the plugin dialog.

2. In the SQL tab, enter following query:
```sql
SELECT "census_wake2000".'pk' AS id, "census_wake2000".'geom' AS Geometry, "census_wake2000".'area', "census_wake2000".'perimeter' FROM "census_wake2000" WHERE "census_wake2000".'perimeter > 100000;
```
You can easily insert the table and column names by double-clicking on them in the
Tables tree on the left-hand side of the dialog.

3. Click on the Run button at the bottom of the dialog to execute the query. The Result tab will open automatically and you can examine the query results in the table representation, as follows:

If the query results contain geometry information (the so-called geometry column), you can display them in QGIS. To do this, perform the following steps:

1. Switch back to the SQL tab.

2. In the Option combobox, select the action that you want to perform, for example,
Create Spatial View & Load in QGIS.
3. In the Table field, enter name of the resulting view, for example, above100k.

4. Ensure that you entered the correct name of the geometry column in the
Geometry field.
5. Click on the Run button at the bottom of the dialog.
6. A dialog will pop up asking for the source geometry table. Select table that you used in the query and click on OK.
7. A new view will be created and added to the QGIS as a new layer.

### How it works

When we click on the Run button, the query is passed to the SpatiaLite database engine for execution, and the results are returned to the plugin and displayed in the table.
If you want to store results permanently, you can export them in a text file or in an OGR-compatible format using the corresponding buttons in the plugin dialog.

### There's more

You can also use the DB Manager plugin (which is bundled with QGIS) to execute SQL-queries directly and load them as layers.

### See also


* A very good introduction to SpatiaLite and SQL can be found at https://www.gaia-gis.it/fossil/libspatialite/wiki?name=misc-docs.
Also, a full list of the supported spatial SQL functions is available at http://www.gaia-gis.it/gaia-sins/spatialite-sql-4.3.0.html.

## 11.6 Adding plugins with Python dependencies

While the most common and widely-used Python packages are shipped with QGIS, and they can be used by plugins without any additional actions, some QGIS plugins need third-party Python packages, which are not available with the default QGIS installation.

This recipe shows you how to add missing Python packages to the QGIS installation.


### Getting ready

To follow this recipe, you may need administrator rights if you are a Windows user, and QGIS is installed in the system drive.


### How to do it

This steps will install pip — a Python package management tool:
1. Download the get-pip.py file from https://raw.githubusercontent.com/pypa/pip/master/contrib/get-pip.py and save it somewhere on your hard drive, for example in D:\Downloads.
2. Open the OSGeo4W command prompt as administrator.
Right-click on the OSGeo4W Shell shortcut on your Desktop and select Run as Administrator from the context menu.
If you cannot find this shortcut on your Desktop, look for it in the Windows Start menu.
3. In the OSGeo4W command prompt, type python D:\Downloads\get-pip.py.
Don't forget to replace D:\Downloads with the correct path to the get-pip.py file.
Wait while the command execution completes.


Now, when pip is ready, you can easily download and install third-party Python packages.
To do this, perform the following steps:
1. Open the OSGeo4W command prompt as administrator.
Right-click on the OSGeo4W Shell shortcut on your desktop and select Run as Administrator from the context menu.
If you cannot find this shortcut on the desktop, look for it in the Windows Start menu.
2. In the OSGeo4W command prompt, type pip install package_name.
Don't forget to replace package_name with the name of the package that you want to install.
For example, if you want to install the PySAL package, use this command: pip install pysal.

If you are a Linux user, use your package manager to install pip.
For example, under Debian and Ubuntu, use the sudo apt-get install python-pip command to install pip.
After doing this, you can use pip to download and install packages as described in the preceding paragraph.

Mac OS users can install pip via Homebrew using the brew install pip command.

Using pip, you also can view installed packages, update, and remove them.
For more information please look at the pip documentation available at https://pip.pypa.io/en/stable/.


### How it works

pip downloads the requested package with all necessary dependencies from the Python Package Index (https://pypi.python.org/) and installs them into Python bundled with QGIS.



### There's more

You can also register Python bundled with QGIS in the Windows registry as the system default Python version.
After this, you can use usual Windows installers to install the required packages.
More information on this topic can be found at https://trac.osgeo.org/osgeo4w/ticket/114.

## 11.7 Using the Python console

QGIS has a built-in Python console, where you can enter commands in the Python programming language and get results.
This is very useful for quick data processing.



### Getting ready

To follow this recipe, you should be familiar with the Python programming language.
You can find a small but detailed tutorial in the official Python documentation at https://docs.python.org/2.7/tutorial/index.html.
Also load the poi_names_wake.shp file from the sample data.



### How to do it

QGIS Python console can be opened by clicking on the Python Console button at toolbar or by navigating to Plugins | Python Console. The console opens as a non-modal floating window, as shown in the following screenshot:

Let's take a look at how to perform some data exploration with the QGIS Python console:

1. First, it is necessary to get a reference to the active (selected in the layers tree) layer and store it in the variable for further use by running this command:
```py
layer = iface.activeLayer()
```

2. After acquiring a reference to the layer, we can examine some of its properties. For example, to get the number of features in the layer, execute the following command:
layer.featureCount()
> At any time, you can use the dir() function to list all the available methods of the object.
Try to execute dir(layer) or dir(QgsFeature).

3. You can also loop over layer features and print their attributes using the following
code snippet:
```py
for f in layer.getFeatures():
print f["featurenam"], f["elev_m"]
```

> Note that you need to press Enter twice after entering this code to exit the loop definition and start executing commands.


You can also use the Python console for more complex tasks, such as exporting features with some attributes to a text file. Here is how to do this:
1. Open the Python Console editor using the Show editor button on the left-hand side of the Python console.
2. Paste the following code into the editor (make sure to change path to file according to your system):
```py
import csv
layer = iface.activeLayer()
with open('c:\\temp\\export.csv', 'wb') as outputFile:
writer = csv.writer(outputFile)
for feature in layer.getFeatures():
geom = feature.geometry().exportToWkt()
writer.writerow([geom, feature["featurenam"], feature['elev_m"]])
```

3. If you are using your own vector layer instead of poi_names_wake.shp, which is provided with this book, adjust attribute names in line 8.
4. Change the file paths for the result file in line 4 depending on your operating system.
5. Save the script and run it. Don't forget to select the vector layer in the QGIS layer tree before running the script.

### How it works


In line 1, we imported the csv module from the standard Python library.
This module provides a convenient way to read and write comma-separated files.
In line 3, we obtained a reference to the currently selected layer, which will be used later to access layer features.

In line 3, an output file opened.
Note that here we use the with statement so that later there is no need to close the file explicitly, context manager will do this work for us.
In line 5, we set up the so-called writer—an object that will write data to the CSV file using specified format settings.

In line 6, we started iterating over features of the active layer.
For each feature, we extracted its geometry and converted it into a Well-Known Text (WKT) format (line 7).
We then wrote  this text representation of the feature geometry with some attributes to the output file (line 8).

It is necessary to mention that our script is very simple and will work only with attributes that have ASCII encoding.
To handle non-Latin characters, it is necessary to convert the output data to the unicode before writing it to file.



### There's more

Using the Python console, you also can invoke Processing algorithms to create complex scripts for automated analysis and/or data preparation.

To make the Python console even more useful, take a look at the Script Runner plugin. Detailed information about this plugin with some usage examples can be found at http://spatialgalaxy.net/2012/01/29/script-runner-a-plugin-to-run-python-scripts-in-qgis/.

### See also

* If you are new to Python and QGIS API, don't forget to look at the following documentation:
* Official Python documentation and tutorial can be found at https://docs.python.org/2/
* QGIS API Documentation can be found at http://qgis.org/api/2.8/
* PyQGIS Developer Cookbook can be found at http://docs.qgis.org/2.8/en/docs/pyqgis_developer_cookbook/

* Another great resource to learn programming with QGIS is QGIS Python Programming Cookbook, Joel Lawhead, published by Packt Publishing


```python

```

## 11.8 Writing Processing algorithms

You can extend the capabilities of QGIS by adding scripts that can be used within the Processing framework.
This will allow you to create your own analysis algorithms and then run them efficiently from the toolbox or from any of the productivity tools, such as the batch processing interface or the graphical modeler.

This recipe covers basic ideas about how to create a Processing algorithm.


### Getting ready


A basic knowledge of Python is needed to understand this recipe.
Also, as it uses the Processing framework, you should be familiar with it before studying this recipe.


### How to do it

We are going to add a new process to filter the polygons of a layer, generating a new layer that just contains the ones with an area larger than a given value. Here's how to do this:

1. In the Processing Toolbox menu, go to the Scripts/Tools group and double-click on the Create new script item. You will see the following dialog:

2. In the text editor of the dialog, paste the following code:
```py
##Cookbook=group
##Filter polygons by size=name
##Vector_layer=vector
##Area=number 1
##Output=output vector
layer = processing.getObject(Vector_layer) provider = layer.dataProvider()
writer = processing.VectorWriter(Output, None, provider.fields(), provider.geometryType(), layer.crs())
for feature in processing.features(layer):
print feature.geometry().area()
if feature.geometry().area() > Area:
writer.addFeature(feature)
del writer
```
3. Select the Save button to save the script. In the file selector that will appear, enter a filename with the .py extension.
Do not move this to a different folder.
Make sure that you use the default folder that is selected when the file selector is opened.

4. Close the editor.

5. Go to the Scripts groups in the toolbox, and you will see a new group called Cookbook with an algorithm called Filter polygons by size.

6. Double-click on it to open it, and you will see the following parameters dialog, similar to what you can find for any of the other Processing algorithms:


### How it works

The script contains mainly two parts:

* A part in which the characteristics of the algorithm are defined.
This is used to define the semantics of the algorithm, along with some additional information, such as the name and group of the algorithm.
* A part that takes the inputs entered by the user and processes them to generate the outputs.
This is where the algorithm itself is located.

In our example, the first part looks like the following:
```py
##Cookbook=group
##Filter polygons by size=name
##Vector_layer=vector
##Area=number 1
##Output=output vector
```
We are defining two inputs (the layer and the area value) and declaring one output (the filtered layer).
These elements are defined using the Python comments with a double Python comment sign (#).
The second part includes the code itself and looks like the following:
```py
layer = processing.getObject(Vector_layer)
provider = layer.dataProvider()
writer = processing.VectorWriter(Output, None, provider.fields(), provider.geometryType(), layer.crs())
for feature in processing.features(layer):
print feature.geometry().area()
if feature.geometry().area() > Area:
writer.addFeature(feature)
del writer
```
The inputs that we defined in the first part will be available here, and we can use them.
In the case of the area, we will have a variable named Area, containing a number.
In the case of the vector layer, we will have a Layer variable, containing a string with the source of the selected layer.

Using these values, we use the PyQGIS API to perform the calculations and create a new layer.
The layer is saved in the file path contained in the Output variable, which is the one that the user will select when running the algorithm.
Apart from using regular Python and the PyQGIS interface, Processing includes some classes and functions because this makes it easier to create scripts, and that wrap some of the most common functionality of QGIS.

In particular, the processing.features(layer) method is important.
This provides an iterator over the features in a layer, but only considering the selected ones.
If no selection exists, it iterates over all the features in the layer.
This is the expected behavior of any Processing algorithm, so this method has to be used to provide a consistent behavior in your script.


### There's more

Some of the core algorithms that are provided with Processing are actually scripts, such as the one we just created, but they do not appear in the scripts section.
Instead, they appear in the QGIS algorithms section because they are a core part of Processing.
Other scripts are not part of processing itself but they can be installed easily from the toolbox using the Tools/Get scripts from on-line collection menu:

You will see a window like the following one:

Just select the scripts that you want to install and then click on OK.
The selected scripts will now appear in the toolbox.
You can use it as you use any other Processing algorithm.


### See also

* All the information about creating scripts and running Processing code from the QGIS Python console can be found in the corresponding section in the QGIS manual.

## 11.9 Writing QGIS plugins

One of the main reasons of the popularity of QGIS is its extensibility.
Using the basic tools and features provided by the QGIS API, new functionality can be implemented and added as a new plugin that can be shared by contributing it to the QGIS plugins repository.



### Getting ready

To be able to develop a new QGIS plugin, you should be familiar with the Python programming language.
If the plugin has a graphical interface, you should have some knowledge of the Qt framework, as this is used for all UI elements, such as dialogs.
To access the QGIS functionality, it is required that you know the QGIS API.

A very handy resource for all these (plus a few others) is the GeoAPIs website, which is created by SourcePole at http://geoapis.sourcepole.com/.

To simplify the creation of a plugin, we will use an additional plugin named Plugin Builder.
It should be installed in your QGIS application.


### How to do it

The following steps create a new plugin that will print out detailed information about the layers currently loaded in your QGIS project:
1. Open Plugin Builder by navigating to Plugin | Plugin Builder.

2. Fill out the dialog that will appear, as shown in the following screenshot:

3. Click on OK.

4. In the folder selector dialog that will appear, select the folder where you want to store your plugin.
Click on OK and the plugin skeleton will be created.
In the selected folder, you will now have a subfolder named LayerInfoPlugin, with the following content (items in square brackets indicate folders):
```
[help]
[i18n]
[scripts]
[test]
icon.png
layerinfo.py
layerinfo_dialog.py
layerinfo_dialog_base.ui
Makefile
metadata.txt
pb_tool.cfg
plugin_upload.py
pylintrc
README.html
README.txt
resources.qrc
init.py
```

5. Open the layerinfo.py file in a text editor.
At the end of it, you will find the run() method, with the following code:
```py
def run(self):
"""Run method that performs all the real work"""
# show the dialog self.dlg.show()
# Run the dialog event loop result = self.dlg.exec_()
# See if OK was pressed if result: pass
```

6. Replace the run method with the following code:
```py
def run(self):
layers = self.iface.legendInterface().layers() print "---LAYERS INFO---"
for layer in layers:
print "Layer name: " + layer.name() print "Layer source " + layer.source()
print "Extent: " + layer.extent().asWktCoordinates() print
```

7. Install the pb_tool application by opening a terminal and running easy_install pb_tool (you can also use pip install pb_tool or any other way of installing a library from PyPI).

8. Open a terminal in the folder where you have the plugin code and run pb_tool and compile.
Then, run pb_tool deploy to install the plugin in your local QGIS.

9. Open QGIS. Go to Plugin Manager and make sure that the new plugin we have created is there and is enabled:

10. In the Plugins menu, you will now have the entry corresponding to the plugin:

11. Populate your project with some layers so that the plugin can display information about them.

12. Open the QGIS Python console.

13. Run the plugin by selecting its menu entry.
Information about the layers in the project will be shown in the console:




### How it works

The Plugin Builder plugin takes some basic information about the plugin to create it, and uses it to create its skeleton.
By default, it includes a menu entry, which becomes the entry point to the plugin from the QGIS interface.

When the menu item is selected, the corresponding action in the plugin code is executed.
In this case, it runs the run() method, where we have added our code.

The plugin will always have a reference to the QGIS instance (an object of class QgsInterface), which can be used to access the QGIS API and connect with the elements in the current QGIS session.
In our case, this is used to access the legend, which contains a list of all the layers loaded in the current project.
Calling the corresponding methods in each one of these layers, the information about them is retrieved and printed out.

The standard output is redirected to the QGIS Python console, so printing a text using the built-in Python print command will cause the text to appear in the console in case it is open.

QGIS stores its plugins in the .qgis2/python/plugin folder under the current user folder.
For instance, this is when you download a new plugin using Plugin Manager.
Each time you start QGIS, it will look for plugins there and load them.
Copying the folder is done by the deploy task that we have run, and this allows QGIS to discover the plugin and add it to the list of available plugins when QGIS is started.


### There's more

The following are some ideas to create better plugins and manage them.

__Creating plugins with more complex UI elements__
The plugin that we have created has no UI elements.
However, among the files created by the plugin builder, you can find a basic dialog file with the .ui extension.
You can edit this to create a dialog that can later be used, by calling it from your plugin code.
To know more about how to create and use UI elements from the Qt framework (QGIS is built on top of this framework), check out the PyQt documentation.

__Documenting you plugin__
Another thing that is created by the Plugin Builder is a Sphinx documentation project where you can write the usage documentation of your plugin using RestructuredText.
To know more about Sphinx, you can visit the Sphinx official site at http://www.sphinx-doc.org/en/stable/.
The documentation project is built when the deploy task is run, and will create HTML files and place them in the plugin folder.

__Releasing your plugin__
Once your plugin is finished, it would be a good idea to share it and let other people use it.
If you upload your plugin to the QGIS plugin server, it would be easy for all QGIS users to get it using Plugin Manager and also get the latest updates.
To upload the plugin, you first need to create a ZIP file containing all its code and resources.
There is a pb_tool task for this, and you just have to run pb_tool zip in a terminal.
With the resulting ZIP file, you can start the release process.
More information about it can be found at http://docs.qgis.org/2.6/en/docs/pyqgis_developer_cookbook/releasing.html.


## 11.10 Using external tools

While QGIS itself is a great and functional program, sometimes it is better to use more suitable tools to perform some simple or complex actions.
This recipe shows you how to use some of these third-party tools.



### Getting ready

To follow this recipe, you will need a btnmeatrack_2014-05-22_13-35-40.nmea file from the book dataset.
We will also use the cookbook.db SpatiaLite database that we created in the Loading vector layers into SpatiaLite recipe in Chapter 1, Data Input and Output, and the PostgreSQL database, which we developed in Chapter 6, Network Analysis.

Besides this, don't forget to install GPSBabel (usually this comes with QGIS), spatialite-gui, and pgAdmin (these should be installed manually).

### How to do it

First we will convert NMEA data to GPX format with GPSBabel, then learn how to use SpatiaLite GUI tools and pgAdmin to work with databases.

__GPSBabel__
GPSBabel is a command-line tool to manipulate, convert, and process GPS data (waypoints, tracks, and routes) in different formats.
To convert a file from the NMEA format to more common GPX, follows these steps:

1. Open the command prompt and go to the directory where the btnmeatrack_2014-05-22_13-35-40.nmea file is located.
Usually, this can be done with the cd command, for example, if the file is located in the data directory on the C: drive, use this command:
```
cd c:\data
```
2. In the command prompt, enter the following command to convert the NMEA file to the GPX file:
```py
gpsbabel -i nmea -f btnmeatrack_2014-05-22_13-35-40.nmea -o gpx -F 2014-05-22_13_35-40.gpx
```

__spatialite-gui__
spatialite-gui is a GUI tool supporting SpatiaLite.
This is lightweight and very useful when you need to quickly perform some queries or just check contents of the SQLite/SpatiaLite database.

To explore spatial or nonspatial tables in the SpatiaLite database, perform these steps:
1. Start spatialite-gui by double-clicking on its executable file.
2. Connect to the database that you want to explore by navigating to Files | Connecting an existing SQLite DB or clicking on the corresponding button on the toolbar.
3. Select the table you want to explore in the table tree on the left-hand side of the spatialite-gui dialog, open its context menu by clicking on the right mouse button, and select the Edit table rows menu entry.
4. If your table contains spatial data, it is possible to view geometry in different representations.
Select the field with the geometry information in the row, open the context menu by clicking the right mouse button and select the BLOB Explore menu entry:

After massive edits, especially when tables were altered or deleted, it is recommended to run VACUUM command to rebuild the database. To do this, perform these steps:
1. Start spatialite-gui by double-clicking on its executable file.
2. Connect to the database that you want to explore by navigating to Files | Connecting an existing SQLite DB or clicking on the corresponding button on the toolbar.
3. Navigate to Files | Optimizing current SQLite DB [VACUUM] or click on the corresponding button on the toolbar.

__pgAdmin__
pgAdmin is an administration tool and development platform for PostgreSQL databases.
It allows you to perform administrative tasks (such as backup and restore), run simple queries as well as develop new databases from scratch.
To create a backup of the database with pgAdmin, follow these steps:
1. Start pgAdmin by clicking on its desktop shortcut or by finding it in the Start menu.
2. Create a connection to your database server if it does not exist by navigating to File | Add Server… or clicking on the corresponding button on the toolbar.
3. Connect to the database server where your database is located by double-clicking on its name in Object Browser on the left-hand side of the pgAdmin window.
4. Select the database that you want to back up, open its context menu by clicking the right mouse button, and select Backup.
A backup settings dialog will be opened, as shown in the following screenshot:
5. Choose a location where your backup will be saved, adjust the backup options according to your needs, and click on the Backup button to start the backup process.
The progress will be displayed in the Messages tab.

To restore the database from the backup, perform the following steps:
1. Start pgAdmin by clicking on its desktop shortcut or by finding it in the Start menu.
2. Create a connection to your database server if it does not exist by navigating to File | Add Server... or clicking on the corresponding button on the toolbar.
3. Connect to the database server where you want to restore the backup and double-click on its name in Object Browser on the left-hand side of the pgAdmin window.
4. Select database that should be restored, open its context menu by clicking the right mouse button, and select Restore.
A restore options dialog will be opened, as shown in the following screenshot:
> It is worth mentioning that the pg_restore tool used by pgAdmin to restore cannot create the database that has to be restored.
It is necessary to create a new empty database manually and then start the restoration with this freshly created database.

5. Select the location of the backup file and adjust the restore options according to your needs.
> Note that you can restore single table or schema, just click on the Display objects button after selecting the backup file and choose desired objects on the Objects tab.

6. Click on the Restore button to start restoring. The progress will be displayed in the Messages tab.



### How it works

All of the tools here work on the same file formats as QGIS.
This allows for greater flexibility when working by being able to use the best tool at the right time.


### There's more


There are many other different tools that can be useful in various situations, for example, exiv2 can be used to manipulate the EXIF tags of the photos, ImageMagic to process rasters, and so on.

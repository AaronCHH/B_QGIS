
# Chapter 12: Up and Coming
<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [Chapter 12: Up and Coming](#chapter-12-up-and-coming)
  * [12.1 Introduction](#121-introduction)
  * [12.2 Preparing LiDAR data](#122-preparing-lidar-data)
    * [Getting ready](#getting-ready)
    * [How to do it](#how-to-do-it)
    * [How it works](#how-it-works)
    * [There's more](#theres-more)
  * [12.3 Opening File Geodatabases with the OpenFileGDB driver](#123-opening-file-geodatabases-with-the-openfilegdb-driver)
    * [Getting ready](#getting-ready-1)
    * [How to do it](#how-to-do-it-1)
    * [How it works](#how-it-works-1)
    * [There's more](#theres-more-1)
    * [See also](#see-also)
  * [12.4 Using Geopackages](#124-using-geopackages)
    * [Getting ready](#getting-ready-2)
    * [How to do it](#how-to-do-it-2)
    * [How it works](#how-it-works-2)
    * [There's more](#theres-more-2)
    * [See also](#see-also-1)
  * [12.5 The PostGIS Topology Editor plugin](#125-the-postgis-topology-editor-plugin)
    * [Getting ready](#getting-ready-3)
    * [How to do it](#how-to-do-it-3)
    * [How it works](#how-it-works-3)
    * [There's more](#theres-more-3)
  * [12.6 The Topology Checker plugin](#126-the-topology-checker-plugin)
    * [Getting ready](#getting-ready-4)
    * [How to do it](#how-to-do-it-4)
    * [How it works](#how-it-works-4)
    * [There's more](#theres-more-4)
  * [12.7 GRASS Topology tools](#127-grass-topology-tools)
    * [Getting ready](#getting-ready-5)
    * [How to do it](#how-to-do-it-5)
    * [How it works](#how-it-works-5)
    * [There's more](#theres-more-5)
    * [See also](#see-also-2)
  * [12.8 Hunting for bugs](#128-hunting-for-bugs)
    * [Getting ready](#getting-ready-6)
    * [How to do it](#how-to-do-it-6)
    * [How it works](#how-it-works-6)
    * [There's more](#theres-more-6)
    * [See also](#see-also-3)
  * [12.9 Reporting bugs](#129-reporting-bugs)
    * [Getting ready](#getting-ready-7)
    * [How to do it](#how-to-do-it-7)
    * [How it works](#how-it-works-7)
    * [There's more](#theres-more-7)
    * [See also](#see-also-4)

<!-- tocstop -->


In this chapter, we will cover the following topics:

* Preparing __LiDAR data__
* Opening File Geodatabases with the OpenFileGDB driver
* Using Geopackages
* The PostGIS Topology Editor plugin
* The __Topology Checker__ plugin
* __GRASS Topology tools__
* Hunting for bugs
* Reporting bugs


## 12.1 Introduction


The software landscape is constantly changing and QGIS is no exception.
There are new  features added weekly, and a huge, growing library of plugins.
This chapter highlights some of the newer features at the time of writing.
These are features that we think will be around for some time due to their usefulness.
Keep in mind, however, that they are still in development and can easily change at any moment; hopefully, this is for the better.
Included in this chapter are the handling of some more recent and additional formats that were not covered earlier, such as LIDAR, File Geodatabases, and Geopackages.
Also included are several recipes on topology usage, editing, and fi  Finally, there are a few recipes on how you can become part of the community that helps evolve QGIS through bug hunting and reporting.



## 12.2 Preparing LiDAR data

LiDAR data is becoming more available, and it represents a fundamental source of detailed elevation data.
This chapter will show you how to work with LiDAR data in QGIS.

### Getting ready

__We will use the Processing framework, so you should be familiar with it.__

We will also use LASTools, which is not included with QGIS.
__Download LASTools binaries from http://lastools.org/download/LAStools.zip and install them on your computer.__
__Processing has to be configured so that it can find and execute LASTools.__
Open the Processing configuration by going to the __Processing | Options__ menu and move to the Tools for LiDAR data section, as shown in the following screenshot:

__In the LAStools folder field, type the path to the folder where you have installed the LASTools executables.__


### How to do it

In the data corresponding to this recipe, you will find a las file with LiDAR data.
This cannot be opened in QGIS, but we will convert it so that it can be opened and rendered as part of a normal QGIS project.
Follow these steps:
1. Open the Processing Toolbox menu.
2. In the Tools for LiDAR data/LASTools branch, double-click on the las2shp algorithm. The parameters dialog of the algorithm looks like the following:
3. Enter the path to the sample LAS file provided with this recipe in the Input LAS/LAZ file field.
4. Enter the path to the output shapefile in the Output SHP file field.
5. Leave the remaining parameters as they are and click on OK to run the algorithm.
6. The resulting shapefile with the point cloud will be added to your QGIS project.




### How it works

The las2shp tool converts a LAS file into a SHP file.
Processing calls las2shp using the provided parameters, and then loads the resulting layer into your QGIS project.



### There's more

You can also convert a LAS file into a raster layer using the las2dem algorithm instead.

In the Tools for LiDAR data/LASTools branch, double-click on the las2dem algorithm.
The Parameters dialog of the algorithm looks like the following:

1. Enter the path to the sample LAS file that is provided with this recipe in the Input LAS/LAZ file field.
2. Enter the path to the output TIFF file in the Output file field.
3. Enter 0.005 in the step size/pixel size field.
4. Leave the remaining parameters as they are and click on OK to run the algorithm.
5. The resulting raster layer will be added to your QGIS project.

## 12.3 Opening File Geodatabases with the OpenFileGDB driver

File Geodatabases (GDB) are a relatively new format compared to shapefi  s, and they were created by Esri for their Arc product line.
They allow the storage of multiple vector and raster layers in a single database.
Some government agencies release data offi  in this format.
However, only in the last few years has it been possible to open this data with open source tools.


### Getting ready

For this recipe, you will need a File Geodatabase, naturalearthsample.gdb.zip, which is included in the sample data, and GDAL 1.11 or a newer version.

> Check your GDAL version by navigating to Help | About | About.
If your GDAL is a lower number, upgrading your QGIS should get you a new enough version.
Refer to http://qgis.org/en/site/forusers/download.html for more options, especially if on Linux where you may need third-party repositories for a newer version of GDAL.

File Geodatabases are actually folders full of all sorts of binary files.
Typically, you will get them zipped and must extract the zip to a real system folder before you can use it.


### How to do it

1. Unzip the naturalearthsample.gdb.zip file so that you have a folder called naturalearthsample.gdb.
2. Select the Add Vector dialog.
3. Select Directory instead of File for the Source type option.
4. From the Type dropdown, pick OpenFileGDB.
5. Now, choose Browse and navigate to the naturalearthsample.gdb folder (if you haven't unzipped this already you need to do this first).

> Yes, it's a little odd to have .gdb on the end of a folder as this makes it look like a file.
This just seems to be the standard convention.

6. Select the folder (not the contents), and then select Open.
7. Once back in the main dialog, choose Open, as shown in the following screenshot:
8. You should be prompted with a list of available layers. Select the ones that you want, and click OK.
You can select multiple layers to add at the same time:




### How it works

This is fairly straightforward.
You tell QGIS the root folder of the File Geodatabase, and it can figure out how to use all the files inside of the folder appropriately.
As long as GDAL has a driver for a given format, then you should be able to open the data with QGIS.
Support for additional formats is always ongoing and being refined.



### There's more

The key limitation to File Geodatabase drivers currently are that raster layers are not supported and that there is limited write ability for vectors.
There are actually two different drivers.
One is an open source project, which is built by the community, and is the default driver.
The other is based on a development library, which is released publicly by ESRI that has specific license restrictions.

OpenFileGDB, the open source community-built driver, can open multiple versions of GDB (9 and 10), is read only, and comes with most versions of GDAL 1.11+.

The ESRIFileGDB driver can read and write vector layers (this has some limitations, which  are discussed in the link in the See also section).
However, it often can only open the version of GDB it was built for (the newest version only reads newer GDB formats, for example, 10).
Sometimes, it requires you to build the GDAL driver from the SDK code provided by ESRI.
(This is done for Windows users as part of osgeo4w; Linux, and Mac users at this time need to compile GDAL with the FileGDB SDK 1.4.)

To use this driver, pick a different type in the dialog as ESRIFileGDB.
If you don't see it listed, you don't have a version of GDAL that includes this, and you will need to compile GDAL yourself.

If you get a database in this format, consider batch converting it to Spatialite, which will maintain most of the same information and give you full read, write, query, and edit capabilities in QGIS.
You'll need the ogr2ogr command for now until someone writes a plugin (or you could load them one by one with the DB Manager):
```py
ogr2ogr -f SQLite naturalearthsample.sqlite naturalearthsample.gdb -
skip- failures -
nlt PROMOTE_TO_MULTI -dsco SPATIALITE=YES
```

### See also

* The GDAL/OGR information pages about the two formats can be found at http://gdal.org/drv_filegdb.html vs http://gdal.org/drv_openfilegdb.html

## 12.4 Using Geopackages

Geopackage is a new open standard for geospatial data exchange from the Open Geospatial Consortium (OGC), an industry standards organization.
It is intended to allow users to bundle multiple layers of various types into a single file that can easily be passed to others.
This recipe demonstrates how to utilize this new data format and what to expect in the future.

### Getting ready

For this recipe, you will need a Geopackage file, often the extension is .gpkg.
There should be a naturalearthsample.gpkg file in the provided sample data.

You'll also need GDAL 1.11 or newer; if you have QGIS 2.6 or newer, this probably came with a new enough GDAL.
If you don't have a new enough GDAL, consider upgrading QGIS, which usually bundles newer versions.
> Want to check what versions you have? In QGIS, open Help | About | About.

### How to do it

1. Open the Add Vector dialog.
2. Click on the Browse button and select the naturalearthsample.gpkg file:
3. Now, back in the main dialog, click on Open.
4. This should prompt you, asking which layers you want to add from the database:

> Note that the QGIS browser can detect and read Geopackage files.
Just navigate to the file and double-click on it to get the same layer selector, as shown in the preceding screenshot.


### How it works

Consider Geopackage more of a read-only format.
Even though it is not, its whole purpose is to exchange collections of data between systems with a single file, especially mobile systems.
Due to this, once you have loaded layers, consider saving them to another format.
Keep in mind that saving to Shapefiles may cause data loss in the attribute table.
Spatialite or PostGIS are the recommend formats; refer to recipes Loading vector layers into SpatiaLite and Loading vector layers into PostGIS in Chapter 1, Data Input and Output.


### There's more



Geopackage is also a database that is based on SQLite and it is compatible with SpatiaLite.
If you open it with SpatiaLite tools, you should be able to query the tables.
Geopackage and SpatiaLite store geometries differently, so not all functions or spatial index methods are available to Geopackages, but they are very easy to convert.

QGIS 2.10 introduces the ability to write a single layer to a Geopackage.
It's expected that QGIS 2.12 will add the ability to write multiple layers to the same Geopackage (as well as SpatiaLite).
In the meantime, you can use ogr2ogr on the command line to manage layers in a Geopackage.

If you want to batch convert a Geopackage to SpatiaLite, this can be done on the command line (OSGeo4W Shell on Windows, and a Terminal on Mac or Linux), as follows:
```py
ogr2ogr -f SQLite naturalearthsample.sqlite naturalearthsample.gpkg
-skip-failures -nlt
PROMOTE_TO_MULTI -dsco SPATIALITE=YES
```
You can also perform the reverse to create a Geopackage:
```py
ogr2ogr -f GPKG naturalearthsample.gpkg naturalearthsample.sqlite
```
Keep your eyes out for future implementation of raster support.
The Geopackage specification does include limited raster support, which is primarily targeted at including imagery or tiles in the database for use on mobile devices.


### See also

* For more information about the format, visit http://www.geopackage.org/

## 12.5 The PostGIS Topology Editor plugin

Maintaining topology in the vector layers is very important; this results in greater data integrity and leads to more accurate analysis results.
This recipe shows you how to edit PostGIS topology layers (in other words, layers with topology objects, such as edges, faces, and nodes) with QGIS.

> Installation of PostGIS with topology support won't be covered in detail here because instructions for the different operating systems can be found on the project website at http://postgis.net/docs/manual-2.1/postgis_installation.html.
If you are using Windows, PostGIS can be installed directly from the Stack Builder application, which is provided by the standard PostgreSQL installation, as described at http://www.bostongis.com/PrinterFriendly.aspx?content_name=postgis_tut01.



### Getting ready

To follow this exercise, you need a PostGIS database with topology enabled. In QGIS, you should set up the connection to the database using the New button in the Add PostGIS Layers dialog.

Also, it is necessary to install and activate the PostGIS Topology Editor plugin.


### How to do it


These steps will create a topology-enabled vector layer in your PostGIS database:
1.  Open DB Manager by navigating to Database | DB Manager.
2.  In the tree to the left-hand side of the dialog, select the database that you want to create the topology in.
3.  Go to Database | SQL window to open SQL-editor, as shown into following screenshot:
4.  In the editor, paste the contents of the topology.sql file and click on Execute (F5) to run the queries.
5.  After the topology has been created, click on the Refresh button on the DB Manager toolbar to reload the list of available tables. You should see a new topo1 table in Tree.
6.  Select the newly created topo1 table in Tree and go to Schema | TopoViewer to load all the topology layers into QGIS. The result should look like the following:

Once the topology is ready and loaded into QGIS, we can edit it with the PostGIS Topology Editor plugin. It is worth mentioning that, currently, the plugin allows us only to delete nodes and edges. Other editing operations are not supported.
To delete a node, perform the following steps:

1.  In Layers Panel, expand the Nodes group and select the topo1.node layer.
2.  Using the Select features by area or single click tool, select the QGIS canvas isolated node that you want to delete, for example node 17.
3.  Click on the Remove node button, and the node will be deleted. In case of any error, you will see an error message with a possible reason.
> Remember that the PostGIS Topology Editor plugin operates on the database level and all actions performed by it can not be reverted.

To delete edges, perform the following steps:
1.  In Layers Panel, expand the Edges group and select the edge layer that you want to edit, for example, the topo1.edge layer.
2.  Using the Select features by area or single click tool, select the edges that you want to delete.
3.  Click on the Remove edge button to remove the edges, and they will be deleted. In case of any error, you will see an error message with a possible reason.

As QGIS currently does not support dynamic updates of topology, it is necessary to reload topology layers with TopoViewer to reflect our edits:
1.  Create a new project by clicking on the New button on the QGIS toolbar.
2.  Open DB Manager by navigating to Database | DB Manager.
3.  In the tree to the left-hand side of the dialog, find the database with your topology layers.
4.  Select the topo1 table in the tree, and go to Schema | TopoViewer to load all topology layers into QGIS:

You will see that the previously deleted nodes and edges now disappear.

### How it works

The PostGIS Topology Editor plugin issues SQL queries directly to the corresponding topology tables in the PostGIS database to remove edges and nodes.


### There's more

* More information about PostGIS topology support can be found in the official PostGIS documentation at http://postgis.net/docs/manual-2.1/Topology.html

## 12.6 The Topology Checker plugin

Topology is a set of rules that defines the spatial relationship between adjacent features, and it also defines and enforces data integrity and validity.

This recipe shows you how to use the built-in Topology Checker plugin to find topology errors in vector layers.

### Getting ready


To follow this recipe, load the census_wake2000_topology.shp and roadsmajor.shp layers from the sample data.
Additionally, make sure that the Topology Checker plugin is enabled in Plugin Manager.


### How to do it

The Topology Checker plugin allows us to test a vector layer or its part for different topology errors. Before we can use this, we should load all the layers that we want to test in QGIS and then configure topology rules:
1. Enable the Topology Checker panel in the View | Panels menu.
This should add the plugin panel to the user interface, as shown in the following screenshot:
2. Click on the Configure button at the bottom of the plugin panel to open the Topology Rule Settings dialog:
3. To set a rule, choose a layer that you want to check in the first combobox. Select the roadsmajor layer.
4. Then, select the rule from the second combobox, for example, must not have dangles.
As this rule needs only one layer, the third combobox disappears.
5. Note that the list of available rules depends on the geometry type of the target layer; also, some rules allow the testing of the spatial relationship between two layers.
6. Click on the Add Rule button to add rule to the list of current rules:
7. Let's add another rule, this time to check the polygonal layer. Select census_ wake2000_topology as the target layer.
8. Select must not overlap as the rule and click on the Add Rule button to create a new rule.
9. Using the same approach, add another two rules to check this layer: must not have gaps and must not have duplicates:
10. Click on the OK button to save your settings and close the dialog.
11. Now, when we have defined the rules, we can check topology of the whole layers by clicking on the Validate All button or only features within visible area by clicking on the Validate Extent button.
For this recipe, we will validate all layers, so click on the Validate All button.
12. After some time (this depends on the number of the features in the layer and computer speed), all detected topology errors will be displayed in the plugin panel, as shown in the following screenshot:
13. If the Show errors checkbox is activated (this is the default setting), topology errors that can be visualized will also be highlighted in red on the map:
14. Selecting the error in the plugin panel will center the QGIS map canvas on the problematic feature and highlight it in green if possible.



### How it works

The Topology Checker plugin uses the GEOS library as well as its own algorithms to check spatial relationships between features in the vector layer.


### There's more

*  A short introduction to vector topology can be found in the Gentle GIS introduction at https://docs.qgis.org/2.2/en/docs/gentle_gis_introduction/topology.html
*  More information about the available rules of the Topology Checker plugin can be found in the QGIS User Guide at http://docs.qgis.org/2.8/en/docs/user_manual/plugins/plugins_topology_checker.html

## 12.7 GRASS Topology tools


Having vector data without topology errors is important for further analysis, as these errors may lead to incorrect results.

This recipe shows you how to use the built-in GRASS tools to fix various topology errors in vector layers.



### Getting ready

QGIS has very good integration with GRASS GIS; there is a GRASS plugin that provides access to the GRASS GIS database and functionality.
GRASS algorithms are also available from the Processing plugin.
The latter is simpler because you don't need to bother with setting up GRASS locations and mapsets and importing and exporting data.

To follow this recipe, load the nonbreak.shp, dangles.shp, and nosnap.shp layers from the sample data.
Additionally, make sure that the Processing plugin is enabled in Plugin Manager.



### How to do it

The following steps show you how to fix various topology errors with the GRASS v.clean toolset using the Processing toolbox:

First, we will learn how to remove dangling lines. Dangling lines are lines that have no connection with other lines on one or either end nodes:

To remove them, perform the following steps:
1. In the Processing Toolbox menu, find the v.clean algorithm by typing its name in the filter field at the top of the toolbox.
Double-click on the algorithm name to open its dialog.
2. In the Layer to clean combobox, select the dangling layer.
3. In the Cleaning tool combobox, select rmdangle—a tool for the removal of dangles.
4. The Threshold field is used to define the maximum length of the dangling line.
For our example, enter 6.000000:
5. Click on the Run button to remove dangles.
When the algorithm is finished, two new layers will be added to QGIS: the Cleaned layer contains cleaned geometries (shown in green) and the Errors layer contains dangles that were removed (shown in red):

Another topology issue is missed line breaks in the intersection nodes.
To add break at intersections, perform the following steps:
1. In the Processing Toolbox menu, find the v.clean algorithm by typing its name in the filter field at the top of the toolbox. Double-click on the algorithm name to open its dialog.
2. In the Layer to clean combobox, select the nobreaks layer.
3. In the Cleaning tool combobox, select break.
4. Leave all other parameters unchanged and click on the Run button to break lines on intersections. When the algorithm is finished, a new layer will be added to QGIS. You can easily verify that now lines are split at the intersection point.

Another very common topology issue is undershots, which happen when the line feature is not connected with another one at the intersection point and overshoots, which happens when the line ends beyond another line instead of connecting to it.
Such errors often appear after inaccurate digitizing.
To fix them, perform the following steps:

1. In the Processing Toolbox menu, find the v.clean algorithm by typing its name in the filter field at the top of the toolbox. Double-click on the algorithm name to open its dialog.
2. In the Layer to clean combobox, select the nosnap layer.
3. In the Cleaning tool combobox, select snap.
4. The Threshold field is used to define the snapping tolerance in map units.
For our example, you can leave this unchanged.
5. Click on the Run button to remove overshoots and undershots.
When the algorithm is finished, two new layers will be added to QGIS: the Cleaned layer contains features with fixed errors and the Errors layer contains original invalid features.


### How it works

The rmdangle tool simply sequentially removes all dangling lines with length less than the given threshold.
If the threshold is less than 0, then all dangles will be removed.

The break tool breaks lines at intersections, so all lines will have a common node.
This tool does not need a threshold value to be specified.

The snap tool tries to snap vertices to another one within the given threshold, if no appropriate vertices are found, then no snapping is done.
It is worth mentioning that large threshold values may break the topology of polygonal features.

### There's more

If you need more control over the topology cleaning process, try to use v.clean.advanced from the Processing Toolbox menu or consider using the GRASS plugin.

Also, there are other ways to clean vector topology, for example, using the lwgeom functions or external tools such as prepair and pprepair.
Both tools are available as Processing plugins, and they can be installed via Plugin Manager.

### See also


* More information about the GRASS v.clean toolset can be found at http://grass.osgeo.org/grass64/manuals/v.clean.html

## 12.8 Hunting for bugs

While QGIS developers do their best to make every QGIS release as stable as possible, sometimes you may encounter bugs or even crashes.
To get them fixed in the future, it is necessary to inform the developers about issues.

This recipe shows you how to perform basic debugging and collect information that will help developers understand the problem better and help to fix it.


### Getting ready

As the QGIS development process is very quick, bugs that are present in older versions are very likely already fixed in the latest version.
So, it is necessary to ensure that you have the most recent QGIS version.
If you use the development version of QGIS (so called "nightly" builds), upgrade to the last available build.
If you prefer stable releases, then ensure that you have the latest stable version.

### How to do it

1. Repeat the same actions again using the same data and settings to ensure that this is not an accidental error.
2. Test your vector data (if any) with geometry checking tools to ensure that data is valid and has no geometry errors. If the data has geometry errors, then try to reproduce the bug with valid data.
3. Check whether the same error happens with other data to ensure that this is not related to the specific dataset.
4. If the error happens only on some specific features, extract them into a separate layer and make a small self-containing test dataset that allows you to reproduce bug. The same approach should be used if the dataset is large.
5. Sometimes, errors may be caused by third-party plugins. Disable all plugins and try  to reproduce the error. If you cannot reproduce the bug with the disabled plugins, probably this bug is somehow related to some plugin. To find this problematic plugin, activate the plugins one by one and try to reproduce the error.
6. Look in the QGIS message log, it may contain useful debug and/or error messages that are related to your problem:
7. To open the Log Messages window, click on the Messages button located in the right corner of the QGIS status bar.
8. If QGIS crashes, try to create a backtrace and/or collect debug messages (refer to the following sections). This will be extremely useful if your bug is not reproducible on the developer's computer.

__Creating a backtrace under Linux__
Under Linux, QGIS automatically tries to use gdb to produce a backtrace when crashed.
> To see the backtrace, it is necessary to start QGIS from the terminal emulator.

If you see no backtrace after the crash, this may mean that the possibility to connect debugger to the running processes is disabled in your distribution (for example, Ubuntu after         version 10.10). This behavior is controlled by the ptrace_scope sysctl value. If it equals to 1 ptrace calls from external processes are not allowed. A value that equals to 0 allows external processes to examine memory of the other process.
In such cases, to enable a backtrace creation, temporarily open the root shell and execute the following command:
```
echo 0 > /proc/sys/kernel/yama/ptrace_scope
```

If you want to enable a backtrace creation permanently, you need to edit the /etc/ sysctl.d/10-ptrace.conf file as root, and set the value to 0. Then, run as root to reload sysctl settings, as follows:
```
sysctl -p
```

After this, repeat the steps to reproduce the crash, copy the backtrace, and attach it to your bug report or e-mail.

__Capturing debug output with DebugView under Windows__
DebugView is a small program for the Windows operating system that allows you to view and save the debug output of programs. With its help, you can easily get the QGIS debug output and add it to your bug report.

> Note that you will see no debug output if your QGIS compiled without debugging support. Official packages from the OSGeo4W installer and the QGIS standalone installer are built with the debugging output.

To get the debug output with DebugView, follow these steps:
1. Download DebugView from the Microsoft site at https://technet.microsoft.com/en-us/sysinternals/bb896647.aspx.
2. Extract the archive to some folder on your hard drive and launch Dbgview.exe.
3. Start QGIS and perform the actions that lead to a crash or an error:
4. Save the log to a file using the Save button on the DebugView toolbar.
5. Attach the saved file to your bug report or e-mail.
Also, if QGIS crashes, it produces a minidump file (usually these files are created in your Temp directory and have the tmp.mdmp extension), as shown in the following screenshot:
6. This file should also be attached to the bug report, as it allows developers to understand the problem better even if they cannot reproduce the crash on their computers.


### How it works

A backtrace is a summary of program functions that are still active.
In other words, it shows all nested functions and calls from the program's start to the crash point.
With the help of a backtrace, developers can isolate place where the bug is.



### There's more

If you have access to computers with different operating systems, it would be good to check whether this error is reproduced in different environments.

Almost all modern computers and laptops have enough performance to run virtual machines.
The snapshots feature is available in the most popular virtual machines.
You can have a clean and up-to-date system with recent QGIS for testing and debugging purposes.



### See also


* More information about backtrace creation can be found on the QGIS site at http://qgis.org/en/site/getinvolved/development/index.html#creating-a-backtrace

## 12.9 Reporting bugs

Once you have found a bug and collected all the potentially useful information about its occurrence, it is time to create a bug report.

This recipe shows you how to file a bug report in a right way.


### Getting ready


While QGIS project hosts its own bugtracker, you still need an OSGeo User ID to use it.
If you don't have an OSGeo account, create one by filling in the form at https://www.osgeo.org/cgi-bin/ldap_create_user.py.

### How to do it

Go to the QGIS bugtracker at http://hub.qgis.org/projects/quantum-gis/issues and use your OSGeo User ID and password to log in.
The Login link is located in the top-right corner of the page.

__Before creating a new bug report, it is necessary to make sure this bug has not yet been reported.__
To do this, perform the following steps:
1. Go to the Issues tab.
2. In the Filters group, add and configure the necessary filters. For example, the following filters:
* Status: Configure this to find only open issues
* Subject: Configure this to find issues with the given substring in the Subject field
3. Click on the Apply link above the issues list to apply your filters:
4. Check whether the resulting list contains issues that are similar to one that you found.
All tickets that match your criteria will be listed in the table.

__Also, it makes sense to try to find similar issues with the ordinal search functionality:__
1. Open the Search page at https://hub.qgis.org/search/index/quantum-gis.
2. Enter the keywords in the field.
3. Select QGIS Application from the combobox.
4. If necessary, perform the search only in ticket titles by activating the corresponding checkbox.
5. Deactivate all checkboxes under the search field except Issues to find only issues with given keywords.
6. Click on the Submit button to start the search.

Results will be displayed as a raw list of all existing tickets (open and closed), which contain keywords in their titles and description:

__If your issue is already reported, add your observations to it.
If you cannot find  anything similar to your problem, it is time to submit a bug report. To do this, perform the following steps:__
1. Log in to the QGIS bugtracker with your OSGeo user ID and password.
2. Go to the QGIS Application project at http://hub.qgis.org/projects/quantum-gis.
3. Open the New issue tab by clicking on the corresponding link in the menu and populate the form with the requested information:

__This form contains many fields, the most important ones are listed as follows:__
* __Tracker__: This defines the ticket type. For bug reports, select Bug Report.
* __Subject__: This is a short and clear description of the problem. It will be used as the ticket title.
* __Description__: This is a full description of the issue. Describe your problem in detail and provide steps to reproduce it.
If there are any debug messages in the console, backtraces, or minidumps, include or attach them, as well as the sample dataset.
If you suspect that bug is related to the specific platform version or a specific version of the third-party dependency package (for example, GDAL, GEOS, SpatiaLite, and so on) provide this information too.
* __Priority__: This is where you set the anticipated importance of the bug. Currently, the following classification is used:
* __Low__: This is used for bugs that does not affect day-to-day usage of QGIS
* __Normal__: This is the default value for all new bug reports and feature requests
* __High__: This is used for bugs that have a significant impact on QGIS usability in some cases, but at the same time, they do not block QGIS usage in other tasks
* __Blocker__: This is used for bugs that make QGIS totally unusable, leads to data loss or corruption, or for regressions from previous QGIS versions
* __Component__: This chooses the most appropriate subsystem of QGIS, which is closely related to the issue.
* __Platform and Platform version__: This specifies the operating system and its version, respectively.
* __Causes crash or corruption__: This activates the checkbox if the bug causes a QGIS crash or data loss or corruption.

Check the formatting of the bug report by clicking on the Preview link at the bottom. To submit a bug report, click on the Submit button.



### How it works

Bug Tracker is a database with information about bugs.
Developers look over bug queue and arrange them according to priorities, available resources, and fix them.
Fixes usually go to the development version (the so-called "master"), but fixes for regressions and important bugs also go to the long-term release branch.



### There's more


Using the QGIS Bug Tracker, you also can leave feature requests and submit patches.
However, for the latter, creating a pull-request at GitHub is preferable.



### See also

* More information about OSGeo User ID can be found at http://www.osgeo.org/osgeo_userid.
* Additional information about using QGIS Bug Tracker can be found at the following wiki page https://hub.qgis.org/wiki/17/Bugreports.
* Also, the BUGS document in the QGIS source tree contains some useful tips.
You can find it in the QGIS GitHub repository at https://github.com/qgis/QGIS/blob/master/BUGS.

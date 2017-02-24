
# Chapter 1: A Refreshing Look at QGIS

* Downloading QGIS and its installation  
* The QGIS graphical user interface  
* Loading data  
* Working with coordinate reference systems  
* Working with tables  
* Editing data  
* Styling data  
* Composing a map  
* Finding and installing plugins  

## 1.1 QGIS download and installation

### 1.1.1 Installing QGIS on Windows
### 1.1.2 Installing QGIS on Mac OS X
### 1.1.3 Installing QGIS on Ubuntu Linux
* __Installing QGIS only__   
* __Installing QGIS and other FOSSGIS Packages__  


```python
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:ubuntugis/ubuntugis-unstable
sudo apt-get update
sudo apt-get install qgis python-qgis qgis-plugin-grass
```

> QGIS is also available for Android.  
We have not provided detailed installation instructions because it is in alpha testing at the moment.  
However, there are plans to have a normalized installation process in a future release.  
You can fid more information about this at http://hub.qgis.org/projects/ android-qgis.  
The download page is available at http://qgis.org/ downloads/android/.  
A related app has recently been announced and it is named QField for QGIS.  
For a short time, it was named QGIS Mobile.  
It is described as a fild data capture and management app that is compatible with QGIS.  
At the time of writing this, it was in invite-only alpha testing.  
It is eventually expected to be available in the Android Play Store.  
You can fid more information on this app at http://www.opengis.ch/tech-blog/.  


## 1.2 Tour of QGIS

### 1.2.1 QGIS Desktop
### 1.2.2 QGIS Browser

* __Param__: This tab displays details of data that is accessed through connections, such as a database or WMS.  
* __Metadata__: This tab displays the metadata (if any) of the selected data.  
* __Preview__: This tab renders the selected data. You can zoom into the data using your mouse wheel and pan using the arrow keys on your keyboard.  
* __Attribute__: This tab displays the attribute table associated with the selected data. You can sort the columns by clicking on the column headings.  

## 1.3 Loading data

### 1.3.1 Loading vector data

To load vector fies, click on __Add Vector Layer__ by navigating to __Layer | Add Layer__.  
This will open the __Add Vector Layer__ dialog that will allow us to choose the source type and source of the dataset that we wish to load.  
The source type contains four options: __File, Directory, Database, and Protocol__.  
When you choose a source type, the source interface will change to display the appropriate options.  
Let's take a moment to discuss what type of data these four source types can load:   


* __File__: This can load flat files that are stored on disk. The commonly used flat file types are as follows:  
  * ESRI shapefile (.shp)  
  * AutoCAD DXF (.dxf)  
  * Comma separated values (.csv)  
  * GPS eXchange Format (.gpx)  
  * Keyhole Markup Language (.kml)  
  * SQLite/SpatiaLite (.sqlite/.db)  
* __Directory__: This can load data stored on disk that is encased in a directory. The commonly used directory types are as follows:  
  * U.S. Census TIGER/Line  
  * Arc/Info Binary Coverage  
* __Database__: This can load databases that are stored on disk or those available through service connections. The commonly used database types are as follows:  
  * ODBC  
  * ESRI Personal GeoDatabase  
  * MSSQL  
  * MySQL  
  * PostgreSQL  
* __Protocol__: This can load protocols that are available at a specific URI. QGIS currently supports loading the GeoJSON protocol.  

### 1.3.2 Loading raster data

To load raster data into QGIS, click on __Add Raster Layer__ by navigating to __Layer | Add Layer__.  
This will open a fie browser window and allow you to choose a GDAL-supported raster file.  
The commonly used raster types supported by GDAL are as follows:   


* ArcInfo ASCII Grid (.asc)  
* Erdas Imagine (.img)  
* GeoTIFF (.tif/.tiff)  
* JPEG/JPEG-2000 (.jpg or .jpeg/.jp2 or .j2k)  
* Portable Network Graphics (.png)  
* Rasterlite (.sqlite)  
* USGS Optional ASCII DEM (.dem)  

To add an __Oracle GeoRaster__, click on __Add Oracle GeoRaster Layer__ by navigating to __Layer | Add Layer__, then connect to an Oracle database to load the raster.  
More information about loading database layers is in the following section.  

The __Geospatial Data Abstraction Library (GDAL)__ is a free and open source library that translates and processes vector and raster geospatial data formats.  
QGIS, as well as many other programs, use GDAL to handle many geospatial data processing tasks.  
You may see references to OGR or GDAL/OGR as you work with QGIS and GDAL.  
OGR is short for OGR Simple Features Library and references the vector processing parts of GDAL.  
OGR is not really a standalone project, as it is part of the GDAL code now; however, for historical reasons, OGR is still used.  
More information about GDAL and OGR can be found at http://gdal.org.  
GDAL is an Open Source Geospatial Foundation (http://osgeo.org) project.  


### 1.3.3 Loading databases

QGIS supports __PostGIS, SpatiaLite, MSSQL, and Oracle__ databases.  
Regardless of the type of database you wish to load, the loading sequence is very similar.  
Therefore, instead of covering specifi examples, the general sequence will be covered.  
First, click on __Add Layer__ under __Layer__ and then choose the database type you wish to load.  
This will open a window with options for adding the data stored in a database.  
As an example, the following screenshot shows the window that opens when you navigate to __Layer | Add Layer | Add SpatiaLite Layer__:   


![](Chapter01/fig1_3_3.PNG)

### 1.3.4 Web services

QGIS supports the loading of OGC-compliant web services such as __WMS/WMTS, WCS, and WFS__.  
Loading a web service is similar to loading a database service.  
In general, you will create a new server connection, connect to the server to list the available services, and add the service to the QGIS project.  
  


## 1.4 Working with coordinate reference systems

When working with spatial data, it is important that a __coordinate reference system (CRS)__ is assigned to the data and the QGIS project.  
To view the CRS for the QGIS project, click on __Project Properties__ under __Project__ and choose the __CRS tab__.  

It is recommended that all data added to a QGIS project be projected into the same CRS as the QGIS project.  
However, if this is not possible or convenient, QGIS can project layers "on the fly" to the project's CRS.  
  


> If you want to quickly search for a CRS, you can enter the EPSG code to quickly fiter through the CRS list.  
An EPSG code refers to a specifi CRS stored in the EPSG Geodetic Parameter Dataset online registry that contains numerous global, regional, and local CRS.  
An example of a commonly used EPSG code is 4326 that refers to WGS 84.  
The EPSG online registry is available at http://www.epsg-registry.org/.  
  


__To enable the "on the fly" projection, perform the following steps:__  
1. Click on Project Properties under Project.  
2. Choose the CRS tab and Enable 'on the fly' CRS transformation.  
3. Set the CRS that you wish to apply to the project and make all layers that are not set to the project's CRS transform "on the fly".  
  
__To view the CRS for a layer, perform the following steps:__  
1. Open the layer's properties by either navigating to Layer | Properties or by right-clicking on the layer in the Layers panel.  
2. Choose Properties from the context menu and then choose the General tab.  
3. If the layer's CRS is not set or is incorrect, click on Specify to open the CRS selector window and select the correct CRS.  
  
__To project a layer to a different CRS, perform the following steps:__  
1. Right-click on the layer in the Layers panel and then choose Save As from the context menu.  
2. In the Save vector layer as dialog, set the fie format and fiename, then set CRS to Selected CRS and click on Change to set the target CRS, and save the fie.  
  
__To create a new CRS or modify an existing CRS, perform the following steps:__  
1. Click on Custom CRS under Settings to open the Custom Coordinate Reference System Defiition window.  
2. Click on the Add new CRS button to add a new entry to the CRS list.  
3. With the new CRS selected, we can set the name and parameters of the CRS. The CRS properties are set using the Proj.4 format. To modify an existing CRS, click on Copy existing CRS and select the CRS from which you wish to copy parameters; otherwise, enter the parameters manually.  


Proj.4 is another Open Source Geospatial Foundation (http://osgeo.org) project used by QGIS, and it is similar to OGR and GDAL.  
This project is for managing coordinate systems and projections.  
For a detailed user manual for the Proj.4 format used to specify the CRS parameters in QGIS, download it from  
ftp://ftp.remotesensing.org/proj/OF90-284.pdf.   
https://github.com/OSGeo/proj.4/blob/master/docs/old/of90-284.pdf  


```python

```

## 1.5 Working with tables

There are two types of tables you can work with in QGIS: attribute tables and standalone tables.  
Whether they are from a database or associated with a shapefie or a flt fie, they are all treated the same.  
Standalone tables can be added by clicking on the Add Vector Layer menu by navigating to Layer | Add Layer.  
QGIS supports the table formats supported by OGR along with database tables.  
__Tables are treated like any other GIS layer; they simply have no geometry.__  
Both types of tables can be opened within Desktop by selecting the layer/table in the Layers panel, and then by either clicking on Open Attribute Table under Layer or by right-clicking on the data layer, and choosing Open Attribute Table from the context menu.  
They can also be previewed in Browser by choosing the Attribute tab.  
  
  
__The table opens in a new window that displays the number of table rows and selected records in the title bar.__  
Below the title bar are a series of buttons that allow you to toggle between editing, managing selections, and adding and deleting columns.  
Most of the window is filed with the table body.  
The table can be sorted by clicking on the column names.  
An arrow will appear in the column header, indicating either an ascending or a descending sort.  
Rows can be selected by clicking on the row number on the left-hand side.  
In the lower-left corner is a Tables menu that allows you to manage what portions of the table should be displayed.  
You can choose Show All Features (default setting), Show Selected Features, Show Features Visible on Map (only available when you view an attribute table), Show Edited and New Features, create column fiters, and advanced fiters (expression).  
The lower-right corner has a toggle between the default table view and a forms view of the table.  
  


![](Chapter01/fig1_5.PNG)

> Attribute tables are associated with the features of a GIS layer.  
__Typically, one record in the attribute table corresponds to one feature in the GIS layer.__  
The exception to this is multipart features, which have multiple geometries linked to a single record in the attribute table.  
__Standalone tables are not associated with GIS data layers.__  
__However, they may have data of a spatial nature from which a spatial data layer can be generated (for more information, see Chapter 6, Advanced Data Creation and Editing.__  
They may also contain data that you wish to join to an existing attribute table with a table join.  

### 1.5.1 Table joins

Let's say that you need to make a map of the total population by county.  
However, the counties' GIS layer does not have population as an attribute.  
Instead, this data is contained in an Excel spreadsheet.  
It is possible to join additional tabular data to an existing attribute table.  
There are two requirements, which are as follows:  
* The two tables need to share fields with attributes to match for joining  
* There needs to be a cardinality of __one-to-one__ or __many-to-one__ between the attribute table and the standalone table  
  
To create a join, load both the GIS layer and the standalone table into QGIS Desktop.  
QGIS will accept a variety of standalone table fie formats including Excel spreadsheets, .dbf fies, and comma delimited text fies.  
  
You can load this tabular data using the __Add Vector Layer__ menu by navigating to __Layer | Add Layer__ and setting the fie type fiter to All fies (*) (*.*) as shown in the following screenshot:   
Once the data is loaded, a join can be completed by following these steps:  
1. Select the GIS layer in the Layers panel that will receive the new data from the join.  
2. Navigate to __Layer | Properties__ and choose the __Joins menu__.  
3. Click on the add join button (the one with green plus sign).  
4. Choose the Join Layer, Join Field, and Target Field values. The Join Layer and Join Field values represent the standalone table. The Target Field value is the column in the attribute table on which the join will be based.  
5. At this point, you can choose Cache the join in virtual memory, Create attribute index on the join fild, and Choose which filds are joined. The last option lets you to choose which filds from the join layer to append to the attribute table. At this point, the Add vector join window will look like the following screenshot.  
6. Once created, the join will be listed on the Joins tab. The extra attribute columns from the join layer will be appended to the attribute table, where the value in the join fild matched the value in the target fild.  
7. Joins can be removed by clicking on the remove join button (the one with red minus sign).  

![](Chapter01/fig1_5_1.PNG)

## 1.6 Editing data

### 1.6.1 Snapping

Snapping is an important editing consideration.  
It is a specifid distance (tolerance) within which vertices of one feature will automatically align with vertices of another feature.  
The specifi snapping tolerance can be set for the whole project or per layer.  
The method for setting the snapping tolerance for a project varies according to the operating system, which is as follows:   
* For Windows, navigate to __Settings | Options | Digitizing__  
* For Mac, navigate to __QGIS | Preferences | Digitizing__  
* For Linux, navigate to __Edit | Options | Digitizing__  


In addition to setting the snapping tolerance, here the snapping mode can also be set to vertex, segment, or vertex and segment.  
Snapping can be set for individual layers by navigating to __Settings | Snapping Options.__  
Individual layer snapping settings will override those of the project.  
The following screenshot shows examples of multiple snapping option choices.  
  


![](Chapter01/fig1_6_1.PNG)

### 1.6.2 Styling vector data

When you load spatial data layers into QGIS Desktop, they are styled with a random single symbol rendering.  
To change this, navigate to Layer | Properties | Style.  
There are several rendering choices available from the menu in the top-left corner, which are as follows:   
* __Single Symbol__: This is the default rendering in which one symbol is applied to all the features in a layer.  
* __Categorized__: This allows you to choose a categorical attribute field to style the layer. Choose the field and click on Classify and QGIS will apply a different symbol to each unique value in the field. You can also use the Set column expression button to enhance the styling with a SQL expression.  
* __Graduated__: This allows you to classify the data by a numeric field attribute into discrete categories. You can specify the parameters of the classification (classification type and number of classes) and use the Set column expression button to enhance the styling with a SQL expression.  
* __Rule-based__: This is used to create custom rule-based styling. Rules will be based on SQL expressions.  
* __Point displacement__: If you have a point layer with stacked points, this option can be used to displace the points so that they are all visible.  
* __Inverted polygons__: This is a new renderer that allows a feature polygon to be converted into a mask. For example, a city boundary polygon that is used with this renderer would become a mask around the city. It also allows the use of Categorized, Graduated, and Rule-based renderers and SQL expressions. 

The following screenshot shows the Style properties available for a vector data layer:  

![](Chapter01/fig1_6_2.PNG)

__In the preceding screenshot, the renderer is the layer symbol.__  
For a given symbol, you can work with the fist level, which gives you the ability to change the transparency and color.  
You can also click on the second level, which gives you control over parameters such as fil, border, fil style, border style, join style, border width, and X/Y offsets.  
These parameters change depending on the geometry of your layer.  
__You can also use this hierarchy to build symbol layers, which are styles built from several symbols that are combined vertically.__  


### 1.6.3 Styling raster data

You also have many choices when styling raster data in QGIS Desktop. There is a different choice of renderers for raster datasets, which are as follows:  
* __Singleband gray__: This allows a singleband raster or a single band of a multiband raster to be styled with either a black-to-white or white-to-black color ramp. You can control contrast enhancement and how minimum and maximum values are determined.  
* __Multiband color__: This is for rasters with multiple bands. It allows you to choose the band combination that you prefer.  
* __Paletted__: This is for singleband rasters with an included color table. It is likely that it will be chosen by QGIS automatically, if this is the case.  
* __Singleband pseudocolor__: This allows a singleband raster to be styled with a variety of color ramps and classification schemes.  

The following is a screenshot of the Style tab of a raster fie's Layer Properties showing where the aforementioned style choices are located:  

![](Chapter01/fig1_6_3.PNG)

### 1.6.4 Contrast enhancement

Another important consideration with raster styling is the settings that are used for contrast enhancement when rendering the data.  
Let's start by loading the Jemez_ dem.img image and opening the Style menu under Layer Properties (shown in the fiure below).  
This is an elevation layer and the data is being stretched on a black to-white color ramp from the Min and Max values listed under Band rendering.  
By default, these values only include those that are from 2 percent to 98 percent of the estimation of the full range of values in the dataset, and cut out the outlying values.  
This makes rendering faster, but it is not necessarily the most accurate.  
  
  
  


![](Chapter01/fig1_6_4.PNG)

Next, we will change these settings to get a full stretch across all the data values in  
the raster. To do this, perform the following steps:  
1. Under the Load min/max section, choose Min / max and under Accuracy, choose Actual (slower).  
2. Click on Load.  
3. You will notice that the minimum and maximum values change. Click on Apply.  


![](Chapter01/fig1_6_4-2.PNG)

You can specify the default settings for rendering rasters by navigating to __Settings | Options | Rendering__.  
Here, the defaults for the __Contrast enhancement, Load min/max values, Cumulative count cut thresholds__, and the __standard deviation multiplier__ can be set.  


### 1.6.5 Blending modes

The blending modes allow for more sophisticated rendering between GIS layers.  
Historically, these tools have only been available in graphics programs and they are a fairly new addition to QGIS.  
Previously, only layer transparency could be controlled.  
__There are now 13 different blending modes that are available: Normal, Lighten, Screen, Dodge, Addition, Darken, Multiply, Burn, Overlay, Soft light, Hard light, Difference, and Subtract.__  
These are much more powerful than simple layer transparency, which can be effective but typically results in the underneath layer being washed out or dulled.  
With blending modes, you can create effects where the full intensity of the underlying layer is still visible.  
__Blending mode settings can be found at the bottom of the Style menu under Layer Properties in the Layer Rendering section along with the Layer transparency slider.__  
They are available for both vector and raster datasets.  
  
In this example of using blending modes, we want to show vegetation data __(Jemez_ vegetation.tif)__ in combination with a hillshade image __(Jemez_hillshade.img)__.  
Both data sets are loaded and the vegetation data is dragged to the top of the layer list.  
Vegetation is then styled with a Singleband pseudocolor renderer; you can do this by performing the following steps:   
  
1. Choose __Random colors__.  
2. Set Mode to __Equal interval__.  
3. Set the number of __Classes to 13__.  
4. Click on __Classify__.  
5. Click on __Apply__.  
  
The following screenshot shows what the Style properties should look like after following the preceding steps.  


![](Chapter01/fig1_6_5.PNG)

At the bottom of the __Style menu__ under __Layer Properties__, set the __Blending mode__ to __Multiply__ and the __Contrast to 45__ and click on Apply.  
The blending mode allows all the details of both the datasets to be seen.  
Experiment with different blending modes to see how they change the appearance of the image.  
The following screenshot shows an example of how blending and contrast settings can work together to make a raster 'pop' off the screen:   


![](Chapter01/fig1_6_5-2.PNG)


```python

```

## 1.7 Composing maps

With QGIS, you can compose maps that can be printed or exported to image and graphic fies.  
To get started, click on __New Print Composer__ under __Project__.  
Give the new composition a name, click on OK, and the composer window will open.  
  
The composer presents you with a blank sheet of paper upon which you can craft your map.  
Along the left-hand side, there are a series of tools on the Composer Items toolbar.  
The lower portion of the toolbar contains buttons for adding map elements to your map.  
These include the map body, images, text, a legend, a scale bar, graphic shapes, arrows, attribute tables, and HTML frames.  
Map elements become graphics on the composition canvas.  
By selecting a map element, graphic handles will appear around the perimeter.  
These can be used to move and resize the element.  
The upper portion of the Composer Items toolbar contains tools for panning the map data, moving other graphic content, and zooming and panning on the map composition.  
  
The majority of the map customization options can be found in the composer tabs.  
To specify the sheet size and orientation, use the Composition tab.  
Once map elements have been added to the map, they can be customized with the Item properties tab.  
The options available on the Item properties tab change according to the type of map element that is selected.  
The Atlas generation tab allows you to generate a map book.  
For example, a municipality could generate an atlas by using a map sheet GIS layer and specifying which attribute column contains the map sheet number for each polygon.  
The Items tab allows you to toggle individual map elements on and off.  
  


The toolbars across the top contain tools for aligning graphics __(the Composer Item Actions toolbar)__, navigating around the map composition (the Paper Navigation toolbar), and tools for managing, saving, and exporting compositions __(the Composer toolbar)__.  
__Maps can be exported as images, PDFs, and SVG graphic files.__  
To export the map, click on the Composer menu and select one from among __Export as image..., Export as SVG..., or Export as PDF...__  
depending on your needs.  
The following is a screenshot showing parts of the composer window.  
  


![](Chapter01/fig1_7.PNG)

There are so many potential workflws, analysis settings, and datasets within the broad fild of GIS that no out-of-the-box software could contain the tools for every scenario.  
Fortunately, QGIS has been developed with a plugin architecture.  
Plugins are add-ons to QGIS that provide additional functionality.  
Some are written by the core QGIS development team and others are written by QGIS users.  
  


You can explore the QGIS plugin ecosystem by navigating to __Plugins | Manage and Install Plugins__.  
This opens the Plugins Manager window (shown in fiure below) that will allow you to browse all plugins, those that are installed, and those that are not installed, and adjust the settings.  
If there are installed plugins with available upgrades, there will also be an Upgradable option.  
The search bar can be used to enter search terms and fid available plugins related to the topic.  
This is the fist place to look if there's a tool or extra type of functionality that you need! To install a plugin, simply select it and click on the Install Plugin button.  
Installed plugins can be toggled on and off by checking the box next to each.  
  
You will be notifid by a link at the bottom of the QGIS Desktop application if there are updates available for your installed plugins.  
Clicking on the link will open the Plugins Manager window, where the Upgrades tab will allow you to install all or some of the available updates.  
__Plugins themselves may show up as individual buttons, toolbars, or as items under the appropriate menu, such as Plugins, Vector, Raster, Database, Web, or Processing.__  
  


## 1.8 Adding functionality with plugins

There are so many potential workflws, analysis settings, and datasets within the broad fild of GIS that no out-of-the-box software could contain the tools for every scenario.  
Fortunately, QGIS has been developed with a plugin architecture.  
Plugins are add-ons to QGIS that provide additional functionality.  
Some are written by the core QGIS development team and others are written by QGIS users.  
  


You can explore the QGIS plugin ecosystem by navigating to __Plugins | Manage and Install Plugins.__  
This opens the __Plugins Manager__ window (shown in fiure below) that will allow you to browse all plugins, those that are installed, and those that are not installed, and adjust the settings.  
If there are installed plugins with available upgrades, there will also be an Upgradable option.  
The search bar can be used to enter search terms and fid available plugins related to the topic.  
This is the fist place to look if there's a tool or extra type of functionality that you need! To install a plugin, simply select it and click on the Install Plugin button.  
Installed plugins can be toggled on and off by checking the box next to each.  
  
You will be notifid by a link at the bottom of the QGIS Desktop application if there are updates available for your installed plugins.  
Clicking on the link will open the __Plugins Manager__ window, where the __Upgrades__ tab will allow you to install all or some of the available updates.  
__Plugins themselves may show up as individual buttons, toolbars, or as items under the appropriate menu, such as Plugins, Vector, Raster, Database, Web, or Processing.__  


> To add a base map to QGIS, enable the __OpenLayer plugin__.  
It appears under the Web menu and allows you to add base maps from OpenStreetMap, Google Maps, Bing Maps, Map Quest, OSM/Stamen, and Apple Maps.  
This plugin requires an Internet connection.  
  


![](Chapter01/fig1_8.PNG)

> You can also browse the QGIS Python Plugins Repository at https://plugins.qgis.org/plugins/ 

## 1.9 Summary

This chapter provided a refresher in the basics of Desktop and QGIS Browser.  
We covered how to install the software on several platforms and described the layout of both QGIS Desktop and QGIS Browser.  
We then covered how to load vector, raster, and database data layers.  
Next, you were shown how to work with coordinate reference systems and style data.  
We covered the basics of working with tables, including how to perform a table join.  
The chapter concluded with a refresher on composing maps and how to fid, install, and manage plugins.  
The next chapter will cover creating spatial databases.  
Data is the foundation of any GIS.  
Now that you have had a refresher on the basics of QGIS, it is time to learn how to expand your work to include spatial databases.  
In Chapter 2, Creating Spatial Databases, you will learn how to create and manage spatial databases within QGIS.  
  



```python

```

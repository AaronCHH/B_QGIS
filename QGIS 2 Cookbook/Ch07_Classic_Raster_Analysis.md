
# Chapter 7: Classic Raster Analysis
<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [Chapter 7: Classic Raster Analysis](#chapter-7-classic-raster-analysis)
  * [7.1 Introduction](#71-introduction)
  * [7.2 Using the raster calculator](#72-using-the-raster-calculator)
    * [Getting ready](#getting-ready)
    * [How to do it](#how-to-do-it)
    * [How it works](#how-it-works)
    * [There's more](#theres-more)
    * [See more](#see-more)
  * [7.3 Preparing elevation data](#73-preparing-elevation-data)
    * [Getting ready](#getting-ready-1)
    * [How to do it](#how-to-do-it-1)
    * [How it works](#how-it-works-1)
    * [There's more](#theres-more-1)
  * [7.4 Calculating a slope](#74-calculating-a-slope)
    * [Getting ready](#getting-ready-2)
    * [How to do it](#how-to-do-it-2)
    * [How it works](#how-it-works-2)
    * [There's more](#theres-more-2)
    * [See also](#see-also)
  * [7.5 Calculating a hillshade layer](#75-calculating-a-hillshade-layer)
    * [Getting ready](#getting-ready-3)
    * [How to do it](#how-to-do-it-3)
    * [How it works](#how-it-works-3)
    * [There's more](#theres-more-3)
  * [7.6 Analyzing hydrology](#76-analyzing-hydrology)
    * [Getting ready](#getting-ready-4)
    * [How to do it](#how-to-do-it-4)
    * [How it works](#how-it-works-4)
    * [There's more](#theres-more-4)
  * [7.7 Calculating a topographic index](#77-calculating-a-topographic-index)
    * [Getting ready](#getting-ready-5)
    * [How to do it](#how-to-do-it-5)
    * [How it works](#how-it-works-5)
    * [There's more](#theres-more-5)
    * [See more](#see-more-1)
  * [7.8 Automating analysis tasks using the graphical  modeler](#78-automating-analysis-tasks-using-the-graphical-modeler)
    * [Getting ready](#getting-ready-6)
    * [How to do it](#how-to-do-it-6)
    * [There's more](#theres-more-6)
    * [See more](#see-more-2)

<!-- tocstop -->

In this chapter, we will cover the following recipes:

* Using the raster calculator
* Preparing elevation data
* Calculating a slope
* Calculating a hillshade layer
* Analyzing hydrology
* Calculating a topographic index
* Automate analysis tasks using the graphical modeler



## 7.1 Introduction

Raster analysis is a classic area in GIS analysis.
This chapter shows you some of the most important and common tasks of raster analysis.
Elevation data is commonly stored as raster layers, and in this format, it is particularly suitable to run a large variety of analysis.
For this reason, terrain analysis has traditionally been one of the main areas of raster analysis, and we will show you some of the most common operations that are related to Digital Elevation Models (DEM), from simple analysis, such as slope calculation, to more complex ones, such as drainage network delineation or watershed extraction.


## 7.2 Using the raster calculator

The raster calculator is one of the most flexible and versatile tools in QGIS.
This allows you to perform algebraic operations based on raster layers, and compute new layers.
This recipe shows you how to use it.


### Getting ready

Open the catchment_area.tif file. The file should look like the following screenshot:

### How to do it

1. Open the Processing Toolbox option and find the algorithm called Raster calculator by searching for it using the search box. Double-click on the algorithm item to execute it, as shown in the following screenshot:
2. Click on the button in the Input layers field to open the layer selector. There is only one layer available: the catchment_area layer. Select this layer.
3. In the Formula field, enter ln(a).
4. Click on Run to run the algorithm. The resulting layer will be added to the QGIS project, as follows:


### How it works

The layers selected in the layer selector are referred to using a single letter in alphabetical order (a for the first one, b for the second one, and so on).
In this case, we selected just one layer, so we can refer to it as a in the formula.

The formula calculates a natural logarithm of the values in the catchment area layer.
The distribution of values in this layer is not homogeneous because it contains a large number of cells with low values and just a few of them with very large values.
This causes the rendering of the layer to be not very informative with most of the colors in the color ramp not even being used.

The resulting layer is much more informative because applying the logarithm alters the distribution of values, resulting in a more explicit rendering.



### There's more

QGIS contains a raster calculator module outside of Processing.
You can find this by navigating to Raster | Raster calculator...:

This interface resembles an actual calculator, and it is more intuitive and user friendly.
On the other hand, this lacks many of the functions that are available in the Processing raster calculator (the logarithm that we have computed, for instance, is not available).
This also cannot be used in automated processes, such as scripts or graphical models, which are only available for the Processing algorithms.

On the other hand, the QGIS built-in calculator supports multiband layers, while the Processing one is limited to single-band ones.


### See more

* The QGIS raster calculator is described in more detail in the QGIS manual at http://docs.qgis.org/2.8/en/docs/user_manual/working_with_raster/raster_calculator.html

## 7.3 Preparing elevation data

In this recipe, we will show you how to perform terrain analysis in QGIS.
Terrain analysis algorithms assume certain characteristics in the DEMs that are used as inputs, so it is important to know them and prepare these DEMs if they are needed.
This recipe shows you how to do this.


### Getting ready

Open the dem_to_prepare.tif layer.
This layer contains a DEM in the EPSG:4326 CRS and elevation data in feet.
These characteristics are unsuitable to run most terrain analysis algorithms, so we will modify this layer to get a suitable one.


### How to do it

1. Reproject the layer to the EPSG:3857 CRS, using the Save as...
entry in the context menu that appears by right-clicking on the layer name.
2. Open the resulting reprojected layer.
3. Open the Processing raster calculator and select the reprojected layer as the only raster input in the Input layers field.
Enter a * 0.3048 in the Formula field. Run the algorithm.


### How it works

Most of the algorithms that we are going to use assume that the horizontal units (the unit used to measure the size of the cell) are the same as the units used in the elevation values that are contained in the layer.
If the layer does not meet this requirement, the result of the analysis will be wrong.

Our input layer uses a CRS with geographic coordinates (degrees).
As elevation cannot  be measured in degrees, the layer cannot have the same units for horizontal and vertical distances, and it is not ready to be used for terrain analysis.

By reprojecting the layer to the EPSG:3857 CRS, we get a new layer in which coordinates are expressed in meters.
This is a unit that is more suitable for the type of analysis that we plan to run.
Actually, after the reprojection, the units are meters only near the equator, but this gives us enough precision for this case.
If more precise calculations are needed, a local projection system should be used.

The next step is converting the elevation values in feet to elevation values in meters.
Knowing that 1 foot = 0.3048 meter, we just have to use the calculator to apply this formula and convert the values in the reprojected layer.


### There's more

There are other things that must be taken into account when running a terrain analysis algorithm to ensure that results are correct.

One common problem is dealing with different cell sizes.
An assumption that is made by most terrain analysis algorithms (and also most of the ones not related to terrain analysis) is that cells are square.
That is, their horizontal and vertical values are the same.
This is the case in our input layer (you can verify this by checking the layer properties), but it may not be true for other layers.

In this case, you should export the layer and define the sizes of the cells of the exported layer to have the same value.
Right-click on the layer name and select Save as....
In the save dialog that will appear, enter the new sizes of the cells in the lower part of the dialog:


## 7.4 Calculating a slope

### Getting ready

### How to do it

1. In the Processing Toolbox option, find the Slope algorithm and double-click on it to open it:
2. Select the DEM in the Input layer field.
3. Click on Run to run the algorithm.

The slope layer will be added to the QGIS project.


### How it works

### There's more

There are several ways of using the slope algorithms in QGIS.
Here are some comments and ideas about this.

__Using a ratio for elevation values__
If the units of elevation are not the same as the horizontal units, you can convert them, as we did in the previous recipe, using the raster calculator.
However, the slope module contains an option to convert them on-the-fly by entering the conversion factor in the Scale field.
Note that this option is not available in other terrain analysis modules that we will use, so it's still good practice to create a layer with the correct units, which can be used without any further processing.

__Other slope algorithms__
The Processing framework contains algorithms that rely on several external applications and libraries.
These libraries sometimes contain similar algorithms, so there is more than one option for a given analysis.
If you switch the presentation mode of the toolbox from simplified to advanced using the lower part of the drop-down list and then type slope in the search box, you will see something like the following screenshot:

__Calculating the slope__
Try using the GRASS or SAGA algorithm to calculate the slope.
Each of them has different parameters and options, but all of them perform similar calculations and create slope layers.
Apart from Processing, you can also perform analysis using the Raster Terrain Analysis plugin.



### See also

* The Using the raster calculator recipe in the beginning of this chapter

## 7.5 Calculating a hillshade layer

A hillshade layer is commonly used to enhance the appearance of a map and display topography in an intuitive way, by simulating a light source and the shadows it casts.
This can be computed from a DEM by using this recipe.


### Getting ready

### How to do it

1. In the Processing Toolbox option, find the Hillshade algorithm and double-click on it to open it:
2. Select the DEM in the Input layer field. Leave the rest of the parameters with their default values.
3. Click on Run to run the algorithm.

The hillshade layer will be added to the QGIS project, as follows:


### How it works

As in the case of the slope, the algorithm is part of the GDAL library.
You will see that the parameters are very similar to the slope case.
This is because slope is used to compute the hillshade layer.
Based on the slope and the aspect of the terrain in each cell and using the position of the sun that is defined by the Azimuth and Altitude fields, the algorithm computes the illumination that the cell will receive.
This is based on a focal analysis, so shadows are not considered and are not a real illumination value, but they can be used to render and to display the topography of the terrain.

You can try changing the values of these parameters to alter the appearance of the layer.


### There's more

As in the case of slope, there are alternative options to compute the hillshade.
The SAGA one in the Processing Toolbox option has a feature that is worth mentioning.

The SAGA hillshade algorithm contains a field named method.
This field is used to select the method that is used to compute the hillshade value, and the last method that is available.
Raytracing differs from the other ones as it models the real behavior of light, making an analysis that is not local but that uses the full information of the DEM instead because it takes into account the shadows that are cast by the surrounding relief.
This renders more precise hillshade layers, but the processing time can be notably larger.

__Enhancing your map view with a hillshade layer__
You can combine the hillshade layer with your other layers to enhance their appearance.

As you used a DEM to compute the hillshade layer, it should be already in your QGIS project along with the hillshade itself.
However, this will be covered by the hillshade because of the new layers produced by Processing are added on top of the existing ones in the layers list.
Move it to the top of the layer list so that you can see the DEM (and not the hillshade layer) and style it to something like the following screenshot:

Lets see the steps to enhance the map view with a hillshade layer:

1.  In the Properties dialog of the layer, move to the Transparency section, and set the Global transparency value to 50%, as shown in the following screenshot:
2.  Now, you should see the hillshade layer through the DEM, and the combination of both of them will look like the following screenshot:

Another way of doing this is using the blending modes in QGIS.
You can find more information about this in the recipe, Understanding the feature and layer blending modes of Chapter 10, Cartography Tips, or in the QGIS manual at http://docs.qgis.org/2.8/en/docs/user_manual/working_with_vector/vector_properties.html#style-menu.


## 7.6 Analyzing hydrology

A common analysis from a DEM is to compute hydrological elements, such as the channel network or the set of watersheds.
This recipe shows you the steps to do these analysis.

### Getting ready

### How to do it

1. In the Processing Toolbox option, find the Fill Sinks algorithm and double-click on it to open it:

2. Select the DEM in the DEM field and run the algorithm.
This will generate a new filtered DEM layer.
From now on, we will just use this DEM in the recipe and not the original one.

3. Open Catchment Area and select the filtered DEM in the Elevation field:

4. Run the algorithm.
This will generate a catchment area layer:

5. Open the Channel network algorithm and fill it in, as shown in the following screenshot:

6. Run the algorithm.
This will extract the channel network from the DEM, based on the catchment area, and it will then generate it as both a raster and vector layer:

7. Open the Watershed basins algorithm and fill it in, as shown in the following screenshot:

8. Run the algorithm.
This will generate a raster layer with the watersheds calculated  from the DEM and the channel network.
Each watershed is a hydrological unit that represents the area that flows into a junction, which is defined by the channel network:


### How it works

Starting from the DEM, the preceding steps follow a typical workflow for hydrological analysis:

* First, the sinks are filled.
This is a required preparation whenever you plan to perform a hydrological analysis.
The DEM may contain sinks where a flow direction cannot be computed, which represents a problem to model the movement of water across these cells.
Removing these sinks solves this problem.
* The catchment area is computed from the DEM.
The values in the catchment area layer represent the area that is upstream of each cell.
That is, the total area in which if water is dropped, it will eventually pass through the cell.
* Cells with high values of the catchment area will likely contain a river, while cells with lower values will have overland flow.
By setting a threshold on the catchment area values, we can separate the river cells (the ones above the threshold) from the remaining ones and extract the channel network.
* Finally, we compute the watersheds associated with each junction in the channel network that was extracted in the last step.



### There's more

The key parameter in the preceding workflow is the catchment area threshold.
If a larger threshold is used, fewer cells will be considered as river cells, and the resulting channel network will be sparser.
As the watershed is computed based on the channel network, this will result in a lower number of watersheds.

You can try this yourself with different values of the catchment area threshold.
Here, you can see the result for threshold is equal to 1,000,000 in the following screenshot:

The channel network has been added to help you understand the structure of the resulting set of watersheds.
Here, you can see the result for a threshold of 50,000,000 in the following screenshot:

Note that in this last case, with a higher threshold value, there is only one single watershed in the resulting layer.
The threshold values are expressed in the units of the catchment area which, as the size of the cell is assumed to be in meters, are in square meters.


## 7.7 Calculating a topographic index

As the topography defines and influences most of the processes that take place in a given terrain, the DEM can be used to extract many different parameters, which give us information about these processes.
This recipe shows you how to calculate a popular one, which is called the Topographic wetness index, which estimates the soil wetness based on the topography.



### Getting ready

### How to do it

1. Calculate a slope layer using the Slope, aspect, curvature algorithm from SAGA in the Processing Toolbox option.
Calculate a catchment area layer using the Catchment area algorithm from the Processing Toolbox option.
Note that you must use a sink-less DEM, such as the one that we generated in the previous recipe with the Fill sinks algorithm.
Open the Topographic wetness index algorithm from the Processing Toolbox option and fill it in, as shown in the following screenshot:

2. Run the algorithm.
This will create a layer with the topographic wetness index, indicating the soil wetness in each cell:


### How it works

The index combines slope and catchment area, two parameters that influence the soil wetness.
If the catchment area value is high, this means that more water will flow into the cell, thus, increasing its soil wetness.
A low value of slope will have a similar effect because the water that flows into the cell will not flow out of it quickly.

This algorithm expects the slope to be expressed in radians.
This is the reason why the Slope, aspect, curvature algorithm has to be used because it produces its slope output in radians.
The other Slope algorithm that you will find, which is based on the GDAL library, creates a slope layer with values expressed in degrees.
You can use this layer if you convert its units using the raster calculator.



### There's more

Other indices that are based on the same input layers can be found in different algorithms in the Processing Toolbox option.
The Stream Power Index and the LS factor both use the slope and catchment area as inputs as well, and they can be related to potential erosion.


### See more

## 7.8 Automating analysis tasks using the graphical  modeler

Most analysis tasks involve using several algorithms.
Repeating the same analysis with a different dataset or different input parameters requires using them one by one, making this task tedious and error-prone.
You can automate analysis workflows using the Processing graphical modeler, which allows you to define a workflow graphically and wrap it in a single algorithm.
This recipe introduces the main ideas about the modeler and creates a simple model as an example.


### Getting ready

### How to do it

1. Open the graphical modeler by navigating to Processing | Graphical modeler:

2. Double-click on the Raster Layer item to add a raster input.
In the dialog that will appear to define the input, name it DEM and set it as mandatory:

3. Click on OK to add the input to the canvas:

4. Move to the Algorithms tab.
Double-click on the Slope, aspect, curvature algorithm and set the algorithm definition, as shown in the following screenshot:

5. Close the dialog by clicking on the OK button.
This will be added to the modeler canvas, as follows:

6. Add the Catchment area algorithm to the model by double-clicking on it in the algorithm list and filling in the dialog, as shown in the following screenshot:

7. Finally, add the Topographic wetness index algorithm, defining it as shown in the following screenshot:

8. The final model should look like the following screenshot:

9. Enter a name and a group to identify the model and save it by clicking on the Save button.
Do not change the save location folder, because Processing will only look  for it in the default location, you can however change the name of the model.
Close the modeler dialog.
If you now go to the Processing Toolbox option, you will find a new algorithm in the Models section, which corresponds to the workflow that you have just defined:


### There's more

### See more

* More information about the graphical modeler can be found in the Processing chapter of the QGIS manual at http://docs.qgis.org/2.8/en/docs/user_manual/processing/modeler.html


```python

```


# Chapter 8: Raster Analysis II
<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [Chapter 8: Raster Analysis II](#chapter-8-raster-analysis-ii)
  * [8.1 Introduction](#81-introduction)
  * [8.2 Calculating NDVI](#82-calculating-ndvi)
    * [Getting ready](#getting-ready)
    * [How to do it](#how-to-do-it)
    * [How it works](#how-it-works)
    * [There's more](#theres-more)
  * [8.3 Handling null values](#83-handling-null-values)
    * [How to do it](#how-to-do-it-1)
    * [How it works](#how-it-works-1)
    * [There's more](#theres-more-1)
  * [8.4 Setting extents with masks](#84-setting-extents-with-masks)
    * [How to do it](#how-to-do-it-2)
    * [How it works](#how-it-works-2)
    * [There's more](#theres-more-2)
  * [8.5 Sampling a raster layer](#85-sampling-a-raster-layer)
    * [Getting ready](#getting-ready-1)
    * [How to do it](#how-to-do-it-3)
    * [How it works](#how-it-works-3)
    * [There's more](#theres-more-3)
  * [8.6 Visualizing multispectral layers](#86-visualizing-multispectral-layers)
    * [Getting ready](#getting-ready-2)
    * [How to do it](#how-to-do-it-4)
    * [How it works](#how-it-works-4)
    * [How it works](#how-it-works-5)
    * [There's more](#theres-more-4)
    * [See also](#see-also)
  * [8.7 Modifying and reclassifying values in raster layers](#87-modifying-and-reclassifying-values-in-raster-layers)
    * [Getting ready](#getting-ready-3)
    * [How to do it](#how-to-do-it-5)
    * [How it works](#how-it-works-6)
    * [There's more](#theres-more-5)
    * [See also](#see-also-1)
  * [8.8 Performing supervised classification of raster layers](#88-performing-supervised-classification-of-raster-layers)
    * [Getting ready](#getting-ready-4)
    * [How to do it](#how-to-do-it-6)
    * [How it works](#how-it-works-7)
    * [There's more](#theres-more-6)
    * [See also](#see-also-2)

<!-- tocstop -->

In this chapter, we will cover the following recipes:

* Calculating NDVI
* Handling null values
* Setting extents with masks
* Sampling a raster layer
* Visualizing multispectral layers
* Modifying and reclassifying values in raster layers
* Performing supervised classification of raster layers


## 8.1 Introduction

Following the previous chapter, this chapter introduces some additional techniques for raster analysis.
This chapter will show you how to work with images, how to modify raster values and classify them, and how raster layers can be used along with vector layers, thus extending the set of tools that were introduced in the recipes in the previous chapter.


## 8.2 Calculating NDVI

The Normalized Differential Vegetation Index is a very popular vegetation index that gives us useful information about the presence or absence of live green vegetation.


### Getting ready

NDVI is calculated using a band with red spectral reflectance values, and another one with near-infrared reflectance values.
In the sample dataset, you will find two image files named red.tif and nir.tif that can be used to compute NDVI.
A project named ndvi.qgs is available, which contains these two layers and a landsat image corresponding to this same area.
Open this project.

### How to do it

1. Open the Processing Toolbox menu and find the algorithm called Vegetation index[slope based].
Double-click on the algorithm item to execute it:

2. Select the red.tif layer in the Red Band field and the nir.tif layer in the Near Infrared Band field.
Click on Run to run the algorithm.

3. The algorithm will produce a set of layers with different vegetation indices, NDVI is among them:


### How it works

All vegetation indices that are computed by the algorithm are based on the relation between red and near-infrared reflectances.
Leaf cells scatter solar radiation in the near-infrared reflectance and absorb radiation in the red reflectance, which can be used to predict the location of healthy green vegetation based on these two values.

NDVI is computed with the formula given in the following section.


### There's more

As the formula of the NDVI is rather simple, you can calculate it without using a specific algorithm, just by going to the raster calculator.
You can use the one integrated in the Processing Framework or the QGIS built-in on.
You can see how you should fill the parameters in the QGIS Raster calculator to compute the NDVI, based on the two proposed sample layers in the following screenshot:

__Extracting bands__
The vegetation indices algorithm requires the red and infrared values to be in two separate layers, each of them with a single band.
However, it's common to have both of them in a multiband image.
To be able to use these bands, you must separate them, extracting them into two separate files.

This can be done using the GDAL translate algorithm.
The project contains a multiband image named landsat.tif with the red band in band number 3 and infrared band in band number 4:
1.  Open the Translate algorithm in the Processing Toolbox menu.
2.  Fill its parameters, as shown in the following screenshot, to export the infrared band:
3.  Run this again, as shown in the following screenshot, to export the red band:

The Translate algorithm uses the GDAL library underneath.
You can also use this library as an independent tool from the console.
At the lower part of the algorithm dialog, you will find a text field where you will see the equivalent console call to your current algorithm configuration.



## 8.3 Handling null values

Null values are a particular type of values that are used to indicate cells where the value for a given layer is not defined.
Understanding how to use them is important to avoid wrong results when performing analyses but also to use them as a tool to get better and more correct results.
This recipe explains some of the fundamental ideas about null values in raster layers.


### How to do it

The watershed.tif layer contains the area of a watershed.
Cells inside the watershed   are cells from which water will eventually flow into the outlet point of the watershed.
The remaining cells belong to a different watershed.
To mask the DEM with the watershed mask, follow these steps:
1. Open the watershed.tif layer.
2. Open the identify tool and check whether the cells that belong to the watershed have a value of 1, and the ones outside, have a value of no data.
3. Try clicking inside and outside the watershed; in your Identify Results dialog, you will see the results, as shown in the following screenshot:
4. Now, let's calculate some statistics of the raster layer.
Open the Raster layer statistics algorithm in the Processing Toolbox menu.
5. Select the watershed layer in the input layer field and click on OK to run the algorithm.
The result is a short text output that looks like the following screenshot:

Only the cells with a value of 1 have been considered, and the average value in the layer is equal to 1.
The layer has 610 columns and 401 rows, but the total number of valid cells is much lower than 610 x 410.
These are the cells that have been used to compute the statistics.


### How it works

Raster layers always cover a rectangular region.
However, in some circumstances, the land object that the layer represents might not be rectangular.
This might be due to a purely geophysical reason (imagine a layer with water temperature that contains non-water cells), political ones (a layer with a DEM of a given country with no data available for a neighboring country), or many others.
In any case, a value is needed for these cells to indicate that no data is available.
An arbitrary value is selected and used.
As such, this is usually a value that is not a logical and/or feasible value for the variable that is stored in the layer.

In the case of the example layer, the value used is -99999, which is the default value set for no-data values.
This means that, when the identify tool shows no data, it has actually selected a value of -99999 in this case.


Algorithms in the Processing framework systematically ignore no-data cells, and do not use their values.
You can clearly see this in the preceding example.
A large part of the cells in the layer have a value of 1 (the ones that belong to the watershed), but many of them have a value of -99999.
The average value of the cells should then be different from 1, but as -99999 is defined as the no-data value, all cells with this value are ignored.
The average of the layer is, therefore, equal to 1.


### There's more

Null values should be considered not only when performing an analysis, but also when we just want to render a layer that contains them.

__Controlling the rendering of null values__
Null values are also considered separately when rendering a raster layer.
You can choose to select them using a given color (as set by the current color palette), or to not render them at all.
To make all cells with null values transparent, open the layer properties and go to the Transparency section.
Make sure that the No data value checkbox is checked, as shown in the following screenshot:


## 8.4 Setting extents with masks

The extent of a layer can be set using a second layer, which acts as a mask. This recipe shows you how to do this.


### How to do it

To mask the DEM with the watershed mask, follow these steps:

1. Open the watershed.tif layer and the dem.tif layer.
2. Open the Raster Calculator algorithm present in the Processing Toolbox menu.
3. In Main input layer, select the DEM, and in the Additional layers field, select the watershed layer.
4. In the Formula field, enter the formula, a*b.
5. Click on Run to run the algorithm. You will get a masked DEM, as follows:


### How it works

When using the raster calculator, all operations involving a no-data value will result in another no-data value.
This means that, when multiplying the DEM layer and the mask layer, in the cells that contain no-data values in the mask layer, the value in the resulting layer will be a no-data value, no matter which elevation value is found in the DEM layer for this cell.

As cells inside the watershed in the mask layer have a value of 1, the result is a layer with elevation values for watershed cells and no-data values for the remaining ones.

### There's more

Here are some additional ideas about masks.

__Restricting analysis to a given area__
Once we have masked the area of interest (in this case, the watershed), all analysis that we perform will be restricted to this.
For instance, let's calculate the average elevation of the watershed:
1.  Open the Basic statistics for raster layers algorithm.
2.  Select the masked DEM in the Input layer field and click on Run to run the algorithm.
You will get the statistics on the Results window:
These values have been computed using only valid cell values and ignoring the no-data ones, which means that they refer to the watershed and not the the full extent of the raster layer.

__Removing superfluous no-data values__
Sometimes, you might have more no-data values that are needed in a raster layer, as in the case of the proposed watershed layer.
To reduce the extent of the layer and just have the minimum extent that covers the valid data, you can use the Crop to data algorithm:
1.  Open the Crop to data algorithm in the Processing Toolbox menu.
2.  Enter the masked DEM in the Input layer field and click on Run to run the algorithm.
The resulting layer should look like this when you disable transparency for no-data values:

> Note that, if you have opted to render no-data cells as transparent pixels, you will see no visual difference between the original and the cropped layer.

__Masking using a vector mask__
Masking a raster layer can also be done using a polygon vector layer.
The watershed.shp file contains a single polygon with the area of the watershed that we have already used to mask the DEM.

Here is how to use this to mask that DEM without using the raster mask:
1.  Open the Clip grid with the polygon algorithm.
2.  Select the DEM in the Input field.
3.  Select the vector layer in the Polygons field.
4.  Click on Run to run the algorithm.
The clipped layer will be added to the QGIS project.

In this case, the clip algorithm automatically reduces the extent of the output layer to the minimum extent defined by the polygon layer, so there is no need to run the Crop to data algorithm afterwards.

## 8.5 Sampling a raster layer

Data from a raster layer can be added to a points layer by querying the value of the layer in the coordinates of the points.
This process is known as sampling, and this recipe explains how to perform it.


### Getting ready

### How to do it

1. In the Processing Toolbox menu, find the Add grid values to points algorithm and double-click on it to open it:
2. Select the DEM in the Grids field.
3. Select the point layer in the Points field.
4. Click on Run to run the algorithm.

A new vector layer will be created.
This contains the same points as the input layer, but the attribute table will have an additional field with the name of the selected raster layer and the values corresponding to this layer in each point:


### How it works

The coordinates of the points are taken, and the value of the pixel in which the layer falls is added to the resulting points layer.

This method assumes that the value of a cell is constant in all the area covered by this cell.
A different approach is to consider that the value of the cell represents its value only in the center of the cell and perform additional calculations to compute the value at the exact sampling point using the values of the surrounding cells as well.
This can be done using several different interpolation methods, which can be selected in the Interpolation method selector, changing the default value, which only uses the value of the cell where the sampling point falls.

Layers are assumed to be in the same CRS and no reprojection is performed.
If this is not the case, the value added to the vector layer might not be correct.




### There's more

Here, you can find some ideas about how to combine a raster and vector layer in different
situations.

__Other raster-vector data transfer operations__
Data coming from a raster layer can also be added to other types of vector layers.
In the case of a vector layer with polygons, the Grid statistics for polygons algorithm can be used, as follows:
1. Open the watershed.shp file that we used in the previous recipe.
2. Open the Grid statistics in the Polygons field.
3. Select the raster layer to clip in the Grids field.
4. Select the polygon layer with the mask in the Polygons field.
5. Select the statistics to be calculated from the remaining parameters.
For instance, to calculate just the mean elevation, leave the Mean field selected and unselect the others.
6. Click on Run to run the algorithm.

The resulting layer is a new polygon layer with the watershed and an additional field in the attributes table, containing the mean elevation value for each polygon.

If more statistics are selected, the result will have a larger number of additional fields added, one for each new parameter computed and each selected grid.


## 8.6 Visualizing multispectral layers

Multispectral layers can be rendered in different ways depending on how bands are used.
This recipe shows you how to do this and discusses the theory behind it.

### Getting ready

### How to do it

1. The Landsat image, when opened with the default configuration, looks something like
the following screenshot:
2. Double-click on the layer to open its properties and move to the Style section:
1. Select the band number 4 in the Red band field.
2. Select the band number 3 in the Green band field.
3. Select the band number 2 in the Blue band field.
Your style configuration should be like the following:
3.  Click on OK.
The image should now look like the following:


### How it works

### How it works

Colors representing a given pixel are defined using the RGB color space, which requires three different components.
A normal image (such as the one you will get from a digital camera) has three bands containing the intensity for each one of these three components: red, green, and blue.

Multispectral bands, such as the one used in this recipe, have more than three bands and provide more detail in different regions of the electromagnetic spectrum.
To visualize these, three bands from the total number of available bands have to be chosen and their intensities have to be used as intensities of the basic red, green, and blue components (although they might correspond to a different region of the spectrum, even outside the visible range).
This is known as a false color image.

Depending on the combination of the bands that are used, the resulting image will convey a different type of information.

The combination chosen is frequently used for vegetation studies, as it allows you to separate coniferous from hardwood vegetation as well as providing information about vegetation health.

The combination is applied, in this case, to a Landsat 7 image, which is taken with the ETM+ sensor.
The wavelengths covered by each band are as follows (in micrometers):
* Band 1: 0.45 - 0.515
* Band 2: 0.525 - 0.605
* Band 3: 0.63 - 0.69
* Band 4: 0.75 - 0.90
* Band 5: 1.55 - 1.75
* Band 6: 10.40 - 12.5
* Band 7: 2.09 - 2.35


### There's more

Different combinations are frequently used for Landsat layers. One of them is the following:
* Select the band number 3 in the Red band field
* Select the band number 2 in the Green band field
* Select band number 1 in the Blue band field

This is a natural color combination, as the bands used for the R, G, and B components actually have the wavelengths corresponding to the colors red, green, and blue:

If you are using an image that is not a Landsat 7 one, each band will have a different meaning, and using the same combination of band numbers will yield different results.
The meaning of each band must be checked in order to understand the information displayed by the rendered image.



### See also

Landsat data is freely available.
If you want to download Landsat data corresponding to a given region, visit http://landsat.gsfc.nasa.gov/.
Here, you can find more information about where and how you can download it.



## 8.7 Modifying and reclassifying values in raster layers

A very useful technique to work with raster data is changing their values or grouping them into categories.
In this recipe, we will see how to do this.



### Getting ready

### How to do it

We will classify the elevation in three groups:
* Lower than 1,000m
* Between 1,000 and 2,000m
* Higher than 2,000m

To do this, follow these steps:
1. Open the Change grid values algorithm from the Processing Toolbox menu.
Set the Replace condition parameter to Low Value <= Grid Value < High Value.
2. Click on the button in the Lookup table parameter and fill the table that will appear, as shown in the following screenshot:
3. Run the algorithm. This will create the reclassified layer:


### How it works

Values for each cell are compared with the range limits in the lookup table, considering the specified comparison criteria.
Whenever a value falls into a given range, the class value specified for this range will be used in the output layer.

### There's more

Other strategies can be used to automate a reclassification, especially when this involves dividing the raster layer values into classes with some constant property. Here, we show two of these cases.

__Reclassifying into classes of equal amplitude__
A typical case of reclassification is dividing the total range of values of the layer into a given number of classes.
This is similar to slicing it, and if applied on a DEM, such as our example data, this will have a result similar to that of defining contour lines with a regular interval (although the result is a not vector layer with lines in this case, but a new raster layer).
To reclassify in equal intervals, follow these steps:
1. Open Raster calculator from the Processing Toolbox menu.
2. Select the DEM as the only layer to use.
3. Enter the formula, int((a-514)/(2410-514) * 5).

The reclassified layer will look like the following:
The numerical values in the formula correspond to the minimum and maximum values of the layer.
You can find these values in the properties window of the layer.

To create a different number of classes, just use another value instead of 5 in the formula.

__Reclassifying into classes of equal area__
There is no tool to reclassify into a set of n classes, as each of them occupies the same area, but a similar result can be obtained using some other algorithms.
To show you how this is done, let's reclassify the DEM into five classes of the same area:
1. Open the Sort grid algorithm and enter the DEM as the input layer.
Click on Run to execute the algorithm.
The resulting layer has the cells ordered according to their value in the DEM, so the cell with a value of 1 represents the cell with the lowest elevation value, 2 is the second lowest, and so on.
2. Reclassify the ordered layer into five classes of equal amplitude using the procedure described earlier.
The final layer should look like the following:




### See also

The Processing Toolbox menu contains additional classification algorithms, most of them based on SAGA.
One algorithm that has a different approach is Cluster analysis for grids.
This will create a given number of classes in such a way that it minimize the variances in the groups, trying to make them as homogeneous as possible.
This is also known as unsupervised classification.



## 8.8 Performing supervised classification of raster layers

In the previous recipes, we saw how to change the values of a raster layer and create classes.
When you have several layers, classifying might not be that easy, and defining the patterns to perform this classification might not be obvious.
A different technique to be used in this case is to define zones that share a common characteristic and let the corresponding algorithm extract the statistical values that define them so that this can later be applied to perform the classification itself.
This is known as Supervised classification, and this recipe explains how to do this in QGIS.


### Getting ready

### How to do it

1. The image has to be separated into individual bands.
Run Split RGB bands using the provided image as the input, and you will obtain three layers named R, G, and B.
2. Open the Supervised classification algorithm from the Processing Toolbox menu.
3. Fill in its parameter window, as shown in the following screenshot:
4. In the first field, you should select the three layers resulting from the last step (R, G, and B).
5. Click on OK to run the algorithm.
Two layers and a table will be created.
The layer named Classification contains the classified raster layer:


### How it works

The supervised classification needs a set of raster layers and a vector layer with polygons that define the different classes to create.
The identifier of the class is defined in the Class field in the attributes table.
If you open the attributes tables, you will see that it looks something like the following:

There are five different classes, each of them represented by a feature and with a text ID along with a numerical ID.
The classification algorithms analyzes the pixels that fall within the polygons of each class and computes statistics for them.
Using these statistics assigns a class to each pixel in the image, trying to assign the class that is statistically more similar among the ones defined in the vector layer.
The numerical ID is used to identify the class in the resulting raster layer.




### There's more

There are other ways of performing a supervised classification in QGIS.
One of them, which allows more control over the different elements in the process, is to use the QGIS semi- automatic classification plugin.

Other more sophisticated classification methods can be used from the Processing Toolbox menu.
They can be found in the Advanced interface of the toolbox, under the Orfeo Toolbox group, as shown in the following screenshot:


### See also

You can download and install this using the QGIS plugin manager.
For more information about how to use the plugin, you can check its website at http://fromgistors.blogspot.fr/p/semi-automatic-classification-plugin.html.

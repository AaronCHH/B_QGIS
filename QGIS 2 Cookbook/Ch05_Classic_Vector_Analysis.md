
# Chapter 5: Classic Vector Analysis
<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [Chapter 5: Classic Vector Analysis](#chapter-5-classic-vector-analysis)
  * [5.1 Introduction](#51-introduction)
  * [5.2 Selecting optimum sites](#52-selecting-optimum-sites)
    * [Getting ready](#getting-ready)
    * [How to do it](#how-to-do-it)
    * [How it works](#how-it-works)
    * [There's more](#theres-more)
  * [5.3 Dasymetric mapping](#53-dasymetric-mapping)
    * [Getting ready](#getting-ready-1)
    * [How to do it](#how-to-do-it-1)
    * [How it works](#how-it-works-1)
  * [5.4 Calculating regional statistics](#54-calculating-regional-statistics)
    * [Getting ready](#getting-ready-2)
    * [How to do it](#how-to-do-it-2)
    * [How it works](#how-it-works-2)
  * [5.5 Estimating density heatmaps](#55-estimating-density-heatmaps)
    * [Getting ready](#getting-ready-3)
    * [How to do it](#how-to-do-it-3)
    * [How it works](#how-it-works-3)
  * [5.6 Estimating values based on samples](#56-estimating-values-based-on-samples)
    * [Getting ready](#getting-ready-4)
    * [How to do it](#how-to-do-it-4)
    * [How it works](#how-it-works-4)
    * [There's more](#theres-more-1)
    * [See also](#see-also)

<!-- tocstop -->

In this chapter, we will cover the following recipes:

* Selecting optimum sites
* Dasymetric mapping
* Calculating regional statistics
* Estimating density using heatmaps
* Estimating values based on samples


## 5.1 Introduction

This chapter will provide you with an introduction to some of the most-common GIS analysis use cases.
The recipes focus on step-by-step instructions, as well as a closer explanation of the tools that are used to achieve the desired analysis results.
This chapter includes recipes on optimum site selection, using interpolation, and creating heat maps, as well as calculating regional statistics.



## 5.2 Selecting optimum sites

Optimum site selection is a pretty common problem, for example, when planning shop or warehouse locations or when looking for a new apartment.
In this recipe, you will learn how to perform optimum site selection manually using tools from the Processing Toolbox option, but you will also see how to automate this workflow by creating a Processing model.

In the optimum site selection in this recipe, we will combine different vector analysis tools to find potential locations in Wake County that match the following criteria:

* Locations are near a big lake (up to 500 m)
* Locations are close to an elementary school (up to 500 m)
* Locations are within a reasonable distance (up to 2 km) from a high school
* Locations are at least 1 km from a main road



### Getting ready

### How to do it

The following steps show you how to perform optimum site selection using the Processing Toolbox option:
1. First, we have to filter the lakes layer for big lakes.
To do this, we use the Select by expression tool from the Processing toolbox, select the lakes layer, and enter __``` "AREA" > 1000000 AND "FTYPE" = 'LAKE/POND' ```__in the Expression textbox, as shown in the following screenshot:

2.  Next, we create the buffers that will represent the proximity areas around lakes, schools, and roads.
Use Fixed distance buffer from the Processing Toolbox option to create the following buffers:
1. For the lakes, select Distance of 500 meters and set Dissolve result by checking the box as shown in the following screenshot.
By dissolving the result, we can make sure that the overlapping buffer areas will be combined into one polygon.
Otherwise, each buffer will remain as a separate feature   in the resulting layer:
> It's your choice whether you want to save the buffer results permanently by specifying an output file, or you just want to work with temporary files by leaving the Buffer output file field empty.
2. To create the elementary school buffers, first select only the schools with "GLEVEL" = 'E' using the Select by Expression tool like we did for the lakes buffer.
Then, use the buffer tool like we just did for the lakes buffer.
3. Repeat the process for the high schools using "GLEVEL" = 'H' and a buffer distance of 2,000 meters.
4. Finally, for the roads, create a buffer with a distance of 1,000 meters.

3. With all these buffers ready, we can now combine them to fulfill these rules:
1. Use the Intersection tool from the Processing Toolbox option on the buffers around elementary and high schools to get the areas that are within the vicinity of both school types.
2. Use the Intersection tool on the buffers around the lakes and the result of the previous step to limit the results to lakeside areas.
Use the Difference tool to remove areas around major roads (that is, the buffered road layer) from the result of the previous (Intersection) steps.
4. Check the resulting layer to view the potential sites that fit all the criteria that we previously specified.
You'll find that there is only one area close to WAKEFIELD ELEMENTARY and WAKEFIELD HIGH that fits the bill, as shown in the following screenshot:


### How it works

In step 1, we used Intersection to model the requirement that our preferred site would be near both an elementary and a high school.
Later, in step 3, the Difference tool enabled us to remove areas close to major roads.
The following figure gives us an overview of the available vector analysis tools that can be useful for similar analyses.
For example, Union could be  used to model requirements, such as "close to at least an elementary or a high school".
Symmetrical Difference, on the other hand, would result in "close to an elementary or a high school but not both", as illustrated in the following figure:


### There's more

We were lucky and found a potential site that matched all criteria.
Of course, this is not always the case, and you will have to try and adjust your criteria to find a matching site.
As you can imagine, it can be very tedious and time-consuming to repeat these steps again and again  with different settings.
Therefore, it's a good idea to create a Processing model to automate this task.

The model (as shown in the following screenshot) basically contains the same tools that we used in the manual process, as follows:
* Use two select by expression instances to select elementary and high schools.
As you can see in the following screenshot, we used the descriptions Select "GLEVEL" = 'E' and Select "GLEVEL" = 'H' to name these model steps.
* For elementary schools, compute fixed distance buffers of 500 meters.
This step is called Buffer "GLEVEL" = 'E'.
* For high schools, compute fixed distance buffers of 2,000 meters.
This step is called Buffer "GLEVEL" = 'H'.
* Select the big lakes using Select by expression (refer to the Select big lakes step) and buffer them using fixed distance buffer of 500 meters (refer to the Buffer lakes step).
* Buffer the roads using Fixed distance buffer (refer to the Buffer roads step).
The buffer size is controlled by the number model input called road_buffer_size.
You can extend this approach of controlling the model parameters using additional inputs to all the other buffer steps in this model.
(We chose to show only one example in order to keep the model screenshot readable.)
* Use Intersection to get areas near schools (refer to the Intersection: near schools step).
* Use Intersection to get areas near schools and lakes (refer to the Intersection: schools and lakes step).
* Use Difference to remove areas near roads (refer to the Difference: avoid roads step).

This is how the final model looks like:

You can run this model from the Processing Toolbox option, or you can even use it as a building block in other models.
It is worth noting that this model produces intermediate results in the form of buffer results (near_elementary, near highschool, and so on).
While these intermediate results are useful while developing and debugging the model, you may eventually want to remove them.
This can be done by editing the buffer steps and removing the Buffer <OutputVector> names.


## 5.3 Dasymetric mapping

Dasymetric mapping is a technique that is commonly used to improve population distribution maps.
By default, population is displayed using census data, which is usually available for geographic units, such as census tracts whose boundaries don't necessarily reflect the actual distribution of the population.
To be able to model population distribution better, Dasymetric mapping enables us to map population density relative to land use.
For example, population counts that are organized by census tracts can be more accurately distributed by removing unpopulated areas, such as water bodies or vacant land, from the census tract areas.

In this recipe, we will use data about populated urban areas, as well as data about water bodies to refine our census tract population data.



### Getting ready

To follow this exercise, please load the population data from census_wake2000_pop.shp (the file that we created in Chapter 2, Data Management), as well as the urban areas from urbanarea.shp, and the lakes from lakes.shp.

As all the datasets in our sample data already use the same CRS, we can get right into the analysis.
If you are using different data, you may have to first get all datasets into the same CRS.
In this case, please refer to Chapter 1, Data Input and Output, for details.


### How to do it

To create a new and improved population distribution map, we will first remove the unpopulated areas from the census tracts.
Then, we will recalculate the population density values to reflect the changes to the area geometries by performing the following steps:
1. Use Clip from the Processing Toolbox option (or Clip by navigating to Vector | Geoprocessing tools if you prefer this option—the results will be identical) on the census tracts and urban area layers to create a new dataset, containing only those parts of the census tracts that are within urban areas.
2. Refine the results of the previous step further by removing the water bodies (the lakes layer) using the Difference tool.
The following screenshot shows the results of this so far:
3. Now, we can calculate the population density of the resulting areas, as follows:
1. Enable editing.
2. Open Field calculator.
3. Calculate a new population density (inhabitants per square km) using the formula, ```"_POP2000" / ($area / 1000000):```
4. Deactivate editing and save the changes.
> It is worth noting that you don't necessarily have to make a new column.
If you only want to use the density values for styling purposes, you can also enter the expression directly in the style configuration.
On the other hand, if you create a new column, you can inspect the density values in the attribute table, export them, or analyze them further.


We are done, and you can now visualize the results using a Graduated renderer with, for example, the Natural Breaks (Jenks) classifi  n mode.
The Jenks Natural Breaks classifi   n is designed to arrange values into "natural" classes by maximizing the variance between  different classes while reducing the variance within the generated classes.
The following fi e shows the population density based on the original census data (on the left) and the results after Dasymetric mapping (on the right):

### How it works

In the first step of this recipe, we used the Clip operation.
As you most likely noticed, the results of a Clip operation look very similar to the results of the Intersection tool, which we used in the previous recipe of this chapter, Selecting optimum sites.
Compare both the results, and you will see the following differences:
* The layer resulting from an Intersection operation contains attributes from both input layers, while the result of a Clip operation only contains attributes of the first input layer.
* This also means that the layer order is important when using Clip, but this does not change the output of Intersection (except for the attribute order in the attribute table).
* The Intersection result is also very likely to contain more features than the Clip result (164 instead of 105 if you use our sample data census tracts and urban areas).
This is because the Intersect tool needs to create a new feature for every combination of intersecting census tracts and urban areas, while the Clip tool only removes the parts of the census tracts that are not within any urban area.

A popular way of thinking about the Clip operation is to imagine one layer as the cookie cutter and the other layer as the cookie dough.


## 5.4 Calculating regional statistics

Another classic spatial analysis task is calculating the areas of a certain type within regions, for example, the area within a county that is covered by certain land use types, or the share of different crops that is farmed in given municipalities.

In this recipe, we will calculate statistics of geological data for zip code areas.
In particular, we will calculate the total area of each type of rock per zip code area.


### Getting ready

To follow this recipe, load zipcodes_wake.shp and geology.shp from our sample data.
Additionally, install and activate the Group Stats plugin using Plugin Manager.


### How to do it

Using the following steps, we can calculate the areas of certain rock types per zip code area:

1. Calculate the intersections between zip code areas and geological areas using the Intersection tool in the Processing Toolbox option or from the Vector menu.
2. Using the Group Stats plugin, you can now calculate the total area per rock type and zip code area, as follows:
1. Select the Intersection result layer as the input Layer.
2. Drag the ZIPCODE field to the Rows input area and the GEO_NAME field to the Columns input area.
3. Drag the sum function and the Area value to the Value input area.
4. Click on Calculate to start the calculations.

The following screenshot displays the complete configuration, as well as the results:


### How it works

The Group Stats plugin brings functionality, which is commonly known as pivot tables, to QGIS.
A pivot table is a data summarization tool, which is commonly found in applications, such as spreadsheets or business intelligence software.
As shown in this example, pivot tables can aggregate data from an input table.
Additionally, the Group Stats plugin offers extended geometry functions, such as, Area and Perimeter for polygon input layers, or Length for line layers.
This makes using the plugin more convenient because it is not necessary to first use Field calculator to add these geometric values to the attribute table.

It is worth noting that you always need to put the following two entries into the Value input area:
* An aggregation function, such as sum, average, or count
* A value field (from the input layer's attribute table) or geometry function, which should be aggregated

## 5.5 Estimating density heatmaps

Whether they are animal sightings, accident locations, or general points of interest, many point datasets can be interpreted more easily by visualizing the point density using a heatmap.
In this recipe, we will estimate the density of POIs in Wake county to find areas with a high density.


### Getting ready

Load the poi_names_wake.shp POI dataset from our sample data.
Make sure that the Heatmap plugin, which comes with QGIS by default, is enabled in Plugin Manager.


### How to do it

Using the following steps, we can calculate the POI heatmap:

1.  Start the Heatmap plugin from the Raster menu.
2.  Make sure that poi_names_wake is selected as Input point layer.
3.  Select a location and filename for Output raster.
You don't need to specify the file extension because this will be added automatically, based on the selected Output format.
GeoTIFF is usually the first choice.
4.  Select a search Radius of 1000 meters.
5.  The Add generated file to map option should be activated by default.
Click on OK to create the default heatmap.
6.  By default, the heatmap layer will be rendered using the Singleband gray render type.
Change the render type to Singleband pseudocolor and apply a color ramp that you like to improve the visualization, as show in the following screenshot:

> If you want to control the size of the output raster, just enable the Advanced section and adjust the number of Rows and Columns or Cell size X and Cell size Y, accordingly.
Note that changing rows and columns will automatically recalculate the size of the cell and vice versa.


### How it works

The search radius, which is also known as the kernel bandwidth, determines how smooth the heatmap will look because it sets the distance around each point at which the influence of the point will be felt.
Therefore, smaller radius values result in heatmaps that display finer details, while larger values result in smoother heatmaps.

Besides the kernel bandwidth, there are also different kernel shapes to choose from.
The kernel shape controls the rate at which the influence of a point decreases with increasing distance from the point.
The kernel shapes that are available in the Heatmap plugin can be seen in the following figures.
For example, a Triweight kernel (the first on the bottom row) creates smaller hotspots than the Epanechnikov kernel (the second on the bottom) because the Triweight shape gives features a higher influence for distances that are closer to the point:

The triangular kernel shape can be further adjusted using the Decay ratio setting.
In the preceding figure, you can see the shape for ratios of 0 (a solid red line), 0.5 (a dashed black line), and 1 (a dotted black line), which is equal to the uniform kernel shape.
You can even specify values greater than 1.
In this case, the influence of a feature will increase with the distance from the point.

## 5.6 Estimating values based on samples

__Interpolation is the idea that, with a set of known values, you can estimate the values of additional points based on their proximity to these known values.__
This recipe shows you how to use known values at point locations to create a continuous surface (raster) of value estimates.
Classic examples include weather data estimations that are based on weather station data (think temperature or rainfall maps), crop yield estimates that are based on sampling parts of a field, and like in this example in this recipe, elevation estimations that are based on the elevation of sampled points.


### Getting ready

Activate Interpolation Plugin via Plugin Manager.

Load a point layer with numeric columns, representing the feature of interest.
For this recipe, use the poi_names_wake.shp, and the elev_m column, which contains elevation in meters for each point.


### How to do it

1. Start by loading poi_name_wake.
2. Zoom to the layer extent.
3. Open the Interpolation tool by navigating to Raster | Interpolation | Interpolation.
> Yes, it's on the Raster menu; the source data must be a vector, but the results are a raster.
4. Select poi_names_wake for Input.
5. Select elev_m for Interpolation attribute.
6. Click on the Add button, your selection should appear in the box on the left-hand side.
7. Select Inverse Distance Weighted (IDW) for Interpolation Method.
8. Now, set the Extent and Cell Size properties.
In Cellsize X and Cellsize Y, enter 100 and 100.
This forces the output cells to be 100x100 units of the current projection.
> Generally, if this was for analysis, you would attempt to match the region of interest or other raster layers.
In this case, we just want to go for sensibly-sized cells.
As the map is in UTM, we will want cells to be integers that represent metric units; 100 meters by 100 meters makes interpreting the results easier.
9. Click on the Set to current extent button in the middle.
10. Next to Output file box, click on the button labeled ... to set the output path to save the results:
11. Pick the folder and type in a name with no file extension, such as idw100m (the result will be an ASCII raster .asc file), as shown in the following screenshot:
> The wrench tool in the upper-right corner will let you change the P value, which is the exponent in the denominator and directly sets how much a point influences a nearby location, as compared against more distanced points.
12. Check all your settings and then click on the OK button.
13. Now, wait patiently for your results, the smaller the size of the cell and the larger the number of columns and data points, the longer the calculation will take, as shown in the following screenshot:




### How it works

The basic idea is that, at a given cell, you take the average of all the nearby points that are weighted by their distance to the cell in order to estimate the value at your current location.
Inverse Distance Weighted (IDW) takes this one step further by giving more weight to values that are closer to the given cell and less weight to values that are further.
This function uses an exponent factor P in order to greatly increase the role of closer points over distant points.



### There's more

Are the results not quite what you expected? There are a few parameters that can be adjusted; these are primarily the P value and the size of the cell.
Is this still not coming out the way that you want? There are several other Interpolation tools that are accessible in Processing under the SAGA, GRASS, and GDAL toolboxes, which allow you to manipulate more of the formula parameters to refine the results.

Finally, depending on your data, IDW may not do a good job of interpolating.
In the example here, you can actually see how there are distinct circles around isolated points.
This is generally not a good result, and this needs a smoother transition to nearby points.
If you have any control over fi  ld sampling to begin with, keep in mind that regularly-spaced grids will usually provide better results.

Do you not have control over the source data or you didn't get good results? Then, you may need to look into other more complicated formulas that compensate for skew, strong directionality, obstructions, and non-regular spacing of samples, such as Splines or Kriging, or Triangulated Irregular Networks (TINs).
There is lot of science and statistics behind the methods and diagnostic tools to determine the best parameters.
This is far too complicated a topic for this recipe, but it is well-covered in books on geostatistics.


### See also


* http://docs.qgis.org/2.2/en/docs/user_manual/plugins/plugins_interpolation.html
* http://en.wikipedia.org/wiki/Inverse_distance_weightinging


```python

```


```python

```

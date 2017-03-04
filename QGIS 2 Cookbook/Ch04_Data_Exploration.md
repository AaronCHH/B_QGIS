
# Chapter 4: Data Exploration
<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [Chapter 4: Data Exploration](#chapter-4-data-exploration)
  * [4.1 Introduction](#41-introduction)
    * [Getting ready](#getting-ready)
    * [How to do it](#how-to-do-it)
    * [How it works](#how-it-works)
    * [There's more](#theres-more)
    * [See more](#see-more)
  * [4.2 Listing unique values in a column](#42-listing-unique-values-in-a-column)
    * [Getting ready](#getting-ready-1)
    * [How to do it](#how-to-do-it-1)
    * [How it works](#how-it-works-1)
    * [There's more](#theres-more-1)
    * [See more](#see-more-1)
  * [4.3 Exploring numeric value distribution in a column](#43-exploring-numeric-value-distribution-in-a-column)
    * [Getting ready](#getting-ready-2)
    * [How to do it](#how-to-do-it-2)
    * [How it works](#how-it-works-2)
    * [There's more](#theres-more-2)
    * [See more](#see-more-2)
  * [4.4 Exploring spatiotemporal vector data using Time Manager](#44-exploring-spatiotemporal-vector-data-using-time-manager)
    * [Getting ready](#getting-ready-3)
    * [How to do it](#how-to-do-it-3)
    * [How it works](#how-it-works-3)
    * [See more](#see-more-3)
  * [4.5 Creating animations using Time Manager](#45-creating-animations-using-time-manager)
    * [Getting ready](#getting-ready-4)
    * [How to do it](#how-to-do-it-4)
    * [How it works](#how-it-works-4)
  * [4.6 Designing time-dependent styles](#46-designing-time-dependent-styles)
    * [Getting ready](#getting-ready-5)
    * [How to do it](#how-to-do-it-5)
    * [How it works](#how-it-works-5)
    * [See more](#see-more-4)
  * [4.7 Loading BaseMaps with the QuickMapServices plugin](#47-loading-basemaps-with-the-quickmapservices-plugin)
    * [Getting ready](#getting-ready-6)
    * [How to do it](#how-to-do-it-6)
    * [How it works](#how-it-works-6)
    * [There's more](#theres-more-3)
    * [See more](#see-more-5)
  * [4.8 Loading BaseMaps with the OpenLayers plugin](#48-loading-basemaps-with-the-openlayers-plugin)
    * [Getting ready](#getting-ready-7)
    * [How to do it](#how-to-do-it-7)
    * [How it works](#how-it-works-7)
    * [There's more](#theres-more-4)
    * [See more](#see-more-6)
  * [4.9 Viewing geotagged photos](#49-viewing-geotagged-photos)
    * [Getting ready](#getting-ready-8)
    * [How to do it](#how-to-do-it-8)
    * [How it works](#how-it-works-8)
    * [There's more](#theres-more-5)
    * [See more](#see-more-7)

<!-- tocstop -->

In this chapter, we will cover the following recipes:

* Listing unique values in a column
* Exploring numeric value distribution in a column
* Exploring spatiotemporal data using Time Manager
* Creating animations using Time Manager
* Designing time-dependent styles
* Loading BaseMaps with the QuickMapServices plugin
* Loading BaseMaps with the OpenLayers plugin
* Viewing geotagged photos


## 4.1 Introduction

This chapter focuses on recipes that will help you visually inspect and better comprehend  your data.
The recipes in this chapter include methods of summarizing, inspecting, filtering, and styling data, based on spatial and temporal attributes so that you can get a better feeling for your data before you perform analysis.
The primary goal is to create some visuals or summaries of data that allow you, the human, to utilize your brain's ability to identify patterns of interest.
The better you understand your data, the easier it is to pick appropriate analysis methods later.


### Getting ready
### How to do it
### How it works
### There's more
### See more

## 4.2 Listing unique values in a column

### Getting ready

### How to do it

If you are simply looking for a solution based on the GUI, the List unique values tool is available both in Vector | Analysis Tools as well as in the Processing Toolbox menu.
You can use either one of these to get a list of the unique values in a column.
Having this tool available in the Processing Toolbox menu makes it possible to include it in processing models and, thus, automate the process.
__The following steps to list unique values use the Processing Toolbox menu:__
1.  Start List unique values from the Processing Toolbox menu.
2.  Select the poi_names_wake layer as Input layer and the class attribute as Target field.
3.  Click on Run and wait for the tool to finish. The results will be displayed in the
Results view, which will open automatically.

__If you want to further customize this task, for example, by counting how often the values appear in this dataset, it's time to fire up Python console:__

1.  Start Python Console, which you will find in the Plugins menu, and click on the Show editor button (in the toolbar to the left of Python console) to open the editor window.
2.  Paste the following short script into the editor, save it, and then click on the Run script button. (Make sure that the POI layer is selected in the layer list.)
It loops through all features in the active layer and creates a dictionary object, which contains all unique values and the corresponding counts:

```py
import processing
layer = iface.activeLayer() classes = {}
features = processing.features(layer)
for f in features:
attrs = f.attributes()
class_value = f['class']
if class_value in classes:
classes[class_value] += 1
else:
classes[class_value] = 1
print classes
```

### How it works

### There's more

If you are using an SQL database, such as Spatialite or PostGIS, you can achieve similar results with a query using the COUNT and GROUP BY functions to count the number of features or records per class:
```sql
SELECT class, COUNT(*)
FROM poi_name_wake
GROUP BY class
```

### See more

## 4.3 Exploring numeric value distribution in a column

In this recipe, we will look at how to explore the properties of a column of numeric values.
We will look at the tools that QGIS offers and apply them to analyze the elevation values in our sample POI dataset.



### Getting ready

To follow this recipe, please load poi_names_wake.shp.
If you followed the previous recipe, Listing unique values in a column, you can continue directly from there.


### How to do it

A good way to get a first impression of the properties of a numeric column is using the Basic Statistics tool from Vector.
This allows you to calculate statistical values, such as the minimum and maximum values, mean and median, standard deviation, and sum.

If you want to examine the distribution of elevation values, there is the handy Statist plugin.
Statist generates an interactive histogram representation of the value distribution:

1. Install Statist using Plugin Manager.
2. Start Statist from the Vector menu.
3. Specify Input vector layer and the attribute that you want to analyze (Target field), then click on OK to compute the statistics.
4. Using the buttons below the diagram, you can zoom and pan the diagram, as well as save the diagram image.
5. You can even customize the diagram by changing the title and axis labels and ranges.
Just use the right-most button with the green tick mark on it to open the customization dialog:


### How it works

### There's more

### See more

* For those who want to perform more advanced graphing and numerical analysis, consider using the matplotlib python library or reading your data sources into R.
Aggregate functions in SatialLitetgis PostGIS can also provide you with min, max, average, sum, and other summarization functions.
For PostGIS, refer to http://www.postgresql.org/docs/9.1/static/functions-aggregate.html.


## 4.4 Exploring spatiotemporal vector data using Time Manager


In this recipe, we will look at exploring spatiotemporal vector data using the Time Manager plugin.
We'll use event data from the ACLED (Armed Conflict Location and Event Data Project) at http://www.acleddata.com/about-acled/.


### Getting ready

To follow this recipe, please load ACLED_africa_fatalities_dec2013.shp.
The layer style that you will see in the following screenshots consists of a simple circle marker at 50% transparency with the data-defined size set to the number of fatalities of the incident.
(You can read more about styling in Chapter 10, Cartography Tips, and Learning QGIS book by Packt Publishing.)
If you want some additional geographic context, you can also load NE_africa.shp, which contains the outline of Africa.


### How to do it

Once the data is loaded, all event positions will be displayed.
The default way to filter the events, for example, to only see the events from December 1, is to use Layer | Query and enter a filter expression or query, such as the following:
```sql
"EVENT_DATE" >= '2013-12-01' AND "EVENT_DATE" < '2013-12-02'
```
1. It's easy to see that updating this query manually for each day will not be a very convenient way to explore spatiotemporal data. Therefore, we will use the Time Manager plugin (installed using Plugin Manager).
2. The Time Manager panel will be added to the bottom of the QGIS window once the plugin is installed. Click on the Settings button to open the Time manager settings dialog. We can configure Time Manager here.
3. Click on Add Layer to open the Select layer and column(s) dialog.
4. Select the ACLED_africa_fatalities_dec2013 layer and EVENT_DATE as starting time and then click on OK to add the event point layer to the list of managed layers, as shown in the following screenshot:
> Optionally, you can enable display frame start time on map to add a small label with the corresponding
timestamp to the rendered map.
5. Click on OK when you are done. At this point, Time Manager applies the temporal fi er
to the dataset, so this can take some time depending on the size of the dataset used.
6. By default, after the first layer has been added, Time Manager will display all the events that occurred during the first day of the dataset. It is easy to adjust the filter by changing the Time frame size settings. You can increase the number of days that should be displayed or change to one of the other time units, including seconds, minutes, hours, weeks, and months, as shown in the following screenshot:
7. Once you are happy with the settings, you have multiple options to navigate through time:
* Click on the play button in the bottom-left corner of the Time Manager panel to start an automatic animation
* Move the time slider to the center of the panel like you would do to navigate within a video or music player application
* Click on the forward or backward button on either side of the slider to advance or go back by one time frame
* Of course, you can also edit the Time frame start setting directly


### How it works

For performance reasons, Time Manager relies on the layer query/filter expression capability of QGIS. This comes with the following limitations:

* Time Manager can only be used with data sources that support layer queries or fi er expressions.
Most notably, this means that it cannot be used with delimited text layers.
* As the layer queries or filter expressions have to work with strings, it has to be possible to order the date-time values correctly using text sort.
Therefore, the values have to be stored in one of the following formats:
```PY
%Y-%m-%d %H:%M:%S.%f
%Y-%m-%d %H:%M:%S
%Y-%m-%d %H:%M
%Y-%m-%dT%H:%M:%S
%Y-%m-%dT%H:%M:%SZ
%Y-%m-%dT%H:%M
%Y-%m-%dT%H:%MZ
%Y-%m-%d
%Y/%m/%d %H:%M:%S.%f
%Y/%m/%d %H:%M:%S
%Y/%m/%d %H:%M
%Y/%m/%d
%H:%M:%S
%H:%M:%S.%f
%Y.%m.%d %H:%M:%S.%f
%Y.%m.%d %H:%M:%S
%Y.%m.%d %H:%M
%Y.%m.%d
%Y%m%d%H%M%S
```

> If your data uses a different format, which is ordered correctly as well, you can add it to timevectorlayer.py or change the format using Field Calculator.



### See more

* The following recipes will show you how to create videos and more sophisticated time- dependent styles using Time Manager.

## 4.5 Creating animations using Time Manager


In this recipe, we will use the Time Manager plugin to create an image series out of our spatiotemporal QGIS project and turn it into a video, which is ready to be uploaded on Youtube or added into a presentation using easily available and free tools.

### Getting ready


To follow this recipe, it's advisable that you complete the previous recipe, Exploring spatiotemporal vector data using Time Manager, to set up this project.

To turn the image series exported by Time Manager into a video, we can use external programs, such as the command-line tool Mencoder, or the free Windows Movie Maker.

Mencoder is a very useful command-line tool to encode videos, which is available from repositories for many Linux distributions and for Mac.
Windows users can download it from Gianluigi Tiesi's site at http://oss.netfarm.it/mplayer-win32.php.

If you're using Windows, you can also create the video using the free Windows Movie Maker application, which can be downloaded from http://windows.microsoft.com/en-us/windows/get-movie-maker-download.


### How to do it

Before starting the export, it is a good idea to check all the settings, as follows:
1. The time slider should be moved to the beginning of the time line, and an appropriate Time frame size should be set.
2. Additionally, it can be useful to enable display frame start time on map (refer to the screenshots in the previous recipe) if you haven't done so already in the previous recipe because, otherwise, the exported animation frames won't contain any information about the time of the displayed events.
3. When you have found the best settings for your dataset, click on the Export Video button.
A dialog will open, which will allow you to select the folder that you want   to export your video to.
After you click on the Select Folder button, Time Manager will automatically start to export the video frames.
As displayed in the Export video information popup, you should now wait for the export to finish.
There will be another popup once this process is finished, which looks like the following screenshot:
4. If you open the export folder, you will see the animation frames that Time Manager just created.
5. This is how you can use Mencoder to create an .avi video from all .png images  in the current working directory.
Make sure that you are in the folder containing the images before running the following command:
```PY
mencoder "mf://*.png" -mf fps=10 -o output.avi -ovc lavc
-lavcopts vcodec=mpeg4
```
6.  You can control the speed of the animation using the frames per second (fps) parameter.
Higher values create a faster animation.

__If you're using Windows Movie Maker, perform the following steps:__
1. Load the animation frame images.
2. To adjust the speed of the animation, go to Video Tools | Edit, and reduce the Duration value.
Note that you should have all images selected if you want to apply the same duration to all images at once.
3. To save the animation, just go to File | Save Movie and select your preferred resolution and quality.


### How it works

## 4.6 Designing time-dependent styles

In this recipe, we will use the animation_datetime() function, which is exposed by Time Manager to create a time-dependent style for our animation.
The style will represent the age of the event feature: the event marker's fill color will fade towards gray the older the event gets.


### Getting ready

To follow this recipe, please load ACLED_africa_fatalities_dec2013.shp and configure Time Manager, as shown in the Exploring spatiotemporal vector data using Time Manager recipe, with the following exception: when adding the layer to Time Manager, set the End time value to the FOREVER attribute.


### How to do it

To create a time-dependent style, we use the Data defined properties option of the Simple marker:
1. In the Exploring spatiotemporal vector data using Time Manager recipe, we mentioned that we already used the FATALITIES attribute to scale the marker size.
For the time-dependent style, we will add a new definition to the Fill color property, as shown in the following screenshot:
2. Confirm the changes and start the animation to watch the effect.


### How it works


Here is our color expression in more detail:
```
color_hsla(
0,
scale_linear( day(age(todatetime(animation_datetime()), todatetime("EVENT_DATE"))),
0,31,
100,0
),
50,
128
)
```
The expression consists of multiple parts, as follows:

* day(age(todatetime(animation_datetime()),todatetime("EVENT_ DATE"))): This calculates the number of days between the current animation time given by animation_datetime() and calculates EVENT_DATE
* scale_linear: This transforms the age value (in days between 0 to 31 days) into a value between 100 and 0 which is suitable for the saturation parameter of the following color function
* color_hsla: This is one of many functions that are available in QGIS to create colors.
It returns a string representation of a color, based on its attributes for hue (0 equals red), saturation (between 0 and 100 depending on our age function), lightness (50 equals medium lightness) and alpha (128 equals 50% transparency)

You can speed up the fading effect by reducing the scale_linear parameter, domain_max, from 31 days to a smaller value, such as 7, for a complete fade to gray within one week.

### See more

* If you are interested in learning more about color models, such as the HSL color model used in this recipe, we recommend the Wikibook on color models at http://en.wikibooks.org/wiki/Color_Models:_RGB,_HSV,_HSL

## 4.7 Loading BaseMaps with the QuickMapServices plugin

### Getting ready

### How to do it
### How it works
### There's more
### See more

## 4.8 Loading BaseMaps with the OpenLayers plugin

### Getting ready

### How to do it
### How it works
### There's more
### See more

## 4.9 Viewing geotagged photos

### Getting ready

### How to do it

### How it works

### There's more

Don't have a camera or phone with built-in geotagging? This is not a problem.
There are many ways to add location information by yourself.
One such method is with the Geotag and import photos plugin that lets you link photo data to known locations, and this can be found at http://hub.qgis.org/projects/geotagphotos/wiki.

If you need something more sophisticated, there are many other tools out there.
Digikam, an open source photo management program, includes a geotagging tool that will attempt to automatch a GPX file from a GPS to your photos, based on timestamps.

Geotagged photos are also supported by many online photos services, so you can easily browse a map of the photos that you've uploaded.
Flickr is probably the most well-known for this, and it also includes a concept of geo-fences, where you can exclude certain locations from being publicly known.

On the flip-side, you now have an idea about how to remove geotags from photos in case you don't want their locations known if you share them online.


### See more

* There are other methods of seeing photos in the map besides eVis, including HTML map tips.
Refer to Nathan's blog at http://nathanw.net/2012/08/05/html-map-tips-in-qgis/.
* More information about geotagging with Digikam can be found at http://docs.kde.org/development/en/extragear-graphics/kipi-plugins/geolocation.html.
* You can also use Flickr to geotag and re-export your images, you can or create online map mash-ups with their API.



```python

```

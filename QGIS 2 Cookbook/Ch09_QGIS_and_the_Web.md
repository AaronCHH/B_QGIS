
# Chapter 9: QGIS and the Web
<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [Chapter 9: QGIS and the Web](#chapter-9-qgis-and-the-web)
  * [9.1 Introduction](#91-introduction)
  * [9.2 Using web services](#92-using-web-services)
    * [Getting ready](#getting-ready)
    * [How to do it](#how-to-do-it)
    * [How it works](#how-it-works)
    * [There's more](#theres-more)
    * [See also](#see-also)
  * [9.3 Using WFS and WFS-T](#93-using-wfs-and-wfs-t)
    * [Getting ready](#getting-ready-1)
    * [How to do it](#how-to-do-it-1)
    * [How it works](#how-it-works-1)
    * [There's more](#theres-more-1)
  * [9.4 Searching CSW](#94-searching-csw)
    * [Getting ready](#getting-ready-2)
    * [How to do it](#how-to-do-it-2)
    * [How it works](#how-it-works-2)
    * [There's more](#theres-more-2)
    * [See also](#see-also-1)
  * [9.5 Using WMS and WMS Tiles](#95-using-wms-and-wms-tiles)
    * [Getting ready](#getting-ready-3)
    * [How to do it](#how-to-do-it-3)
    * [How it works](#how-it-works-3)
    * [There's more](#theres-more-3)
    * [See also](#see-also-2)
  * [9.6 Using WCS](#96-using-wcs)
    * [Getting ready](#getting-ready-4)
    * [How to do it](#how-to-do-it-4)
    * [How it works](#how-it-works-4)
    * [There's more](#theres-more-4)
  * [9.7 Using GDAL](#97-using-gdal)
    * [Getting ready](#getting-ready-5)
    * [How to do it](#how-to-do-it-5)
    * [How it works](#how-it-works-5)
    * [There's more](#theres-more-5)
    * [See also](#see-also-3)
  * [9.8 Serving web maps with the QGIS server](#98-serving-web-maps-with-the-qgis-server)
    * [Getting ready](#getting-ready-6)
    * [How to do it](#how-to-do-it-6)
    * [How it works](#how-it-works-6)
    * [There's more](#theres-more-6)
    * [See also](#see-also-4)
  * [9.9 Scale-dependent rendering](#99-scale-dependent-rendering)
    * [Getting ready](#getting-ready-7)
    * [How to do it](#how-to-do-it-7)
    * [How it works](#how-it-works-7)
    * [There's more](#theres-more-7)
    * [See also](#see-also-5)
  * [9.10 Hooking up web clients](#910-hooking-up-web-clients)
    * [Getting ready](#getting-ready-8)
    * [How to do it](#how-to-do-it-8)
    * [How it works](#how-it-works-8)
    * [There's more](#theres-more-8)
    * [See also](#see-also-6)
  * [9.11 Managing GeoServer from QGIS](#911-managing-geoserver-from-qgis)
    * [Getting ready](#getting-ready-9)
    * [How to do it](#how-to-do-it-9)
    * [How it works](#how-it-works-9)
    * [There's more](#theres-more-9)
    * [See also](#see-also-7)

<!-- tocstop -->

In this chapter we will cover the following recipes:

* Using web services
* Using WFS and WFS-T
* Searching CSW
* Using WMS and WMS Tiles
* Using WCS
* Using GDAL
* Serving web maps with the QGIS server
* Scale-dependent rendering
* Hooking up web clients
* Managing GeoServer from QGIS



## 9.1 Introduction

QGIS is a classic desktop geographic information system (GIS).
However, these days only working with local data just isn't enough.
You want to be able to freely use data from the web without spending days downloading this data.
At the same time, you want to be able to put maps online for a much wider audience than a paper map or PDF.
This is where web services come in.
QGIS can be both a web service client and a web server, providing you with lots of options to use and share geographic data.
__This chapter covers the basics of using open web services for geographic data and a few methods to put your maps online.__



## 9.2 Using web services

There are quite a few different types of web-based map service that can be loaded in QGIS.
Each type of web service provides data; often, this is the same data in different ways.
This recipe is about helping you figure out what type of web service you want to consume, and conversely what type of web service you may want to create for others to use.



### Getting ready

This recipe is all about thinking, so you don't need anything in particular to start. It does help if you have a project in mind and some type of data you are interested in using or creating.
Most, if not all, of these methods require an Internet connection or a local server, providing these services. Of course, if you have the data locally, you should probably just load it directly.
To help with the following section, here's a list of acronyms:

* __CSW:__ This is Catalog Service for the Web
* __WFS:__ This is Web Feature Service
* __WFS-T:__ This is Web Feature Service, Transactional
* __WCS:__ This is Web Coverage Service
* __WMS:__ This is Web Map Service
* __WMTS:__ This is Web Map Tile Service
* __TMS:__ This is Tile Map Service
* __XYZ:__ This is an "X Y Z" Service (there really isn't a formal name for this one because it's not an official standard)




### How to do it

1. Start by answering the following questions with regards to using data over web services:
* Do you already know where to find the data that you want?
* Do you need to edit or apply custom styling to this data?
* Do you care if the data is vector or raster?
* Do you need the data at its original resolution or quality?
* Do you need data at specific resolutions or in a specific projection?

2. Use the following decision matrix to pick out which services are appropriate to your use case:

Now that you've found the appropriate recipe for the web service that you want to use, jump  to this recipe later in this chapter.
If you're still not sure, read on for more hints on how to pick the correct service.
This recipe applies both to how you use web services and how to decide what web services to offer (if you put up a web service for other people to use).

Generally speaking, from left (WFS) to right (Tiles) the speed of the service increases.
Tiles serve data the fastest to end users but with the most limitations.


### How it works

For each of the services, there is a QGIS tool (built-in or plugin).
This tool stores your list of web servers and connection settings for each service.
When you go to load a layer from a chosen server for a particular protocol an up-to-date list of layers is requested from the server (that is, the GetCapabilities XML).
You then get to pick off this list the layers that you would like to add to the map canvas (and depending on service type, the projection, and the file type).


### There's more

Vector is generally slower, as more data needs to be transmitted as the data grows.
Raster formats are a fixed number of pixels onscreen, so it's always approximately the same amount of data per screen load.

> WMS, WMTS, WFS, and WCS are sometimes referred to as W*S as a collective group of related services that behave similarly.

Each situation will have additional considerations.
For example, if you need a specific projection, you probably can't use a Tile service because these are usually only in very specific projections (Web/Spherical Mercator).
Or perhaps, you want to print large paper maps.
Then, you probably want WFS or WCS in order to get the full resolution possible over your entire region.

One of the most common mistakes is to think that you need vector data when you actually just need a background tile that incorporates vector data.
A great example of this is road data.
If you don't actually need to style, select, or individually manipulate road data, and then a Tile or WMS type layer will be much faster.


### See also

* For more information, read the standards at the OGC website, http://opengeospatial.org

## 9.3 Using WFS and WFS-T

Web Feature Services (WFS) is an OGC standard method to access and, in some cases, edit (WFS-T) vector data over the Internet.
When you need full attribute tables, local style control, or editing, WFS is the way to go.
Like most other web services, the biggest advantage over a local layer is that you don't have to copy or load the whole layer at once.

> If you just need to view the layer, often WMS or a Tile service (described in other recipes within this chapter) are more efficient.


### Getting ready

You need the URL of a WFS service to use and a working Internet connection.
We will use the public Mapserver demo website (http://demo.mapserver.org/).

> To try WFS-T, which involves editing, you will need to get access to a service (typically, password protected) or make one yourself.
Do you need a WFS-T test server? This is a great case where OSGeo-Live comes in handy, as you can run your own WFS-T server in a virtual machine at http://live.osgeo.org.


### How to do it

1. Find a WFS service that you want to use and copy the GetCapabilities URL. For this example, we will use the Mapserver demo website, http://demo.mapserver.org/.
> As with other web services, it's more efficient if you load your local layers and zoom to their extent first.
This enables you to not waste time loading data from web services for extents outside your area of interest.

2. Open the add WFS dialog by clicking on the following icon:
3. Create a New connection.
4. Assign Name of your choosing and paste in the URL field for the WFS service, (http://demo.mapserver.org/cgi-bin/wfs?SERVICE=WFS&VERSION=1.0. 0&REQUEST=GetCapabilities):

> On future usage of the same service, this will already be in your list of services, so you only have to add it once.

5. Save your edits by clicking on the OK button.
6. Now select the service from the drop-down menu.
7. Query for a list of layers by clicking on the Connect button.
8. Select the layer or layers that you want to add to the map; either continents or
cities works for this example:

9. When your selection is complete, use the Add button to place the layer in the map.
10. Rearrange the render order of the map by dragging layers up and down in the list.
11. Pan and zoom to make fresh requests for WFS data to be loaded to the view.

> WFS layers can be restyled with standard layer properties.
Also, the information tool and the attribute table will appear as other vector layers.



### How it works

For each web service, there is a main URL.
When you browse to this URL and add the GetCapabilities parameter (QGIS does this for you), the returned result is an XML file, which describes the services that are offered by the server.
The client, QGIS, parses the list of layers for you to choose from, and once you pick the layer(s), uses the additional information in the XML to look up the data at the specific URL.

Data requests are limited to the visible bounding box of the map canvas.
This limits the amount of data that is requested.
At least this is how it should work.
However, features that go off screen will likely be included in their entirety to maintain geometry integrity.
So, expect that loading large vector layers over WFS has the potential to be extremely slow.


### There's more

WFS-T services typically require passwords and are designed to work over the Internet.
If you are working within a local network, you may consider just using PostGIS layers.
Either way, it should also be noted that versioning and conflict resolution are not automatic, requiring the service backend to be configured to support such features.


## 9.4 Searching CSW

CSW is a catalog web service.
Its main function is to provide discoverability of geographic data and link you to usable data either by download or by any other of the web services that are mentioned in this chapter.

### Getting ready

This recipe uses the MetaSearch plugin.
It requires the pycsw and owslib libraries installed in your system's Python.
Refer to the Adding plugins with Python dependencies recipe of Chapter 11, Extending QGIS, for help on installing pycsw and owslib if you don't know how to do this.

### How to do it

1. Open the MetaSearch plugin by navigating to __Web | MetaSearch | MetaSearch.__
If you don't see the Web menu, check the plugin manager and ensure that MetaSearch is enabled (checkmark).

2. Pick a service from the dropdown on the right: UK Location Catalogue Publishing Service.
> If you don't see any services in the drop-down list, click on the Services tab and use the Add default services button.

3. Type a search term in box on the left: Park.

4. (Optional) Set an extent to limit the search. In this case, use Map Extent.
> Global searches often return too many results, or they cause the connection to time out while waiting for all the results.
As with other web services, it is advisable to load a reference layer and zoom to the area of interest first before trying to search them.
The third tab, Settings, allows you to adjust the timeout.
Increase this if you're getting too many timeout errors.

5. Click on the Search button and wait for the results:
1. Double-click on any of the results to see additional details.
2. If a selected result is available as a loadable layer, one or more of the service buttons at the bottom of the screen will be enabled.
To understand more about how to use each of these choices, refer to the other recipes in this chapter on WMS, WFS, and WCS:


### How it works

MetaSearch queries websites that provide catalogs in the CSW standard, which is defined by the OGC.
Once your request parameters are sent, the receiving website queries its online database for matches.
If matches are found, metadata about the results is sent back to the client, in this case, QGIS.

CSW currently includes options to search by keyword and spatial extent.
Future versions may enable setting time frames.


### There's more


If you pick opening an additional service that is based on the results, Metasearch will create a temporary service registration and open the correct service dialog.
Unfortunately, at this time, you need to then scroll through the available layers to find the one that you want and actually add it to the map.

> Additional future CSW searches will ask if you want to override the existing connection.
You must say yes.
If you find yourself using the same W*S service, consider copying the GetCapabilities URL and making a new permanent entry in the correct service dialog.

You can add more catalogs to search on the Services tab within the plugin.
You will need to find the CSW GetCapabilities URL on the website that you want to query.
Most of the common geoportal-type websites now support CSW, including (but not limited to) Geonode, Geonetwork, and the ESRI Geoportal.

CSW is a relatively new standard when compared to some of the others, and it seems to be hard to find services that consistently work and actually offer WMS, WCS, or WFS of the layers in their catalog.



### See also

* The Adding plugins with Python dependencies recipe of __Chapter 11, Extending QGIS__, for help on installing pycsw and owslib if you don't know how

## 9.5 Using WMS and WMS Tiles

Web Map Services (WMS), one of the first OGC web services created, provides a method for dynamic raster generation served over the Web.
They are a compromise between the flexibility of WFS and the speed of Tile services.


### Getting ready

There are several iterations of WMS, and QGIS supports most of them.
To use a WMS, you need to give QGIS the GetCapabilities URL of the service that you want to view data from.


### How to do it

1. Find a WMS service that you want to use and copy the GetCapabilities URL. In this recipe, we can use the Geoserver demo website (http://demo.opengeo.org/geoserver/web/).
http://suite.opengeo.org/dashboard/
> As with other web services, it's more efficient if you load your local layers and zoom to their extent first.
This enables you to not waste time loading data from web services for extents outside your area of interest.

2. Open the Add WMS dialog.
3. Create a New connection.
4. Assign a Name of your choosing and paste in the URL (http://demo.opengeo.org/geoserver/ows?service=wms&version=1.3.0&request=GetCapabilities):
> On future usage of the same service, this will already be in your list of services, so you only have to add it once.

5. Save your edits by clicking on OK.
6. Now select the service from the drop-down list.
7. Query for a list of layers using the Connect button.
8. Select the layer or layers that you want to add to the map:
> You can select one or more layers.
If you select multiple layers, they will be merged and only appear as a single layer in the QGIS Layers list. The Layer Order tab lets you arrange the WMS layers within the combined layer. This is important when one of the layers is opaque and has 100% continuous data, allowing you to put other data on top of it visually.

9. There are several other options, including image type and projection:
* For an image type, PNG is a good default as it supports lossless compression and transparency.
If you don't need transparency and are okay with a little data loss, JPG can be used for smaller files, so they are faster to load.
* When picking projection, if you can use the original projection of the data (if you know it), you will get the least resampling.
Otherwise, pick something that matches the other data that you plan to use in conjunction with the WMS.

> Not all image types and projections are available; this depends on what the server offers.
If one image type doesn't seem to work, try a different one before reporting a bad server.

10. When your selection is complete, use the Add button to place the layer in the map.
11. Rearrange the render order of the map by dragging layers up and down in the list.
12. Pan and zoom to make fresh requests for WMS data to be loaded to the view.


### How it works

When you pan and zoom the map, a request with the bounding box of the viewable extent and scale is sent to the service.
The server then renders an image that matches the request and passes it back to the client (in this case, QGIS).


### There's more


Some WMS services now also support tiling under the Web Map Tiling Service (WMTS) protocol.
From the client's perspective, this not really different from WMS in usage.
On the server side, after each request the results are cached so that if the same extent and scale is requested, the cached version can be delivered instead of creating the results from scratch.
For you, the end user, this should result in faster loading if a service provides WMTS.

When configuring a WMTS, use the WMTS URL instead of the WMS URL.
One example would be the Geoserver demo site's WMTS:

http://demo.opengeo.org/geoserver/gwc/service/wmts?REQUEST=GetCapabilities

Once successful, this will take you to the Tilesets tab, where you can pick which layer and projection of the available options you want to load.
As the Tiles are premade or cached, you will usually not have the option to combine multiple layers at once and will need to load them one at a time:

> WMS-C is an earlier version of the WMTS standard. In usage, it's pretty much the same, though the URL pattern may look more similar to the WMS.


### See also

* See the QGIS documentation for more information about the WMS capabilities of QGIS at http://docs.qgis.org/2.8/en/docs/user_manual/working_with_ogc/ogc_client_support.html#ogc-wms

## 9.6 Using WCS

A Web Coverage Service (WCS) differs greatly in use case from the other services, but it behaves very similarly. The goal of WCS is to allow users to extract a region of interest from a large raster data that is hosted remotely. Unlike a WMS or Tiled set, WCS is a clip of the original data in full resolution and usually in the original projection. This format is ideal if you need the raster data for analysis purposes and not just visualization.


### Getting ready

For this recipe, you need a WCS to connect to. Check with your data providers to see whether they offer WCS. For this recipe, we can use the OpenGeo Geoserver Demo site at http://demo.opengeo.org/geoserver/web/.

### How to do it

1. Open a web browser and go to http://demo.opengeo.org/geoserver/web/.
2. On the right-hand side, you'll see a list of web services that are available; right-click on WCS 1.1.1 and copy the link.
3. In QGIS, open the WCS dialog.
4. Select New to create a new server entry.
5. In the boxes, perform the following:
1. Provide a name so that you remember which service this is.
2. Paste the URL that you copied earlier in the URL box (http://demo.opengeo.org/geoserver/ows?service=wcs&version=1.1.1&request=GetCapabilities):

6. Click on the OK button.
7. Now, you'll be back on the Add Layer(s) from a WCS Server dialog:
1. Click on the Connect button.
2. After the list is populated, select a layer to add to the map.
Click on the Add button.
Try the Blue Marble layer.
3. Now, click on the Close button to return to your map:

8. You should now see the Blue Marble layer loaded.
> If you zoom in to the level of a US State or a European country, you will see the image start to pixelate.
Blue Marble is a low resolution image put together by NASA that roughly shows what the whole world looks like from space, cloud free.
It is meant as a general view of the whole world and does not contain fine details.

9. (Optional) Use the Save As option to download a portion or the entire WCS layer at its full original resolution.



### How it works

WCS, like other web services, sends a bounding box request to the server, which in turn delivers the raster data to QGIS.
Unlike WMS, no rendering is done on the server side, the raw original raster data is sent.
This could mean the following:
* There was no resampling of the image before it was sent
* You can apply your own styling to the data that is delivered

Keep in mind that requesting the full extent of a high resolution raster will result in a large amount of data transfer.
This is unlike Tiles or WMS, which at most return the exact number of pixels in the viewable area at the resolution that is requested.

> As with other web services, it is recommended that you zoom in to your area of interest before loading a WCS.

### There's more

Another bonus of WCS over WMS is that because WCS delivers the original data, it is not limited to a 3-band RGB image.
You can use WCS to view and download Multi or Hyperspectral data (4+ bands common in remote-sensing applications).

> Currently, QGIS only supports 1.0.x and 1.1.x, not WCS 2.x; at least not yet!

## 9.7 Using GDAL

The QuickMapsServices and OpenLayers plugins, as described in the Loading BaseMaps with the QuickMapServices plugin and Loading BaseMaps with the OpenLayers plugin recipes in Chapter 4, Data Exploration, are awesome as they put a reference layer in your map session.
The one downside, however, is that it is a hassle to add new layers.
So, if you come across or build your own Tile service and want to use it in QGIS, this recipe will let you use almost any Tile service.


### Getting ready

You will need a web browser, text editor, and the URL of a web-based XYZ (sometimes called TMS) service—one that allows you to make requests without an API key.
We're going to use the maps at http://www.opencyclemap.org/.
Viewing the JavaScript source (a good tool for this is Firebug, or other web-developer tools for the browser), we can view the source URLs for the tiles.


### How to do it

1. Open http://www.opencyclemap.org/ in a web browser.
2. Now, figure out the URL for the tiles by looking at the source code:
1. Look in map.js and you'll see the layer definition:
```js
var cycle = new OpenLayers.Layer.OSM("OpenCycleMap",
["https://a.tile.thunderforest.com/cycle/${z}/${x}/${y}. png",
"https://b.tile.thunderforest.com/cycle/${z}/${x}/${y}. png",
"https://c.tile.thunderforest.com/cycle/${z}/${x}/${y}. png"],
{ displayOutsideMaxExtent: true,
attribution: cycleattrib, transitionEffect: 'resize'}
);
```
2. Or, you can look at the image files your browser downloads. If you put https://a.tile.thunderforest.com/cycle/13/1325/3143.png into a browser, it will load that one tile.

3. The pattern is pretty straight forward:
```
<server name>/<layer>/<zoom>/<tile index X>/<tile index X>.<image format>
```
> In this particular case, the Tile Index pattern is the TMS style; refer to http://www.maptiler.org/google-maps-coordinates-tile-bounds-projection/.

4. To turn this into a layer in QGIS, open up a text editor and paste in the following definition.
This definition tells GDAL which driver to use and the server URL pattern with z, x, and y as variables. Save the file as opencyclemap.xml:
```html
<GDAL_WMS>
<Service name="TMS">
ServerUrl>http://c.tile.thunderforest.com/cycle/${z}/${x}/${y}.png</ServerUrl>
</Service>
<DataWindow>
<UpperLeftX>-20037508.34</UpperLeftX>
<UpperLeftY>20037508.34</UpperLeftY>
<LowerRightX>20037508.34</LowerRightX>
<LowerRightY>-20037508.34</LowerRightY>
<TileLevel>18</TileLevel>
<TileCountX>1</TileCountX>
<TileCountY>1</TileCountY>
<YOrigin>top</YOrigin>
</DataWindow>
<Projection>EPSG:3785</Projection>
<BlockSizeX>256</BlockSizeX>
<BlockSizeY>256</BlockSizeY>
<BandsCount>3</BandsCount>
<Cache />
</GDAL_WMS>
```
5. You can now load the layer using the Raster dialog or the browser:
>　Note that there are two listings for opencyclemap.xml; only the one with the square-shaped icon will work (that is, a raster), as tiles are a raster format.

### How it works

The XML file defines the parameters of the service; however, because XYZ-style servers don't follow a standard, the URL pattern varies slightly for each server and the servers do not have a GetCapabilities function that describes available layers.
By telling GDAL how to handle the URL, you are wrapping a nonstandard format into a typical GDAL layer, which QGIS can easily be loaded as a raster.


### There's more

One additional tip when using Spherical Mercator (EPSG:3785) is that you can set a custom list of scales (Zoom Levels) in QGIS.
The following set of scales can be loaded per QGIS project, and will change the dropdown at the bottom right.
These scales match the scales that most servers will provide, so you get the best viewing experience:
1. Go to File | Project Properties.
2. Select the General tab.
3. Check the Project Scales checkbox.
4. Load the scales.xml file that is provided:

This technique is not limited to just tile services.
Many other formats that GDAL works with can be wrapped for easier usage in QGIS.
This is a similar method to Virtual Raster Tables (VRT) layers mentioned in the Creating raster overviews (pyramids) recipe in Chapter 2, Data Management.

Lastly, you may ask why a new plugin using this method doesn't replace the OpenLayers plugin.
Such an idea has been under discussion for a while; the key sticking point is that accessing some layers, such as Google, Bing, and so on, with this method may violate the Terms of Service as they do not keep the Copyright, Trademark, and Logo in the correct place.
Also, caching and printing such layers may not be legal.
In general, avoid using proprietary data when possible to reduce licensing issues.


### See also

* This recipe and method has actually been known and discussed in many QGIS venues.
The most frequently cited example is available at http://www.3liz.com/blog/rldhont/index.php?post/2012/07/17/OpenStreetMap-Tiles-in-QGIS.
* The full explanation of options for GDAL can be found at http://www.gdal.org/frmt_wms.html.


## 9.8 Serving web maps with the QGIS server

QGIS and the Web is not all about consuming data, it can also be used to serve data over the Web for others to view online or consume in other web clients (such as QGIS).
Keep in mind that setting up your own web service is not the easiest way to make a web map (refer to the Hooking up web clients recipe in this chapter).
This is, however, a great way to transition all the hard work that you've put into a QGIS project file into something other people can see and use.


### Getting ready


For this recipe, you need a working installation of the QGIS server.
This involves running a standard web server (such as Apache or Nginx).
There are many ways to set up the server, so please see the official documentation at http://hub.qgis.org/projects/quantum-gis/wiki/QGIS_Server_Tutorial.
Once you have the QGIS server running, then you just need a QGIS project with the confi ration outlined in this recipe.


### How to do it

1. Open QGIS.
2. Load up and style some layers:
* You need at least one vector layer to offer a WFS.
* You need at least one raster layer to offer a WCS.
* WMS can be any combination of layers, you can choose to server each as an independent layer or as a combined layer.
3. Edit the Project properties in File | Project Properties:
1. Open the OWS server tab.
2. Check the Service Capabilities box to enable GetCapabilities.
3. Fill out some of the boxes so that end users know what your server is about, who runs it, and how to contact you:

4. Now examine the WMS capabilities section:
> Most of these features are optional optimizations.
Pick and choose what suits your needs.

5. Here, you can set the maximum extent that clients should expect.

6. The CRS restrictions option lets you limit what projections are allowed.

7. Exclude Layers allows you to have layers in your project that don't show up on the web.

8. Add geometry to feature response is an optional enhancement if you are building a web map and you want to be able to work with the actual vectors (if it is a vector to begin with).

9. GetFeatureInfo precision is about how close a user has to click to query a location.
If you have a lot of data, you probably want this number to be small;
but if you have only a few features, making this bigger makes it easier for end users.

10. Set Maximums for GetMap request if you want to reduce the load on your server by limiting how much data a user can request at once.
This is a good idea for a public server.
2560 x 2048, as shown in the screenshot, is enough pixels for an HD-resolution screen to be filled in a single request.

11. Next, take a look at the WFS capabilities section:
> Only enable WFS if you want users to be able to request vector data as vectors. This can be more intensive than WMS on your bandwidth. Also, do not enable WFS-T features unless you secure your server to only permitted users.

1. Check the Published box next to any vector layers that you want to be usable over WFS.
2. To enable WFS-T, check the Update, Insert, and Delete checkboxes.
As they are separate, you can choose to only allow new data (Insert), only allow  edits to existing data (Update), or only allow removal of data (Delete).
Insert would be the safest option as it prevents editing or deletion of existing data.

12. Finally, take a look at WCS capabilities:
> This an all or none feature. Don't enable this unless you want users to be able to download the original raster data.

13. When you are done setting options, click on the OK button.

14. Now, save the project in a place where the QGIS server has access to it.
In Apache, this is usually a folder such as /var/www/.

15. Once saved, you can test access from any OGC-compliant web client:
1. For a simple test, use a fresh QGIS project and the Add WMS dialog.
2. The GetCapabilties URL will look something like http://localhost/cgi-bin/qgis_mapserv.fcgi?map=/usr/local/share/qgis/QGIS-NaturalEarth-Example.qgs.

> The key part of this URL that is somewhat unique to QGIS server is the map parameter, which is followed by the full system path to the QGIS project file.
This may seem odd, but adding your QGIS server as a WMS in QGIS is a great way to test whether it's working.


### How it works

The QGIS server is a middleman that takes in web service requests and translates them into QGIS internal calls, returning the requested data or rendered images, which are delivered to the end user via the web server.

### There's more


The QGIS server contains many options that allow you to control which types of service to offer, which layers to offer over each service, and how to style these services.
Alternatives to the QGIS server include MapServer and GeoServer (refer to the Managing GeoServer from QGIS section in this chapter).


### See also


* For more details, refer to the main documentation for the QGIS server at http://hub.qgis.org/projects/quantum-gis/wiki/QGIS_Server_Tutorial.
* Once you create a service, test it by adding your service to a QGIS project.
Refer to the previous recipes in this chapter for how to add WMS, WFS, or WCS services.

## 9.9 Scale-dependent rendering

While they are not specifically for web services, being able to change the styling and presence of data based on the scale of the map can have a huge impact on the speed and readability of web services.
Unlike printed maps, web maps are viewed at multiple scales.
This variation in scales often requires different cartography to keep the map legible and usable.


### Getting ready

You'll need a QGIS project, preferably one with a high data density or differing levels of information.
A good example is road data, where you have major, minor, local, and other variants of road classification.
caryStreets.shp converted from CAD in a previous chapter is a good example.


### How to do it

1. Open QGIS and load caryStreets.shp.
2. Now, open the attribute table and look for an attribute to filter in.
In caryStreets.shp, there are several potential columns to use, such as StreetType, Major_Road, and Main_Road.
StreetType appears to be classes, whereas the other two columns appear to be True or False flags.
Any of these are decent candidates for filtering rules.

3. Now, open the Properties section for the layer:
1. Switch to the Style tab to edit the symbology.
2. Change the top-right dropdown to Rule Based Rendering.
3. Create a new rule (green plus sign).
4. In the pop up dialog set Label to Major Roads and Filter to "Major_Road" = 't'.
5. (Optional) You can use the expression builder to build the filter statement and test it.
Click on the … button to open the dialog.
> You could create two copies of Major with different scale ranges so that as you zoom in, the major roads become thicker at the same time that minor roads are enabled.
This is what your layer looks like before and after you create the first rule:

6. Now, add another rule for minor roads by filtering for "Major_Road" = 'f'.

7. This time, you're going to enable the Scale range option.

8. Set Minimum (exclusive) to 1:100,000. For any scale bigger than this, the features will be hidden. For Maximum (inclusive), type in 1:0, which will disable the Max filter.

9. Pick a different line type and/or color for the minor roads:

10. You should now have two rules, one for major roads and one for minor roads:
> You don't have to open the edit rule dialog; you can directly modify parts of the rules in the Rule Based Rendering page.

11. Go back to the map and zoom in to 1:50,000, then zoom out to 1:250,000.
The minor roads should appear and disappear as you change past the 1:100,000 scale:


### How it works

The goal with scale-dependent rendering is generally to make your map readable at many different zoom levels.
By setting the Min and Max scales for each layer or subfeatures within a layer, you can declutter a map for readability.
The rendering engine just checks the scale against each rule before deciding what to render.


### There's more


Scale-dependent rendering can be used in several ways.
This can be used to change the styling based on zoom or hide or reveal data based on the zoom level.
However, it's also   not limited to just changing styles or layers.
You can also perform scale-dependent labeling, which is part of data-driven labeling described in the Configuring data-defined labels recipe in Chapter 10, Cartography Tips, of this book.

Scale rules also work on raster layers; however, this only allows you to turn a raster on and off.
It doesn't allow you to change its appearance.

If you have a QGIS server set up from earlier in this chapter, the scaling rules should apply to your web services (WMS and WFS).

> You probably don't want to use something as complex as a street layer via WFS in a web browser because it's almost guaranteed to crash.
Stick to pushing such complex layers as Tiles or WMS.




### See also

The Rule Based Rendering has a lot of features crammed into it.
However, this is not yet a comprehensive guide to everything that it can do, so you'll need to explore and perform Internet searches for now.


## 9.10 Hooking up web clients

Sometimes, the best way to share a map is to build a website with a map embedded in it.
There are many methods to accomplish this goal, ranging from a simple dump of a few layers to a highly-interactive map, which is based on web services.
There are many web clients that will work with standard OGC services.
This recipe will show you how to build a simple web map using Leaflet—a popular JavaScript library that is used to create web maps.

### Getting ready

You will need the qgis2leaf plugin and some sample data.
The schools_wake.shp (Points) and census_wake_2000.shp files make for a good example.

### How to do it

1. Install and enable the qgis2leaf plugin.
> Make sure to check out the qgis2web plugin, which is a newer variant that works similarly but has some different options.

2. Load up some layers to make a map composition.
> Make a copy of your layer and eliminate unnecessary columns that you don't need to show on the web map.
Reducing the size of the attribute table will make it easier to read popups with information and speed up web page loading.

3. Style the map as you want it to appear online.
> Styling can be really tricky.
Leaflet and other web map libraries don't support 100% of the same options as QGIS.
Try making a few maps, changing settings, and re-exporting these maps a few times to  figure out how to get it the way that you want.
It may not look good  in QGIS but look good in the export.

4. (Optional) Configure labels. In this example, label the School names.
> Only black labels are currently supported.
Though you can probably customize the CSS and JavaScript (js) after the export if you need labels in a different style.

5. Open the qgis2leaf plugin from its icon on the toolbar or from the Web menu:
1. Click on the GetLayers button to add the layers from your map to the export list.
2. There are lots of options here, and they are optional. Go ahead and check Create Legend. If you made labels, also check Export Labels and labels on hover.
> Create Cluster is a fantastic option if you have a lot of points on the map.
This will group points into a circle with a number indicating how many points are near there.
As you zoom in, they will split apart into smaller groups, until at some zoom, all the points are in their original spot.
3. For the frame size, you can pick a size of the page that you want the map to take up (in pixels).
However, fullscreen works well if the map is the only thing that you care about.
4. Go ahead and add a tile-based base layer; Stamen Terrain is an interesting choice.
Keep in mind that you can only have one of these on at a time, but you can toggle between them.
5. Pick an output folder location and fill in the remaining map information describing how you want it to show up in the results.
6. Export the project:

6. After exporting, the map should open in your web browser.
If it doesn't, open your operating system file explorer (or web browser) and navigate to the output folder.
You should see a new folder called export_year_month_day_hour_minute_ seconds (for example, export_2015_02_19_11_34_05).
Inside this folder is index.html.
Open this file with a web browser to see your map:

7. Note that all the vectors are clickable, and the popup will display the attribute table information.
If you turned on labels and hover, then hovering over a point will display the name.


### How it works

The qgis2leaf plugin converts your map into something that is compatible with the web.
Generally, this means converting vector data to the GeoJSON format and generating an HTML page (web page) with some JavaScript to create and populate the map.

Raster layers are trickier, and if you can, try to stick to using Tile or WMS services to serve them.
Refer to the next section to see how to use Tiles or WMS.

> If you need host tiles locally, try using the QTiles plugin to generate them.


### There's more

The next logical step is to make the map dynamic based on a web service.
To do this, you can swap static files for web services:

1. Add a WMS layer to the map (you can use the previous recipe in this chapter on QGIS server if you have it running).
Add an external source WMS, such as the USGS NAIP Airphoto.
(Here's the GetCapabilities URL, http://isse.cr.usgs.gov/arcgis/services/Orthoimagery/USGS_EROS_Ortho_NAIP/ImageServer/WMSServer?request=GetCapabilities&service=WMS).
2. Re-export with the same settings:
> Now that you've created the Leaflet map, if you wanted to get into JavaScript programming, all of the code that you need is in the directory produced, either directly in index.html or in the js folder.
In particular, you can see exactly how layers are styled and added to the map.

You don't have to use Tiles or WMS for raster layers but this is recommended.
If you do want to use a local file, be warned there is a bug currently where some exports fail unless your raster is converted to a .jpg format image in EPSG:4326 projection.


### See also


* Don't forget to look at the documentation for the Leaflet JavaScript library on how to customize your results after the export at http://leafletjs.com/.

* qgis2web plugin aims to combine qgis2leaflet and qgis2ol3 plugins (https://plugins.qgis.org/plugins/qgis2web/), which means it also includes export to OpenLayers 3 that is very similar to Leaflet but uses the OpenLayers JavaScript library.
Lizmap (http://www.3liz.com/en/lizmap.html) and QGIS Web Client (https://github.com/qgis/QGIS-Web-Client) are two more popular options that add more elaborate prebuilt interfaces but require a little more setup.

## 9.11 Managing GeoServer from QGIS


QGIS does not only serve as a frontend for the QGIS server, but it can also serve as a frontend for other similar servers.
GeoServer is one of the most popular ones, and you can configure it from QGIS, upload layers, or even edit the style of a GeoServer layer using the QGIS symbology tools.



### Getting ready

For this recipe, you will need the __GeoServer Explorer plugin__.
This can be installed using Plugin Manager.

You will also need a running instance of GeoServer.
We will assume that you have a local one running on port 8080, but you can replace the corresponding URL with the one of the GeoServer instance that you have available, whether local or remote.

### How to do it

1. Open the GeoServer Explorer by navigating to Web | GeoServer | Geoserver Explorer. The explorer will appear on the right-hand side of the QGIS window.

2. Click on the GeoServer Catalogs item in the explorer tree, and then select the New catalog button.

3. Complete the fields in the dialog that will appear to define a new catalog and
click on OK:

4. The new catalog will be added to the explorer tree, and you can now browse
its content.

5. Open the zipcodes_wake.shp layer and style it.

6. In the QGIS Layer List, drag the entry corresponding to the zipcode_wake.shp layer and drop it on the catalog item in the GeoServer Explorer. The layer will be uploaded and added to the default workspace of the catalog.

7. You can check whether the layer is now in the catalog by opening a web browser and going to the GeoServer web interface at http://localhost:8080/geoserver/web/.



### How it works

The GeoServer Suite plugin communicates with GeoServer using its REST API.
By linking QGIS with the GeoServer REST API, it allows you to easily configure many elements that, otherwise, should be configured manually, such as the styling of layers.


### There's more

The Geoserver Explorer plugin has a lot of features to work with GeoServer.
Here are some additional ideas so that you can explore them.
For more information, check out the plugin help at http://boundlessgeo.github.io/qgis-geoserver-plugin/.

__Editing a remote style__
Once the layer is in the GeoServer catalog, you can edit its style without having to upload the layer again.
Just open the Styles branch in the explorer tree under the corresponding catalog and double-click on the style to edit, or select the Edit...
item in the context menu that is shown when right-clicking on the element:
The QGIS symbology dialog will be opened, and you can edit the style in there.
Once you close the dialog, the style will be uploaded and updated in the catalog.

__Support for multiple formats__
The GeoServer REST API only supports shapefiles for vector layers, but you can drag and drop any layer in any format that is supported by QGIS.
This plugin will take care of converting it before uploading, in case this is needed.


Don't have a Geoserver instance, it's pretty easy to setup for testing. See http://geoserver.org/ for details.

### See also

Don't have a Geoserver instance, it's pretty easy to setup for testing. See http://geoserver.org/ for details.

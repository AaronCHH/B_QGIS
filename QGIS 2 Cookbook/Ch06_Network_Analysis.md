
# Chapter 6: Network Analysis
<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [Chapter 6: Network Analysis](#chapter-6-network-analysis)
  * [6.1 Introduction](#61-introduction)
  * [6.2 Creating a simple routing network](#62-creating-a-simple-routing-network)
    * [Getting ready](#getting-ready)
    * [How to do it](#how-to-do-it)
    * [How it works](#how-it-works)
    * [There's more](#theres-more)
    * [See also](#see-also)
  * [6.3 Calculating the shortest paths using the Road graph plugin](#63-calculating-the-shortest-paths-using-the-road-graph-plugin)
    * [Getting ready](#getting-ready-1)
    * [How to do it](#how-to-do-it-1)
    * [How it works](#how-it-works-1)
    * [See also](#see-also-1)
  * [6.4 Routing with one-way streets in the Road graph plugin](#64-routing-with-one-way-streets-in-the-road-graph-plugin)
    * [Getting ready](#getting-ready-2)
    * [How to do it](#how-to-do-it-2)
    * [How it works](#how-it-works-2)
  * [6.5 Calculating the shortest paths with the QGIS network analysis library](#65-calculating-the-shortest-paths-with-the-qgis-network-analysis-library)
    * [Getting ready](#getting-ready-3)
    * [How to do it](#how-to-do-it-3)
    * [How it works](#how-it-works-3)
    * [See also](#see-also-2)
  * [6.6 Routing point sequences](#66-routing-point-sequences)
    * [Getting ready](#getting-ready-4)
    * [How to do it](#how-to-do-it-4)
    * [How it works](#how-it-works-4)
    * [There's more](#theres-more-1)
    * [See also](#see-also-3)
  * [6.7 Automating multiple route computation using batch processing](#67-automating-multiple-route-computation-using-batch-processing)
    * [Getting ready](#getting-ready-5)
    * [How to do it](#how-to-do-it-5)
  * [6.8 Matching points to the nearest line](#68-matching-points-to-the-nearest-line)
    * [Getting ready](#getting-ready-6)
    * [How to do it](#how-to-do-it-6)
    * [How it works](#how-it-works-5)
  * [6.9 Creating a routing network for pgRouting](#69-creating-a-routing-network-for-pgrouting)
    * [Getting ready](#getting-ready-7)
    * [How to do it](#how-to-do-it-7)
    * [How it works](#how-it-works-6)
    * [See also](#see-also-4)
  * [6.10 Visualizing the pgRouting results in QGIS](#610-visualizing-the-pgrouting-results-in-qgis)
    * [Getting ready](#getting-ready-8)
    * [How to do it](#how-to-do-it-8)
    * [How it works](#how-it-works-7)
    * [See also](#see-also-5)
  * [6.11 Using the pgRoutingLayer plugin for convenience](#611-using-the-pgroutinglayer-plugin-for-convenience)
    * [Getting ready](#getting-ready-9)
    * [How to do it](#how-to-do-it-9)
    * [How it works](#how-it-works-8)
    * [See also](#see-also-6)
  * [6.12 Getting network data from the OSM](#612-getting-network-data-from-the-osm)
    * [Getting ready](#getting-ready-10)
    * [How to do it](#how-to-do-it-10)
    * [How it works](#how-it-works-9)

<!-- tocstop -->

In this chapter, we will cover the following recipes:
* Creating a simple routing network
* Calculating the shortest paths using the Road graph plugin
* Routing with one-way streets in the Road graph plugin
* Calculating the shortest paths with the QGIS network analysis library
* Routing point sequences
* Automating multiple route computation using batch processing
* Matching points to the nearest line
* Creating a network for pgRouting
* Visualizing the pgRouting results in QGIS
* Using the pgRoutingLayer plugin for convenience
* Getting network data from the OSM

## 6.1 Introduction

This chapter focuses on the common use cases that are related to routing within networks.
By far, the most common networks that are used to route are street networks.
Other less common cases include networks for indoor routing, that is, through rooms inside buildings, or networks of shipping routes.

Networks and routing are in no way a GIS-only topic.
You will find a lot of math literature related to this, called Graph Theory.
In this chapter, we will use the following terms to talk about networks:
* A network (also known as graph) is a collection of connected objects
* These objects are called nodes (also known as vertices)
* The connections between nodes are called links (also known as edges)

The following figure explains these terms:
The two routing tools that are commonly used with QGIS are as follows:
* The Road graph plugin, which is one of the QGIS core plugins; that is, this plugin is available in every QGIS installation, but you may have to activate it in Plugin Manager
* The PostGIS extension pgRouting, which can be used directly through the QGIS DB Manager, or more comfortably through the pgRoutingLayer plugin, which can be installed from the QGIS plugin repository using Plugin Manager


## 6.2 Creating a simple routing network

In this recipe, we will create a routing network from scratch using the QGIS editing tools.
Even though more and more open network data is available, there will still be numerous use cases where necessary network data does not exist or is not available for use.
Therefore, it is good to know how to create a network and what to pay attention to in order to avoid common pitfalls.

For the task of network creation, the main difference between the Road graph plugin and pgRouting is that pgRouting needs a network node (that is, link start or end node) at each intersection while the Road graph plugin will also use intermediate link geometry nodes   to infer intersections if two links share a node.
In this recipe, we will create a network, which can be used in both tools.


### Getting ready

### How to do it

Before we can start to create the network, there are a few things that need to be set up first:

1.  Create a new shapefile line layer for the network.
You don't need to add any extra attributes besides the default ID attribute yet.
> You can read more about creating new shapefiles in the Learning QGIS book by Packt Publishing and the QGIS user guide at http://docs.qgis.org/2.2/en/docs/user_manual/working_with_vector/editing_geometry_attributes.html#creating-a-new-shapefile-layer.
2.  To ensure that we can digitize the network with valid topology, we'll activate snapping next.
Go to Settings | Snapping Options and activate snapping for your line layer by enabling the checkbox to the left of it.
Additionally, set the mode to to vertex and choose a tolerance of at least 5.00000 pixels:
3.  Now, we can enable editing for the line layer and then select the Add Feature tool from the Digitizing toolbar to start digitizing.
4.  Create the first line feature now, and give it the ID number 1.
The line can have as many nodes as you wish.
We'll create a line with four nodes, as shown in the following screenshot:
5.  To draw the second line feature, start at the first or the last node of line 1.
As we have activated snapping, you will see that the node is being highlighted if you hover over it with the mouse cursor.
Draw a second line and give it the ID number 2.
> The line in the preceding screenshot is drawn with a style that has circles on the starting and ending points.
You can reproduce this style by adding the Marker line symbol levels to the line style or load network_links.
qml from our sample data.
For more details about styling features, please refer to Chapter 10, Cartography Tips.
6. Draw a few more lines (around 12 in total) forming a network.
Make sure to pay attention to the snapping and assign link IDs:
7. Disable editing, and confirm that you want to save the changes.
We will use this basic network as a starting point for the remaining recipes in this chapter.



### How it works

### There's more

You can validate the network topology by running the Topology Checker plugin, which is installed with QGIS by default (you can read more about Topology Checker in Chapter 12, Up and Coming):
1. Start Topology Checker from the Vector menu.
2. Click Configure to set up a topology rule, as shown in the following screenshot, and click on Add Rule to add it to the list of rules to check. Then, close the settings by clicking on OK:
3. Once this tool is configured, click on Validate All (the button with the checkmark) to initiate the check.
You will see the list of discovered errors displayed in the list above the buttons, as shown in the following screenshot.
Additionally, the dangling ends are highlighted in red in the map:
4. You can select the error entries in the list to jump to the line features that failed the check.
In our network, only lines with dead-ends should be listed.
If you see an error at an intersection, you should zoom closer and try to correct the node positions.


### See also

## 6.3 Calculating the shortest paths using the Road graph plugin

### Getting ready

### How to do it

The Road graph plugin enables us to route between two points that are selected on the map.
Before we can use this, we have to configure it first, as follows:
1. Enable the Shortest path panel by navigating to View | Panels.
This should add the plugin panel to the user interface.
2. Go to Vector | Road graph | Settings to get to the configuration dialog.
For now, the default settings, as shown in the following screenshot, should be fine.
Note that the network layer is selected as Transportation layer.
Click on OK to confirm the settings:
3. Once the settings are configured, we can calculate our first route.
Select the Start and Stop locations using the buttons marked with crosshair icons.
Activate the crosshair button and then click in the map to select a location.
This location will be marked on the map, and the coordinates will be automatically inserted into the Start or Stop input field.
4. Click on Calculate to initiate route computation.
Depending on the size of the network used, this step will either be very fast or it can take much more time.
The route will be highlighted in the map, and the route length and travel time will be displayed, as shown in the following screenshot:
5. If you want to store the computed shortest path, click on Export and you will be able to choose whether you want to create a new layer for the path or add the route to an existing line layer.
6. To compute a new route, simply change the start and stop locations and click on Calculate again.
7. Click on Clear to remove the route highlights when you are done.


### How it works

The Road graph plugin uses the QGIS network analysis library, which implements Dijkstra's algorithm.
For a given starting node, the algorithm finds the path with the lowest cost (that is, the shortest path if the cost criterion is length or the fastest path if the cost criterion is time) between this node and every other node in the network.
This can also be used to find costs of the shortest paths from a start node to a destination node by stopping the algorithm once the shortest path to the destination node has been determined.

In contrast to many simple routing tools, the Road graph plugin builds the network topology automatically.
As our network dataset is topologically sound (that is, there are no tiny gaps where network edges meet), we can set up Road graph plugin settings with Topology tolerance as 0.
If you are using a network from a different source, it may not have been created with the same attention to detail, and you may have to increase Topology tolerance to get routing to work.


### See also

* If you are interested in learning more about this algorithm, you can start at http://wiki.gis.com/wiki/index.php/Dijkstra's_algorithm


## 6.4 Routing with one-way streets in the Road graph plugin

### Getting ready

### How to do it

To demonstrate routing with one-way street information, we will first visualize the one-way values, and then we will configure the Road graph plugin to use the one-way information, as follows:
1.  Before we start routing with one-way information, it is helpful to visualize the one- way streets.
It is worth noting that the one-way direction will depend on the direction of the link geometry (that is, the direction the link was digitized in).
The best way to visualize the link direction is by assigning arrow symbols, as shown in the following screenshot.
You can load network_pgr.qml from our sample data to get the style: There are many different ways to encode one-way information.
In our dataset, a forward direction is encoded as FT for "from-to", a backward direction as TF for "to-from", and both ways as B for "both".

2.  Then, we can configure the Road graph plugin to use the one-way information.
To do this, we have to choose the dir attribute as Direction field and enter the values for forward (in link geometry) direction and reverse (against link geometry) direction:

3.  Once the plugin is configured, you can compute the shortest path as described in the previous recipe, Calculating the shortest paths using the Road graph plugin.
You will see how the resulting routes differ from the normal (without one-way restrictions) paths, as shown in the following screenshot where the algorithm avoids the one-way links on the direct route and takes the longer route instead:



### How it works

When we use the default two-way setting, each network link is interpreted as a connection from the start to end node, as well as a connection from the end to start node.
By adding one-way restrictions, this changes and the link is only interpreted as one connection now.

Besides FT, TF, and B, another common way to encode one-ways is 1 for in-link direction, -1 for against-link direction, and 0 for both ways.
In OpenStreetMap, you will find yes for the in-link direction, no for both ways and -1 for the against-link direction (refer to http://wiki.openstreetmap.org/wiki/Key:oneway for more details).


## 6.5 Calculating the shortest paths with the QGIS network analysis library

As mentioned in the recipe, Calculating the shortest paths using the Road graph plugin, QGIS comes with a network analysis library, which can be used from the Python console, inside plugins, to process scripts, and basically anything else that you can think of.
In this recipe, we will introduce the usage of the network analysis to compute the shortest paths in the Python console.


### Getting ready

### How to do it

Instead of typing or copying the following script directly in the Python console, we recommend opening the Python console editor using the Show editor button on the left-hand side of the Python console:
1.  Paste the following script into the editor:
```py
import processing
from processing.tools.vector
import VectorWriter
from PyQt4.QtCore import *
from qgis.core import *
from qgis.networkanalysis import *
# create the graph
layer = processing.getObject('network_pgr')
director = QgsLineVectorLayerDirector(layer,-1,'','','',3)
director.addProperter(QgsDistanceArcProperter())
builder = QgsGraphBuilder(layer.crs())
from_point = QgsPoint(2.73343,3.00581)
to_point = QgsPoint(0.483584,2.01487)
tied_points = director.makeGraph(builder,[from_point,to_point])
graph = builder.graph()
# compute the route from from_id to to_id
from_id = graph.findVertex(tied_points[0])
to_id = graph.findVertex(tied_points[1])
(tree,cost) = QgsGraphAnalyzer.dijkstra(graph,from_id,0)
# assemble the route route_points = [] curPos = to_id
while (curPos != from_id):
in_vertex = graph.arc(tree[curPos]).inVertex()
route_points.append(graph.vertex(in_vertex).point())
curPos = graph.arc(tree[curPos]).outVertex()
route_points.append(from_point)
# write the results to a Shapefile
result = 'C:\\temp\\route.shp'
writer = VectorWriter(result,None,[],2,layer.crs())
fet = QgsFeature()
fet.setGeometry(QgsGeometry.fromPolyline(route_points))
writer.addFeature(fet)
del writer
processing.load(result)
```

2.  If you are using your own network dataset instead of network_pgr.shp, which is provided with this book, adjust the coordinates of from_point and to_point for the route's starting and ending points.
3.  Change the file paths for the result layer depending on your operating system.
4.  Make sure that the network layer is loaded and selected in the QGIS layer list.
5.  Save the script and run it.



### How it works

On line 8, we created a QgsLineVectorLayerDirector object (http://qgis.org/api/classQgsLineVectorLayerDirector.html), which contains the network configuration.
The constructor (QgsLineVectorLayerDirector(layer,-1,'','','',3)) parameters are as follows:

* The network line layer
* The ID of the direction field: we set it to -1 because this script does not consider one-ways
* The following three parameters are the values for the in link direction: reverse link direction, and two-way
* The last parameter is the default direction: 1 for the in link direction, 2 for the reverse direction, and 3 for the two-way

Line 10 creates the QgsGraphBuilder (http://qgis.org/api/classQgsGraphBuilder.html) instance, which will be used to create the routing graph on line 14.

On lines 11 and 12, we defined the starting and ending points of our route.
To be able to route between these two points, they have to be matched to the nearest network link.
This happens on line 13 in the makeGraph() function, which returns the so-called tied_points.

The actual route computation takes place on line 18 in the QgsGraphAnalyzer.
dijkstra() (http://qgis.org/api/classQgsGraphAnalyzer.html) function.

The while loop, starting on line 22, is where the script moves through the tree created by Dijkstra's algorithm to collect all the vertices on the way and add them to the route_points list, which becomes the resulting route geometry on line 31.

The writer for output route line layer is created on line 29, where we pass the file path, None for default encoding, the [] for empty fields list, and the 2 for geometry type, which equals to lines as well as the resulting layer CRS.
The following lines, 30 to 32, create the route feature and add it to the writer.

Finally, the last line loads the resulting shapefile, and this is displayed on the map, as illustrated by the following screenshot:


### See also

You can read more about QGIS's network analysis library online in the PyQGIS Developer Cookbook at http://docs.qgis.org/testing/en/docs/pyqgis_developer_cookbook/network_analysis.html.



## 6.6 Routing point sequences

In the recipes so far, we routed from one starting point to one destination point.
Another use case is when we want to compute routes that connect a sequence of points, such as the points in a GPS track.
In this recipe, we will use the point layer to route processing script to compute a route for a point sequence.
At its core, this script uses the same idea that was introduced in the previous recipe, Calculating the shortest paths with the QGIS network analysis library, but this computes several shortest paths one after the other.


### Getting ready

To follow this recipe, load network_pgr.shp and sample_pts_for_routing.shp, which contains a point layer that should be routed from the sample dataset.
Additionally, you need to get the point layer to route script from https://raw.githubusercontent.com/anitagraser/QGIS-Processing-tools/master/2.6/scripts/point_layer_to_route.py and save it in the Processing script folder, which is set to C:\Users\youruser\qgis2\processing\scripts (on Windows), /home/ youruser/.qgis2/processing/scripts (on Linux), and /Users/youruser/.qgis2/ processing/scripts (on Mac) by default.
Alternatively, save the point layer to route to the folder configured in the Processing menu under Options | Scripts | Scripts folder.



### How to do it

To compute the route between the input points, you need to perform the following tasks:

1. Load the network and the point layer.
2. If you are using your own data, make sure that both layers are in the same CRS. If they are in different CRS, you need to reproject them (for example, using the Reproject layer tool from the Processing Toolbox option) before you continue.
3. Start the point layer to route tool from the Processing Toolbox option.
4. Pick the points and network input layers, make sure that Open output file after running algorithm option is activated, and click on Run to start the route computation. The resulting route layer will be loaded automatically:



### How it works

The point layer to route tool uses the QGIS network analysis library.
We already discussed the basic use of this library in the previous recipe, Calculating the shortest paths with the QGIS network analysis library.
The main difference is that we now have to handle more than two points.
Therefore, the script fetches all points from the input point layer and ties or matches them to the graph:
```py
points = []
features = processing.features(point_layer)
for f in features:
points.append(f.geometry().asPoint())
tiedPoints = director.makeGraph(builder, points)
```

For each pair of consecutive points, the script then computes the route between the two points just like we did in the Calculating the shortest paths with the QGIS network analysis library recipe:
```py
point_count = point_layer.featureCount()
for i in range(0,point_count-1):
# compute the route between two consecutive points
```

The resulting route line layer contains one line feature for each consecutive point pair.



### There's more

### See also

There is also a version of this script, which takes one-way information into account at https://raw.githubusercontent.com/anitagraser/QGIS-Processing-tools/master/2.2/scripts/point_layer_to_route_with_oneways.py.

## 6.7 Automating multiple route computation using batch processing

If you have multiple input point layers, you can use Processing's batch processing capabilities to speed up the process.
In this recipe, we will compute routes for two input point layers at once, but the same approach can be applied to many more layers.


### Getting ready

To follow this recipe, load network_pgr.shp and the two point layers, sample_pts_for_ routing.shp and sample_pts_for_routing2.shp.

### How to do it

To get started, right-click on the point layer to route tool in the Processing Toolbox option and select Execute as batch process. Then perform the following steps:
1. In the points column, click the … button and use the Select from open layers option to select sample_pts_for_routing and sample_pts_for_routing2.
2. Select and remove the third line using the Delete row button at the bottom of the dialog.
3. In the network column, click the … button and use the Select from open layers option to select network_pgr.
You can avoid having to pick the file twice by double- clicking on the table header entry (where it reads network).
This will autofill all rows with the same network file path.
4. In the routes column, you need to pick a path for the resulting route files.
In the Save file dialog, which opens when you click on the … button, you can specify one base filename and click on Save.
The following Autofill settings dialog lets you specify if and how you want to have the rows filled.
Use Autofill mode Fill with numbers and Processing will automatically append a running number to the filename that you specified.
You can see an example in the following screenshot where we specified route as the base filename.
5. Click on Run to execute the batch process.
Both routes will be computed and loaded automatically:


## 6.8 Matching points to the nearest line

In this recipe, we will use the QGIS network analysis library from Python console to match points to the nearest line.
This is the simplest form of what is also known as map matching.

### Getting ready

To follow this recipe, load network_pgr.shp from the sample data.

### How to do it

In this recipe, we will use the QGIS network analysis library from Python console to match points to the nearest line. This is the simplest form of what is also known as map matching.The following script will match three points, QgsPoint(3.63715,3.60401), QgsPoint(3.86250,1.58906), and QgsPoint(0.42913,2.26512), to the network:
1. Open Python console and its editor and then load or paste the following network_analysis_match_points.py script:
```py
import processing
from processing.tools.vector import VectorWriter from PyQt4.QtCore import *
from qgis.core import *
from qgis.networkanalysis import *
layer = processing.getObject('network_pgr')
director = QgsLineVectorLayerDirector(layer,-1,'','','',3)
director.addProperter(QgsDistanceArcProperter())
builder = QgsGraphBuilder(layer.crs())
additional_points = [QgsPoint(3.63715,3.60401),QgsPoint(3.86250,1.58906),QgsPoi nt(0.42913,2.26512)]
tied_points = director.makeGraph(builder,additional_points)
result = 'C:\\temp\\matched_pts.shp'
writer = VectorWriter(result,None,[],1,layer.crs())
fet = QgsFeature()
for pt in tied_points:
fet.setGeometry(QgsGeometry.fromPoint(pt))
writer.addFeature(fet)
del writer
processing.load(result)
```
2. Make sure that the network layer is selected in the layer list.
3. Run the script. The results should be loaded automatically.



### How it works

This script uses the QGIS network analysis library's ability to match points to lines using the makeGraph() function.
The resulting tied_points list contains the coordinates of the points on the network that are closest to the input points.
The 1 option on line 15 specifies that the output layer is of type point.
The for loop finally goes through all points in the tied_points list and creates point features, which are then added to the result writer.



## 6.9 Creating a routing network for pgRouting

This recipe shows you how to import a line layer into PostGIS and create a routable network out of it, which can be used by PostGIS's routing library, pgRouting. (For details about pgRouting, please visit the project website at http://docs.pgrouting.org.)

The installation of PostGIS with pgRouting won't be covered in detail here because instructions for the different operating systems can be found on the project's website at http://docs.pgrouting.org/2.0/en/doc/src/installation/index.html.

If you are using Windows, both PostGIS and pgRouting can be installed directly from the Stack Builder application, which is provided by the standard PostgreSQL installation, as described at http://anitagraser.com/2013/07/06/pgrouting-2-0-for-windows-quick-guide/.


### Getting ready

To follow this exercise, you need a PostGIS database with pgRouting enabled.
In QGIS, you should set up the connection to the database using the New button in the Add PostGIS Layers dialog.
Additionally, you should load network_pgr.shp from the sample data.


### How to do it

These steps will create a routable network table in your PostGIS database:

1. Open DB Manager by navigating to Database | DB Manager.
2. In Tree on the left-hand side of the dialog, select the database that you want to load the network to.
3. Go to Table | Import Layer/File to load the network_pgr layer into your database, as shown in the following screenshot:
4. After network_pgr has been imported successfully, open the SQL window of DB Manager by pressing F2, clicking on the corresponding toolbar button, or in the Database menu.
5. pgRouting is a little picky when it comes to column data types.
You will notice this when you see Error, columns 'source', 'target' must be of type int4, 'cost' must be of type  fl  t8.
When we import network_pgr with QGIS's DB Manager, it creates the cost column as numeric.
As pgRouting won't accept numeric, we will use Table | Edit Table in DB Manager to edit the cost column.
Click on the Edit column button and change Type from numeric to double precision (which equals the required flat8).

6. Now that the data is loaded and ready, we can build the network topology.
This will create a new network_pgr_vertices_pgr table, which contains the computed network nodes:
```sql
SELECT pgr_createTopology('network_pgr',0.001);
```

7. Once this topology is ready, we can test the network by calculating a simple shortest path from the node number 16 to the node number 9:
```sql
SELECT pgr_dijkstra('SELECT id, source, target, cost FROM network_pgr', 16, 9, false, false);
```
This will result in the following:
```
(0,16,6,1)
(1,17,7,1)
(2,5,8,1)
(3,6,9,1)
(4,11,15,1)
(5,9,-1,0)
```


### How it works

The preceding pgr_dijkstra query consists of the following parts:
* 'SELECT id, source, target, cost FROM network_pgr': This is a SQL query, which returns a set of rows with the following columns:
* id: This is the unique edge ID (type int4)
* source: This is the ID of the edge source node (type int4) f  target: This is the ID of the edge target node (type int4) f  cost: This is the cost of the edge traversal (type float8)
* 16, 9: These are the IDs of the route source and target nodes (type int4)
* false: This is true if the graph is directed
* false: If true, the reverse_cost column of the SQL-generated set of rows will be used for the cost of the traversal of the edge in the opposite direction

The results of pgr_dijkstra contain the list of network links that our route uses to get from the start to the destination.
The four values in reach result row are as follows:

* seq: This is the sequence number, which tells us the order of the links within the route starting from 0
* id1: This is the node ID
* id2: This is the edge ID
* cost: This is the cost of the link (can be distance, travel time, a monetary value, or any other measure that you chose)


### See also

In the following recipe, Visualizing pgRouting results in QGIS, we will see how to use the results of pgr_dijkstra to visualize the route on a map.
If you are interested in more pgRouting SQL recipes, you will find a whole chapter on this topic
in PostGIS Cookbook by Packt Publishing.



## 6.10 Visualizing the pgRouting results in QGIS

In the previous recipe, Creating a routing network for pgRouting, we imported a network layer, built the topology, and finally tested the routing.
Building on these results, this recipe will show you how to visualize the routing results on a map in QGIS.


### Getting ready

You should first go through the previous recipe, Creating a routing network for pgRouting, to set up the necessary PostGIS tables.
Alternatively, you can use your own network tables, but be aware that you may have to alter some of the SQL statements if your table uses different column names.


### How to do it

To visualize the results in QGIS, we can use the DB Manager SQL window, as shown in the following screenshot.
The extended query that we use here joins the routing results back to the original network table to get the route link geometries, which we want to display on the map:
1. Open DB Manager by navigating to Database | DB Manager.
2. In Tree to the left of the dialog, select the database that you want to load the network to.
3. Open the SQL window of DB Manager and configure it, as shown in the following screenshot:
> Note that there must not be a semicolon at the end of the SQL statement.
Otherwise, loading the results as a new layer will fail.
4. Make sure that the Geometry column is selected correctly and click on the Load now! button to load the query result as a new layer, as shown in the following screenshot:



### How it works

As pgr_dijkstra only returns a list with the IDs of the route edges, we need to get the edge geometries from the original network table in order to display the route on the map.
Therefore, we join the routing results with the network table on id2 (which contains the edge ID) and the network table's id column.


### See also

To make using pgRouting from within QGIS more convenient, the pgRoutingLayer plugin provides a GUI to access many of pgRouting's functions.
You will find an introduction to this plugin in the Using the pgRoutingLayer plugin for convenience recipe.



## 6.11 Using the pgRoutingLayer plugin for convenience

The previous recipe, Visualizing pgRouting results in QGIS, showed you how to manually add pgRouting results to the map.
In this chapter, we will use the pgRoutingLayer plugin to get more convenient access to the functions that pgRouting offers, including the most basic algorithms, such as Dijkstra's algorithm, which we have used so far, to more complex algorithms, such as drivingDistance and alphashape, which can be used to visualize catchment zones, also known as service areas.



### Getting ready

You should first go through the previous recipe, Creating a routing network for pgRouting, to set up the necessary PostGIS tables. Alternatively, you can use your own network tables, but be aware that you may have to alter some of the SQL statements if your table uses different column names.
Additionally, install the pgRoutingLayer plugin from Plugin Installer. You will need to enable
experimental plugins in Settings to view this.


### How to do it

The pgRoutingLayer plugin adds a new panel to the QGIS GUI, which allows convenient access to the available routing functions.
The following steps show you how to use this plugin:
1. First, you should select a database from the Database field that contains your routing network table. The drop-down list contains all the configured PostGIS connections.
2. Next, you can select a function from the Function field that you want to use.
Let's try Dijkstra's algorithm first; select the dijkstra function. You will recognize the parameters from the previous recipes where we wrote the pgRouting SQL  query manually.
3. Specify the parameters for the network table (edge_table) and the geometry, id, source, target, and cost columns, as shown in the following screenshot:
4. Now, you can use the green + buttons beside the source_id and target_id input
fields to select the source and target nodes in the map.
5. When everything is configured, you can click on the Run button to compute and display the route.
6. Next, you can switch functions and compute a service area. Select the alphashape
function. The rest of the input fields adapt automatically to the selected function.
7. Now, you can use the green + button right beside the source_id input field to select
the starting or center node of the service area.
8. Then, select the size of the service area by specifying the distance limit.
9. Finally, click on the Run button to compute and display the service area, as shown in the following screenshot:



### How it works

When we click on the Run button, the query results are visualized as a temporary overlay   on the map.
If you want to save the output permanently, you can click on the Export button.
Currently, the Export button is only available for the routing functions but not for the service area functions.


### See also

For a detailed documentation on the pgRouting algorithms, refer to the project documentation website at http://docs.pgrouting.org/2.0/en/doc/index.html.

## 6.12 Getting network data from the OSM

A popular data source for real-world routing applications is OpenStreetMap (OSM).
This recipe shows you how to prepare OSM data for usage with pgRouting using the osm2po command- line tool to convert OSM data to an insert script for PostGIS.
Finally, we will test the data  import using the pgRoutingLayer plugin.


### Getting ready

Download osm2po from http://osm2po.de and unpack the download.
Note that osm2po requires Java to be installed on your machine.

You also need a pgRouting-enabled database to follow this recipe.

Additionally, you should have the pgRoutingLayer plugin installed and enabled because we will use this to test the OSM data import.

You can use the wake.pbf OSM file from our sample data, or download your own data from services such as http://download.geofabrik.de.


### How to do it

Open the command line to perform the following steps.
If you are working on Windows, we recommend using the osgeo4W Shell:
1.  Go to the osm2po folder and open osm2po.config in a text editor.
Look for the following configuration line and remove the # at the beginning of the line to activate the pgRouting export: postp.0.class = de.cm.osm2po.plugins.postp.PgRoutingWriter
2.  Now use osm2po to convert the OSM .pbf file to SQL.
Adjust the file paths for your system, as follows:
```
D:\osm2po-5.1.0>java -jar osm2po-core-5.1.0-signed.jar prefix=wake "C:\tmp\OSM_NorthCarolina\wake.pbf"
```
3.  When osm2po is finished, you should see the following:
INFO Services started. Waiting for requests at http://localhost:8888/Osm2poService
4.  You should now find a folder with the name of the prefix (that is, wake) inside the osm2po folder.
This contains a log file, which in turn provides a command-line template to import the OSM network to PostGIS:
INFO commandline template:
```
psql -U [username] -d [dbname] -q -f "D:\osm2po-5.1.0\wake\wake_2po_4pgr.sql"
```
5.  Using this template, we can easily import the .sql file into an existing database, as follows:
```
D:\osm2po-5.1.0\wake>psql -U [username] -d cookbook -q -f
D:\osm2po-5.1.0\wake\wake_2po_4pgr.sql
```
6.  Now, the data is ready for use in QGIS.
When we connect to the cookbook database, we can see the wake_2po_4pgr table:
7.  Finally, we can use the pgRouting Layer plugin to test the OSM data import by calculating a service area of 0.1 hours (the distance value) around the 43679 (source_id) source node number:


### How it works

The network table created by osm2po contains, among others, the following useful columns:
* km: This is the length of the network edge
* kmh: The is the speed on the edge, depending on the street class and the values specified in the osm2po configuration
* cost: This is the travel time computed using km/kmh

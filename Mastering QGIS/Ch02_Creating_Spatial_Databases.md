
# Chapter 2: Creating Spatial Databases

* Core concepts of database construction  
* Creating spatial databases  
* Importing and exporting data  
* Editing databases  
* Creating queries  
* Creating views  

## 2.1 Fundamental database concepts

A database is a structured collection of data.  
__Databases provide multiple benefis over data stored in a flt fie format, such as shapefie or KML.__  
The benefis include complex queries, complex relationships, scalability, security, data integrity, and transactions, to name a few.  
__Using databases to store geospatial data is relatively easy, considering the aforementioned benefis.__  
  


### 2.1.1 Database tables

A relational database stores data in tables.  
A table is composed of rows and columns, where each row is a single data record and each column stores a fild value associated with each record.  
A table can have any number of records; however, each fild is uniquely named and stores a specifi type of data.  
  
A data type restricts the information that can be stored in a fild, and it is very important that an appropriate data type, and its associated parameters, be selected for each fild in a table.  
__The common data types are as follows:__  
* Integer  
* Float/Real/Decimal  
* Text  
* Date  
  
Each of these data types can have additional constraints set, such as setting a default value, restricting the fild size, or prohibiting null values.  
__In addition to the common data types mentioned previously, some databases support the geometry fild type, allowing the following geometry types to be stored:__  
* Point  
* Multi-point  
* Line  
* Multi-line  
* Polygon  
* Multi-polygon  
  
__The multi-point/line/polygon types store multi-part geometries so that one record has multiple geometry parts associated with it.__  
  


### 2.1.2 Table relationships

A table relationship connects records between tables.  
The benefi of relating tables is reducing data redundancy and increasing data integrity.  
In order to relate two tables together, each table must contain an indexed key field.  
  
A field can be defied as an index.  
A field set as an index must only contain values that are unique for each record, and therefore, it can be used to identify each record in a table uniquely.  
An index is useful for two reasons.  
Firstly, it allows records to be quickly found during a query if the indexed field is part of the query.  
Secondly, an index can be set to be a primary key for a table, allowing for table relationships to be built.  
  
A primary key is one or more fields that uniquely identify a record in its own table.  
A foreign key is one or more fields that uniquely identify a record in another table.  
When a relationship is created, a record(s) from one table is linked to a record(s) of another table.  
With related tables, more complex queries can be executed and redundancy in the database can be reduced.  


### 2.1.3 Structured Query Language

Structured Query Language (SQL) is a language designed to manage databases and the data contained within them.  
Covering SQL is a large undertaking and is outside the scope of this book, so we will only cover a quick refresher that is relevant to this chapter.  
  
SQL provides functions to select, insert, delete, and update data.  
Four commonly used SQL data functions are discussed as follows:   


```
* SELECT: This retrieves a temporary set of data from one or more tables based on an expression. A basic query is SELECT <field(s)> FROM <table> WHERE <field> <operator> <value>;  where <field> is the name of the field from which values must be retrieved and <table> is the table on which the query must be executed. The <operator> part checks for equality (such as =, >=, LIKE) and <value> is the value to compare against the field.  

* INSERT: This inserts new records into a table. The INSERT INTO <table> (<field1>, <field2>, <field3>) VALUES (<value1>, <value2>, <value3>);  statement inserts three values into their three respective fields, where <value1>, <value2>, and <value3> are stored in <field1>, <field2>, and <field3> of <table>.  

* UPDATE: This modifies an existing record in a table. The UPDATE <table> SET <field> = <value>;  statement updates one field's value, where <value> is stored in <field> of <table>.  

* DELETE: This deletes record(s) from a table. The following statements deletes all records matching the WHERE clause: DELETE FROM <table> WHERE <field> <operator> <value>;  where <table> is the table to delete records from, <field> is the name of the field, <operator> checks for equality, and <value> is the value to check against the field.  
```

Another SQL function of interest is view.  
A view is a stored query that is presented as a table but is actually built dynamically when the view is accessed.  
To create a view, simply preface a SELECT statement with CREATE VIEW <view_name> AS and a view named <view_name> will be created.  
You can then treat the new view as if it were a table.  
  


## 2.2 Creating a spatial database

Creating a spatial database in QGIS is a simple operation.  
QGIS supports __PostGIS, SpatiaLite, MSSQL, SQL Anywhere, and Oracle Spatial databases.__  
We will cover SpatiaLite, an open source project that is cross-platform, simple, and lightweight, and provides quite a bit of functionality.  
__SpatiaLite is a spatial database management system (DBMS) built on top of SQLite, a lightweight personal DBMS.__  
  


> SpatiaLite (and thus, SQLite) is built on a personal architecture, which makes installation and management virtually nonexistent.  
The trade-off, however, is that it neither does a good job of supporting multiple concurrent connections nor does it support a client-server architecture.  
For a more complex DBMS, PostGIS is an excellent open source option.  
  


We will create a new SpatiaLite database that we will use for the remaining exercises in this chapter; to do this, perform the following steps:   
  
1. Open QGIS Desktop and open the Browser panel. If the Browser panel is missing, click on Browser by navigating to View | Panels. In the Browser panel, you will fid the SpatiaLite entry below your hard drive folders.  
  
2. Create a new SpatiaLite database by right-clicking on SpatiaLite and then choose Create database… (as shown in the following screenshot).  
  
3. Create a new folder on disk and save the new database as GiffordPinchot. sqlite. The newly created database will appear under the SpatiaLite database entry.  
  
  


![](Chapter02/fig2_2.PNG)

Now that we have a new SpatiaLite database, let's look at its initial structure and contents.  
To do this, we will use DB Manager, a built-in QGIS plugin.  
DB Manager provides a simple graphical interface to manage PostGIS and SpatiaLite databases.  
Using DB Manager, we will be able to view and manage our SpatiaLite database.  
Let's start by getting familiar with the DB Manager interface.  
  


1. Click on __DB Manager__ under __Database__ to open the DB Manager. The DB Manager interface (as shown in the following screenshot) is composed of four parts: menu bar, toolbar, tree view, and information panel.   
2. Navigate to __SpatiaLite | GiffordPinochet.sqlite__ to see a tree listing of all tables, views, and general information about the database, as shown in in the following screenshot:  


![](Chapter02/fig2_2-2.PNG)

When a new SpatiaLite database is created, it is automatically populated with multiple tables and views.  
These tables and views hold records used by the DBMS to manage the structure and operation of the database.  
__You should not modify or delete these tables or views unless you are absolutely sure of what you are doing.__  
  


## 2.3 Importing data into a SpatiaLite database

Importing data into a SpatiaLite database is easy using the DB Manager. SpatiaLite supports the following formats for importing fies:  
* Shapefile (.shp)  
* Dbase (.dbf)  
* Text (.txt), Commas Separate Values (.csv), and Excel spreadsheets (.xls)  
* Well-known Text (.wkt) and Well-known Binary (.wkb)  
* PostGIS (.ewkt / .ewkb)  
* Geography Markup Language (.gml)  
* Keyhole Markup Language (.kml)  
* Geometry JavaScript Object Notation (.geojson)  
* Scalable Vector Graphics (.svg)  
Let's use DB Manager to import data in a few different formats into our GiffordPinochet.sqlite database.  


### 2.3.1 Importing KML into SpatiaLite

To import a KML file into a SpatiaLite database, complete the following steps:  
1. Open DB Manager by clicking on DB Manager under Database. Expand SpatiaLite and select GiffordPinochet.sqlite on the Tree panel.  
2. Navigate to __Table | Import layer/file__ to open the Import vector layer dialog.  
3. Click on the ellipsis button at the right-hand side of the Input drop-down box and select and open streams.kml from the sample dataset that is available for download on the Packt Publishing website.  
4. Click on the Update options button to load the remainder of the dialog box. The output table name will populate as streams, and it will match the base name of the input file.  
5. Set the following options as shown in the next screenshot:  
    * Select Source SRID and enter 4326. This is the EPSG code for all KML datasets.  
    * Select Target SRID and enter 26910. This is the EPSG code for NAD 83/UTM Zone 10 North.  
    * Select Create spatial index.  
6. Refer the following screenshot to make sure your settings match. If so, click on the OK button to import the file.    
7. After a few moments, you will be notifid that the import is complete. To view the newly created table, you'll need to refresh the Tree panel by selecting GiffordPinochet.sqlite in the tree and then click on Refresh under Database, or press the F5 key on your keyboard. The streams table should now appear and have the polyline icon next to it.  
8. To preview the attribute table, click on the Table tab on the information panel. To preview the geometry, click on the Preview tab on the information panel. To view the newly created SpatiaLite layer in QGIS Desktop, right-click on streams on the Tree panel, and then choose Add to canvas.  


![](Chapter02/fig2_3_1.PNG)

### 2.3.2 Importing a shapefile into SpatiaLite

1. Open DB Manager by clicking on DB Manager under Database. Expand SpatiaLite and select GiffordPinochet.sqlite on the Tree panel.  
2. Navigate to __Table | Import layer/file__ to open the Import vector layer dialog, as shown in the following screenshot.  
3. Click on the ellipsis button at the right-hand side of the Input drop-down box and select and open NF_roads.shp from the sample dataset that is available for download on the Packt Publishing website.  
4. Click on the Update options button to load the remainder of the dialog box. The output table name will populate as NF_roads, and it will match the base name of the input file.  
5. Set the following options:  
    * Select Source SRID and enter 26910. This is the EPSG code for NAD 83/UTM Zone 10 North. Since we don't want to change the coordinate system during import, we do not need to set Target SRID.  
    * Select Create spatial index.  
6. Click on the OK button to import the file.  
7. After a few moments, you will be notifid that the import is complete. To view the newly created table, you'll need to refresh the Tree panel by selecting GiffordPinochet.sqlite in the tree, and then click on Refresh under Database, or press the F5 key on your keyboard. The NF_roads table should now appear and have the polyline icon next to it.  
8. To preview the attribute table, click on the Table tab on the information panel. To preview the geometry, click on the Preview tab on the information panel. To view the newly created SpatiaLite layer in QGIS Desktop, right-click on NF_roads in the tree, and then choose Add to canvas.  


![](Chapter02/fig2_3_2.PNG)


```python

```

### 2.3.3 Importing tables into SpatiaLite

To import a table file into a SpatiaLite database, complete the following steps:  
1. Open DB Manager by clicking on DB Manager under Database. Expand SpatiaLite and select GiffordPinochet.sqlite on the Tree panel.  
2. Navigate to Table | Import layer/file to open the Import vector layer dialog.  
3. Click on the ellipsis button to the right-hand side of the Input drop-down box and select and open Waterfalls.xls from the sample dataset that is available for download on the Packt Publishing website.  
4. Click on the Update options button to load the remainder of the dialog box. The output table name will populate as Waterfalls, and it match the base name of the input file. Note that all options related to spatial datasets are not modifible and are grayed out (as shown in in the following screenshot). This is because SpatiaLite treats the input as a nonspatial table, even though it has coordinates stored in the table. We will add the spatial component to the table in a later step.  
5. Click on the OK button to import the file.  
![](Chapter02/fig2_3_3-1.PNG)  
6. After a few moments, you will be notifid that the import is complete. To view the newly created table, you'll need to refresh the Tree panel by selecting GiffordPinochet.sqlite in the tree and then click on Refresh under Database, or press the F5 key on your keyboard. The Waterfalls table should now appear and have the table icon next to it.  
7. Select the Waterfalls table. Click on the Info tab on the information panel. Note the Northing and Easting filds. These filds contain the coordinates of the waterfalls in NAD 83/UTM Zone 10 North (EPSG 26910). Click on the Table tab on the information panel to view the entries in the table. Note that the Preview tab is not selectable, because the selected table does not have any geometry fild.  
  
At this point, the table import is complete. However, since the Waterfalls table has coordinate pairs, a point geometry column can be added to the table that would essentially convert the table to a point layer. Let's do this now:  
1. With the Waterfalls table selected in the Tree panel, navigate to Table | Edit Table to open the Table properties window.  
2. Click on the Add geometry column button. In the new window, set the following options to match the following screenshot and then click on OK to create the geometry fild:  
  * __Name__: geom (the name of the fileld that will contain the geometry information)  
  * __Type__: POINT (the type of geometry the fileld will hold)  
  * __Dimensions__: 2 (the number of dimensions (values) the geometry fileld will hold for each record)  
  * __SRID__: 26910 (the coordinate system of the geometry fileld)  
![](Chapter02/fig2_3_3-2.PNG)   
3. Close the table properties. To view the newly edited table, you'll need to refresh the Tree panel by selecting GiffordPinochet.sqlite in the tree and then clicking on Refresh under Database, or press the F5 key on your keyboard. The Waterfalls table should now appear and have the point icon next to it.  
  
Now that the Waterfalls table has a geometry fild, we need to populate it with the coordinates.  
We will accomplish this by writing a SQL update query and using the SpatiaLite MakePoint function.  
To do this, perform the following steps:  
  
1. In the SQL window, click on the Clear button to clear the SQL query text area.  
2. Enter the following query in the SQL query text area: 
```        
UPDATE Waterfalls 
SET geom = MakePoint(Easting,Northing,26910) ;  
```
>Let's discuss the MakePoint function.  
__MakePoint(Easting,Northing,26910)__ is a SpatiaLite function that creates a new point geometry object.  
Easting and Northing are the columns in the same row that hold the values for the x and y coordinates respectively.  
26910 is the SRID of the x and y coordinates.  

3. Click on the Execute (F5) button to execute the query. The query will return no result but will indicate that 100 rows were affected. This indicates that the geometry fild of 100 rows have been populated with point geometry. The following screenshot shows the query and the indication that 100 rows were affected:  
![](Chapter02/fig2_3_3-3.PNG)  
4. On the SQL window, click on the Close button to close the window.  
5. To view the changes made to the Waterfalls table, you'll need to refresh the Tree panel by selecting GiffordPinochet.sqlite in the tree and then clicking on Refresh under Database, or press the F5 key on your keyboard.  
6. Note that the Waterfalls table now has the point icon next to it. Click on the Info tab on the information panel. Under the SpatiaLite section of the information printout, note that a warning is displayed stating that no spatial index has been defiled (shown in following fiure). To improve access speed, it is best that a spatial index be set. Click on create it and then click on the Yes button on the pop up.  
![](Chapter02/fig2_3_3-4.PNG)  
7. To preview the attribute table, click on the Table tab on the information panel. To preview the geometry, click on the Preview tab on the information panel. To view the newly created SpatiaLite layer in QGIS Desktop, right click on NF_roads in the tree and then choose Add to canvas.  


## 2.4 Exporting tables out of SpatiaLite as a shapefile

To export a table as a shapefie, perform the following steps:  
1. Open DB Manager by clicking on DB Manager under Database. Expand SpatiaLite and select the database from which you wish to export a table in the Tree panel.  
2. In the Tree panel, select the table that you wish to export.  
3. Navigate to Table | Export to fie to open the Export vector fie dialog.  
4. Click on the ellipsis button at the right-hand side of the Output fie text box and name the output fie. Note that you can only export to the shapefie format using this tool.  
5. Set the Source SRID, Target SRID, and Encoding options or leave them unselected to use the default values. Select Drop existing one if you wish to overwrite an existing shapefie.  
  
The following screenshot shows the Export to vector fie dialog ready to export to waterfalls.shp.  


![](Chapter02/fig2_4.PNG) 

## 2.5 Managing tables

DB Manager provides functions to create, rename, edit, delete, and empty tables using tools found under the Table menu.  
In this section, we will discuss each tool.  

### 2.5.1 Creating a new table
### 2.5.2 Renaming a table
### 2.5.3 Editing table properties


```python

```

### 2.5.4 Deleting a table
### 2.5.5 Emptying a table


```python

```

## 2.6 Creating queries and views

### 2.6.1 Creating a SQL query
### 2.6.2 Creating a spatial view
### 2.6.3 Dropping a spatial view
## 2.7 Summary

This chapter provided you with the steps to handle databases in QGIS.  
While QGIS can handle multiple databases, we used SpatiaLite as it provides a good amount of functionality with little overhead or administration.  
  
Using the DB Manager you can perform a number of operations on databases.  
Operations of note are: creating indices, spatial and aspatial views, importing and exporting, and performing queries.  
From the introduction to DB Manager and SpatiaLite in this chapter, you are now well-equipped to write more complex queries that take full advantage of the SQL commands and SpatiaLite SQL extension commands.  
A full listing of SQLite SQL commands are available at http://www.sqlite.org/lang.html.  
A full listing of the SpatialLite SQL extension commands are available at http://www.gaia-gis.it/gaia-sins/spatialite-sql-4.2.0.html.  
  
The next chapter moves us from the storage of geospatial data to the display of geospatial data.  
The styling capabilities of QGIS will be covered for both vector and raster fies.  
Additionally, the rendering options that were fist introduced in QGIS 2.6 will be covered.  
  



```python

```

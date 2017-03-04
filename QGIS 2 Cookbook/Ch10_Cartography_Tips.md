
# Chapter 10: Cartography Tips
<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [Chapter 10: Cartography Tips](#chapter-10-cartography-tips)
  * [10.1 Introduction](#101-introduction)
  * [10.2 Using Rule Based Rendering](#102-using-rule-based-rendering)
    * [Getting ready](#getting-ready)
    * [How to do it](#how-to-do-it)
    * [How it works](#how-it-works)
    * [There's more](#theres-more)
  * [10.3 Handling transparencies](#103-handling-transparencies)
    * [Getting ready](#getting-ready-1)
    * [How to do it](#how-to-do-it-1)
    * [How it works](#how-it-works-1)
    * [There's more](#theres-more-1)
  * [10.4 Understanding the feature and layer blending modes](#104-understanding-the-feature-and-layer-blending-modes)
    * [Getting ready](#getting-ready-2)
    * [How to do it](#how-to-do-it-2)
    * [How it works](#how-it-works-2)
  * [10.5 Saving and loading styles](#105-saving-and-loading-styles)
    * [Getting ready](#getting-ready-3)
    * [How to do it](#how-to-do-it-3)
    * [How it works](#how-it-works-3)
    * [There's more](#theres-more-2)
  * [10.6 Configuring data-defined labels](#106-configuring-data-defined-labels)
    * [Getting ready](#getting-ready-4)
    * [How to do it](#how-to-do-it-4)
    * [How it works](#how-it-works-4)
    * [There's more](#theres-more-3)
  * [10.7 Creating custom SVG graphics](#107-creating-custom-svg-graphics)
    * [Getting ready](#getting-ready-5)
    * [How to do it](#how-to-do-it-5)
    * [How it works](#how-it-works-5)
    * [There's more](#theres-more-4)
    * [See also](#see-also)
  * [10.8 Making pretty graticules in any projection](#108-making-pretty-graticules-in-any-projection)
    * [Getting ready](#getting-ready-6)
    * [How to do it](#how-to-do-it-6)
    * [How it works](#how-it-works-6)
    * [There's more](#theres-more-5)
    * [See also](#see-also-1)
  * [10.9 Making useful graticules in printed maps](#109-making-useful-graticules-in-printed-maps)
    * [Getting ready](#getting-ready-7)
    * [How to do it](#how-to-do-it-7)
    * [How it works](#how-it-works-7)
    * [There's more](#theres-more-6)
  * [10.10 Creating a map series using Atlas](#1010-creating-a-map-series-using-atlas)
    * [Getting ready](#getting-ready-8)
    * [How to do it](#how-to-do-it-8)
    * [How it works](#how-it-works-8)
    * [There's more](#theres-more-7)

<!-- tocstop -->

In this chapter, we will cover the following recipes:

* Using Rule Based Rendering
* Handling transparencies
* Understanding the feature and layer blending modes
* Saving and loading styles
* Configuring data-defined labels
* Creating custom SVG graphics
* Making pretty graticules in any projection f Making useful graticules in printed maps f Creating a map series using Atlas


## 10.1 Introduction

Cartography has changed quite a bit in the past decade as more people transition to purely electronic map products on a device or on the Web.
While some types of visualizations are better suited to different media, many of the underlying tools and techniques can actually be applied across the board.
This chapter covers a variety of tools that enable you, the QGIS user, to maximize the readability and beauty of your maps.


## 10.2 Using Rule Based Rendering

In the past, if you wanted to apply a wildly different style to more than one type of data in  the same source, the only way to do this was to duplicate or subset a layer.
With Rule Based Rendering, you now just have to create rules that are applied on-the-fly.
This opens a huge door on cartographic possibilities with different features in the same layer not only having different colors but also different fill types, transparency, line type, and all manner of other customizations.
Extending from categorized symbology, rules also allow for mixing and inheritance, allowing for intermediate categories or some shared properties and reducing the amount of work to create elegant symbology.


### Getting ready

Rule Based Rendering is built-in to vector symbology.
So, you'll need a good complicated vector layer to fully utilize its potential.
A road layer is often a good use case, but for this example we'll go slightly simpler with busroutesall.shp.


### How to do it

1. Load the busroutesall.shp layer.

2. Right-click on the layer name in the Layers window, select Properties, then pick Style on the left-hand side of the new window.

3. Change the symbology drop-down type to Rule-Based.
4. Pick the attributes that you want to use to differentiate between groups of features:
1. In this case, let's edit the initial rule (double-click on the rule or the Edit icon between + (add) and - (remove).
2. Rules can be based on attribute table values or geometry properties, including on-the-fly calculated values.
First let's style routes shorter than 2,000 map units apply here.
In the Filter box type $length < 2000 (Do you want to see all the options? Then, open the filter tool with the … button).
Name your rule and click on OK.
Back in the main Style dialog, apply the rule to see the results in Canvas. Make sure to use the Test button to verify that your rule works:
> You can apply more than one rule to objects, the rendering being a combination of the rules and the rendering order.

5. Now to make it more interesting, let's add another rule that's the inverse:
1. Add a new rule with the green + button below the rule list.
2. For the filter, use $length > 2000 (don't forget to test this).
3. Pick some other symbology that differs quite a bit so that it's easy to tell them apart (such as a different line type).
Click on OK and then click on Apply to see to the two rules in action.

6. Now, things get really interesting.
Let's add a subrule by either right-clicking on a rule or by highlighting a rule and clicking on the Refine current rules dropdown:

7. Select Add categories to rule:
1. In the subdialog, select Route.
2. Pick a color ramp and/or line style, click on Classify, and then click on OK:
3. Before you click on Apply, edit the main rule and uncheck the Symbol box (otherwise, the Rule and Sub Rules list will be additive, which can be useful in some cases).

8. Now, when you look at the Rule list, you will see subrules under their parents.

9. Finally, let's add a third top-level rule that is not based on the length:
1. Make a rule filter on the ROUTE name that contains a.
The rule will look like:  "Route" LIKE '%a'.
2. Pick a line symbol that will make these routes stick out even with their current coloring and click on Apply:

10. Play around some more; there are all sorts of things you can do, from partial string matching to splitting by even or odd numbers ```("ROUTE" % 2 = 0 is even-numbered)```.




### How it works

Each rule is processed in the rendering order specified from top to bottom, the last rule being drawn last and, therefore, on top.
The rules are added to any existing style that is already applied to feature.
You can change the rendering order by changing the rule order or by applying a render order.
The filters work just like attribute filters in the field calculator or the table search.
All of the symbology options are available to vectors and can be applied to one or many rules.
You can group rules by scale-rendering rules too.


### There's more


There are way too many possible ways to use Rule Based Rendering than can be described here.
You can create rendering groups that inherit rules from their parent and apply their own.
Each feature given a unique ID could have a completely different look.
The big improvement over using traditional single symbol, categorized, or graduated symbology is that you don't have to edit every possible group, and you can more easily stack rules, mixing and matching all the original methods.
There are some catches.
Not everything you do with Rule Based Rendering is possible with web services; so, before you go too crazy, consider your output format and test your ideas before spending too much time on this.

## 10.3 Handling transparencies

Transparency is a lack of pigment or color, such that you can see through one feature to the feature beneath.
You can think of this as being similar to tinted or stained glass; some light is allowed to pass through and reflect off what's inside.
When used right, transparency can help emphasize or de-emphasize features in a map composition.
It can also be used to blend two layers to look as if they are one layer.



### Getting ready

This recipe demonstrates transparency for both vectors and rasters, so we'll need an example of each.
The lakes.shp and elevlid_D782_6.tif layers will work well for demonstration purposes.
Load both of these layers in a fresh project, putting lake.shp on top.

### How to do it

1. With a vector layer loaded, open Properties and the Style tab.
2. On the right-hand side of the dialog, you will see a Transparency slider at 0% (this means 100% solid or opaque).
3. Adjust the slider to the right and apply the changes to see them in the map:
> Using a bold or dark color will make it easier to notice the change.
You will also notice that the Simple fill option shows the original color.

4. Now to demonstrate this on a raster, first reset the lakes back to 0%.
5. Swap the order in the Layers list so that elevlid is on top.
6. Now, open Properties of elevid and the Transparency tab.
7. The Global transparency option will change the value evenly for the whole raster.
Set it to 50% and apply it.
You should now be able to see the lakes layer, which was hidden below:
>No data value is always 100% transparent no matter what Global transparency is set to.
Use this to easily eliminate values that you don't want to show up at all.

8. (Optional) You may have noticed that below the Global transparency slider is Custom transparency options.
This will allow you to make particular values more transparent than others.
You can either assign specific values to specific transparencies, or you can add a band to the raster (or use a multiband raster), which specifies the amount of transparency to apply to the rest of the raster (some data formats, such as GeoTiff, call this an Alpha Transparency band):
* Reset the global back to 0% (otherwise, this is applied in too)
* Use the green + sign to add some values
* From 100 To 125, 25% transparent
* From 125 To 150, 75% transparent
* Click Apply and notice the lower elevation lakes are harder to see:



### How it works

This is really a computer graphics thing, but the simplest explanation is that you're telling the computer to combine a percentage of two different layers in the same location instead of the top layer's value covering.
Based on the math of the original colors and their transparency, a blended color is calculated for each pixel on the screen.

This doesn't begin to explain all the possible variations of appearance that can be achieved by mixing multiple layers and multiple transparencies, only tinkering can show you this.

### There's more


One classic example of transparencies is to mix hillshades and airphotos.
You can place either layer on top and then adjust the transparency to let the other show through.
Generally, you would place the hillshade underneath in this case (but either can work).
The end result is a landscape that appears to have 3D relief, but it looks like an airphoto.

Another classic example is to create a mask layer with a hole cut out around the region that you want to emphasize.
You now place the mask layer on top.
Before adding transparency, it blocks everything but the hole.
Then, you slowly add transparency so that you can see surrounding regions, but they are muted and stand out less.
For this technique, try a black, gray, or white fill for the mask layer.
Each will have a slightly different look.

When styling vectors, you can apply different transparencies to different features in the same layer if you use Rule Based Rendering.
Each rule can have a different transparency value and the entire layer can have yet another transparency modifier in the Layer Rendering section.

Lastly, keep in mind that not all output formats handle transparency well.
In particular, be careful using color gradients with transparencies when exporting to PDF.
Generally, PNG handles transparency, SVG may work or at least allow to you to edit the transparency after export, unlike image formats.

## 10.4 Understanding the feature and layer blending modes

In this recipe, we will look at the different layer and feature blending modes.
Using these tools, we can achieve special rendering effects, which you may already know from other graphics programs.



### Getting ready

To follow this recipe, you just need to load stamen.png and effect.png from our sample data.
Make sure that stamen (left-hand side in the following screenshot) is the lower layer and effect (right-hand side in the following screenshot) is the upper layer.
To test the feature blending modes, load blending.shp:

### How to do it

Using the two raster layers, we can try the different blending modes.
Of course, this works for vector layers, as well:
1. Double-click on the effects layer to open Layer Properties.
2. You can find the blending settings by going to Layer Properties | Style | Color Rending together with other helpful controls for Brightness, Contrast, Saturation, and more, as shown in the next screenshot:
3. Change the Blending mode and click on Apply to see the results.
4. Similarly, for vector layers, such as our blending layer, we find the blending mode settings by going to Layer Properties | Style | Layer rendering, as shown in the following screenshot:

The main difference is that, for vector layers, we can control how features are blended together, and how the result is then blended to the underlying layers using the Feature blending and Layer blending modes, respectively.
The feature blending mode is applied on a per-feature-basis.

The following screenshot shows the differences between feature and layer blending:

Feature and/or layer blending in action (from left to right): feature blending only, layer blending only, feature and layer blending combined (background maps "Watercolor" and "Toner" by Stamen Design, under CC BY 3.0.
Data by OpenStreetMap, under CC BY SA).

The following is an explanation of the preceding screenshots:

* The leftmost image shows that Feature blending mode is set to Multiply, while Layer blending mode is set to default, Normal.
This results in a map where the vector features are rendered on top of each other using the Multiply mode before the whole layer is overlaid on top of the lower layer(s).
* The center image instead shows Normal Feature blending mode combined with Multiply Layer blending mode.
You can see how the features can block each other out because they are drawn on top of each other.
* Finally, the rightmost image shows both Layer blending mode and Feature blending modes being set to Multiply.
In this combination, the Multiply rule is applied on both the feature and the layer level and, therefore, we can see features and the underlying background layer(s) shining through the features in the upper layer.



### How it works

Based on the selected blending mode, the pixel colors (in the RGB mode) of the lower and upper layers are mixed as described next.
For illustration and quick reference, the following figure shows the results of all 12 blending modes (from left to right and top to bottom), except for the Normal setting, which does not mix the colors but only uses the alpha channel of the upper layer to blend with the layer below it:
* __Lighten:__ The Lighten mode selects the maximum of each RGB component from the foreground and background pixels.
Be aware that the results tend to be jagged and harsh.
* __Screen:__ The Screen mode paints light pixels from the upper layer over the lower layer, but it skips the dark pixels.
* __Dodge:__ The Dodge mode will brighten and saturate the lower layer based on the lightness of the upper layer.
This means that brighter colors in the upper layer cause the saturation and brightness of the lower layer to increase.
This works best if the top pixels aren't too bright; otherwise, the effect is quite extreme.
* __Addition:__ The Addition mode adds the pixel values of both layers.
If the result exceeds 1 (in the case of RGB), the respective areas are displayed in white.
* __Darken:__ The Darken mode creates a result that retains the smallest RGB components of both layers.
Therefore, this is the opposite of the Lighten mode and, just as with Lighten, the results tend to be jagged and harsh.
* __Multiply:__ The Multiply mode multiplies the values of both layers, thus resulting in a darker picture.
* __Burn:__ The Burn mode causes darker colors in the upper layer to darken the lower layer.
Burn can be used to tweak and colorize underlying layers.
* __Overlay:__ The Overlay mode combines the Multiply and Screen blending modes.
As a result, light parts become lighter and dark parts become darker.
* __Soft __light: The Soft light mode is very similar to Overlay, but it uses a combination of Burn and Dodge.
This is supposed to emulate shining a soft light on an image.
* __Hard __light: The Hard light mode is also very similar to Overlay.
It is supposed to emulate projecting a very intense light on an image.
* __Difference:__ The Difference mode subtracts the values of the upper layer from the lower layer (or the other way around) to always get a positive value.
Blending with black (which has an RGB value of 0,0,0) produces no change.
* __Subtract:__ The Subtract mode subtracts the values of one layer from the other.
In the case of negative values, black is displayed:






## 10.5 Saving and loading styles

What's better than making an awesome style for your feature layers? Being able to easily share and reuse them.
Both vector and raster styles can be saved and reused—however, in slightly different ways.



### Getting ready

For this recipe, you need two similar vector layers and a set of two similar raster layers.
In the example data that is provided, use two of the bus route shapefiles and two of the elevation rasters (for example, elevlid_D782_6.tif).


### How to do it

First we'll start by copying and pasting styles for vector layers:

1. Load up two bus route shapefiles and two elevlid rasters.
2. The simplest method is to copy styles for vectors or rasters.
Just right-click on the layer name in the list and select Copy Style from the Style menu.
Then, right-click on the layer that you want to apply this to and select Paste Style from the Style menu.
>You can only copy styles between layers of the same type (for example, Point to Point):
Try to copy and paste the style of one bus route to the second bus route using the right-click menus:
3. Now to export styles for later use, right-click on a layer and navigate to Properties | Symbology.
4. In the bottom-right, there is a Save Style button in the Style menu.
The output choices are QGIS Layer Style File… (aka .qml) or SLD File....
Both formats are XML text files, QGIS Layer Style...
is recommended for maximum compatibility:
5. SLD is compatible with some other web map systems, but it will not capture your QGIS style 100% except in the simplest cases (not all the same options exist in SLD).
QML is the native QGIS style file. Note the Load style option for later usage.
6. Go ahead and apply a new style to one of the bus route layers.
7. Then, save the symbology to QGIS Layer Style File (qml).
8. In the property dialog of the second bus route file, click Load Style... and pick the file
that you just saved.
> You can open this file in a text editor and make customizations or, for example, batch-find and replace values.

Rasters are slightly trickier in that you can save a symbology file, but you can also save a color table.
The color table is a text file that lists raster value ranges and associated color codes.
It's a much simpler format to hand-edit than XML (QGIS Layer Style File), but does not retain things like transparency or classification settings:
1. Go ahead and apply a new color gradient to one of the elevlid layers.
2. Save just the color table to a .txt file with the disk button (above the color table on the right end of the button row; refer to the screenshot).
3. In the property of the second elevid file, load the color table and pick the file that you just saved.
4. Apply the changes to your layer style:
> Note that the same Style menu is available as was in the vector properties.
You can use this to save and load QGIS Layer Style File (.qml) just as we did earlier.







### How it works

Normally the style information for a layer is saved in the .qgs (if you save your project) project file.
The various export methods just package up the style information for a layer into a separate file in a generic manner (not associated with the original data).
This lets you apply similar styles to similar data sources.

Vector symbology is stored in a special XML file that ends in the .qml extension.
You can read or edit the file if you want, and it can be produced via scripts or copied and pasted to create mashups of multiple styles.

Raster symbology can also be stored in a .qml file.
However, there's an additional option to export the classification ranges and colors to a simple text file, one value or range of values and one color code per line.


### There's more


The second format SLD (Style Layer Descriptors) is very common in web services.
While not all features of QGIS styling have equivalents in SLD, it's still a good starting point to share your style across software platforms such as Mapserver or Geoserver.

## 10.6 Configuring data-defined labels

If there was a list of top features of QGIS, data-defined labels would be high on that list.
They offer the ease of automatic labeling with the customization of freehand labeling.
You can mix and match automatic and custom edits, storing the values in a table for later reference.

### Getting ready

There are a couple of useful plugins for data-defined labeling which will add the extra attribute fields that you need to either an existing layer or make a new layer just for labels.
Download and install Layer to labeled layer and Create labeled layer.

### How to do it

1. Open QGIS and load census_wake2000.shp.

2. Create a copy of the layer using the Save As dialog, and save the layer as census_ wake2000_label.shp. (You don't always have to do this but this process does modify the table, so it's a good idea to make a backup.)

3. Highlight census_wake2000_label.shp in the layer list.

4. Run the Layer to labeled layer plugin (Plugins | Layer to Labeled layer plugin):
1. Set Label Field to STFID.
2. Click on OK:

5. If you look at the attribute table now, you will see a whole bunch of new fields, starting with the Lbl prefix, which are NULL:
6. Now, ensure that you have the Label toolbar open (View | Toolbars | Label):

7. Either in the layer (by navigating to Properties | Labels) or using the first button on Label Toolbar, Layer Labeling Options, open the label management dialog.

8. Throughout the dialogs, you will see markers next to each field.
A yellow one indicates a data-defined attribute, a white marker is the same setting for all:
> If you want to control additional attributes at this point, add a new field to the layer.
Then, return to this dialog and select the white icon to pick the name of the field to use.

9. Now, you are ready to make custom edits to various labels and have the table store the settings.
Depending on the setting, there are a couple of ways to make the edits. Note that you must toggle editing on the layer before you can change the labels:
1. You can edit the field directly in the table either by hand, or you can use the field calculator to automate repetitive patterns (for example, give all major roads the same Font and Color label).
2. For some attributes, such as X,Y and rotation, you can also edit by hand in the map using the Label Toolbar option.

__Example: moving and rotating a label__
1. Toggle editing by clicking on the following icon:
2. On the Label Toolbar menu, select the Move Label button. Now, click on a label and drag it to a new location, releasing the mouse button when you are done moving the label. Note that you must ensure that the X and Y fields in step 38 are set for this tool to be usable:
> If you check the attribute table you will see that in the LblX and LblY fields, the values have now been saved for the labels that you moved.
3. Now, try the Rotate Label button. See if you can make some of the labels fit inside their polygons using the move and rotate:
You can also use the Change Label button to edit the other properties of a specific label that you select. This is really nice when you just need some fine-tuning.
4. Save your edits and toggle editing off to keep your changes.


### How it works

The basic premise is that you keep an extra set of attributes in a table often as additional fields to your existing table.

> You could add fields to your attribute table by hand, and assign them to label properties.
Using the Layer to Labeled Layer plugin does this for you.

These fields if you set them are used in determining the location, size, font, color, angle, and so on, of the label for the given row.
If you don't set them, then the automatic settings from the labeling engine are kept.




### There's more

Data-defined labels are powerful in that you can combine automated, calculated, and custom-edited values.
They are automated from the built-in labeling engine and calculated using the field calculator to populate the data-defined fields (for example, with if statements or calculations that are based on other attributes).
Lastly, by just making these little hand tweaks, you can fix a few not-quite labels that misbehave.

Note that you don't have to use data-defined labeling on an existing layer.
You can create just a label layer with the Create labeled layer plugin.
In other software, user-defined labeling is often called Annotation layers.
QGIS also has annotation layers.
These are layers where you click to add a label to the map and then write and style it however you want.
The biggest problem is that these layers are not associated with the data that they label.
You can't easily give them to someone else, and if a label name or style changes, you have to chase down and hand-edit every fix.
In QGIS, data-defined labeling solves almost all the shortcomings of annotation layers because it actually saves to a shapefile with all its properties as fields.

## 10.7 Creating custom SVG graphics

This recipe is all about making your map unique by creating custom icons, north arrows, or even fill patterns.


### Getting ready

You will need a vector illustration program (for example, Inkscape or Adobe Illustrator).
> Don't have one? There are several free and open source options available on all platforms.
Many people in the QGIS community use Inkscape (http://inkscape.org), but you can also use LibreOffice Draw or OpenOffice Draw.
The most common proprietary software equivalent is Adobe Illustrator.

You will also need a text editor, such as TextEdit (Mac), Notepad, Notepad++ (Windows).

### How to do it

1. Open up your vector illustration program.
2. Set the canvas to a reasonable size to work with.
Square ratios tend to work well because the icon will eventually be used to mark points in QGIS; 100x100 pixels is fine.
3. Draw a simple shape such as a square, circle, or star.
Make sure you go most of the way towards the edges and fill the whole page.
> Remember that you will be using this drawing at sizes closer to 8-32 pixels; it's just really annoying to work at these scales. when creating and editing illustrations

4. Save the drawing as an .svg file.
5. Now, open the .svg file in a text editor, search and find the style line of your object, and replace it with the following lines:
stroke-width="param(outline-width) 1"
stroke="param(outline) #000" fill="param(fill) #FFF"
> If working with a complex icon, set your line to a specific color code that is different from all other colors in the drawing.
Make a note of the color code so that you can use it to search the .svg file in your text editor.

6. Save your changes.
7. Now, start up QGIS and load a point layer.
8. Go to Properties | Style.
9. In the symbology options, there are two levels of objects that make a symbol: the marker and then a sublevel of actual symbols that combine to make it.
10. Select the subobject, which is usually labeled Simple Marker by default.
11. Now, change the dropdown in the upper-right to SVG Marker.
12. Below the box displaying the symbol options look for the … button and select to load an SVG from file.
Use this to select the .svg file that you previously created.
13. Once imported, you should be able to change the fill color of the symbol (if you performed Step 5):
> You may need to adjust the size and widths in large amounts for changes to be apparent.
Make use of the Apply button to see the changes in the map but keep the dialog open for easy adjustment.



### How it works

The special text that you add to the .svg file is a marker or placeholder.
QGIS looks for these particular words and then utilizes them to insert symbol changes on-the-fly as the SVG is read into the program.


### There's more

While this recipe demonstrated a very simple SVG, this method applies to more complicated symbols.

Also note that in Settings | Options | System, you can set paths to folders of SVGs so that all of them will be available in the symbology dialogs all the time.

### See also


QGIS also lets you customize fill patterns using SVG symbols.
The QGIS Training Manual has a good example of this at http://docs.qgis.org/2.8/en/docs/training_manual/basic_map/symbology.html#hard-fa-creating-custom-svg-fill.

## 10.8 Making pretty graticules in any projection

A graticule is a set of reference lines on a map that help orient a map reader.
They are often set at, and labeled, with the coordinates.
The tricky part about using graticules, however, is projections.
If you don't make them correctly, instead of smooth curves between the line intersections, you get awkward unusual shapes (mostly straight lines).
The default QGIS graticule creator is not projection-friendly, so in this recipe, you'll see an add-on processing algorithm that does this.
This recipe is about ensuring you get nice, smooth, and properly- labeled graticules.



### Getting ready

You don't really need much for this recipe other than a bounding box and a coordinate interval that you want to space the lines at.
Usually, these will be in Latitude, Longitude WGS 84 (EPSG:4326), and decimal degrees, respectively, since the whole point of a graticule is to add reference lines that help orient a user.

### How to do it

1. Start by downloading a Processing Toolbox algorithm specifically for this task called
Lines Graticule:
1. Open the Processing Toolbox.
2. Go to Scripts | Tools | Get scripts from on-line scripts collection:
3. In the Not Installed section, check the box for the Lines Graticule algorithm.
4. Click on the OK button to install the algorithm.
Every time that you use a tool, it's good to check for updates.
You will see something like the following screenshot:

2. Now that you've downloaded the algorithm, open it by navigating to Scripts | Vector
(it's called Lines graticule though the code is actually pygraticule.py):
3. You can fill in the parameters by hand if you know them or use the … button to get values from your existing project.
4. For now, you can use the defaults that will make a graticule for the whole world.
The outputs are determined by outfile and graticule.
These parameters are optional, you can choose to pick one, both, or neither. If you want a GeoJSON file, set the outfile.
If you want a shapefile, set the graticule (if you want the results to autoload afterwards, make sure that the second output is set to temporary or a real file, just not blank).
Refer to the Help tab for details about each parameter. There are two really important values to control the graticule:
1. The spacing value denotes how often to draw a line (when doing world-scale maps, 20 or 30 degrees works well).
2. The density value denotes how often to put nodes:
The more nodes, the smoother the curves; however, you get a bigger file that takes longer to make.
Picking the right density may require trial and error to find the largest density before you notice the lines stop curving smoothly for a given map scale.

5. Once you've chosen your settings, click on Run.
6. After it runs, a vector layer should get loaded with the results.
This won't look all that exciting, just straight lines making a grid.
7. The real magic is to now enable projection on-the-fly with one of the many decent world-wide projections such as "World Robinson (EPSG:54030):
8. (Optional) If it doesn't look like the image, but instead still has straight lines that are oddly spaced, you need to disable the QGIS rendering simplification:
1. Pick the layer from Properties | Rendering.
2. Make sure that Simplify geometry is disabled:

9. (Bonus) Generate a vector grid from Vector | Research tools. The difference looks like the following:


### How it works

Graticules are basically line layers (though sometimes they are also polygons).
If you draw a grid with nodes only at the points where two lines intersect, you can easily see how distorting the grid will lead to blocky shapes.
The key to smooth graticules is adding additional line nodes in between the intersections (that is, increase the node density).

It's important to note that, when using projections that don't cover the whole world (for example, polar or stereographic projections), pick bounding box values that fall within the projection limits; otherwise, you may get errors when trying to reproject.



### There's more

The primary advantages of graticules in the main map canvas are that you can use them as references while working in QGIS, include them in web and digital maps, and have full control of the labels and symbology.
The method used here differs from other graticule (grid) tools in QGIS because it focuses on putting Latitude/Longitude lines with smooth curves as references into any projection.
Other grid tools focus more on making regular squares across a map to subdivide a region.

The main advantages of the print composer method (next recipe) are its ability to make multiple coordinate systems easily and to add tick marks around the outside edge of a map.
Tick marks are what you commonly see on navigation-oriented maps, such as USGS Topo quads, and other printed maps.


### See also

Lines graticule (aka Pygraticule) can also be used as a pure Python script; for updates and more information, refer to https://github.com/wildintellect/pyGraticule.

To learn how to write your own processing toolbox algorithms, refer to the Writing processing algorithms recipe in Chapter 11, Extending QGIS.

## 10.9 Making useful graticules in printed maps

A graticule is a set of reference lines on a map that help orient a map reader.
They are often set at and labeled with the coordinates.
For traditional printed maps that are intended for navigation and surveying tasks where you want to mark the geographic coordinates, sometimes in multiple coordinate systems.
This recipe is about adding such reference lines to a Print Composer map.


### Getting ready

You will need a map, typically of a small area (several miles or km across).
For this recipe, elevlid_D782_6.tif works well.


### How to do it

1. Load elevlid_D782_6.tif.

2. __Turn on Projection on-the-fly by selecting UTM Zone 17 N, WGS 84 (EPSG:32617).__

3. Now create New Print Composer.

4. In Print Composer, select Add New Map, and then draw a rectangle on the canvas.

5. Now that you have the map, in the dialogs on the right-hand side of the screen select the Item Properties tab.

6. Scroll down or collapse sections until you see the Grids section.

7. Use the green plus (+) symbol to add a new grid.

8. Now, edit Interval X and Interval Y to 1,000 map units.
(Make sure to tab to the next field or click on Enter for the values to stick.):
> The current map units are UTM-based, meters, which means the lines will be 1,000 meters or 1 km apart.

9. Just below the Interval section, change Line Style and make the lines red so that they are easier to see.

10. Now, scroll down even further to the Draw coordinates section and check the box to enable labels for the grid lines:

11. Once this is enabled, change the top and bottom orientation to vertical, and change the font color to red.

12. Now, to create a second grid, scroll back up to the Grids section and click the green plus sign (+) again to add a second grid.

13. This time, change the CRS (Coordinate Reference System) to WGS 84 (EPSG:4326), which is the most common Latitude and Longitude system that people use.

14. Make this grid be spaced 0.01 map units (that is, degrees in this case) and change the style to blue to contrast with the other grid.

15. Now, scroll down and add Draw coordinates.
Also, make the top and bottom vertical-oriented so that they avoid the first grid.
You can also change the font color to match the lines:


### How it works

Reference graticules are evenly spaced lines with marked coordinates.
Based on your settings the composer calculates the positioning of the lines from the map data coordinates.
The key  to making useful graticules in the print composer is to select intervals that are often enough  to provide reference but not so often that they cover a large portion of the map.
It's also important to pick intervals that have nice rounded numbers, so that it's easy to calculate the value half way between two lines.


### There's more

There are two ways to make grids/graticules in QGIS: the print composer for printed maps or as a layer in QGIS for printed web maps as an internal usage.

The main advantages of the print composer method are the ability to do multiple coordinate systems easily and to add tick marks around the outside edge of a map.
Tick marks are what you commonly see on navigation oriented maps, such as USGS Topo quads.

The primary advantages of graticules in the main map canvas are that you can use them as references while working with QGIS and have full control of the labels and symbology.
Refer to the Making pretty graticules in any projection recipe in this chapter for how to make graticules in the main map interface.

## 10.10 Creating a map series using Atlas

In this recipe, we will use the Print Composer Atlas functionality to automatically create a PDF map book with a series of maps.



### Getting ready

To follow this recipe, load zipcodes_wake.shp and geology.shp from our sample data.
In the following screenshots, the zipcodes_wake layer was styled with a simple white border, while the geology layer is styled with random colors.

### How to do it

The Print Composer Atlas feature will create one map for each feature in the so-called Coverage layer. In this recipe, the zipcodes layer will serve as a Coverage layer, and we will create one map for each zipcode feature:
1. Click on the New Print Composer button or press Ctrl + P to get started. You will be prompted to set a title for the new composer.
This can be left empty if you want QGIS to generate a title automatically.
2. Click on the Add new map button and drag open a rectangle on the composer page to create a map item for the main map.
3. To activate the Atlas functionality, we enable the map item's Controlled by atlas checkbox.
The following screenshot shows the fully configured map's item properties.
In the Controlled by atlas section, we can select which zoom mode Atlas should use:
1. Margin around feature: This is the most flexible option, which tells Atlas to zoom to the feature while keeping the specified margin percentage around the feature.
2. Predefined scale (best fit): This tells Atlas to use the one predefined project scale (configurable in Project Properties | General | Project scales) where the feature best fits in.
3. Fixed scale: This keeps the same scale for all maps of the series; the scale is configured in the map's Main properties, that is, 100,000 in the following screenshot:

4. Next, we add a label for the title using the Add new label button.
This title label will display the zip code polygon feature's NAME value which will be automatically updated by Atlas for each map in the series.
To achieve this, we insert the following expression in the input field of the label item's Main properties:
```[%attribute( @atlas_feature, 'NAME' ) %]```

5. To finalize the Atlas configuration, we need to go to the Atlas generation tab.
There, we first have to enable the Generate an atlas checkbox.
This activates the Configuration section, where we can pick the Coverage layer and set it to the zipcodes_wake layer, as shown in the following screenshot.

6. To preview the Atlas output, we can now click on the Preview Atlas button.
This  button is only active if the Generate an atlas checkbox in the Atlas generation tab is enabled.
Once the preview mode is active, you can step through the map series using the arrow buttons right besides the Preview Atlas button.

7. When we are happy with the preview, we can export the map series.
The output behavior is controlled by the configuration in the Atlas generation tab's Output section, which you can also see in the following screenshot.
Atlas supports exporting to separate image, SVG, or PDF files.
Activate the Single file export when possible option to combine all maps into one PDF and click on the Export Atlas as PDF button, as shown in the following screenshot:


### How it works

The Atlas feature provides access to a series of variables related to the current feature.
We already used this to display the NAME value of the current feature in the title label using the __```[%attribute( @atlas_feature, 'NAME' ) %] expression```__.
Besides __```@atlas_feature``__, you have access to the following variables:

* __```@atlas_feature```__: This is the feature ID of the current Atlas feature.
This makes it possible to use this information in rules to, for example, hide or highlight features based on their ID.
* __```@atlas_geometry```__: This is the geometry of the current Atlas feature and can be used in rules to, for example, only show features of other layers if their geometry intersects the Atlas feature geometry.
* __```@atlas_featurenumber```__: This is the number of the current Atlas feature.
* __```@atlas_totalfeatures```__: This is the total number of features in the Atlas coverage layer.


### There's more

Overview maps are a great way to provide context to more detailed main maps.
To add an overview map (as shown in the upper-right corner of the composition in the following
screenshot), you need to add a second map item to the composition.
To turn this map item into an overview map, go to Item properties | Overviews and click on the button with the green plus sign.
This will add an Overview 1 entry and enable the Draw "Overview 1" overview configuration GUI:
* __Map frame__: The Map frame drop-down list enables us to define the main map that should be referenced by the overview map.
By default, the map items are named Map 0, Map 1, Map 2, and so on, depending on the order they were added to the composition.
Therefore, we will select the Map 0 entry if the main map was the first item that was added to the composition.
* __Frame style__: The Change … button can be used to choose a style for the overview frame. Usually, this will be a simple fill with transparency.
* __Blending mode__: These are supported by overview frames, as explained in detail in the Understanding the feature and layer blending modes recipe.
* __Invert overview__: Enable the Invert overview checkbox if you want to apply the overview frame style to the areas outside the extent of the main map.
* __Center on overview__: Enable the Center on overview checkbox if you want the overview map to automatically pan to center on the extent of the main map.

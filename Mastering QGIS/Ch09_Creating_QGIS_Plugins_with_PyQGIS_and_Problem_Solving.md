
# Chapter 9: Creating QGIS Plugins with PyQGIS and Problem Solving


<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [Chapter 9: Creating QGIS Plugins with PyQGIS and Problem Solving](#chapter-9-creating-qgis-plugins-with-pyqgis-and-problem-solving)
  * [9.1 Webography - where to get API information and PyQGIS help](#91-webography-where-to-get-api-information-and-pyqgis-help)
    * [9.1.1 PyQGIS cookbook](#911-pyqgis-cookbook)
    * [9.1.2 API documentation](#912-api-documentation)
    * [9.1.3 The QGIS community, mailing lists, and IRC channel](#913-the-qgis-community-mailing-lists-and-irc-channel)
  * [9.2 The Python Console](#92-the-python-console)
    * [9.2.1 Getting sample data](#921-getting-sample-data)
    * [9.2.2 My first PyQGIS code snippet](#922-my-first-pyqgis-code-snippet)
    * [9.2.3 My second PyQGIS code snippet – looping the layer features](#923-my-second-pyqgis-code-snippet-looping-the-layer-features)
  * [9.3 Exploring iface and QGis](#93-exploring-iface-and-qgis)
  * [9.4 Exploring a QGIS API in the Python Console](#94-exploring-a-qgis-api-in-the-python-console)
  * [9.5 Creating a plugin structure with Plugin Builder](#95-creating-a-plugin-structure-with-plugin-builder)
    * [9.5.1 Installing Plugin Builder](#951-installing-plugin-builder)
    * [9.5.2 Locating plugins](#952-locating-plugins)
    * [9.5.3 Creating my first Python plugin – TestPlugin](#953-creating-my-first-python-plugin-testplugin)
  * [9.6 A simple plugin example](#96-a-simple-plugin-example)
    * [9.6.1 Adding basic logic to TestPlugin](#961-adding-basic-logic-to-testplugin)
  * [9.7 Setting up a debugging environment](#97-setting-up-a-debugging-environment)
    * [9.7.1 What is a debugger](#971-what-is-a-debugger)
    * [9.7.2 Installing Aptana](#972-installing-aptana)
    * [9.7.3 Setting up PYTHONPATH](#973-setting-up-pythonpath)
    * [9.7.4 Starting the Pydevd server](#974-starting-the-pydevd-server)
    * [9.7.5 Connecting QGIS to the Pydevd server](#975-connecting-qgis-to-the-pydevd-server)
  * [9.8 Debugging session example](#98-debugging-session-example)
    * [9.8.1 Creating a PyDev project for TestPlugin](#981-creating-a-pydev-project-for-testplugin)
    * [9.8.2 Adding breakpoints](#982-adding-breakpoints)
    * [9.8.3 Debugging in action](#983-debugging-in-action)
  * [9.9 Summary](#99-summary)

<!-- tocstop -->


* Where to get help to solve your PyQGIS problems
* How to setup a development environment that can resolve PyQGIS and PyQt API names during code editing
* How to interactively test your snippet using the QGIS Python console and some useful classes that can be used everywhere
* Creating a basic plugin using Plugin Builder
* Analyze your first basic plugin to know its structure
* Setting up a runtime debugging environment that can be useful in developing complex plugins

## 9.1 Webography - where to get API information and PyQGIS help
### 9.1.1 PyQGIS cookbook
### 9.1.2 API documentation
### 9.1.3 The QGIS community, mailing lists, and IRC channel
* __Mailing lists__
* __IRC channel__
* __The StackExchange Community__
* __Sharing your knowledge and reporting issues__

## 9.2 The Python Console
### 9.2.1 Getting sample data
### 9.2.2 My first PyQGIS code snippet
### 9.2.3 My second PyQGIS code snippet – looping the layer features
## 9.3 Exploring iface and QGis
## 9.4 Exploring a QGIS API in the Python Console
## 9.5 Creating a plugin structure with Plugin Builder
### 9.5.1 Installing Plugin Builder
### 9.5.2 Locating plugins
### 9.5.3 Creating my first Python plugin – TestPlugin
* __Setting mandatory plugin parameters__
* __Setting optional plugin parameters__
* __Generating the plugin code__
* __Compiling the icon resource__
* __Plugin file structure – where and what to customize__

## 9.6 A simple plugin example
### 9.6.1 Adding basic logic to TestPlugin
* __Modifying the layout with Qt Designer__
* __Modifying GUI logic__
* __Modifying plugin logic__

## 9.7 Setting up a debugging environment
### 9.7.1 What is a debugger
### 9.7.2 Installing Aptana
### 9.7.3 Setting up PYTHONPATH
### 9.7.4 Starting the Pydevd server
### 9.7.5 Connecting QGIS to the Pydevd server
## 9.8 Debugging session example
### 9.8.1 Creating a PyDev project for TestPlugin
### 9.8.2 Adding breakpoints
### 9.8.3 Debugging in action
## 9.9 Summary


```python

```

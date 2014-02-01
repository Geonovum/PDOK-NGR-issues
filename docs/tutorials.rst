#########
Tutorials
#########


***************************************************************
Vector data downloaden via Web Feature Service (WFS) en ogr2ogr
***************************************************************

TODO: convert `this presentation <https://speakerdeck.com/ndkv/open-geo-data-in-the-netherlands-and-beyond>`_ to a tutorial. Aim for the style of e.g. `OpenLayers 3 documentation <http://ol3js.org/en/master/doc/tutorials/concepts.html>`_.

* Nationaalgeoregister is a registry, it does not contain datasets itself but links to other sources. 
* Natioaalgeoregister holds links to `services <OGC Services>`_.

TODO: Include image of that depicts how  NGR is connected to databases.

This tutorial shows how to get datasets from the Dutch national geoportal through WFS using the GDAL/OGR toolset.  

The GDAL/OGR library is the Swiss army knife for handling geospatial data. GDAL provides functions to read, write and transform raster files (e.g. GeoTIFF). OGR provides the same functionality for vectors.

On Linux you can grab the library through your package manager. Apt-getting it on Ubuntu is as easy as::

    $ sudo apt-get install libgdal

Easiest way to get it on Windows is thriugh the `OSGeo4W <http://trac.osgeo.org/osgeo4w/>`_ installer. 

.. NOTE::

    This tutorial assumes you are familar with the Web Feature Service. Not sure what that is? Review it :ref:`here <wfs>`. 


BAG
===

In this tutorial we will work with the :ref:`Bassisregistratie Adressen en Gebouwen dataset <bag>`. It contains, amongst others, all Dutch building footprints. It's the basis for this `CitySDK <http://citysdk.waag.nl/buildings/>`_ visualisation. To get vector data itself, we'll query the WFS endpoint located at::

    http://geodatan.nationaalgeoregister.nl/bagviewer/wfs

.. WARNING::

    This particular service is limited to serving a maximum of 15000 features per request. If you need more you'll have to obtain the whole dataset from the ATOM feed or through ExtractNL. 


.. NOTE::

    Although the focus of this tutorial is on the BAG, the demonstrated worklfow and commands can be used to query any WFS endpoint. See ... for more information on how to search specifically for WFS endpoints in the register.  

We'll first investigate the endpoint with the *ogrinfo* utility and retrieve the data with the *ogr2ogr* utility.  

Investigating the data source with ogrinfo 
==========================================
The *ogrinfo* utility retrieves the metadata of a service. It displays information such as the number and names of contained layers, number of features, coordinate reference system of the dataset. Basic usage  

::

    $ ogrinfo -so WFS:"<url>"

where 

* -so retrieves a summary of the statistics
* <url> points to a WFS endpoint

We can query the BAG endpoints as

::

    $ ogrinfo -so WFS:"http://geodatan.nationaalgeoregister.nl/bagviewer/wfs"

which results in::

   INFO: Open of 'WFS:' 
         using driver 'WFS' successful.

   1. bagviewer:ligplaats (Polygon)
   2. bagviewer:pand (Polygon)
   3. bagviewer:standplaats (Polygon)
   4. bagviewer:verblijfpaats (Point)
   5. bagviewer:woonplaats (Multi Polygon)


where the enumerated items represent the available layers and their type. The building footprints are contained in the 2nd layer and are of type polygon. We can use *ogrinfo* to investigate a specific layer by appending its name at the end::

    $ ogrinfo -so WFS:"http://geodatan.nationaalgeoregister.nl/bagviewer/wfs" pand

The result is the number of features contained in the layer, a listing of the features' attributes (i.e. columns), the coordinate reference system in which the layer is saved and a bounding box of the features.

.. NOTE::

    Observe the afore mentioned limit: the reported number of features is 15000. There are, of course, more than 15000 buildings in the Netherlands.  


Getting data with ogr2ogr
=========================

The ogr2ogr utility allows for reading and writing of many different vector formats. The most basic use of ogr2ogr is built as::

    ogr2ogr -f output_format destination source layer

Getting the gemeenten2011 layer as GeoJSON is thus achieved as::

    $ ogr2ogr -f GeoJSON footprints.geojson \ 
    WFS:"http://geodatan.nationaalgeoregister.nl/bagviewer/wfs" \
    bagviewer:pand


Transforming
------------

ogr2ogr's primary function is to transform vector data into different formats and coordinate reference systems. We can do the same with the WFS source; transforming the data from the Dutch coordinate system to lat/lng is done as::

    $ ogr2ogr -f GeoJSON gemeenten2011.geojson WFS:"" -t_srs EPSG:4326 gemeenten2011

See (stukje over coordinatenstelsels)

ogr2ogr can also transform between formats. This is handy when you've obtained a file in a certain format, but need it another (e.g. CartoDB doesn't support GeoJSONs). Transforming e.g. a shapefile into a GeoJSON file is done as::

    $ ogr2ogr -f GeoJSON input.shp output.geojson


Filtering
---------

ogr2ogr supports filtering of datasources through a simple -where clause as well as an SQL API. Retrieving a single column from the footprints dataset through SQL is done as::

    $ ogr2ogr -f GeoJSON gemeenten_2011_84.geojson 
    WFS:"http://geodatan.nationaalgeoregister.nl/bagviewer/wfs" 
    -sql "SELECT CAST('bouwjaar' AS intege AS 'bouwjaar' 
    FROM 'bagviewer:pand'" 
    -t_srs EPSG:4326

TODO: add bounding box query

Next steps: visualising and publishing data  
===========================================

You can now load the vector file in a popular GIS pacakge for furhter analysis, load it in a PostGIS database publish it to Mapbox or CartoDB, etc. 

TODO: add SpatiaLite examples

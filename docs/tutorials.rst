#########
Tutorials
#########


***************************************************************
Vector data downloaden via Web Feature Service (WFS) en ogr2ogr
***************************************************************

TODO: convert `this presentation <https://speakerdeck.com/ndkv/open-geo-data-in-the-netherlands-and-beyond>`_ to a tutorial. Aim for the style of e.g. `OpenLayers 3 documentation <http://ol3js.org/en/master/doc/tutorials/concepts.html>`_.

This tutorial shows how to get datasets from the Dutch national geoportal through WFS. 

Introduction
============

* Nationaalgeoregister is a registry, it does not contain datasets itself but links to other sources. 
* Natioaalgeoregister holds links to `services <OGC Services>`_.

TODO: Include image of that depicts how  NGR is connected to databases.


GDAL/OGR
========

The GDAL/OGR library is the Swiss army knife for handling geospatial data. GDAL provides functions to read, write and transform raster files (e.g. GeoTIFF). OGR provided the same functionality for vectors. 


In this tutorial we will work with the BAG dataset. It's WFS endpoint is located at 

    http://geodatan.nationaalgeoregister.nl/bagviewer/wfs

.. WARNING::

    This particular service is limited to serving a maximum of 15000 features per request. If you need more you'll have to obtain the whole dataset from the ATOM feed or through ExtractNL. 

The commands used in this tutorial can be used to query all the WFS sources. See ... for a discussion how to search the register. 

ogrinfo 
-------

::

    ogrinfo -so WFS:"<url>"

where 

* -so retrieves a summary of the statistics
* url points to a WFS endpoint

We can query the BAG endpoints as::

    $ ogrinfo -so WFS:"http://geodatan.nationaalgeoregister.nl/bagviewer/wfs"

which results in::

   INFO: Open of 'WFS:' 
         using driver 'WFS' successful.

   1. bagviewer:ligplaats (Polygon)
   2. bagviewer:pand (Polygon)
   3. bagviewer:standplaats (Polygon)
   4. bagviewer:verblijfpaats (Point)
   5. bagviewer:woonplaats (Multi Polygon)


where the enumerated items represent the available layers and their type. 

ogr2ogr
-------
The ogr2ogr utility allows for reading and writing of many different vector formats. The most basic use of ogr2ogr is built as::

    ogr2ogr -f output_format destination source

Querying WFS sources
^^^^^^^^^^^^^^^^^^^^
Getting the gemeenten2011 layer as GeoJSON is thus achieved as::

    ogr2ogr -f GeoJSON gemeenten2011.geojson \ 
    WFS:"http://geodatan.nationaalgeoregister.nl/bagviewer/wfs" \
    gemeenten2011


Transforming
^^^^^^^^^^^^
ogr2ogr's primary function is to transform vector data ... hence we can do the same with the WFS source as::

    ogr2ogr -f GeoJSON gemeenten2011.geojson WFS:"" -t_srs EPSG:4326 gemeenten2011

See (stukje over coordinatenstelsels)

ogr2ogr can also transform between formats. This is handy when you've obtained a file but would like to have it in another format. Transforming e.g. a shapefile into a GeoJSON file is done as::

    ogr2ogr -f GeoJSON input.shp output.geojosn



Filtering
^^^^^^^^^
ogr2ogr supports filtering of datasources through a simple -where clause as well as an SQL API. Retrieving a single column from the municipality dataset through SQL is done as::

    ogr2ogr -f GeoJSON gemeenten_2011_84.geojson 
    WFS:"http://geodatan.nationaalgeoregister.nl/bagviewer/wfs" 
    -sql "SELECT 'gemeentenaam' , CAST('aantal_inwoners' AS integer" 
    AS 'aantal_inwoners' 
    FROM 'wijkenenbuurten2011':gemeenten2011'" 
    -t_srs EPSG:4326

You can now load the vector file in a popular GIS pacakge for furhter analysis or publish it to Mapbox or CartoDB.

.. _webapps.ol3:

Building web map applications with OpenLayers
=============================================

This section will show you how to deploy a simple web mapping application using OpenLayers.

OpenLayers is a JavaScript library that allows for putting a dynamic map in any web page. It can display map tiles and markers loaded from any source.

For more information about OpenLayers, including download instructions, refer to the `OpenLayers website <http://openlayers.org>`_.

Complete working example
------------------------

Here is a complete working example of a map on a web page.

.. code-block:: html

    <!doctype html>
    <html lang="en">
      <head>
        <link rel="stylesheet" href="http://openlayers.org/en/latest/css/ol.css" type="text/css">
        <style>
          .map {
            height: 400px;
            width: 100%;
          }
        </style>
        <script src="http://openlayers.org/en/latest/build/ol.js" type="text/javascript"></script>
        <title>OpenLayers example</title>
      </head>
      <body>
        <h2>My Map</h2>
        <div id="map" class="map"></div>
        <script type="text/javascript">
          var map = new ol.Map({
            target: 'map',
            layers: [
              new ol.layer.Tile({
                source: new ol.source.MapQuestOpenAerial()
              })
            ],
            view: new ol.View2D({
              center: ol.proj.transform([37.41, 8.82], 'EPSG:4326', 'EPSG:3857'),
              zoom: 4
            })
          });
        </script>
      </body>
    </html>

View this map
-------------

To view this example, create a new file called :file:`map.html`, copy the contents of the above into this file, and save it in the :file:`apps` directory. This directory is usually found here:

Windows::

  C:\Program Files (x86)\Apache Software Foundation\Tomcat 8.0\webapps

Linux::

  /usr/share/tomcat8/webapps

Next, open that file in a browser. Assuming Boundless Server web applications deployed at ``http://localhost:8080``, this file should be viewable at ``http://localhost:8080/apps/map.html``.

.. figure:: img/ol3map.png

   OpenLayers map

Details
-------

There are three components to including a map in a web page:

#. Include the OpenLayers library
#. ``<div>`` map container
#. JavaScript to create a simple map

Include the OpenLayers library
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: html

   <script src="http://openlayers.org/en/latest/build/ol.js" type="text/javascript"></script>

The first step is to include the JavaScript library. For the purpose of this tutorial, here we simply point to the library hosted at ``ol3js.org``. In a production environment, we would build a custom version of the library including only the module needed for our application.

``<div>`` map container
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: html

   <div id="map" class="map"></div>

The map in the application is contained in a ``<div>`` `HTML element <http://en.wikipedia.org/wiki/Span_and_div>`_. Through this ``<div>`` the map properties like width, height, and border can be controlled through CSS. This is the CSS element used to make the map 400 pixels high and as wide as the browser window:

.. code-block:: html

  <style>
    .map {
      height: 400px; width: 100%;
    }
  </style>

JavaScript to create a simple map
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: html

  var map = new ol.Map({
    target: 'map',
    layers: [
      new ol.layer.Tile({
        source: new ol.source.MapQuestOpenAerial()
      })
    ],
    view: new ol.View2D({
      center: ol.proj.transform([37.41, 8.82], 'EPSG:4326', 'EPSG:3857'),
      zoom: 4
    })
  });

With this JavaScript code, a map object is created with a MapQuest Open Aerial layer zoomed on the African East coast. Let's break this down:

The following line creates an OpenLayers ``Map`` object. Just by itself, this does nothing since there are no layers or interaction attached to it.

.. code-block:: html

  var map = new ol.Map({ ... });

To attach the map object to the ``<div>``, the map object takes a ``target`` into arguments. The value is the ``id`` of the ``<div>``:

.. code-block:: html

    target: 'map'

The ``layers: [ ... ]`` array is used to define the list of layers available in the map. The first and only layer right now is a tiled layer:

.. code-block:: html

    layers: [
      new ol.layer.Tile({
        source: new ol.source.MapQuestOpenAerial()
      })
    ]

Layers in OpenLayers are defined with a type (Image, Tile or Vector) which contains a source. The source is the protocol used to get the map tiles. You can consult the `list of available layer sources <http://openlayers.org/en/latest/apidoc/ol.source.html>`_.

The next part of the ``Map`` object is the ``View``. The view allows you to specify the center, resolution, and rotation of the map. Currently, only ``View2D`` is supported. The simplest way to define a view is to define a center point and a zoom level. Note that zoom level ``0`` means zoomed out.

.. code-block:: html

    view: new ol.View2D({
      center: ol.proj.transform([37.41, 8.82], 'EPSG:4326', 'EPSG:3857'),
      zoom: 4
    })

You will notice that the ``center`` specified is in lat/lon coordinates (EPSG:4326). Since the only layer we use is in Spherical Mercator projection (EPSG:3857), we transform the coordinates to be able to zoom the map on the right coordinates.

Where to find OpenLayers
--------------------------

For more information on OpenLayers including full source code and documentation, please go to `openlayers.org <http://openlayers.org>`_.

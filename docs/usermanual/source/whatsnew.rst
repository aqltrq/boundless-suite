.. _whatsnew:

What's new in |version|
=======================

This version contains numerous component upgrades and bug fixes. Highlights include:

* **Boundless Suite** has been renamed to **Boundless Server**, and the version has been reset to |version|. This follows from **Boundless Suite 4.10.0**.

* The GeoServer REST API has been updated, and includes improved `documentation <geoserver/rest/index.html>`_. The updated API should still be backwards-compatible.

* The :ref:`WPS <processing.intro.wps>` and :ref:`MBTiles <dataadmin.mbtiles>` extensions have been removed, and are instead included by default.

* The :ref:`GeoServices REST API <intro.extensions.gsr>` extension has been added. It adds a REST API which allows limited compatiblity with ArcGIS clients.

* The :ref:`Printing <intro.extensions.printng>` extension has been added. It provides a REST API for rendering documents as images or PDFs.

* Improved **support for MBstyle** `markup language <geoserver/styling/mbstyle/index.html>`_, including a new :ref:`tutorial <cartography.mbstyle.tutorial>`.

* **Layer groups have a new entry type** called `style group <geoserver/data/webadmin/layergroups.html>`_ for defining a layer group based on the contents of a style document.

**Components included:**

* GeoServer 2.12
* GeoWebCache 1.12
* GeoTools 18
* PostgreSQL 9.6
* PostGIS 2.3
* GDAL 1.11.5

.. _sysadmin.ubuntu:

Administration on Ubuntu
========================

This document contains information about various tasks specific to Boundless Server for Ubuntu Linux. For more details, please see the :ref:`sysadmin` section.

.. note:: This section is also applicable to those running the Boundless Server virtual machine on any operating system.

Starting and stopping Boundless Server services
-----------------------------------------------

Boundless Server is comprised of two main services:

#. The `Tomcat <http://tomcat.apache.org/>`_ web server that contains all the web applications such as GeoServer and GeoWebCache. 

#. The `PostgreSQL <http://www.postgresql.org/>`_ database server with the PostGIS spatial extensions.

.. note:: The Tomcat service used by Boundless Server is pulled in from standard repository sources, and (other than configuration) is not specific to Boundless Server.

Controlling the Tomcat service
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To start|stop|restart the Tomcat service:

.. code-block:: bash
 
   service tomcat8 start|stop|restart

Other options in addition to the above are ``try-restart``, ``force-restart``, and ``status``.

Controlling the PostgreSQL service
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To start|stop|restart the PostgreSQL service:

.. code-block:: bash
 
   service postgresql start|stop|restart

Other options in addition to the above are ``reload``, ``force-reload``, and ``status``.

.. note:: If you have multiple versions of PostgresSQL installed you can specify which version to control with a third argument. For example:

   .. code-block:: bash

      sudo service postgresql start 9.6

Service port configuration
--------------------------

The Tomcat and PostgreSQL services run on ports **8080** and **5432** respectively. These ports can often conflict with existing services on the system, in which case the ports must be changed. 

Changing the Tomcat port
^^^^^^^^^^^^^^^^^^^^^^^^

To change the Tomcat port:

#. Edit the file :file:`/etc/tomcat8/server.xml`. 

#. Search for the following line::

    <Connector port="8080" protocol="HTTP/1.1"

#. Change the port value.

#. Save the file.

#. Restart Tomcat.

.. note:: If you are using a virtual machine, you will need to adjust the port forwarding settings in your virtualization software to ensure that the new port is accessible to the host machine.

Changing the PostgreSQL port
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To change the PostgreSQL port:

#. Edit the file :file:`/etc/postgresql/9.6/main/postgresql.conf`.

#. Search or the ``port`` property and change it to the desired value.

#. Restart PostgreSQL.

.. note:: If you are using a virtual machine, you will need to adjust the port forwarding settings in your virtualization software to ensure that the new port is accessible to the host machine.

Working with Tomcat
-------------------

Changing the Tomcat Java
^^^^^^^^^^^^^^^^^^^^^^^^

If you wish to use the Oracle Java 8 JRE (rather than the OpenJDK 8 installed by default):

#. Download and install Oracle Java 8 JRE.

#. Open :file:`/etc/tomcat8/tomcat8.conf` and update the ``JAVA_HOME`` environment variable.::
      
      export $JAVA_HOME=/usr/lib/jvm/jre1.8.0_77

   .. note:: Make sure the line is uncommented (does not start with ``#``).

#. Save and close the file.

#. Restart Tomcat.

Adding other system parameters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can add other system or application-specific parameters that will be picked up upon restart.

* To provide an environmental variable, open :file:`/etc/tomcat8/tomcat8.conf` in a text editor, add the desired parameters to the bottom of the file, such as ``GDAL_DATA=/usr/share/gdal``.
   
* System properties are read in from the files in :file:`/etc/tomcat8/server-opts/`. So you could create a text file named :file:`memory` and populate it with ``-Xmx2G``. See the section on :ref:`sysadmin.startup`.

* Context Parameters are application-specific, and are read in from the files in :file:`/etc/tomcat8/Catalina/localhost/`. All parameters should be under the top-level ``<Context>`` tag. For example, the GeoServer data directory context parameter in :file:`/etc/tomcat8/Catalina/localhost/geoserver.xml` looks like this:

  .. code-block:: xml

     <Parameter name="GEOSERVER_DATA_DIR" 
          value="/var/opt/boundless/server/geoserver/data" override="false"/>

After making any changes, be sure to restart Tomcat.

Working with GeoServer
----------------------

GeoServer data directory
^^^^^^^^^^^^^^^^^^^^^^^^

The **GeoServer data directory** is the location on the file system where GeoServer stores all of its configuration, and (optionally) file-based data. By default, this directory is located at: :file:`/var/opt/boundless/server/geoserver/data`. 

To point GeoServer to an alternate location:

#. Edit the file :file:`/etc/tomcat8/Catalina/localhost/geoserver.xml`.

   Define ``GEOSERVER_DATA_DIR`` with an appropriate value accordingly.
   
   .. code-block:: xml
      
      <Parameter name="GEOSERVER_DATA_DIR" 
        value="/var/opt/boundless/server/geoserver/data" override="false"/>

#. Restart Tomcat.

Enabling spatial reference systems with Imperial units
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A fix is available for spatial reference systems measured in Imperial units (feet). This setting is recommended for all users, and strongly recommended for those working with **US State Plane** projections measured in feet.

To enable this fix:

#. Add the following parameter to :file:`/etc/tomcat8/server-opts/scale`
   
   .. code-block:: bash
      
      -Dorg.geotoools.render.lite.scale.unitCompensation=true

#. Restart Tomcat.

Update GeoJSON output
^^^^^^^^^^^^^^^^^^^^^
 
GeoServer GeoJSON output is now provided in x/y/z order as required by the specification. In addition, the ``crs``  output has changed to support full URN representation of spatial reference systems::

      "crs": {
         "type": "name",
         "properties": {
            "name": "urn:ogc:def:crs:EPSG::4326"
         }
      }

.. note::

   Previously, the output was::
   
         "crs": {
            "type": "EPSG",
            "properties": {
               "code": "4326"
            }
         }
   
To restore the previous ``crs`` representation for compatibility reasons (especially when working with OpenLayers):

#. Navigate to :file:`/etc/tomcat8/Catalina/localhost/` and edit the file :file:`geoserver.xml`.

#. Add the following context parameter to :file:`geoserver.xml`:

   .. code-block:: xml

     <Parameter name="GEOSERVER_GEOJSON_LEGACY_CRS" value="true" override="false"/>      

#. Restart Tomcat.

PostgreSQL configuration
------------------------

PostgreSQL configuration is controlled within the ``postgresql.conf`` file. This file is located at :file:`/etc/postgresql/9.6/main/postgresql.conf`. 

You will want to ensure that you can connect to the database. Please see the section on :ref:`dataadmin.pgGettingStarted.firstconnect` to set this up.

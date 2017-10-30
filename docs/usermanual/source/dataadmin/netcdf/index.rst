.. _dataadmin.netcdf:

Working with NetCDF data
========================

Boundless Server supports data saved in `NetCDF <http://www.unidata.ucar.edu/software/netcdf/>`_ format. NetCDF is a community standard for sharing scientific data.

This data can be loaded and published through GeoServer. Both NetCDF 3 and NetCDF 4 formats are supported.

.. note:: :ref:`GRIB <dataadmin.grib>` is also supported via a separate extension. 

GeoServer also has the ability to add NetCDF as an output format.

.. note:: There are different extensions for NetCDF: One to create a **data store** (reading), one to provide an **output format**, and one to provide an output format specifically for **NetCDF 4**.

Installing NetCDF support
-------------------------

NetCDF support isn't enabled by default, so it must be separately installed through an extension.

Installation instructions are dependent on your operating system and method of install:

* **Tomcat**: :ref:`Ubuntu <install.ubuntu.tomcat.geoserver.binary>`, :ref:`Red Hat <install.redhat.tomcat.geoserver.binary>`, :ref:`Windows <install.windows.tomcat.extensions.netcdf>`
* **Packages**: Install the :file:`boundless-server-gs-netcdf` and :file:`boundless-server-gs-netcdf-out` packages: :ref:`Ubuntu <install.ubuntu.packages.list>`, :ref:`Red Hat <install.redhat.packages.list>` 

.. note:: The Boundless Server virtual machine has most extensions pre-installed.

Verifying installation
----------------------

.. include:: ../../install/include/ext/netcdf_verify.txt

.. include:: ../../install/include/ext/netcdf-out_verify.txt

.. include:: ../../install/include/ext/netcdf4-out_verify.txt

For more information on adding a store and publishing layers, please see the `GeoServer documentation for NetCDF <../../geoserver/extensions/netcdf/netcdf.html>`_.

.. _dataadmin.netcdf.uninstall:

Uninstallation
--------------

When you install NetCDF-out, your :file:`global.xml` file is edited. This file is located in the root of your GeoServer data directory.

When uninstalling NetCDF-out, the information in this file needs to be removed manually.

.. warning:: Failure to manually edit the :file:`global.xml` file to remove the NetCDF-out content may result in GeoServer failing to load.

#. Open :file:`global.xml` in a text editor.

#. Find the section containing ``NetCDFOutput.Key`` and delete it. It will look something like this:

   .. code-block:: xml

      <metadata>
        <map>
          <entry>
            <string>NetCDFOutput.Key</string>
            <netCDFSettings>
              <compressionLevel>0</compressionLevel>
              <shuffle>true</shuffle>
              <dataPacking>NONE</dataPacking>
            </netCDFSettings>
          </entry>
        </map>
      </metadata>

#. Save and close the file.

#. Restart the server.
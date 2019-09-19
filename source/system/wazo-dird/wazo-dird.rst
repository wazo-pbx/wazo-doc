.. _wazo-dird:

=========
wazo-dird
=========

wazo-dird is the directory server for Wazo. It offers a simple REST interface to query all
directories that are configured. wazo-dird is extendable with plugins.

.. toctree::
   :maxdepth: 1

   configuration
   developer
   stock_plugins


.. _wazo-dird-usage:

Launching wazo-dird
===================

::

   usage: wazo-dird [-h] [-c CONFIG_FILE] [-d] [-f] [-l LOG_LEVEL] [-u USER]

   optional arguments:
     -h, --help            show this help message and exit
     -c CONFIG_FILE, --config-file CONFIG_FILE
                           The path where is the config file. Default: /etc/wazo-dird/config.yml
     -d, --debug           Log debug messages. Overrides log_level. Default:
                           False
     -f, --foreground      Foreground, don't daemonize. Default: False
     -l LOG_LEVEL, --log-level LOG_LEVEL
                           Logs messages with LOG_LEVEL details. Must be one of:
                           critical, error, warning, info, debug. Default: info
     -u USER, --user USER  The owner of the process.


Terminology
===========

Back-end
--------

A back-end is a connector to query a specific type of directory, e.g. one back-end to query LDAP
servers, another back-end to query CSV files, etc.


Source
------

A source is an instance of a back-end. One backend may be used multiples times to query multiple
directories of the same type. For example, I could have the customer-csv and the employee-csv
sources, each using the CSV back-end, but reading a different file.


Plugins
-------

A plugin is an extension point in wazo-dird. It is a way to add or modify the
functionality of wazo-dird. There are currently three types of plugins:

* Back-ends to query different types of directories (LDAP, CSV, etc.)
* Services to provide different directory actions (lookup, reverse lookup, etc.)
* Views to expose directory results in different formats (JSON, XML, etc.)


API
===

See http://api.wazo.community, section Wazo Dird.

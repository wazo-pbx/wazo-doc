.. _wazo-confgend-developer:

===============================
Wazo confgend developer's guide
===============================

wazo-confgend uses drivers to implement the logic required to generate
configuration files.  It uses `stevedore <http://docs.openstack.org/developer/stevedore/>`_
to do the driver instantiation and discovery.

Plugins in wazo-confgend use setuptools' entry points. That means that
installing a new plugin to wazo-confgend requires an entry point in the plugin's
*setup.py*.


Drivers
-------

Driver plugin are classes that are used to generate the content of a
configuration file.

The implementation of a plugin should have the following properties.

#. It's ``__init__`` method should take one argument
#. It should have a ``generate`` method which will return the content of the file
#. A setup.py adding an entry point


The ``__init__`` method argument is the content of the configuration of
wazo-confgend. This allows the driver implementor to add values to the
configuration in ``/etc/wazo-confgend/conf.d/*.yml`` and these values will be
available in the driver.

The generate method has no argument, the configuration provided to the
``__init__`` should be sufficient for most cases. ``generate`` is called within a
``scoped_session`` of xivo-dao, allowing the usage of xivo-dao without prior setup
in the driver.

The namespaces used for entry points in wazo-confgend have the following form:

    wazo_confgend.<resource>.<filename>

as an example, a generator for sip.conf would have the following namespace:

    wazo_confgend.asterisk.sip.conf

Example
-------

Here is a typical ``setup.py``:

.. code-block:: python
   :linenos:

   #!/usr/bin/env python
   # -*- coding: utf-8 -*-
   # Copyright 2016 The Wazo Authors  (see the AUTHORS file)
   # SPDX-License-Identifier: GPL-3.0-or-later

   from setuptools import setup
   from setuptools import find_packages


   setup(
       name='Wazo confgend driversample',
       version='0.0.1',

       description='An example driver',

       packages=find_packages(),

       entry_points={
           'wazo_confgend.asterisk.sip.conf': [
               'my_driver = src.driver:MyDriver',
           ],
       }
   )

With the following package structure::

   .
   ├── setup.py
   └── src
       └── driver.py


``driver.py``:

.. code-block:: python
   :linenos:

   # -*- coding: utf-8 -*-
   # Copyright 2016 The Wazo Authors  (see the AUTHORS file)
   # SPDX-License-Identifier: GPL-3.0-or-later


   class MyDriver(object):

       def __init__(self, config):
           self._config = config

       def generate(self):
           return 'Hello World!'


To enable this plugin, you need to:

#. Install the plugin with::

    python setup.py install

#. Create a config file in ``/etc/wazo-confgend/conf.d``::

    plugins:
      asterisk.sip.conf: my_driver

#. Restart wazo-confgend::

    systemctl restart wazo-confgend.service

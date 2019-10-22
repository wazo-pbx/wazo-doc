Upgrade from Wazo < 18.01
=========================

Those procedures are valid if your Wazo installation is newer than 16.08 and older than 18.01.

Upgrade to Wazo < 18.01
-----------------------

Example to upgrade to Wazo 17.02::

  xivo-dist wazo-17.02
  apt-get update
  apt-get install xivo-upgrade/wazo-17.02
  wazo-upgrade
  xivo-dist phoenix


Upgrade to Wazo >= 18.01
------------------------

Example to upgrade to Wazo 18.02::

  wazo-upgrade

This will upgrade your xivo to Wazo 17.17. From there:

1. Read the :ref:`upgrade notes<upgrade-notes-stretch>`
2. upgrade to Wazo 18.02::


     wazo-dist-upgrade -t wazo-18.02
     wazo-dist phoenix-stretch


.. note:: Upgrading to a specific version between 18.03 and 19.12 is not supported


My Wazo is stuck in a specific version
--------------------------------------

Procedures for upgrading to specific versions may freeze the version of your xivo. Run the following commands to get the latest updates::

  xivo-dist phoenix
  wazo-upgrade

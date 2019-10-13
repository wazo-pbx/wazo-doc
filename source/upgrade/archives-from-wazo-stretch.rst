Upgrade from Wazo < 19.04
=========================

Those procedures are valid if your Wazo installation is newer than 18.01 and older than 19.04.


Upgrade to Wazo <= 18.03
------------------------

Example to upgrade to Wazo 18.03::

  wazo-dist -a wazo-18.03
  apt-get update
  apt-get install xivo-upgrade/wazo-18.03
  wazo-upgrade
  wazo-dist -m phoenix-stretch


Upgrade to Wazo < 19.12
-----------------------

Not supported


Upgrade to Wazo < 19.13
-----------------------

Example to upgrade to Wazo 19.12::

  wazo-upgrade


Upgrade to Wazo >= 19.13
------------------------

Example to upgrade to Wazo 19.13::

  wazo-upgrade

This will upgrade your Wazo to 19.12. From there:

1. Read the :ref:`upgrade notes<upgrade_notes_buster>`
2. Upgrade to Wazo 19.13::

     wazo-dist-upgrade -t wazo-19.13
     wazo-dist -m pelican-buster


My Wazo is stuck in a specific version
--------------------------------------

Procedures for upgrading to specific versions may freeze the version of your Wazo. Run the following
commands to get the latest updates::

  wazo-dist phoenix-stretch
  wazo-upgrade

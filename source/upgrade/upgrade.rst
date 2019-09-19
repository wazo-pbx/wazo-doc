.. _upgrade:

*********
Upgrading
*********

Upgrading a Wazo is done by executing commands through a terminal on the
server. You can connect to the server either through SSH or with a physical
console.

To upgrade your Wazo to the latest version, you **must** use the ``wazo-upgrade`` script. You can
start an upgrade with the command::

   wazo-upgrade

.. note::
   * You can't use wazo-upgrade if you have not run the wizard yet
   * Upgrading from a :ref:`deprecated version<deprecated_versions>` is not supported.
   * When upgrading Wazo, you **must** also upgrade **all** associated Wazo Clients. There is
     currently no retro-compatibility on older Wazo Client versions. The only exception is Wazo
     16.16, which is compatible with Wazo Client 16.13.

This script will update Wazo and restart all services.

There are 2 options you can pass to wazo-upgrade:

* ``-d`` to only download packages without installing them. **This will still upgrade the package containing wazo-upgrade**.
* ``-f`` to force upgrade, without asking for user confirmation

``wazo-upgrade`` uses the following environment variables:

* ``WAZO_CONFD_PORT`` to set the port used to query the :ref:`HTTP API of wazo-confd <confd-api>`
  (default is 9486)


Upgrade procedure
=================

* Read all existing :ref:`upgrade-notes` starting from your version to the latest version.
* For custom setups, follow the required procedures described below (e.g. HA cluster).
* To download the packages beforehand, run ``wazo-upgrade -d``. This is not mandatory, but it does
  not require stopping any service, so it may be useful to reduce the downtime of the server while
  upgrading.
* When ready, run ``wazo-upgrade`` which will start the upgrade process. **Telephony services will
  be stopped during the process**
* When finished, check that all services are running (the list is displayed at the end of the upgrade).
* Check that services are correctly working like SIP registration, ISDN link status,
  internal/incoming/outgoing calls, Wazo Client connections etc.


.. _version_specific_upgrade:

Version-specific upgrade procedures
===================================

Upgrading from XiVO 16.13 and before
------------------------------------

When upgrading from XiVO 16.13 or before, you must use the special :ref:`XiVO to Wazo upgrade
procedure <upgrading-to-wazo>` instead of simply running ``xivo-upgrade``.


.. _upgrading-a-cluster:

Upgrading a cluster
===================

Here are the steps for upgrading a cluster, i.e. two Wazo with :ref:`high-availability`:

#. On the master : deactivate the database replication by commenting the cron in
   :file:`/etc/cron.d/xivo-ha-master`
#. On the slave, deactivate the xivo-check-master-status script cronjob by commenting the line in
   :file:`/etc/cron.d/xivo-ha-slave`
#. On the slave, start the upgrade::

    xivo-slave:~$ wazo-upgrade

#. When the slave has finished, start the upgrade on the master::

    xivo-master:~$ wazo-upgrade

#. When done, launch the database replication manually::

    xivo-master:~$ xivo-master-slave-db-replication <slave ip>

#. Reactivate the cronjobs (see steps 1 and 2)


Upgrading to a specific version of Wazo
=======================================

.. toctree::
   :maxdepth: 1

   archives


Upgrading from i386 (32 bits) to amd64 (64 bits)
================================================

.. toctree::
   :maxdepth: 1

   migrate_i386_to_amd64


Unsupported versions
====================

.. toctree::
   :maxdepth: 1

   version_deprecation_policy


Troubleshooting
===============

Postgresql
----------

When upgrading Wazo, if you encounter problems related to the system locale, see
:ref:`postgresql_localization_errors`.


wazo-upgrade
------------

If wazo-upgrade fails or aborts in mid-process, the system might end up in a faulty condition. If in
doubt, run the following command to check the current state of xivo's firewall rules::

   iptables -nvL

If, among others, it displays something like the following line (notice the DROP and 5060)::

   0     0 DROP       udp  --  *      *       0.0.0.0/0            0.0.0.0/0           udp dpt:5060

Then your Wazo will not be able to register any SIP phones. In this case, you must delete the DROP
rules with the following command::

   iptables -D INPUT -p udp --dport 5060 -j DROP

Repeat this command until no more unwanted rules are left.


Upgrade notes
=============

.. toctree::
   :maxdepth: 2

   upgrade_notes

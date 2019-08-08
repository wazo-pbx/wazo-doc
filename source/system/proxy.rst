*******************
Proxy Configuration
*******************

If you use Wazo behind an HTTP proxy, you must do a couple of manipulations for
it to work correctly.


apt
===

Create the :file:`/etc/apt/apt.conf.d/90proxy` file with the following content::

   Acquire::http::Proxy "http://domain\username:password@proxyip:proxyport";


provd
=====

Proxy information is set with ``wazo-provd`` endpoint ``/provd/configuration/http_proxy``.


dhcp-update
===========

*This step is needed if you use the DHCP server of the Wazo. Otherwise the DHCP configuration won't be correct.*

Proxy information is set via the :file:`/etc/xivo/dhcpd-update.conf` file.

Edit the file and look for the ``[proxy]`` section.


xivo-fetchfw
============

*This step is not needed if you don't use xivo-fetchfw.*

Proxy information is set via the :file:`/etc/xivo/xivo-fetchfw.conf` file.

Edit the file and look for the ``[proxy]`` section.

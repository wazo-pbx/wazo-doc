.. _intro-provisioning:

************
Introduction
************

The auto-provisioning feature found in Wazo make it possible to provision, i.e.
configure, a lots of telephony devices in an efficient and effortless way.


How it works
============

Here's a simplified view of how auto-provisioning is supported on a typical SIP hardphone:

#. The phone is powered on
#. During its boot process, the phone sends a DHCP request to obtain its network configuration
#. A DHCP server replies with the phone network configuration + an HTTP URL
#. The phone use the provided URL to retrieve a common configuration file, a
   MAC-specific configuration file, a firmware image and some language files.

Building on this, configuring one of the supported phone on Wazo is as simple as:

#. :ref:`dhcpd-config`
#. :ref:`Installing the required provd plugin <provd-plugins-mgmt>`
#. Powering on the phone
#. Dialing the user's provisioning code from the phone

And *voila*, once the phone has rebooted, your user is ready to make and receive calls.
No manual editing of configuration files nor fiddling in the phone's web interface.


Tenant assignation
==================

On initial insertion into provd, devices are assigned to the tenant of the token used internally by
provd, which is the master tenant. When a device is provisioned, it is transferred to the tenant of
the line to which it is being associated. When the device is reset to autoprov, the device stays in
its tenant. It is not possible to change the tenant of the device once it is set. If you wish to do
it anyway, you must delete the device and restart it manually.


Limitations
===========

* Device synchronisation does not work in the situation where multiple devices are connected from
  behind a NAPT network equipment. The devices must be resynchronised manually.
* There may be an issue if you are using an analog gateway with lines that are not in the same
  tenant. Indeed, in the case that the gateway is only one device and each port is a separate line,
  the device will only be seen by the tenant of the first line that was added.


External links
==============

* `Introduction to provd plugin model <http://blog.wazo.community/introduction-to-the-plugin-model-of-the-new-provisioning-server.html>`_
* `HTTP/TFTP requests processing in provd - part 1 <http://blog.wazo.community/httptftp-requests-processing-in-provd-part-1.html>`_
* `HTTP/TFTP requests processing in provd - part 2 <http://blog.wazo.community/httptftp-requests-processing-in-provd-part-2.html>`_

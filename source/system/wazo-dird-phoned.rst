.. _wazo-dird-phoned:

================
wazo-dird-phoned
================

wazo-dird-phoned is an interface to use directory service with phone. It offers a simple REST
interface to authenticate a phone and search result from :ref:`wazo-dird`.


Usage
=====

wazo-dird-phoned is used through HTTP requests, using HTTP and HTTPS. Its default port is 9498
and 9499. As a user, the common operation is to search through directory from a phone. The phone
need to send 2 informations:

* `xivo_user_uuid`: The Wazo user uuid that the phone is associated. It's used to search
  through personal contacts (see :ref:`dird_services_personal`).
* `profile`: The profile that the user is associated. It's used to format results as configured.

.. note:: Since most phones dont't support HTTPS, a small protection is to configure
          authorized_subnets in :ref:`configuration-files`


Launching wazo-dird-phoned
==========================

On command line, type ``wazo-dird-phoned -h`` to see how to use it.

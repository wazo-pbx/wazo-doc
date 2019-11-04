.. _ethernet:

**********************
Ethernet configuration
**********************

Discover interfaces
===================

Query
-----

::

    GET /discover_netifaces

Example request
---------------

::

   GET /discover_netifaces HTTP/1.1
   Host: wazoserver
   Accept: application/json


Example response
----------------

::

    HTTP/1.1 200 OK
    Content-Type: application/json
    {
       "lo":
       {
           "hwaddress": "00:00:00:00:00:00",
           "typeid": 24,
           "alias-raw-device": null,
           "network": "127.0.0.0",
           "family": "inet",
           "physicalif": false,
           "vlan-raw-device": null,
           "vlanif": false,
           "dummyif": false,
           "mtu": 65536,
           "broadcast": "127.255.255.255",
           "hwtypeid": 772,
           "netmask": "255.0.0.0",
           "carrier": true,
           "flags": 9,
           "address": "127.0.0.1",
           "vlan-id": null,
           "type": "loopback",
           "options": null,
           "aliasif": false,
           "name": "lo"
       },
       "eth0":
       {
           "alias-raw-device": null,
           "family": "inet",
           "hwaddress": "36:76:70:29:69:c2",
           "vlan-id": null,
           "network": "172.17.0.0",
           "physicalif": false,
           "vlan-raw-device": null,
           "vlanif": false,
           "type": "eth",
           "aliasif": false,
           "broadcast": "172.17.255.255",
           "netmask": "255.255.0.0",
           "address": "172.17.0.101",
           "typeid": 6,
           "name": "eth0",
           "hwtypeid": 1,
           "dummyif": false,
           "mtu": 1500,
           "carrier": true,
           "flags": 3,
           "options": null
       }
    }




Get interface
=============

Query
-----

::

    GET /netiface/<interface>

Example request
---------------

::

    GET /netiface/eth0 HTTP/1.1
    Host: wazoserver
    Content-Type: application/json

Example response
----------------

::

    HTTP/1.1 200 OK
    Content-Type: application/json
    {
       "eth0":
       {
           "alias-raw-device": null,
           "family": "inet",
           "hwaddress": "36:76:70:29:69:c2",
           "vlan-id": null,
           "network": "172.17.0.0",
           "physicalif": false,
           "vlan-raw-device": null,
           "vlanif": false,
           "type": "eth",
           "aliasif": false,
           "broadcast": "172.17.255.255",
           "netmask": "255.255.0.0",
           "address": "172.17.0.101",
           "typeid": 6,
           "name": "eth0",
           "hwtypeid": 1,
           "dummyif": false,
           "mtu": 1500,
           "carrier": true,
           "flags": 3,
           "options": null
       }
    }


Change state
============

Query
-----

::

    PUT /change_state_eth_ipv4

Example request
---------------

::

    PUT /change_state_eth_ipv4 HTTP/1.1
    Host: wazoserver
    Content-Type: application/json
    {
        'ifname' : 'eth0',
        'state': True
    }

Delete interface ipv4
=====================

Query
-----

::

    GET /delete_eth_ipv4/<interface>

Example request
---------------

::

    GET /delete_eth_ipv4/eth0 HTTP/1.1
    Host: wazoserver
    Content-Type: application/json

*************
Function Keys
*************

Function keys can be used as shortcuts for dialing a number, or accomplishing other menial tasks, by pushing a button on the phone. A function key's action is determined by its destination.

Function keys can be added directly on a user, or in a template. Templates are useful for creating a set of common function keys that can be used by the same group of people.

This page only describes the data models used by the REST API. Consult the `API documentation <http://api.wazo.community>`_ for further details on URLs.

Function Key Template
=====================

Parameters
----------

+-------+-----------------+----------+---------------------------------------------------------------------------+
| Field | Type            | Required | Description                                                               |
+=======+=================+==========+===========================================================================+
| name  | string          | No       | A name for the template.                                                  |
+-------+-----------------+----------+---------------------------------------------------------------------------+
| keys  | `Function Key`_ | No       | A collection of function keys under the form ``{"position": "funckey"}``. |
|       |                 |          | See the example for more details.                                         |
+-------+-----------------+----------+---------------------------------------------------------------------------+

Example
-------

::

   {
       "name": "Example template",
       "keys": {
            "1": {
               "destination": {
                    "type": "user",
                    "user_id": 34
                }
            },
            "2": {
                "blf": true,
                "label": "Call mom",
                "destination": {
                    "type": "custom",
                    "exten": "5551234567"
                }
            }
        }
    }


Function Key
============

Description
-----------

+-------------+----------------+----------+-------------------------------------------------------+
| Field       | Type           | Required | Description                                           |
+=============+================+==========+=======================================================+
| blf         | boolean        | No       | Turn on BLF when there is activity on the destination |
+-------------+----------------+----------+-------------------------------------------------------+
| label       | string         | No       | Label to display next to the function key             |
+-------------+----------------+----------+-------------------------------------------------------+
| destination | `Destination`_ | Yes      | Destination to call                                   |
+-------------+----------------+----------+-------------------------------------------------------+


Example
-------

::

   {
       "blf": True,
       "label": "Call john",
       "destination": {
            "type": "user",
            "user_id": 34
        }
    }

Destination
===========

A destination determines the number to dial when using a function key. Destinations are composed of a parameter named
``type`` and any additional parameters required by its type.

Available destination types:

agent
    An agent

bsfilter
    Boss/Secretary filter

conference
    Conference room

custom
    A custom number to dial

forward
    Forward a call towards another number

group
    A group

groupmember
    Join or leave a group

onlinerec
    Record a conversation during a call

paging
    A paging

park
    Park a call

park_position
    Pick up a parked call

queue
    Call queue

service
    A call service

transfer
    Transfer a call

user
    A User



Here are the parameters required for each destination:

Agent
-----

+----------+----------------------+----------------------------+
| Field    | Type                 | Value                      |
+==========+======================+============================+
| agent_id | numeric              | Agents's id                |
| action   | login/logout/toggle  | What to do with this agent |
+----------+----------------------+----------------------------+

BSFilter
--------

+------------------+---------+-------------------------+
| Field            | Type    | Value                   |
+==================+=========+=========================+
| filter_member_id | numeric | ID of the filter member |
+------------------+---------+-------------------------+

Conference
----------

+---------------+---------+-----------------+
| Field         | Type    | Value           |
+===============+=========+=================+
| conference_id | numeric | Conference's id |
+---------------+---------+-----------------+

Custom
------

+-------+--------+----------------+
| Field | Type   | Value          |
+=======+========+================+
| exten | string | Number to dial |
+-------+--------+----------------+

Forward
-------

+---------+--------+-----------------------------------------------------------------+
| Field   | Type   | Value                                                           |
+=========+========+=================================================================+
| forward | string | Type of forward. Possible values: busy, noanswer, unconditional |
+---------+--------+-----------------------------------------------------------------+
| exten   | string | Number to dial (optional)                                       |
+---------+--------+-----------------------------------------------------------------+

Group
-----

+----------+---------+------------+
| Field    | Type    | Value      |
+==========+=========+============+
| group_id | numeric | Group's id |
+----------+---------+------------+

Group Member
------------

+----------+-------------------+----------------------------+
| Field    | Type              | Value                      |
+==========+===================+============================+
| group_id | numeric           | Group's id                 |
| action   | join/leave/toggle | What to do with this group |
+----------+-------------------+----------------------------+

Online call recording
---------------------

No parameters are required for this destination

Paging
------

+-----------+---------+--------------+
| Field     | Type    | Value        |
+===========+=========+==============+
| paging_id | numeric | Pagings's id |
+-----------+---------+--------------+

Parking
-------

No parameters are required for this destination

Parking Position
----------------

+---------------+----------------+------------------------------------+
| Field         | Type           | Value                              |
+===============+================+====================================+
| position      | numeric string | Position of the parking to pick up |
+---------------+----------------+------------------------------------+


Queue
-----

+----------+---------+-----------+
| Field    | Type    | Value     |
+==========+=========+===========+
| queue_id | numeric | User's id |
+----------+---------+-----------+

Service
-------

+---------+--------+---------------------+
| Field   | Type   | Value               |
+=========+========+=====================+
| service | string | Name of the service |
+---------+--------+---------------------+

Currently supported services:

phonestatus
    Phone Status

recsnd
    Sound Recording

callrecord
    Call recording

incallfilter
    Incoming call filtering

enablednd
    Enable "Do not disturb" mode

pickup
    Group Interception

calllistening
    Listen to online calls

directoryaccess
    Directory access

fwdundoall
    Disable all forwaring

enablevm
    Enable Voicemail

vmusermsg
    Consult the Voicemail

vmuserpurge
    Delete messages from voicemail


Transfer
--------

+----------+--------+----------------------------------------------------+
| Field    | Type   | Value                                              |
+==========+========+====================================================+
| transfer | string | Type of transfer. Possible values: blind, attended |
+----------+--------+----------------------------------------------------+

User
----

+---------+---------+-----------+
| Field   | Type    | Value     |
+=========+=========+===========+
| user_id | numeric | User's id |
+---------+---------+-----------+

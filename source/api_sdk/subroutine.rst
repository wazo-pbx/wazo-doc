.. _subroutine:

**********
Subroutine
**********

What is it ?
============

The ``preprocess_subroutine`` allows you to enhance Wazo features through the Asterisk dialplan.
Features that can be enhanced are :

* ``/users``
* ``/groups``
* ``/queues``
* ``/conferences``
* ``/incalls``
* ``/outcalls``

There are three possible categories :

* Subroutine for one feature
* Subroutine for global forwarding
* Subroutine for global incoming call to an object

Subroutines are called at the latest possible moment in the dialplan, so that the maximum of
variables have already been set: this way, the variables can be read and modified at will before
they are used.

Here is an example of the dialplan execution flow when an external incoming call to a user being
forwarded to another external number (like a forward to a mobile phone):

.. figure:: images/subroutines.png
   :scale: 50%

   Where subroutines are called in dialplan


Adding new subroutine
=====================

Where
-----

You can write the subroutine:

* add/edit a file directly on the server in :file:`/etc/asterisk/extensions_extra.d`

.. note:: Since all configuration files will be merged together in the end, it does not matter in
          which file you write your subroutine. The different files are only here to find your way
          back more quickly than one big configuration file. So don't be afraid to create new files!

What
----

An example::

   [myexample]
   exten = s,1,NoOp(This is an example)
   same  =   n,Return()

Subroutines should always end with a ``Return()``. You may replace ``Return()`` by a ``Goto()`` if
you want to completely bypass the Wazo dialplan, but this is not recommended.

To plug your subroutine into the Wazo dialplan, you must add ``myexample`` in the
``preprocess_subroutine`` subroutine field of your object.


Global subroutine
=================

There is predefined subroutine for this feature, you can find the name and the activation in the
:file:`/etc/xivo/asterisk/xivo_globals.conf`. The variables are::

   ; Global Preprocess subroutine
   XIVO_PRESUBR_GLOBAL_ENABLE = 1
   XIVO_PRESUBR_GLOBAL_USER = xivo-subrgbl-user
   XIVO_PRESUBR_GLOBAL_AGENT = xivo-subrgbl-agent
   XIVO_PRESUBR_GLOBAL_GROUP = xivo-subrgbl-group
   XIVO_PRESUBR_GLOBAL_QUEUE = xivo-subrgbl-queue
   XIVO_PRESUBR_GLOBAL_MEETME = xivo-subrgbl-meetme
   XIVO_PRESUBR_GLOBAL_DID = xivo-subrgbl-did
   XIVO_PRESUBR_GLOBAL_OUTCALL = xivo-subrgbl-outcall
   XIVO_PRESUBR_GLOBAL_PAGING = xivo-subrgbl-paging

So if you want to add a subroutine for all of your Wazo users you can do this::

   [xivo-subrgbl-user]
   exten = s,1,NoOp(This is an example for all my users)
   same  =   n,Return()


Forward subroutine
==================

You can also use a global subroutine for call forward.

::

   ; Preprocess subroutine for forwards
   XIVO_PRESUBR_FWD_ENABLE = 1
   XIVO_PRESUBR_FWD_USER = xivo-subrfwd-user
   XIVO_PRESUBR_FWD_GROUP = xivo-subrfwd-group
   XIVO_PRESUBR_FWD_QUEUE = xivo-subrfwd-queue
   XIVO_PRESUBR_FWD_MEETME = xivo-subrfwd-meetme
   XIVO_PRESUBR_FWD_VOICEMAIL = xivo-subrfwd-voicemail
   XIVO_PRESUBR_FWD_SCHEDULE = xivo-subrfwd-schedule
   XIVO_PRESUBR_FWD_SOUND = xivo-subrfwd-sound
   XIVO_PRESUBR_FWD_CUSTOM = xivo-subrfwd-custom
   XIVO_PRESUBR_FWD_EXTENSION = xivo-subrfwd-extension


Dialplan variables
==================

Some of the Wazo variables can be used and modified in subroutines (non exhaustive list):

* ``WAZO_AUTO_ANSWER``: adds the SIP headers to auto answer the call automatically for supported devices.

* ``WAZO_CHANNEL_DIRECTION``: can have two values:

  * ``from-wazo`` when the channel was initiated by Wazo: the channel links Wazo to the called
    party. From Asterisk, this is an outbound channel. From the peer, this is an incoming call
  * ``to-wazo`` when the channel was initiated by the user: the channel links Wazo to the calling
    party. From Asterisk, this is an inbound channel. From the peer, this is an outgoing call.

  The default value is ``from-wazo``. If you write scripts using originates to place new calls, you
  should set ``WAZO_CHANNEL_DIRECTION`` to ``to-wazo`` on the originator channel.

* ``WAZO_DST_UUID``: the UUID of the user destination of the call. Only available when calling a user.

* ``WAZO_DST_TENANT_UUID``: the tenant UUID of the user destination of the call. Only available when calling a user.

* ``WAZO_TENANT_UUID``: the tenant UUID of the line that placed the call or receives the call.

* ``XIVO_CALLOPTIONS``: the value is a list of options to be passed to the Dial application, e.g.
  ``hHtT``. This variable is available in agent, user and outgoing call subroutines. Please note
  that it may not be set earlier, because it will be overwritten.

* ``XIVO_CALLORIGIN``: can have two values:

  * ``intern`` when the call does not involve DID or trunks, e.g. a simple call between two phones
    or one phone and its voicemail
  * ``extern`` when the call is received via a DID or emitted through an Outgoing Call

  This variable is used by wazo-agid when :ref:`selecting the ringtone <xivo_ring.conf>` for ringing
  a user. This variable is available only in user subroutines.

* ``XIVO_DSTNUM``: the value is the extension dialed, as received by Wazo (e.g. an internal
  extension, a DID, or an outgoing extension including the local prefix). This
  variable is available in all subroutines.

* ``XIVO_GROUPNAME``: the value is the name of the group being called. This variable is only
  available in group subroutines.

* ``XIVO_GROUPOPTIONS``: the value is a list of options to be passed to the Queue application, e.g.
  ``hHtT``. This variable is only available in group subroutines.

* ``XIVO_INTERFACE``: the value is the `Technology/Resource` pairs that are used as the first
  argument of the `Dial application <https://wiki.asterisk.org/wiki/display/AST/Asterisk+13+Application_Dial>`_.
  This variable is only available in the user subroutines.

* ``XIVO_MOBILEPHONENUMBER``: the value is the phone number of a user, as set in the web interface.
  This variable is only available in user subroutines.

* ``XIVO_QUEUENAME``: the value is the name of the queue being called. This variable is only
  available in queue subroutines.

* ``XIVO_QUEUEOPTIONS``: the value is a list of options to be passed to the Queue application, e.g.
  ``hHtT``. This variable is only available in queue subroutines.

* ``XIVO_RINGSECONDS``: the value is the number of seconds a user will ring before the call is
  forwarded elsewhere, or hungup if no forwards are configured. This variable can only be used in a
  User subroutine.

* ``XIVO_SRCNUM``: the value is the callerid number of the originator of the call: the internal
  extension of a user (outgoing callerid is ignored), or the public extension of an external
  incoming call. This variable is available in all subroutines.

* ``XIVO_USERID``: the user ID of the line that placed the call or receives the call

* ``XIVO_USERUUID``: the user UUID of the line that placed the call or receives the call

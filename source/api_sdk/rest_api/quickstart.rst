.. _rest-api-quickstart:

*******************
REST API Quickstart
*******************

Introduction
============

Wazo REST APIs are HTTP interfaces that allow you to programmatically interact with Wazo. In order
to access the REST APIs of Wazo, you need:

* a Wazo server up and running
* a browser
* somewhere you can copy-paste text (ids, tokens, etc.)


REST API Permissions
====================

First of all, you must have permission to use the REST API. Create a `wazo-auth` user and policy:

``POST /users {"purpose": "external_api", "username": "rest-api-test", ...}``
``POST /policies {"acl_templates": ["#"], ...}``
``PUT /users/{user_uuid}/policies/{policy_uuid}``

* ``acl_templates``: ``#`` is a wildcard that gives access to every REST API. You may want to delete
  this account when you're done, to reduce risks of unauthorized access.

Save the form, and store the login/password somewhere for later use.


Swagger UI
==========

In this article, we will use the Swagger Web UI, a small web application available in every Wazo
installation since XiVO 15.10.

In your browser, go to ``http://<wazo>/api``. You should see:

* a list of available APIs
* input boxes on the top, we will ignore those for now

The list of available APIs reflects the different modules of Wazo. Each module is a Python process
that serves its own REST API. We will concentrate on two of them:

* wazo-auth
* wazo-confd

wazo-auth is the daemon responsible for authentication. Every API is protected by a token-based
authentication mechanism. In order to use any REST API, we will need a valid authentication token,
obtained from wazo-auth.

wazo-confd is the daemon responsible for Wazo configuration. Its REST API allows you to read and
modify users, lines, extensions, groups, etc. This is the programatic equivalent of the Wazo web
interface. However, the wazo-confd REST API is not yet complete, and not all aspects of Wazo
configuration are available in wazo-confd.


HTTPS certificates
==================

Almost all REST APIs use encryption and are available via HTTPS. Unfortunately, Wazo does not come
with a trusted certificate. So you have to manually trust the self-signed certificate of your Wazo.
To that end:

#. Click on wazo-auth in the menu on the left.
#. You should see an error like::

    Can't read from server. It may not have the appropriate access-control-origin settings.

   This is expected. This is the kind of error (quite misleading, admittedly) you get when the
   certificate is not trusted.
#. Copy the URL you see in the text box at the top of the page, something like:
   ``https://wazo:9497/1.1/api/api.yml`` and paste it in your browser.
#. Accept the HTTPS certificate validation exception.
#. You should see a YAML text file describing the wazo-confd API.
#. Go back to ``http://wazo/api``.
#. Click on wazo-auth again.
#. Now you should see a list of sections for the wazo-auth REST API, like ``backends`` or ``token``
#. Repeat the whole procedure for wazo-confd (the port in the URL will be different, and the REST
   API description will take longer to load), and you should be ready to go.


Authentication token
====================

Let's ask wazo-auth for an authentication token:

#. Choose the ``wazo-auth`` service in the list of REST APIs
#. In the top-right text box of the page (left to the "Explore" button), fill "token" with the
   ``rest-api-test:password``: those credentials are the ones from the Web Services Access you
   created earlier.
#. Go to the ``POST /tokens`` section and click on the yellow box to the right of the ``body``
   parameter. This will pre-fill the ``body`` parameter.
#. In the ``body`` parameter, set:

   * ``expiration`` to the number of seconds for the token to be valid (e.g. 3600 for one hour). After
     the expiration time, you will need to re-authenticate to get a new token.

#. Click ``Try it out`` at the end of the section. This will make an HTTP request to wazo-auth.
#. You should see a response to your HTTP request, containing a JSON object. In the response, you
   should see a ``token`` attribute. That little string is your authentication token. Save it
   somewhere, in case you need it later.
#. Copy-paste the ``token`` attribute in the top-right input box, replacing the
   ``rest-api-test:password``. Note that you don't need to click the Explore button to accept the
   change of token.


Use the wazo-confd REST API
===========================

Now that we have an authentication token, we are ready to use the REST API.

#. Click on wazo-confd in the left menu
#. Choose a REST API endpoint, like :menuselection:`users --> GET /users` and click ``Try it out``


And that's it, you are ready to use any REST API with your authentication token.

.. note:: Be aware that this token will expire, and that you will need to get a new one when that
          happens. You can take a look at https://auth.wazo.community for an easier manual token generation
          process. Note that the ``auth.wazo.community`` server will never know the tokens that you
          generate, you browser will ask your Wazo directly.

.. warning:: Also, note that this authentication token gives **all permissions** to anyone who knows
             it. Same goes for the account password we created earlier. Remember to delete this
             account, or at least restrict permissions when you're done.


What's next
===========

* Check our :ref:`rest-api-examples` for more elaborate examples of how to use the REST APIs of
  Wazo.
* :ref:`rest-api-conventions` are also a good read
* Explore the REST API in Swagger, it also serves as the reference documentation for REST API.


Something went wrong...
=======================

Check :ref:`rest-api-troubleshooting`.

******
Consul
******

The default `consul <https://consul.io>`_ installation in Wazo uses the
configuration file in :file:`/etc/consul/xivo/*.json`. All files in this directory
are installed with the package and *should not* be modified by the
administrator. To use a different configuration, the adminstrator can add it's
own configuration file at another location and set the new configuration directory by creating a
systemd unit drop-in file in the :file:`/etc/systemd/system/consul.service.d` directory.

The default installation generates a master token that can be retrieved in
:file:`/var/lib/consul/master_token`. This master token will not be used if a new
configuration is supplied.


Variables
=========

The following environment variables can be overridden in a systemd unit drop-in file:

* ``CONFIG_DIR``: the consul configuration directory
* ``WAIT_FOR_LEADER``: should the "start" action wait for a leader ?

Example, in :file:`/etc/systemd/system/consul.service.d/custom.conf`::

   [Service]
   Environment=CONFIG_DIR=/etc/consul/agent
   Environment=WAIT_FOR_LEADER=no


Agent mode
==========

It is possible to run consul on another host and have the local consul node run
as an agent only.

To get this kind of setup up and running, you will need to follow the following steps.

Downloading Consul
------------------

For a 32 bits system

.. code-block:: sh

   wget --no-check-certificate https://releases.hashicorp.com/consul/0.5.2/consul_0.5.2_linux_386.zip


For a 64 bits system

.. code-block:: sh

   wget --no-check-certificate https://releases.hashicorp.com/consul/0.5.2/consul_0.5.2_linux_amd64.zip


Installing Consul on a new host
-------------------------------

.. code-block:: sh

   unzip consul_0.5.2_linux_386.zip

Or

.. code-block:: sh

   unzip consul_0.5.2_linux_amd64.zip

.. code-block:: sh

   mv consul /usr/bin/consul
   mkdir -p /etc/consul/xivo
   mkdir -p /var/lib/consul
   adduser --quiet --system --group --no-create-home \
           --home /var/lib/consul consul


Copying the consul configuration from the Wazo to a new host
------------------------------------------------------------

On the new consul host, modify :file:`/etc/consul/xivo/config.json` to include to following lines.

.. code-block:: javascript

   "bind_addr": "0.0.0.0",
   "client_addr": "0.0.0.0",
   "advertise_addr": "<consul-host>"

.. code-block:: sh

   # on the consul host
   scp root@<wazo-host>:/lib/systemd/system/consul.service /lib/systemd/system
   systemctl daemon-reload
   scp -r root@<wazo-host>:/etc/consul /etc
   scp -r root@<wazo-host>:/usr/share/xivo-certs /usr/share
   consul agent -data-dir /var/lib/consul -config-dir /etc/consul/xivo/

.. note:: To start consul with the systemd unit file, you may need to change owner and group
          (consul:consul) for all files inside :file:`/etc/consul`, :file:`/usr/share/xivo-certs`
          and :file:`/var/lib/consul`

Adding the agent configuration
------------------------------

Create the file :file:`/etc/consul/agent/config.json` with the following content

.. code-block:: javascript

    {
        "acl_datacenter": "<node_name>",
        "datacenter": "xivo",
        "server": false,
        "bind_addr": "0.0.0.0",
        "advertise_addr": "<wazo_address>",
        "client_addr": "127.0.0.1",
        "bootstrap": false,
        "rejoin_after_leave": true,
        "data_dir": "/var/lib/consul",
        "enable_syslog": true,
        "disable_update_check": true,
        "log_level": "INFO",
        "ports": {
            "dns": -1,
            "http": -1,
            "https": 8500
        },
        "retry_join": [
            "<remote_host>"
        ],
        "cert_file": "/usr/share/xivo-certs/server.crt",
        "key_file": "/usr/share/xivo-certs/server.key"
    }

* ``node_name``: Arbitrary name to give this node, ``wazo-paris`` for example.
* ``remote_host``: IP address of your new consul. Be sure the host is accessible from your Wazo and
  check the firewall. See the documentation :ref:`here <network>`.
* ``wazo_address``: IP address of your Wazo.

This file should be owned by consul user.

.. code-block:: sh

  chown -R consul:consul /etc/consul/agent


Enabling the agent configuration
--------------------------------

Add or modify :file:`/etc/systemd/system/consul.service.d/custom.conf` to include the following lines::

   [Service]
   Environment=CONFIG_DIR=/etc/consul/agent

Restart your consul server.

.. code-block:: sh

   service consul restart

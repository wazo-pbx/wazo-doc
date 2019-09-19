Upgrade an older xivo installation
==================================

Those procedures are valid if your xivo installation is older than 16.08.


Upgrade to Wazo < 18.01
-----------------------

Example upgrade to Wazo 17.02::

   # --no-check-certificate is needed only if you are affected by http://projects.wazo.community/issues/6024
   wget --no-check-certificate https://raw.githubusercontent.com/wazo-platform/wazo-upgrade/master/bin/xivo-to-wazo-upgrade
   chmod +x xivo-to-wazo-upgrade
   XIVO_TO_WAZO_DEB_LINE="deb http://mirror.wazo.community/archive wazo-17.02 main" ./xivo-to-wazo-upgrade
   xivo-dist phoenix


Upgrade to Wazo >= 18.01
------------------------

Example upgrade to Wazo 18.02::

   # --no-check-certificate is needed only if you are affected by http://projects.wazo.community/issues/6024
   wget --no-check-certificate https://raw.githubusercontent.com/wazo-platform/wazo-upgrade/master/bin/xivo-to-wazo-upgrade
   chmod +x xivo-to-wazo-upgrade
   ./xivo-to-wazo-upgrade

This will upgrade your xivo to Wazo 17.17. From there:

1. Read the :ref:`upgrade notes<upgrade-notes-stretch>`
2. upgrade to Wazo 18.02::

     wazo-dist-upgrade -t wazo-18.02
     wazo-dist phoenix-stretch


My xivo is stuck in a specific version
--------------------------------------

Procedures for upgrading to specific versions may freeze the version of your xivo. Run the following commands to get the latest updates::

   # --no-check-certificate is needed only if you are affected by http://projects.wazo.community/issues/6024
   wget --no-check-certificate https://raw.githubusercontent.com/wazo-platform/wazo-upgrade/master/bin/xivo-to-wazo-upgrade
   chmod +x xivo-to-wazo-upgrade
   ./xivo-to-wazo-upgrade

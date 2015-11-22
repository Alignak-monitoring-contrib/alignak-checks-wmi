Alignak checks package for Windows WMI
======================================

Checks pack for monitoring hosts with Windows Management Instrumentation (WMI)


Installation
----------------------------------------

The pack configuration files are to be copied to the monitoring objects configuration directory. The most suitable location is the *arbiter_cfg/objects/packs/* directory in the main alignak configuration directory.

**Note**: The main Alignak configuration directory is usually */usr/local/etc/alignak* or */etc/alignak* but it may depend upon your system and/or your installation.

The pack plugins (if any ...) are to be copied to the executable libraries directories.

**Note**: The Alignak librairies directory is usually */usr/local/libexec/alignak* or */var/lib/alignak* but it may depend upon your system and/or your installation.

From PyPI
~~~~~~~~~~~~~~~~~~~~~~~
To install the package from PyPI:
::
   pip install alignak-checks-wmi


From source files
~~~~~~~~~~~~~~~~~~~~~~~
To install the package from the source files:
::
   git clone https://github.com/Alignak-monitoring-contrib/alignak-checks-wmi
   cd alignak-checks-wmi
   mkdir /usr/local/etc/alignak/arbiter_cfg/objects/packs/wmi
   # Copy configuration files
   cp -R alignak_checks_wmi/*.cfg /usr/local/etc/alignak/arbiter_cfg/objects/packs/wmi
   # Copy plugin files
   cp -R alignak_checks_wmi/plugins/*.py /usr/local/libexec/alignak


Documentation
----------------------------------------

To be completed


Bugs, issues and contributing
----------------------------------------

Contributions to this project are welcome and encouraged ... issues in the project repository are the common way to raise an information.

License
----------------------------------------

Alignak Pack EXAMPLE is available under the `GPL version 3 license`_.

.. _GPL version 3 license: http://opensource.org/licenses/GPL-3.0
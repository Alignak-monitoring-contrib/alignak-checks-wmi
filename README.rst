Alignak checks package for Windows WMI
======================================

Checks pack for monitoring hosts with Windows Management Instrumentation (WMI)


Installation
----------------------------------------

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

Configuration
~~~~~~~~~~~~~~~~~~~~~~~
Install the PERL dependencies for check_wmi_plus plugin
::
   su -
   apt-get install libnumber-format-perl
   apt-get install libconfig-inifiles-perl
   apt-get install libdatetime-perl


Edit the */usr/local/etc/alignak/arbiter_cfg/objects/packs/wmi/resources.cfg* file and configure the domain name, user name and password allowed to access remotely to the monitored hosts WMI.
::
   #-- Active Directory for WMI
   # Replace MYDOMAIN with your domain name or . for local user account
   $DOMAIN$=MYDOMAIN
   # Replace MYUSER with the WMI authorized user (domain or local user account)
   $DOMAINUSERSHORT$=MYUSER
   $DOMAINUSER$=$DOMAIN$\\$DOMAINUSERSHORT$
   # Replace MYPASSWORD with the WMI authorized user password
   $DOMAINPASSWORD$=MYPASSWORD

Prepare Windows host
~~~~~~~~~~~~~~~~~~~~~~~
Some operations are necessary on the Windows monitored hosts if WMI remote access is not yet activated.

Create a user account:

- username/password (example): alignak/alignak
- member of following groups: Administrators, Remote DCOM users
- Deactivate interactive login permissions (more secure)

Check that WMI and RPC services are started

The Windows Firewall must allow inbound trafic for:
   - Windows Firewall Remote Management (RPC)
   - Windows Management Instrumentation (DCOM-In)
   - Windows Management Instrumentation (WMI-In)

This page contains more information about remote WMI configuration: https://kb.op5.com/display/HOWTOs/Agentless+Monitoring+of+Windows+using+WMI

Test remote WMI access with the plugins files:
::
   # Basic wmic command ...
   $ /var/lib/alignak/libexec/wmic -U .\\alignak%alignak //192.168.0.20 'Select Caption From Win32_OperatingSystem'

   # Alignak plugin command ...
   $ /var/lib/alignak/libexec/check_wmi_plus.pl -H 192.168.0.20 -u ".\\alignak" -p "alignak" -m checkdrivesize -a '.'  -w 90 -c 95 -o 0 -3 1  --inidir=/var/lib/alignak/libexec


Alignak configuration
~~~~~~~~~~~~~~~~~~~~~~~

You simply have to tag the concerned hosts with the template `windows-wmi`. The main `windows-wmi` template declares macros used to configure the launched checks. The default values of these macros listed hereunder can be overriden in each host configuration.
::
   _DOMAIN                          $DOMAIN$
   _DOMAINUSERSHORT                 $DOMAINUSERSHORT$
   _DOMAINUSER                      $_HOSTDOMAIN$\\$_HOSTDOMAINUSERSHORT$
   _DOMAINPASSWORD                  $DOMAINPASSWORD$

   _WINDOWS_DISK_WARN               90
   _WINDOWS_DISK_CRIT               95
   _WINDOWS_EVENT_LOG_WARN          1
   _WINDOWS_EVENT_LOG_CRIT          2
   _WINDOWS_REBOOT_WARN             15min:
   _WINDOWS_REBOOT_CRIT             5min:
   _WINDOWS_MEM_WARN                80
   _WINDOWS_MEM_CRIT                90
   _WINDOWS_ALL_CPU_WARN            80
   _WINDOWS_ALL_CPU_CRIT            90
   _WINDOWS_CPU_WARN                80
   _WINDOWS_CPU_CRIT                90
   _WINDOWS_LOAD_WARN               10
   _WINDOWS_LOAD_CRIT               20
   _WINDOWS_NET_WARN                80
   _WINDOWS_NET_CRIT                90
   _WINDOWS_EXCLUDED_AUTO_SERVICES
   _WINDOWS_AUTO_SERVICES_WARN      0
   _WINDOWS_AUTO_SERVICES_CRIT      1
   _WINDOWS_BIG_PROCESSES_WARN      25

   #Default Network Interface
   _WINDOWS_NETWORK_INTERFACE       Ethernet

   # Now some alert level for a windows host
   _WINDOWS_SHARE_WARN              90
   _WINDOWS_SHARE_CRIT              95


To set a specific value for an host, declare the same macro in the host definition file.
::
   define host{
      use                     windows-wmi
      contact_groups          admins
      host_name               sim-vm
      address                 192.168.0.16

      # Specific values for this host
      # Change warning and critical alerts level for memory
      # Same for CPU, ALL_CPU, DISK, LOAD, NET, ...
      _WINDOWS_MEM_WARN       10
      _WINDOWS_MEM_CRIT       20

      # Exclude some services from automatic start check
      # Use a regexp that matches against the short or long service name as it can be seen in the properties of the service in Windows.
      # The matching services are excluded in the resulting list.
      # Example: (ShortName)|(ShortName)| ... |(ShortName)
      _WINDOWS_EXCLUDED_AUTO_SERVICES (IAStorDataMgrSvc)|(MMCSS)|(ShellHWDetection)|(sppsvc)|(clr_optimization_v4.0.30319_32)
   }


Bugs, issues and contributing
----------------------------------------

Contributions to this project are welcome and encouraged ... issues in the project repository are the common way to raise an information.

License
----------------------------------------

Alignak Pack EXAMPLE is available under the `GPL version 3 license`_.

.. _GPL version 3 license: http://opensource.org/licenses/GPL-3.0
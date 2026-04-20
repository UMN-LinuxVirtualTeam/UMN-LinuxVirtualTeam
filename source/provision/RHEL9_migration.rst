Migrating to RHEL9
==================

This document provides an overview of the live migration process of an existing virtual machine(VM) to RHEL9.

---------------
Target Audience
---------------

This overview is targeted at Linux System Administrators for UMN VM environments. Basic Knowledge of SSH, software package management and network management is assumed. Command-line input examples will be in the following syntax:

::

  [current_sys]# echo "hello from $(hostname)"`

Unless specified otherwise, the example commands are executed with elevated (root) privileges. The example also presumes use of the BASH login shell. Potentially useful tips will be highlighted in the following format.

::

  --TIP START--
  
  "The secret of getting ahead is getting started."
  Mark Twain
  --TIP END--


--------------------------
Requirements / Assumptions
--------------------------

It is assumed that the system administrator has elevated permissions on the current production system. If the system is _not_ jointly managed, the first task would be to reach out to OIT_CONTACT and request to have the system converted to a jointly managed machine.

----------------------------------------
Provisioning Development Virtual Machine
----------------------------------------

Requesting a development environment as soon as possible allows making effective use of the time to wait while it is provisioned.

Requesting a dev box may be done by filling out the request for with IT services here.
`Request a Virtual server <https://it.umn.edu/services-technologies/resources/fully-managed-server-hosting-request-0>`_

The advised CPU and Memory resources is to match production, requesting fewer resources introduces different variables may introduce obstacles that would not otherwise be present.

General pointer as the form is filled out

* Load balancing is _not_ needed for most use cases.
* Linux Network Scope : ``Private``
    * E.g. UMN facing web service does not need to have a publicly addressed IP address, they can
        still reach out to the world just not the reverse.
* Environment: ``Development``
   * These will have patches applied first in the cycle.
* Virtual CPUs: ``4``
    * This can be changed later on by request as needed.
* Memory: ``8 GB``
* Data Security Classification: ``Private Restricted``
    * In most cases the private restricted is most applicable.
* Security Level: High
    * It is best practice and less overall effort to open permissions as needed.
* Backup: ``varies``
    * If the workflow already includes backing up to version control resources such as GitHub then
        perhaps backups are not needed.
* Comments can be added at the end of the form to request additional configuration options from the
    provisioning team.

---------------------------------------
Taking Inventory of Software / Services
---------------------------------------

--------------
Considerations
--------------

* Does the VM provide services?
    * e.g., Serving a website or database
    * You will need to consider what Network accommodations would need to be made, such as opening a port
* Does the VM depend on reading from a database?
    * E.g., Querying user data from a database
    * If yes, access will need to be configured for System B
* Does the VM have both a Website AND a Database
    * **This requires provisioning two machines to separate services**

------------------
Installed Packages
------------------

Filter out package installation commands from bash history and save to a text file. This is the most straightforward approach if the bash history for the root user has not been cleared before.

``[current_sys]$# history | grep -vi 'install' >> ~/history-installed.txt``


To preview the list, issue the command: ``less ~/history-installed.txt``

* press ``<Space>`` key to page through
* press ``<Q>`` key to close preview

Make note of the names of the most notable packages that are needed, these will later be installed to the dev system.

------------
Repositories
------------

Certain UMN hosted repositories may be enabled on the current that will also need to be enabled on the dev system. The enabled repositories grant access to specific pools of packages that may be repackaged by UMN and can differ from the off-the-shelf version.

Make note of the repositories that have been enabled.

``[current_sys]$# subscription-manager list``

Output for each repository will look similar to the following.

::

  repo id: XXX
  repo name: XXX
  repo url: XXX
  status: 0 / 1

---------------------------
Additional Package Commands
---------------------------

List out details about a single package with ``dnf``

``[current_sys]$# dnf info <package-name>``

-------------------------------------
Software Installation & Configuration
-------------------------------------

-------------------
Enable Repositories
-------------------

Start by enabling the repositories that are active on current system. Note that some may already be enabled as part of the provisioning process for the dev system.

On dev system, list out repositories:

``[dev_sys]$# subscription-manager list``

Repositories needed to can be enabled with the command:

``[dev_sys]$# subscription repos --enable <repoid> && dnf update``

----------------
Install Packages
----------------

Basic installation procedure for packages

``[dev_sys]$# dnf -y install <package-name>``

---------------------
Package Configuration
---------------------

Example: The ``httpd`` includes a daemon that needs to be enabled/started

``systemctl`` to modify the daemon status with

* Enable/Start the service, will restart automatically upon system reboot
  * ``systemctl enable --now httpd``
* Restart service, typically done to apply recent changes to configuration files
  * ``systemctl restart httpd``
* Check state of service, for confirmation of activity and viewing logs related
  * ``systemctl status httpd``

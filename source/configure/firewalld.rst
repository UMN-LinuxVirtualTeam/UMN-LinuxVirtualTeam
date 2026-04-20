Firewalld
=========

This document covers the typical layout of firewalld configuration files, zones, and enabling
event logging.


The following is a directory tree of a typical firewalld directory, make note of the files/directories

* ``firewalld.conf`` - needs description
* ``zones/`` - zone files provided by HEAT; additional configurations can be added here as needed


::

  [root@lsvt-test-tst-test-01 firewalld]# tree -p .
  .
  ├── [-rw-r--r--]  firewalld.conf
  ├── [drwxr-x---]  helpers
  ├── [drwxr-x---]  icmptypes
  ├── [drwxr-x---]  ipsets
  ├── [-rw-r--r--]  lockdown-whitelist.xml
  ├── [drwxr-x---]  policies
  ├── [drwxr-x---]  services
  └── [drwxr-x---]  zones
      ├── [-rw-r--r--]  AAP.xml
      ├── [-rw-r--r--]  AAP.xml.old
      ├── [-rw-r--r--]  Admin-Net.xml
      ├── [-rw-r--r--]  Admin-Net.xml.old
      ├── [-rw-r--r--]  Admin-Zabbix.xml
      ├── [-rw-r--r--]  Admin-Zabbix.xml.old
      ├── [-rw-r--r--]  Ansible-Tower.xml
      ├── [-rw-r--r--]  Ansible-Tower.xml.old
      ├── [-rw-r--r--]  Bastions.xml
      ├── [-rw-r--r--]  Bastions.xml.old
      ├── [-rw-r--r--]  F5.xml
      ├── [-rw-r--r--]  F5.xml.old
      ├── [-rw-r--r--]  internal.xml
      ├── [-rw-r--r--]  public.xml
      └── [-rw-r--r--]  public.xml.old

  6 directories, 17 files

----------------
Enabling Logging
----------------

On your RHEL9 box, elevate to root and navigate to ``/etc/firewalld/`` and use the following steps to
update the 'firewalld.conf' file.

The main action is setting LogDenied to ``LogDenied=all`` to allow capture of denied access events.

1. Navigate to ``/etc/firewalld`` as root

2. ``grep -i 'logdenied=' firewalld.conf``

    * Check for the current state of LogDenied, by default this is set to 'no'
3. ``sed -i 's/LogDenied=no/LogDenied=all/' firewalld.conf``

    * Update ``firewalld.conf`` and set LogDenied to ``LogDenied=all``
4. ``grep -i 'logdenied=' firewalld.conf``

    * Confirm the changes have been made to firewalld.conf
5. ``systemctl restart firewalld``

    * Restart firewalld service for changes to take effect
    * Note: if there is a typo, firewalld will revert to last working copy for firewalld.conf
6. ``sytemctl status -l firewalld``

    * Confirm the status of firewalld, ensure that it is all 'green'


Full example of enabling firewall deny logging

::
    
    [root@lsvt-test-tst-test-01 ~]# cd /etc/firewalld/
    [root@lsvt-test-tst-test-01 firewalld]# grep -i logdenied= firewalld.conf
    LogDenied=no
    [root@lsvt-test-tst-test-01 firewalld]# sed -i 's/LogDenied=no/LogDenied=all/' firewalld.conf
    [root@lsvt-test-tst-test-01 firewalld]# grep -i logdenied= firewalld.conf
    LogDenied=all
    [root@lsvt-test-tst-test-01 firewalld]# systemctl restart firewalld
    [root@lsvt-test-tst-test-01 firewalld]# systemctl status -l firewalld
    ● firewalld.service - firewalld - dynamic firewall daemon
         Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; preset: enabled)
         Active: active (running) since Thu 2025-08-21 11:49:01 CDT; 5s ago
           Docs: man:firewalld(1)
       Main PID: 2397966 (firewalld)
          Tasks: 2 (limit: 48676)
         Memory: 26.5M
            CPU: 338ms
         CGroup: /system.slice/firewalld.service
                 └─2397966 /usr/bin/python3 -s /usr/sbin/firewalld --nofork --nopid
    
    Aug 21 11:49:01 lsvt-test-tst-test-01.oit.umn.edu systemd[1]: Starting firewalld - dynamic firewall daemon...
    Aug 21 11:49:01 lsvt-test-tst-test-01.oit.umn.edu systemd[1]: Started firewalld - dynamic firewall daemon.

----------------
Specifying Zones
----------------


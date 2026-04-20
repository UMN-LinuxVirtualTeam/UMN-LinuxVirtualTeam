# Installing & Setting up : HTTPD

This document is targeted for OIT Managed RHEL9 boxes. Commands below may vary on a different
operating system.


## Package installation

```
dnf install httpd
```

## Configuration

It is reccomended to configure httpd _before_ enabling its service. If you have an existing system
with httpd, it would be worth doing a diff comparison of the base file and your configuration.

An example with diff

`diff -u old_file new_file`

Make note of the changes and apply where needed.

The typcial paths for configuration files are on the file paths:

* `/etc/httpd/conf`
* `/etc/httpd/conf.d`


## Starting the service

The httpd service can be _enabled_ to ensure that it starts automatically at every boot

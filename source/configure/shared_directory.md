This document provides a provides a demonstration of setting up a shared directory on a RHEL9
system


# Configuring a Shared Directory

<!--toc:start-->
- [Configuring a shared directory](#configuring-a-shared-directory)
    - [Identify the Group That We Want to Grant Access To.](#identify-the-group-that-we-want-to-grant-access-to)
    - [Set ownership and permissions](#set-ownership-and-permissions)
<!--toc:end-->

An exisiting directory can be modified to support collaboration between files/directories owned by
different users.


### Identify the Group That We Want to Grant Access To.

Check groups that a users account is associated to with `id -Gn internetid`

As an example we will use the group `gopher`.


### Set Ownership and Permissions

Change ownership to the group

Apply setgid to directory

`chmod +s path`

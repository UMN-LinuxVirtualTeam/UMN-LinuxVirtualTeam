Access Control
==============

OIT Managed Virtual Machines are provisioned with access provided to users in the unit's CESI Linux admin
list.

---------------------------
Remote Access for New Users
---------------------------

Remote access can be granted to individual UMN AD accounts using the ``realm`` command

* Adding access a single user - ``realm permit <UMN InternetID>``
* Revoking access a single user - ``realm deny <UMN InternetID>``

---------------------
Creating Local Groups
---------------------

Local groups is the most straightforward way to manage permissions for a collection of users. Groups and members lists are defined in the ``/etc/group``.

A new group can be created by using the command ``vigr -g``, similarly to commands above, this will also lock the ``/etc/group`` file to prevent simultaneous edits.

Example group definition

``NAME:*:GID:USERLIST``

* NAME - group names are typically at most 8 characters
* GID  - user group identification numbers start at 100
* USERLIST - comma delimited list of the users that should be a member of the group

------------------------------------
Granting Partial Elevated Privileges
------------------------------------

Standard users may be granted access to reserved commands without granting root access by modifying the sudoers file on ``/etc/sudoers``. The advantage in doing so grants better tracking of command usage as the execution will be associated an identifiable userid instead of root.

It is recommended to use ``visudo`` to make the changes as it the benefits of:

* Locks the sudoers file on to prevent multiple edits
* Performs validity checks and syntax errors
* Only allows saving a configuration that is syntactically correct

After opening the sudoers file with ``visudo``, navigate to the line that looks similar to:

``root    ALL=(ALL)       ALL``

Enter insert mode (press ``i``) and create a newline with the following definition:

``%[GROUP]    ALL=(ALL)       ALL``

* GROUP - Name of a new or existing group that has been created.


Referencing the HEAT doc, we'll need to start with using the realm command to add the user.

* `Users and Permissions <https://github.com/umn-heat/hosting-manual/blob/main/docs/RHEL9/users-and-permissions.md>`_

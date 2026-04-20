If a machine is expecting to call out to a database, the following methods can be used to confirm that a communication line can be opened.

These examples will utilize `netcat` as the `root` user.

Initialize a test connection to a Microsoft SQL database by checking availability of port 1433

TCP connection `nc -zv dba-sqlXXX.oit.umn.edu 1433`

UDP connection `nc -u dba-sqlXXX.oit.umn.edu 1433`

These are the common ports we recommend checking if needed.

| Port   | Service      |
| ------------- | ------------- |
| 22 | SSH/SFTP |
| 80 | http |
| 440 | https |
| 1433 | Microsoft SQL |
| 1521 | Oracle |
| 3306 | Linux MySQL |
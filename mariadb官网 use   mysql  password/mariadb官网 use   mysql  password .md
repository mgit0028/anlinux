04.16 15:23
mariadb官网 use   mysql  password
关键词
 
mariadb

mysql_native_password


https://mariadb.com/kb/en/authentication-plugin-mysql_native_password/


https://mariadb.com/kb/en/authentication-plugin-mysql_native_password/#creating-users


https://mariadb.com/kb/en/password/

Welcome to Termux!

Wiki:            https://wiki.termux.com
Community forum: https://termux.com/community
Gitter chat:     https://gitter.im/termux/termux
IRC channel:     #termux on freenode

Working with packages:

 * Search packages:   pkg search <query>
 * Install a package: pkg install <package>
 * Upgrade packages:  pkg upgrade

Subscribing to additional repositories:

 * Root:     pkg install root-repo
 * Unstable: pkg install unstable-repo
 * X11:      pkg install x11-repo

Report issues at https://termux.com/issues

$ myaql
No command myaql found, did you mean:
 Command mysql in package mariadb
$ mysql
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 15
Server version: 10.4.12-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [mysql]> SET PASSWORD =  PASSWORD('new_secret');
Query OK, 0 rows affected (0.046 sec)

MariaDB [mysql]> SELECT PASSWORD('notagoodpwd');
+-------------------------------------------+
| PASSWORD('notagoodpwd')                   |
+-------------------------------------------+
| *3A70EE9FC6594F88CE9E959CD51C5A1C002DC937 |
+-------------------------------------------+
1 row in set (0.009 sec)

MariaDB [mysql]>

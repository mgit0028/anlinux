04.16 16:45
进入mysql  选择 use mysql
$ mysqld

$ mysql

进入mysql
use mysql；
set session old_passwords=0;
select password('root');
set password for 'root' @'localhost' = password('root'); 


MariaDB [(none)]> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [mysql]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| test               |
+--------------------+
4 rows in set (0.003 sec)


MariaDB [mysql]> SET old_passwords=0;
Query OK, 0 rows affected (0.047 sec)

MariaDB [mysql]> CREATE USER username@hostname IDENTIFIED BY 'mariadb';
Query OK, 0 rows affected (0.114 sec)

MariaDB [mysql]> SELECT User, Host, plugin FROM mysql.user;
+----------+-----------+-----------------------+
| User     | Host      | plugin                |
+----------+-----------+-----------------------+
| root     | localhost | mysql_native_password |
| u0_a190  | localhost | mysql_native_password |
|          | localhost |                       |
| username | hostname  | mysql_native_password |
+----------+-----------+-----------------------+
4 rows in set (0.009 sec)



$ mysql
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 17
Server version: 10.4.12-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [mysql]> set session old_passwords=0;
Query OK, 0 rows affected (0.086 sec)

MariaDB [mysql]> select password('root');
+-------------------------------------------+
| password('root')                          |
+-------------------------------------------+
| *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B |
+-------------------------------------------+
1 row in set (0.010 sec)

MariaDB [mysql]> set password for 'root' @'localhost' = password('root');
Query OK, 0 rows affected (0.013 sec)

MariaDB [mysql]>



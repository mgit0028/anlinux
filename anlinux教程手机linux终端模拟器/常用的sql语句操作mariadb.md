04.12 22:54
常用的sql语句操作mariadb
Welcome to Termux!

termux 安装 mariadb

$  pkg   update

$ pkg  install  mariadb


$ mysqld  //开启mariadb

$ mysqld --skip-grant-tables --user=mysql &  //跳过权限开启mariadb

//  向右滑动屏幕打开隐藏窗口   new session

$ mysql
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 18
Server version: 10.4.12-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| test               |
+--------------------+
4 rows in set (0.002 sec)

MariaDB [(none)]> use test;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A


Database changed
MariaDB [test]> exit
Bye

$ mysql



MariaDB [(none)]> use test;
Database changed


MariaDB [test]> create table bbb(
    -> id int,
    -> name varchar(10)
    -> );
Query OK, 0 rows affected (0.112 sec)

MariaDB [test]> show tables;
+----------------+
| Tables_in_test |
+----------------+
| bbb            |
+----------------+
1 row in set (0.002 sec)

MariaDB [test]> SHOW CREATE TABLE bbb;
+-------+----------------------------------------------------------------------------------------------------------------------------+
| Table | Create Table                                                                                                               |
+-------+----------------------------------------------------------------------------------------------------------------------------+
| bbb   | CREATE TABLE `bbb` (
  `id` int(11) DEFAULT NULL,
  `name` varchar(10) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1 |
+-------+----------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.097 sec)

MariaDB [test]> exit


Bye

$ mysql


MariaDB [(none)]>  create database mydb88;
Query OK, 1 row affected (0.005 sec)

MariaDB [(none)]>



MariaDB [mydb88]> DROP DATABASE  mydb88;
Query OK, 1 row affected (0.078 sec)

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| test               |
+--------------------+
4 rows in set (0.002 sec)

MariaDB [(none)]> exit

Bye


$  mysql -u user_name -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 20
Server version: 10.4.12-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.


MariaDB [(none)]> show create database mysql;
+----------+------------------------------------------------------------------+
| Database | Create Database                                                  |
+----------+------------------------------------------------------------------+
| mysql    | CREATE DATABASE `mysql` /*!40100 DEFAULT CHARACTER SET latin1 */ |
+----------+------------------------------------------------------------------+
1 row in set (0.018 sec)


MariaDB [(none)]> SHOW CREATE DATABASE mysql;
+----------+------------------------------------------------------------------+
| Database | Create Database                                                  |
+----------+------------------------------------------------------------------+
| mysql    | CREATE DATABASE `mysql` /*!40100 DEFAULT CHARACTER SET latin1 */ |
+----------+------------------------------------------------------------------+
1 row in set (0.001 sec)

进入MYSQL命令行工具后 , 使用status;或\s 查看运行环境信息

MariaDB [(none)]> status;
--------------
mysql  Ver 15.1 Distrib 10.4.12-MariaDB, for Android (aarch64) using  EditLine wrapper

Connection id:          20
Current database:
Current user:           user_name@
SSL:                    Not in use
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server:                 MariaDB
Server version:         10.4.12-MariaDB MariaDB Server
Protocol version:       10
Connection:             Localhost via UNIX socket
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    latin1
Conn.  characterset:    latin1
UNIX socket:            /data/data/com.termux/files/usr/tmp/mysqld.sock
Uptime:                 14 hours 4 sec

Threads: 11  Questions: 158  Slow queries: 0  Opens: 35  Flush tables: 1  Open tables: 30  Queries per second avg: 0.003
--------------

MariaDB [(none)]>




更多

https://blog.csdn.net/sunlovefly2012/article/details/8922707

常用命令

clear

whoami

exit

//  向右滑动屏幕打开隐藏窗口   new session

创建数据库用户：只有根用户（root）才有创建新用户的权限

CREATE USER user_name1 

IDENTIFIED BY ‘password’,

 user_name2 IDENTIFIED  BY ‘password’;

一次可以创建多个数据库用户

csdn 博主

South-Fly













04.18 16:54
骏马金龙mariadb教程
骏马金龙
网名骏马金龙，钟情于IT世界里的各种原理和实现机制，强迫症重症患者。爱研究、爱翻译、爱分享。特借此一亩三分田记录自己成长点滴！！！
我本问道人，道心不坚，必将与道无缘！

管理
 
视频教程
 
Linux & Shell
MySQL
 
网站架构
 
Perl
 
Python
Golang
 
Ruby
 
操作系统
Win调整
随笔 - 537  文章 - 1  评论 - 1022
MariaDB/MySQL用户和权限管理
分类: 数据库系列
undefined
MariaDB/MySQL中的user由用户名和主机名构成，如"root@localhost"，同用户名但不同主机名对MySQL/MariaDB来讲是不同的，也就是说"root@localhost"和"root@127.0.0.1"是不同的用户，尽管它们都是本机的root。


1.权限验证
在MariaDB/MySQL服务器启动后会载入权限表到内存中，当用户要连接服务器，会读取权限表来验证和分配权限，即在内存中进行权限的读取和写入。

MariaDB/MySQL中的权限系统经过两步验证：

1.合法性验证：验证user是否合法，合法者允许连接服务器，否则拒绝连接。

2.权限验证和分配：对通过合法性验证的用户分配对数据库中各对象的操作权限。


1.1 权限表
MariaDB/MySQL中的权限表都存放在mysql数据库中。MySQL5.6以前，权限相关的表有user表、db表、host表、tables_priv表、columns_priv表、procs_priv表(存储过程和函数相关的权限)。从MySQL5.6开始，host表已经没有了。MariaDB中虽然有host表，但却不用。



这几个表用的最多的是user表。user表主要分为几个部分：用户列、权限列、安全列、资源控制列以及杂项列，最需要关注的是用户列和权限列。其中权限列又分为普通权限(上表中红色字体)和管理权限列，如select类的为普通权限，super权限为管理权限。且可以看到，db表中的权限全都是普通权限，user表中除了db表中具有的普通权限还有show_db_pirv和create_tablespace_priv，除此之外还有几个管理员权限。也就是说，db中没有的权限是无法授予到指定数据库的。例如不能授予super权限给test数据库。

另外，usage权限在上表中没有列出，因为该权限是所有用户都有的权限，它只用来表示能否登录数据库，它的一个特殊功能是grant仅指定该权限的时候不会影响现有权限，也就是说可以拿grant来修改密码而不影响现有权限。

需要说明的是，从user表到db表再到tables_priv表最后是columns_priv表，它们的权限是逐层细化的。user表中的普通权限是针对所有数据库的，例如在user表中的select_priv为Y，则对所有数据库都有select权限；db表是针对特定数据库中所有表的，如果只有test数据库中有select权限，那么db表中就有一条记录test数据库的select权限为Y，这样对test数据库中的所有表都有select权限，而此时user表中的select权限就为N(因为为Y的时候是所有数据库都有权限)；同理tables_priv表也一样，是针对特定表中所有列的权限；columns_priv则是针对特定列的权限。

所以对于已经通过身份合法性验证的用户的权限读取和分配的机制如下：

1.读取uesr表，看看user表是否有对应为Y的权限列，有则分配。
2.读取db表，看看db表中是否有哪个数据库分配了对应的权限。
3.读取tables_priv表，看看哪些表中有对应的权限。
4.读取columns_priv表，看看对哪些具体的列有什么权限。
例如，为某一用户授予test数据库的select权限。可以看到user表中的select_priv为N，而db表中的select为Y。

GRANT SELECT ON test.* TO 'long'@'192.168.100.1' IDENTIFIED BY '123456';
SELECT host,user,select_priv FROM mysql.user;
SELECT * FROM mysql.db;


 

1.2 图解认证和权限分配的两个阶段
 

 

1.3 权限生效时机
在服务器启动时读取权限表到内存中，从此时开始权限表生效。

之后使用grant、revoke、set password 等命令也会隐含的刷新权限表到内存中。

另外，使用显式的命令flush privileges或mysqladmin flush-privileges或mysqladmin reolad也会将上述几张权限表重新刷到内存中以供后续的身份验证和权限验证、分配。


2.用户管理
用户管理分为几个方面，创建用户、对用户授权、修改和删除用户。


2.1 创建用户
创建账号有几种方法。

1.使用grant直接对账号授权，账号不存在则会创建；
2.向mysql.user表中插入记录；
3.使用create user命令。
后两种方法创建的用户初始时没有任何权限(只有usage登录数据库的权限)，并且修改权限后要使用 FLUSH PRIVILEGES 语句或执行 mysqladmin flush-privileges 或 mysqladmin reload 命令刷新权限表到内存中，而第一种方法简便的多，创建用户后会自动刷新权限表。

grant和revoke语法：

GRANT
    priv_type [(column_list)]
      [, priv_type [(column_list)]] ...
    ON [object_type] priv_level
    TO user  [IDENTIFIED [BY [PASSWORD] 'password'][WITH with_option [with_option]

object_type:
    TABLE
  | FUNCTION
  | PROCEDURE

priv_level:
    *
  | *.*
  | db_name.*
  | db_name.tbl_name
  | tbl_name
  | db_name.routine_name

with_option:  
    GRANT OPTION
  | MAX_QUERIES_PER_HOUR count
  | MAX_UPDATES_PER_HOUR count
  | MAX_CONNECTIONS_PER_HOUR count
  | MAX_USER_CONNECTIONS count
  | MAX_STATEMENT_TIME time 
grant可以在库、表、函数、存储过程、特定列上授权，且一次性可以为多个用户授予多个对象的权限。其中 with grant option 表示拥有该权限后的用户可以给别的用户授予自身所拥有的权限。

revoke表示收回权限，注意revoke无法收回usage权限。

其中user的表示方法是 '用户名'@'主机名' ，主机名部分可以是主机名，可以是IP地址，可以是localhost，可以是通配符组成的主机名(空的host值也表示所有host，等价于'user_name'@'%')。如下示例：



对于网段地址，可以指定掩码来表示，如192.168.100.1/255.255.255.0，不能使用cidr格式的掩码记录方式，也不能指定非8、16、24、32位的掩码，如192.168.100.1/255.255.255.240是不允许的。

如果在user表中的用户有交叉部分，如root既可以从localhost登录，也可以从127.0.0.1登录，还可以从本机IP192.168.100.61登录，还可以从网段地址192.168.100.%登录，那么到底会从哪个登录？

在读取权限表user到内存中的时候，首先会根据host列的具体性进行排序，然后再根据user列进行具体性排序(即理解为order by host,user)，然后从上到下扫描，首次扫描到符合的记录就使用该记录登录。具体性的意思是越具体的user优先级越高，通配符范围越宽的user优先级越低。例如root@localhost的具体性比root@'%'的具体性高，后者又比'%'@'%'的具体性高。


2.2 create user和alter user
在MySQL 5.6.7之前，不要使用这两个命令创建用户和修改用户，因为它们会在mysql.user表的password列设置空串。到mysql5.6.7解决了这个问题。MariaDB可随意使用。

语法：

CREATE [OR REPLACE] USER [IF NOT EXISTS] 
 user_specification [,user_specification] ...
  [WITH resource_option [resource_option] ...]

user_specification:
  username [authentication_option]

authentication_option:
  IDENTIFIED BY 'authentication_string' 
  
resource_option:
  MAX_QUERIES_PER_HOUR count
  | MAX_UPDATE_PER_HOUR count
  | MAX_CONNECTIONS_PER_HOUR count
  | MAX_USER_CONNECTIONS count
例如：

create user 'longshuai'@'127.0.0.1' identified by '123456';
alter user和create user语法基本一致，但在MySQL中有让密码过期的功能，而在MariaDB中不支持该功能。

ALTER USER user_specification [, user_specification] ...
user_specification:
    user PASSWORD EXPIRE
例如，让刚才创建的用户过期。

alter user 'longshuai'@'127.0.0.1' password expire;

2.3 记录创建用户的时间
MariaDB/MySQL中user的元数据信息都存放在mysql.user表中，但是在这个表中的信息分类很少，常用的就只有用户类列和权限类列，没有用户的创建时间。

可以通过新增一列来记录用户的创建时间。

alter table mysql.user add column create_time timestamp default current_timestamp;
这样以后新建用户都会记录创建时间。但是显然，对于已有的用户是没有记录时间的，它们的值都为'0000-00-00 00:00:00'。

MariaDB [mysql]> select host,user,create_time from mysql.user;
+---------------------+-----------+---------------------+
| host                | user      | create_time         |
+---------------------+-----------+---------------------+
| localhost           | root      | 2018-04-21 05:58:19 |
| 127.0.0.1           | root      | 2018-04-21 05:58:19 |
| ::1                 | root      | 2018-04-21 05:58:19 |
| localhost           |           | 2018-04-21 05:58:19 |
| 192.168.100.%       | root      | 2018-04-21 05:58:19 |
| 192.168.100.1       | long      | 2018-04-21 05:58:19 |
| 127.0.0.1           | longshuai | 2018-04-21 05:58:19 |
| 192.168.100.1       | longshuai | 0000-00-00 00:00:00 |
+---------------------+-----------+---------------------+

2.4 查看用户权限
可以使用show grants语句查看某个user的权限信息。

例如：

MariaDB [mysql]> show grants for 'root'@'localhost';

Grants for root@localhost                                                                                    
-----------------------------------------------------------------------------------------------------------------
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED BY PASSWORD '*6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9' WITH GRANT OPTION 
GRANT PROXY ON ''@'' TO 'root'@'localhost' WITH GRANT OPTION    
MariaDB [mysql]>SHOW GRANTS FOR 'long'@'192.168.100.1';

Grants for long@192.168.100.1   
-----------------------------------------------------------------------------------------------------------
GRANT USAGE ON *.* TO 'long'@'192.168.100.1' IDENTIFIED BY PASSWORD '*6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9' 
GRANT SELECT ON `test`.* TO 'long'@'192.168.100.1'   

2.5 revoke命令的严格性
revoke命令回收权限时必须要明确指定回收的数据库对象以及用户名，其中usage权限无法回收。特别要说明的是revoke all，当你以为它会回收所有权限的时候，它可能一点权限都没有回收。也就是说revoke命令的书写非常严格。

用户 'long'@'192.168.100.1' 在 *.* 上具有usage权限，在test.*上具有select权限。

MariaDB [mysql]> SHOW GRANTS FOR 'long'@'192.168.100.1';
Grants for long@192.168.100.1 
-----------------------------------------------------------------------------------------------------------
GRANT USAGE ON *.* TO 'long'@'192.168.100.1' IDENTIFIED BY PASSWORD '*6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9' 
GRANT SELECT ON `test`.* TO 'long'@'192.168.100.1'   
对该用户在 *.* 上进行revoke all，再次查看权限，发现权限根本一点变化都没有。因为usage权限无法回收，而select权限是在test.*上而非*.*上。

REVOKE ALL ON *.* FROM 'long'@'192.168.100.1';
要回收test.*上的select权限，必须在revoke中指定test.*，而不能是 *.* 。以下两个语句都能回收。

revoke select on test.* from 'long'@'192.168.100.1';
revoke all on test.* from 'long'@'192.168.100.1';

2.6 删除用户
直接使用drop user命令或者从mysql.user表中删除对应记录。

drop user user_name1,username2...
注意，删除表中用户记录的时候不会从现有用户中回收对该表的权限，当下次再创建同名表的时候，会自动为用户授予该表的权限造成权限外流。

因此，建议使用drop user语句来删除用户。


3.设置密码和恢复root密码

3.1 设置密码
(1)grant all on *.* to 'root'@'localhost' identified by '123456' with grant option;
(2)grant usage on *.* to 'root'@'localhost' identified by '123456' with grant option;
使用usage权限表示在不影响现有权限的情况下使用grant来修改密码。

(3)set password [for 'root'@'localhost'] =password('123456');
password函数中必须加引号，不写user时是为当前用户修改。

(4)alter user root@localhost identified by '123456';
(5)mysqladmin -uroot -h localhost -p'old_password' password 'new_password';
(6)update mysql.user set password=password('123456') where user='root' and host='localhost';
其中grant和set password语句可以直接刷新权限表，其他语句需要使用 flush privileges 或其他刷新语句。


3.2 恢复root密码
可以在启动mysql服务时使用mysqld_safe服务程序并指定"--skip-grant-tables"选项表示跳过授权表，这样登陆mysql服务器将不需要任何权限，包括密码认证也不需要，但是同样受限的是不能操作任何权限相关的内容，比如修改权限，刷新授权表等。这通常是mysql管理员密码忘记的时候使用的选项。由于跳过授权表使得mysql服务器极不安全，任何用户都能直接登录服务器，所以通常和"--skip-networking"选项一起使用来禁止来自网络的服务器连接请求，这样只能使用localhost或者127.0.0.1作为host来登录。

另外，使用mysqld_safe启动无授权表的服务前要停止已有的MySQL实例。由于跳过授权表无法操作权限相关内容，所以修改mysql.user表中的管理员账号的密码字段是唯一修改方法。修改密码后记得重启MySQL服务。

步骤如下：

[root@xuexi mysql]# service mysqld stop
[root@xuexi mysql]# mysqld_safe --skip-grant-tables --skip-networking &
[root@xuexi mysql]# mysql
mysql> update mysql.user set password=password("123456") where user='root' and host='localhost';
mysql> flush privileges;
mysql> select user,host,password from mysql.user where user='root' and host='localhost';
+------+-----------+-------------------------------------------+
| user | host      | password                                  |
+------+-----------+-------------------------------------------+
| root | localhost | *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9 |
+------+-----------+-------------------------------------------+
1 row in set
mysql> \q
[root@xuexi mysql]# service mysqld stop
[root@xuexi mysql]# service mysqld start
[root@xuexi mysql]# mysql -uroot -p123456
mysql> \q
如果要找回多实例的密码，则在mysqld_safe命令中使用 --defaults-file 指定对应的配置文件即可。

 

转载请注明出处：https://www.cnblogs.com/f-ck-need-u/p/8994220.html

如果觉得文章不错，不妨给个打赏，写作不易，各位的支持，能激发和鼓励我更大的写作热情。谢谢！


作者：骏马金龙
出处：http://www.cnblogs.com/f-ck-need-u/
Linux运维交流群：710427601
Linux&shell系列文章：http://www.cnblogs.com/f-ck-need-u/p/7048359.html 
网站架构系列文章：http://www.cnblogs.com/f-ck-need-u/p/7576137.html 
MySQL/MariaDB系列文章：https://www.cnblogs.com/f-ck-need-u/p/7586194.html 
Perl系列：https://www.cnblogs.com/f-ck-need-u/p/9512185.html 
Go系列：https://www.cnblogs.com/f-ck-need-u/p/9832538.html 
Python系列：https://www.cnblogs.com/f-ck-need-u/p/9832640.html 
Ruby系列：https://www.cnblogs.com/f-ck-need-u/p/10805545.html 
操作系统系列：https://www.junmajinlong.com/o

https://www.cnblogs.com/f-ck-need-u/p/8994220.html

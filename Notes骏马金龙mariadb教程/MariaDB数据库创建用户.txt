04.18 16:46
MariaDB数据库创建用户 阿波罗Apollo
 MARIADB数据库用户创建/删除及权限授权/撤回
1.MariaDB数据库创建用户
1.1 命令

CREATE USER 'username'@'host' IDENTIFIED BY 'password';

1.2 参数

username:(jack)
	创建的用户名
host:(192.168.13.34)
	指定该用户在哪个主机上可以登陆,
	如果是本地用户可用localhost,
	如果想让该用户可以从任意远程主机登陆,可以使用通配符%
password:(jack)
	建议用户名和密码不要一致,上述只是为了演示
	该用户的登陆密码,密码可以为空,
	如果为空则该用户可以不需要密码登陆服务器.
1.3 示例

# 创建用户‘Ann’
MariaDB [(none)]> create user ann@localhost identified by 'ann'
# 创建用户‘Steven’
MariaDB [(none)]> create user ann@localhost identified by 'ann'
# 创建用户‘Jack’
MariaDB [mysql]> create user jack@'192.168.13.34' identified by 'jack';
2.MariaDB数据库给用户授权
2.1 命令

GRANT privileges ON databasename.tablename TO 'username'@'host'
2.2 参数

privileges:
	用户的操作权限,如SELECT,INSERT,UPDATE等,如果要授予所的权限则使用ALL.
databasename:
	数据库名
tablename:
	表名,如果要授予该用户对所有数据库和表的相应操作权限则可用*表示,如*.*.
2.3 示例

# 授权Ann拥有db1数据库的所有权限,允许在localhost登录
MariaDB [(none)]> grant select on db1.* to ann@localhost;
MariaDB [mysql]> flush privileges;

# 授权Jack拥有db1数据库的所有权限,允许在192.168.13.34登录
MariaDB [(none)]> grant all on db1.* to jack@'192.168.13.34';
MariaDB [mysql]> flush privileges;

# 授权Steven拥有db1数据库的所有权限,允许从任意远程主机登陆，
# 注意你授权时用%,你创建用户时,必须也是%,要对应,否则报错.
MariaDB [(none)]> grant all on *.* to steven@'%';
MariaDB [mysql]> flush privileges;
3.MariaDB数据库创建用户并授权的命令
3.1 授权apollo用户拥有db1数据库的所有权限

MariaDB [mysql]> grant all on *.* to john@'192.168.13.34' identified by 'john';
MariaDB [mysql]> flush privileges;
3.2 授予外网登陆权限

MariaDB [mysql]> grant all privileges on *.* to username@'%' identified by 'password';
3.3 授予权限并且可以授权

MariaDB [mysql]>grant all privileges on *.* to username@'hostname' identified by 'password' with grant option;
# 整个命令是一句话,这里换行是因为显示问题.
授权部分参数值:
all privileges,all
select,insert,update,delete,create,drop,index,alter,grant,references,reload,shutdown,process,file

4.MariaDB数据库查看用户
4.1 MariaDB查看当前登录用户

# 方法1
MariaDB [(none)]> select user();
# 方法2
MariaDB [(none)]> select current_user;
# 方法3
MariaDB [(none)]> select current_user();
4.2 MariaDB中如何显示所有用户?

MariaDB [(none)]> select User,Host,Password from mysql.user;
4.3 MariaDB显示所有的用户(不重复)

MariaDB [mysql]> select distinct user from mysql.user;
5.MariaDB数据库删除用户
# 5.1 删除用户'jack'
MariaDB [mysql]> delete from user where user='jack';
# 5.2 删除用户'steven'
MariaDB [(none)]> delete from mysql.user where user='steven' and host='%';
# 5.3 删除用户'john'
MariaDB [(none)]> drop user 'john'@'192.168.13.34';
6.MariaDB数据库撤销用户权限
6.1 命令

REVOKE privileges ON databasename.tablename FROM 'username'@'host';
6.2参数

privileges:
	用户的操作权限,如SELECT,INSERT,UPDATE等,如果要授予所的权限则使用ALL.
databasename:
	数据库名
tablename:
	表名,如果要授予该用户对所有数据库和表的相应操作权限则可用*表示,如*.*.
6.3 示例

# 假如你在给用户授权的时候是这样的：
grant select on db1.user to jack@'%';
# 执行下面sql语句
revoke select on *.* from jack@'%';
# 并不能撤销该用户对db1数据库中user表的SELECT操作.

# 假如你在给用户授权的时候是这样的：
grant select on *.* to jack@'%';
# 执行下面sql语句
revoke select on db1.user from jack@'%';
# 并不能撤销该用户对db1数据库中user表的SELECT操作.
6.4 查看授权信息

MariaDB [(none)]> show grants for 'jack'@'192.168.13.34';
6.5 撤销用户Jack所有权限

revoke ALL PRIVILEGES ON `db1`.* from 'jack'@'192.168.13.34'
分类: MySQL数据库


https://www.cnblogs.com/apollo1616/articles/10294490.html#autoid-0-1-0

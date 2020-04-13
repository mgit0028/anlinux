04.12 22:43
MySQL 常用SQL语句 mariadb也能用

MySQL 常用SQL语句  mariadb也能用

mysql教程

■  在DOS命令行启动MYSQL服务：

net start mysql

■在DOS命令行停止MYSQL服务：

net stop mysql

■查看被监听的端口:

netstat –na | findstr 3306

findstr用于查找后面的端口是否存在。



■创建数据库用户：只有根用户（root）才有创建新用户的权限

CREATE USER user_name1 IDENTIFIED BY ‘password’,

 user_name2 IDENTIFIED  BY ‘password’;

一次可以创建多个数据库用户

■删除数据库用户:

DROP USER user_name;

■选择用户:

select user();

■用户的权限控制：

GRANT库，表级的权限控制：将某个库中的某个表的控制权限赋予某个用户

GRANT ALL　ON db_name.table_name TO  uer_name [indentified by ‘password’];

■查看所有的字符编码:

SHOW CHARACTER SET;  

==============================================================================

■登录MySQL数据库: 在DOS命令行登录MYSQL控制台

mysql -u user_name -p(Enter,回车键入密码，若直接输入则为可显)

Enter password：*********

Mysql –h hostname –u user_name –p

Enter password:*********

例：mysql –h 192.168.5.105 –uroot –p

Enter password:*******

■查看运行环境信息:

进入MYSQL命令行工具后 , 使用status;或\s 查看运行环境信息

■创建数据库：

create database db_name；

[default]CHARACTER SET charset_name        //设置数据库的编码发方式

[default]COLLATE collation_name ;          //设置按collation_name字段排序

//不能写成utf-8，utf8的默认校对为utf8_general_ci（通过show character set查看）

CREATE DATABASE db_name CHARACTER　SET utf8 COLLATE  utf8_general_ci； 

CHARACTER SET:指定数据库采用的字符集

COLLATE:指定数据库字符集的比较方式

■使用数据库：

use db_name;           

■显示数据库：

SHOW DATABASES；

■显示数据库创建语句：

SHOW CREATE DATABASE db_name;

■删除数据库：

DROP DATABASE  db_name;

删除时可先判断是否存在，写成：DROP DATABASE IF EXISTS db_name;

■查看创建数据库的指令并查看数据库使用的编码：

show create databasedb_name;

■查看数据库编码：

showvariables like ‘char%’;

■查看数据库当前引擎：

SHOW CREATE TABLE table_name;

■修改数据库当前引擎：

ALTER TABLE table_name ENGINE=MYISAM| INNODB;( ‘|’表示‘或者’，选其一)

你能用的数据库引擎取决于mysql在安装的时候是如何被编译的。要添加一个新的引擎，就必须重新编译MYSQL。在缺省情况下，MYSQL支持三个引擎：ISAM、MYISAM和HEAP。另外两种类型INNODB和BERKLEY（BDB），也常常可以使用。

■设置数据库编码：

setcharacter_set_client=gbk;//可以存中文

setcharacter_set_results=gbk;//可以看中文

■备份数据库：

MYSQLDUMP –u用户名(根用户) –p密码 db_name >  存放路径级/文件名(文件格式：.sql)

（不是在mysql控制台执行，而要退出控制台在DOS下执行）

例子：

MYSQLDUMP –u root –p******* mydb > D:/mydb.sql;

■恢复数据库：

前提：要创建一个空数据库

SOURCE 存放路径/文件名.sql (在Mysql控制台执行)

■运行脚本文件完成表的操作:

假若创建数据库表的脚本文件位于chapter/schema/sampledb.sql,下面是两种运行脚本的方法:

1.直接通过MySql命令运行,DOS命令窗口下运行:

D:\>mysql -u root -p 1234 --p 3306 < D:\chapter\schema\sampledb.sql;

2.登录mysql后,通过source命令运行脚本:

mysql>source D:\chapter\schema\sampledb.sql;

■如何将大量数据存入数据库中的表中:

首先，将数据按表的结构（字段的顺序要对应）存入文本文档中；

然后，某字段若没有值则填入NULL，注意，每个字段值之间用Tab键隔开（/r/n）。

最后，使用命令：LOAD DATA LOCAL INFILE ‘E:/Test/pet.txt’ INTO TABLE pet  LINES

               TERMINATED BY ‘\r\n’;



Terminate 结束，终止；



■创建表：

1.CREATE TABLE pet (

id int PRIMARY KEY AUTO_INCREMENT ,

name VARCHAR(20) NOT NULL UNIQUE,

owner VARCHAR(20) NOT NULL,

species VARCHAR(20),

sex CHAR(1),

birth DATE ,

);

2.CREATE TABLE table_name(

   field1  datatype,

   field2  datatype,

   ……

)CHARACTER SET 字符集 COLLATE 校对规则;

 field:指定列名    datatype:指定列类型

■显示表:

show tables; 

■显示某个表创建时的全部信息：

SHOW CREATE TABLE table_name;

■显示表的结构信息：

DESCRIBE table_name;    缩写形式 : desc table_name;

■查找数据:

SELECT * FROM table_name;

■显示表的各字段:

DESCRIBE table_name;

■清空表中的数据:

1.TRUNCATE table_name;

此方法会使表中的取号器（ID）从1开始

2.DELETE FROM table_name;
不带where参数的delete语句可以删除mysql表中所有内容，使用truncate table也可以清空mysql表中所有内容。效率上truncate比delete快，但truncate删除后不记录mysql日志，不可以恢复数据。
delete的效果有点像将mysql表中所有记录一条一条删除到删完，而truncate相当于保留mysql表的结构，重新创建了这个表，所有的状态都相当于新表。 

注意：在MySQL中事务的特殊说明：

1.  mysql控制台是默认自动提交事务（dml）.

2.  若在控制台使用事务，应该做以下设置：

*SETAUTOCOMMIT=FALSE;    //解除自动提交功能

*SAVEPOINT point;         //设置保存点point

//一系列操作…

*ROLLBACK TOpoint;      //回滚到保存点

使用TRUNCATE table_name删除的表不能够回滚，但删除速度较快，

使用DELETE FROM table_name删除的表可以回滚，删除速度相对较慢。

■创建表:

CREATE　TABLE table_name;

■删除表:

DROP TABLE　table_name;     

■插入数据:

1.INSERT INTO table_name [(字段1，字段2，字段3，…)] VALUES (值1，值2，…);

如果向表中的每个字段都插入一个值，那么前面[]括号内字段名可写也可不写。

2.INSERT INTO pet  VALUES('Puffball','Diane','hamster','f','1999-03-30',NULL);

3.从 源表 中筛选符合条件的记录，批量插入到 (指定的)目标表 中:

insert into 目标表(字段1, 字段2,...字段n) select 字段1, 字段2,...字段n from 源表 where 条件

4.向表中插入条数据：

insert into articles (id, content,userid)

values (2,’hahaha’,11),(null,’xixixi’,10),(13,’aiaiai’,1)，…;

■删除数据：

DELETE　FROM table_name WHERE id=’10’;

delete语句不能删除某一列的值，可使用update更新列值。

使用delete语句仅删除记录，不删除表本身，删除表使用drop table语句。

■更新数据:

UPDATE pet SET birth = '1989-08-31'WHERE name = 'Bowser' ORDER　BY birth DESC;

ASC(升序，默认方式)；DESC（降序）

WHERE和 ORDER 语句也可用于查询select 与 删除delete

■查询数据:

1.SELECT * FROM pet WHERE name = 'Bowser';

SELECT * FROM pet WHERE birth > '1998-1-1';

SELECT * FROM pet WHERE species = 'dog' AND sex ='f';

SELECT * FROM pet WHERE species = 'snake' OR species= 'bird';

SELECT * FROM pet WHERE (species = 'cat' AND sex ='m')

          OR(species = 'dog' AND sex = 'f');

SELECT name, birth FROM pet;

SELECT chinese+english+math  FROM Students;    //列之间可以进行运算

2.增加关键字DISTINCT检索出每个唯一的输出记录，把相同的记录值取其中一个即可。

SELECT DISTINCT owner FROM pet; //distinct[dɪ'stɪŋkt] 截然不同的

3.使用一个WHERE子句结合行选择与列选择：

SELECT name, species, birth FROM pet

        WHEREspecies = 'dog' OR species = 'cat';

4.为了排序结果，使用ORDER BY子句:

SELECT name, birth FROM pet WHERE species = 'dog' ORDERBY birth;

默认排序是升序，最小的值在第一。要想以降序排序，在你正在排序的列名上增加DESC（降

序）关键字：

SELECT name, birth FROM pet ORDER BY birth DESC;

5.可以对多个列进行排序，并且可以按不同的方向对不同的列进行排序：

SELECT name, species, birth FROM pet

ORDER BY species, birth DESC;

注意DESC关键字仅适用于在它前面的列名（birth）；不影响species列的排列顺序。

6.MySQL提供了几个函数，可以用来计算日期，例如，计算年龄或提取日期部分：

SELECT name, birth, CURDATE(),((YEAR(CURDATE())-YEAR(birth))-(RIGHT(CURDATE(),5)<RIGHT(CURDATE(),5)) AS age FROM pet;

YEAR()提取日期的年部分，RIGHT()提取日期的MM-DD (日历年)部分的最右面5个字符。比较MM-DD值的表达式部分的值一般为1或0，若birth月份比当前月份大，则年份应减去1。

SELECT name, birth, death FROM pet WHERE deathISNOT NULL ORDER BY birth;

使用deathIS NOT NULL而非death != NULL，因为NULL是特殊的值，不能使用普通比较符来比较。

7.MySQL提供几个日期部分的提取函数，例如YEAR( )、MONTH( )和DAYOFMONTH()。

SELECT name, birth, MONTH(birth) FROM pet;

SELECT name, birth FROM pet WHEREMONTH(birth) = 5;

概念上，NULL意味着“没有值”或“未知值”，且它被看作与众不同的值。不能使用算术比较操作符例如=、<或!=。

请注意在MySQL中，0或NULL意味着假而其它意味着真。布尔运算的默认值是1.

8.同表查询:

SELECTa.id,a.nikename,a.address FROM users a,users b WHERE b.nikename=’haha’

anda.id>b.id;

也可写成

SELECT id,nikename,addressFROM users WHERE id>(SELECT id FROM users WHERE nikename=’haha’);

补充：在WHERE子句中经常使用的运算符

 

 

比较运算符

>、< 、<= 、>= 、= 、<>

大于、小于、大于（小于）等于、不等于

BETWEEN   …AND…

显示在某一区间的值

IN(set)

显示在IN列表中的值，例：IN（100，200）

LIKE ‘张pattern’

模糊查询

IS NULL 

判断是否为空

 

逻辑运算符

AND

多个条件同时成立

OR

多个条件任一成立

NOT

不成立，例：where not(salary>100);

注：LIKE语句中，%代表零个或多个任意字符，_代表一个字符，例firstname like ‘_a%’;

 

9.使用GROUP BY子句对列进行分组：

SELECTcolumn1,column2,column3… FROM table_name GROUP BY column;

SELECTproduct,sum(price) FROM orders GROUP BY product;

10.使用HAVING 子句过滤：

SELECT column1，column2，column3…FROMtable_name GROUP BY column HAVING…

SELECTproduct,sum(price) FROM orders GROUP BY product HAVING sum(price)>100;

HAVING和WHERE均可实现过滤，但在HAVING可以使用合计函数，HAVING通常跟在GROUP BY后，它作用于组。

…GROUP BY …HAVING…ORDERBY…

■对表进行重命名:

1.RENAME TABLE table_name TO new_name;
2.ALTER TABLE table_name RENAME TO new_table_name;
■  修改表结构：增加字段

1.增加一个字段

ALTER TABLE table_name ADD COLUMN(字段名 字段类型)；---此方法带括号

2.增加一个字段在指定的位置

ALTER TABLE table_name ADDCOLUMN 字段名 字段类型 AFTER 某字段；

■  修改表结构：删除字段

ALTER TABLE table_name drop 字段名；

■修改表结构：改变字段名称/类型:

ALTER TABLE table_name CHANGE COLUMN field_namenewfield_name varchar(10) not null;

其中char(20) notnull是newcolumn_name字段的create_definition.

■增加约束：约束（主键Primary Key、唯一性Unique、非空Not NULL）

1.ALTER TABLE table_name CHANGE old_id new_id INT(16) NOT NULL PRIMARY KEY;

2.自动增长：

ALTER TABLE table_name CHANGE old_id  new_id INT(16) NOT NULL AUTO_INCREMENT;

■修改表的字符集：

ALTER　TABLE table_name CHARACTER SET UTF8;

■  查看某字段使用的编码：

SELECTCHARSET(column_name) FROM table_name;

==============================================================================

 

插入数据库是出现乱码的参考解决方案：

1。 数据库字符集设置为GB2312。(但就是插不成功显示Datato lang 吧!)

2。关键在创建表的时候:

create table (字段) Default character set gb2312;

3。表创建好的情况下：

修改表编码: alter table 表名 Default character set gb2312;

修改字段编码: ALTERTABLE 表名 CHANGE COLUMN 字段名CHARACTER SET gb2312；

 

==============================================================================

 

#查看数据库的版本，当前日期(不区分大小写)

Mysql> selectVERSION(),CURRENT_DATE,NOW();

更多

https://blog.csdn.net/sunlovefly2008/article/details/8922707

常用的Mysql数据库操作语句大全
博客 weixin_34122604  484 浏览器打开
Mysql数据库-使用的查询语句大全
博客 gymaisyl  3522 浏览器打开
MySQL常用SQL语句大全_数据库_江南极客-CSDN博客
2020-3-31
常用SQL语句_mysql,sql,数据库_qq_44377181的博客-CSDN博客
2020-4-1
[整理]SQL语句学习手册实例版
博客 yangy608  406 浏览器打开
mysql sql常用语句大全
博客 hzw6991  5697 浏览器打开
一看即会完整的MySQL数据库基本SQL语句_qq450855246的博客-CSDN博客
2020-3-24
常用sql语句_myh_fly的博客-CSDN博客
2020-3-20
数据库基本语句
博客 weixin_34080903  624 浏览器打开
SQL 常用基本语句总结大全—增删改查语句
博客 sumaliqinghua  699 浏览器打开
SQL 项目中常用SQL总结 - South-Fly - CSDN博客
2018-11-25
mysql数据库常用sql语句 - 07151012的博客 - CSDN博客
2019-12-2
数据库基础语句
博客 u013617791  9026 浏览器打开
mysql数据库常用sql语句
博客 qq_32648375  1万+ 浏览器打开
mysql常用sql语句_数据库_xiuzhentianting的博客-CSDN博客
2020-4-2
Mysql数据库常用SQL语句_一间草堂的博客-CSDN博客
2020-1-9
常用经典SQL语句大全完整版--详解+实例
博客 WuJun_025  4016 浏览器打开
经典SQL查询语句大全
博客 qq_39539367  1万+ 浏览器打开
mysql 数据库基本sql语句_a1234name的博客-CSDN博客
2020-1-6
undefined
数据库基础（常用SQL语句）
博客 qq_41751237  9677 浏览器打开
SQL数据库基本操作语句
博客 zxyvb  814 浏览器打开
sql语句大全（详细）
博客 hallomrzhang  6898 浏览器打开
SQL语句基础
博客 ziyonghong  3657 浏览器打开
Mssql常用经典SQL语句大全完整版--详解+实例
博客 lifushan123  9370 浏览器打开
数据库常用sql语句总结
博客 Ace_2  2653 浏览器打开
【SQL】SQL数据库基础语句学习（一）
博客 u010591976  3944 浏览器打开
Oracle数据库语句大全(一)
博客 weixin_33796205  627 浏览器打开
数据库---经典SQL语句大全
博客 weixin_42504145  725 浏览器打开
SQL操作库操作表，SQL常用所有类型函数，SQL常用所有数据类型，sql约束，所有sql语句
博客 weixin_43583693  411 浏览器打开
常用sql数据库语言基本语句
博客 u010728594  295 浏览器打开
【MySQL数据库】一条SQL语句为什么执行这么慢？
博客 Moo_Lavender  1183 浏览器打开
mysql 数据库基本sql语句
博客 a1234name  638 浏览器打开
Sql语句实例
博客 alashan007  1918 浏览器打开
数据库基础应用知识总结
博客 niaonao  5512 浏览器打开
数据库语句删除数据库
博客 weixin_44561520  8185 浏览器打开
【数据库学习总结】 —— 基础sql语句
博客 qq_41133986  1458 浏览器打开
SQL基础语句汇总
博客 wenwen091100304  3万+ 浏览器打开
经典SQL语句大全
博客 LQZ8888  7282 浏览器打开
MYSQL 数据库的常用语句
博客 wangshoulixx  1万+ 浏览器打开
数据库操作语句大全(sql)
博客 qq_27695659

 

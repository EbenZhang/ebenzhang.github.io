---
layout: post
category : MySQL
date: 2014/11/06 00:00:00 
title: MySQL主从复制
tags : MySQL
---

###概述
之前放在云笔记中，现在放到博客上，有时间把MySQL读写分离也写上

###场景描述：

主数据库服务器：192.168.1.5，MySQL已经安装，并且无应用数据。

从数据库服务器：192.168.1.12，MySQL已经安装，并且无应用数据。

###主服务器上进行的操作

###启动mysql服务

```/etc/rc.d/inti.d/mysqld start有的是/etc/init.d/mysqld start```

###查看mysql是否已经启动

```/etc/rc.d/init.d/mysqld status```

###通过命令行登录管理MySQL服务器

```mysql -p```或者

```mysql -uroot -p'password' ```

###授权给从数据库服务器192.168.1.12

```mysql> GRANT REPLICATION SLAVE ON *.* to 'root'@'192.168.1.12' IDENTIFIED BY 'password_123';```

```mysql> GRANT FILE ON *.* TO 'root'@'192.168.1.12' IDENTIFIED BY 'password_123';```


###查询主数据库状态:

```Mysql> show master status;```

记录下 FILE 及 Position 的值，在后面进行从服务器操作的时候需要用到。

注意，这个有可能会为空。

如果为空，则有可能是主服务器没有启用-logs-bin，在my.cnf中配置：
log-bin=mysql-bin，重新启动服务再次查看即可。也可以参考http://blog.sina.com.cn/s/blog_4e424e2101000c1o.html
中的设置：如果主服务器运行时没有启用--logs-bin，SHOW MASTER STATUS显示的日志名和位置值为空。在这种情况下，当以后指定从服务器的日志文件和位置时需要使用的值为空字符串('')和4.

![Alt text]({{ BASE_PATH }}/images/mysql1.png "mysql目录")


###配置server-id以及要实行主从复制的库：

```
server-id=1
innodb_flush_log_at_trx_commit=1
sync_binlog=1
binlog-do-db=test
binlog_ignore_db=mysql
binlog_ignore_db=information_schema
```


###配置从服务器

```server-id=10
innodb_flush_log_at_trx_commit=1
sync_binlog=1
replicate-do-db=test   //要主从复制的数据库
replicate-ignore-db=mysql  //不需要主从复制的数据库
replicate-ignore-db=information_schema//不需要主从复制的数据库
```

###启动mysql服务

```/etc/rc.d/inti.d/mysqld start有的是/etc/init.d/mysqld start```


###执行同步SQL语句

```
mysql> change master to
master_host=’192.168.1.5’,
master_user=’rep1’,
master_password=’password’,
master_log_file=’mysql-bin.000002’,
master_log_pos=582;
```

###正确执行后启动Slave同步进程

```mysql> start slave;```

###主从同步检查

```mysql> show slave status\G```

==============================================
**************** 1. row *******************
Slave_IO_State:
Master_Host: 192.168.10.130
Master_User: rep1
Master_Port: 3306
Connect_Retry: 60
Master_Log_File: mysql-bin.000005
Read_Master_Log_Pos: 415
Relay_Log_File: localhost-relay-bin.000008
Relay_Log_Pos: 561
Relay_Master_Log_File: mysql-bin.000005
Slave_IO_Running: YES
Slave_SQL_Running: YES
Replicate_Do_DB:
……………省略若干……………
Master_Server_Id: 1
1 row in set (0.01 sec)

```其中Slave_IO_Running 与 Slave_SQL_Running 的值都必须为YES，才表明状态正常。
```

###2.3 验证主从复制效果

###主服务器上的操作

###在主服务器上创建数据库first_db

```mysql> create database first_db;
Query Ok, 1 row affected (0.01 sec)
```

###在主服务器上创建表first_tb

```mysql> create table first_tb(id int(3),name char(10));
Query Ok, 1 row affected (0.00 sec)
```

###在主服务器上的表first_tb中插入记录

```mysql> insert into first_tb values (001,’myself’);
Query Ok, 1 row affected (0.00 sec)
```

###在从服务器上查看

mysql> show databases;
=============================
+--------------------+
| Database |
+--------------------+
| information_schema |
| first_db |
| mysql |
| performance_schema |
| test |
+--------------------+
5 rows in set (0.01 sec)
=============================

###数据库first_db已经自动生成

```mysql> use first_db
Database chaged
```

```mysql> show tables;```
=============================
+--------------------+
| Tables_in_first_db |
+--------------------+
| first_tb |
+--------------------+
1 row in set (0.02 sec)
=============================

###数据库表first_tb也已经自动创建

```mysql> select * from first_tb;```

=============================
+------+------+
| id | name |
+------+------+
| 1 | myself |
+------+------+
1 rows in set (0.00 sec)
=============================
记录也已经存在


###注意事项：

- 1：主数据库和从数据库在my.ini或者my.cnf中的配置不同。

###主数据库：

```server-id=1
innodb_flush_log_at_trx_commit=1
sync_binlog=1
binlog-do-db=test
binlog_ignore_db=mysql
binlog_ignore_db=information_schema
````

- 2:从数据库：

```server-id=10
innodb_flush_log_at_trx_commit=1
sync_binlog=1
replicate-do-db=test
replicate-ignore-db=mysql
replicate-ignore-db=information_schema
```





###参考资料：

http://www.cnblogs.com/luckcs/articles/2543607.html
http://blog.sina.com.cn/s/blog_8a00577d0101i1up.html
http://yeyuan.iteye.com/blog/1272414
http://www.360doc.com/content/13/0425/11/12094763_280802902.shtml
http://www.jb51.net/article/29818.htm


###启动mysql服务

参见同级目录mysql操作

###授权给从服务器192.168.1.12

```mysql> GRANT REPLICATION SLAVE ON *.* to 'root'@'192.168.1.12' identified by 'password_123';```



http://yeyuan.iteye.com/blog/1272414
配置好server-id之后，再次查看就有了。


```GRANT FILE ON *.* TO 'root'@'192.168.1.12' IDENTIFIED BY 'password_123';
```

查询主数据库状态:

show master status;应该有数据，但是没有,这样在配置从服务器的时候也要为空

如果为空，则需要指定后面两个一个为空，一个为4




start slaves

出错：

http://bg.artuion.com/window_linux/122.html

SHOW VARIABLES LIKE 'server_id';  

http://yeyuan.iteye.com/blog/1272414






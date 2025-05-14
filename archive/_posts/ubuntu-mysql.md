---
title: Ubuntu安装MySQL踩坑
date: 2020-07-29 20:04:35
tags:
- MySQL
- Development
categories:
- Dev
---



# 在阿里云Ubuntu主机上安装MySQL



## 1. 安装

运行如下命令

``` bash
sudo apt-get install mysql-server

sudo apt-get isntall mysql-client

sudo apt-get install libmysqlclient-dev
```



## 2. 改密码（天坑）



#### 1. sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf

找到如下内容

``` 
 31 user        = mysql
 32 pid-file    = /var/run/mysqld/mysqld.pid
 33 socket      = /var/run/mysqld/mysqld.sock
 34 port        = 3306
 35 basedir     = /usr
 36 datadir     = /var/lib/mysql
 37 tmpdir      = /tmp
 38 lc-messages-dir = /usr/share/mysql
 39 skip-external-locking
 40 skip-grant-tables 
```

添加 skip-grant-tables

以安全模式启动MySQL



#### 2. sudo service mysql restart

重启mysql服务

MySQL启动之后就可以不用密码登陆了



#### 3. 查看一下user表

``` bash
mysql> select user, plugin from mysql.user;
+------------------+-----------------------+
| user             | plugin                |
+------------------+-----------------------+
| root             | auth_socket           |
| mysql.session    | mysql_native_password |
| mysql.sys        | mysql_native_password |
| debian-sys-maint | mysql_native_password |
+------------------+-----------------------+
4 rows in set (0.00 sec)
```



#### 4. （重点）update mysql.user set authentication_string=PASSWORD('newPwd'), plugin='mysql_native_password' where user='root';

需要将root的plugin修改为mysql_native_password



#### 5. flush privileges;

刷新权限



#### 6. 将mysqld.cnf改回，重启MySQL服务

``` bas
 31 user        = mysql
 32 pid-file    = /var/run/mysqld/mysqld.pid
 33 socket      = /var/run/mysqld/mysqld.sock
 34 port        = 3306
 35 basedir     = /usr
 36 datadir     = /var/lib/mysql
 37 tmpdir      = /tmp
 38 lc-messages-dir = /usr/share/mysql
 39 skip-external-locking
 40 # skip-grant-tables  
```



# 3. 改编码方式

默认安装MySQL后编码方式不一定是utf8，这时如果我们往数据库中添加含中文的记录会报错。


---
title: mysql常用命令
date: 2016-10-14 13:33:33
categories: mysql
tags: [mysql]
---
## 下载MySQL
> basedir = D:\mysql-5.6.34-winx64
> datadir = D:\mysql-5.6.34-winx64\data
> character-set-server=utf8 # 这一行是出现字符集问题的时候要设置的，暂时不用管

<!-- more -->

## 安装MySQL
```
mysqld install
对应卸载:mysqld remove
```
## 启动MySQL服务
```
net start mysql
对应关闭：net stop mysql
```
## 设置root用户的密码
```
mysql -u root -p
```
### 方法一：
```
GRANT ALL ON *.* TO 'root'@'localhost' IDENTIFIED BY '123456';
```
### 方法二：
```
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123456');
FLUSH PRIVILEGES;
```
### 方法三(不用进入mysql cmd)：
```
mysqladmin -u root -p password "123456"
```
### 方法四：
```
USE mysql;#开启mysql的数据库
UPDATE mysql.user SET password=password('123456') where user='sxw' and host='localhost';#更新password字段
```
## mysql提示符
mysql -uroot -p -P3306 -h127.0.0.1 --promat \D\d\h\u # 分别是具体时间，数据库名字，主机名，用户名
## 新建用户
### 方法一：
```
CREATE user 'sxw'@'localhost' identified by '123456';
GRANT ALL PRIVILEGES ON *.* to 'sxw'@'localhost' identified by '123456';# 如果想远程登陆，将localhost替换为%，表示在任何一台电脑都可以登陆
```
### 方法二：
```
INSERT INTO mysql.user(HOST,USER,PASSWORD) VALUES ('localhost','sxw',PASSWORD('123456'));
GRANT SELECT,UPDATE,CREATE,DROP,DELETE ON sxwDB.* TO 'sxw'@'%'IDENTIFIED BY '123456' WITH GRANT OPTION;#授权sxw用户对sxwDB这个数据库的所有表具有所有权限
FLUSH PRIVILEGES;
```
## 查看MySQL版本
```
方法一：status;
方法二：select version();
```
## 查看MySQL端口号
```
show global variables like 'port';
```
## 查看当前打开的数据库
```
select database();
```
## 查看当前时间
```
select now();
```
## 查看当前用户
```
select user();
```
## 查看当前所有用户名，密码，主机名
```
select user,password,host from mysql.user;
```
## 查看所有数据库
```
show databases;
```
## 打开数据库
```
use sxw;
```
## 列出所有表
```
show tables;
```
## 查看数据表结构
```
describe table_name;
```
## 删除数据库和数据表
```
drop database sxw;
drop table table_name;
```
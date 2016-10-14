---
title: mysql常用命令
date: 2016-10-14 13:33:33
categories: mysql
tags: 
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
# 插入用户名和密码
INSERT INTO mysql.user(HOST,USER,PASSWORD) VALUES ('localhost','sxw',PASSWORD('123456'));
# 授权
GRANT ALL PRIVILEGES ON langying.* TO 'sxw'@'%'IDENTIFIED BY 'sxw' WITH GRANT OPTION;
# 刷新权限
FLUSH PRIVILEGES;
```
## 查看MySQL版本
```
进入mysql cmd
方法一：status;
方法二：select version();
```
## 查看MySQL端口号
```
show global variables like 'port';
```
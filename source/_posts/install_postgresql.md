title: postgresql安装、非root用户配置、登录
date: 2017-03-19 17:23:30
categories: [技术]
tags: [sentry]
description: 在安装Sentry的时候，需要用到pg，才去尝试了解这个数据库。
---

# 简介([来自百度百科](http://baike.baidu.com/item/PostgreSQL))
> PostgresQL是以加州大学伯克利分校计算机系开发的 POSTGRES，现在已经更名为PostgreSQL，版本 4.2为基础的对象关系型数据库管理系统（ORDBMS）。PostgreSQL支持大部分 SQL标准并且提供了许多其他现代特性：复杂查询、外键、触发器、视图、事务完整性、MVCC。同样，PostgreSQL 可以用许多方法扩展，比如， 通过增加新的数据类型、函数、操作符、聚集函数、索引。免费使用、修改、和分发 PostgreSQL，不管是私用、商用、还是学术研究使用。<!--more-->

# 安装
此文介绍linux下安装，所以想到的必然有三种方式：yum、rpm包（本文不介绍）、二进制文件（通常以tar.gz打包压缩）
如果是非root用户的话，直接跳到[二进制文件安装](#二进制文件安装)

## yum安装
### 默认安装版本
| Distribution        | Version    |
| --------   | -----:   |
|RHEL/CentOS/SL/OL 7 | 9.2(also supplies package rh-postgresql95 and rh-postgresql94 via SCL)|
|RHEL/CentOS/SL/OL 6 | 8.4(also supplies package postgresql92)|
|RHEL/CentOS/SL/OL 5 | 8.1 (also supplies package postgresql84)|
|Fedora 24 | 9.5|
|Fedora 23 | 9.4|

### 安装指定版本的yum源再安装
1. 选择合适的yum源
打开[官方网站](https://yum.postgresql.org/repopackages.php)，找到合适的版本，然后执行yum install命令，例如：
`yum install https://download.postgresql.org/pub/repos/yum/9.4/redhat/rhel-6-x86_64/pgdg-centos94-9.4-3.noarch.rpm`
2. 安装postgresql
`yum install postgresql94-server postgresql94-contrib`
3. 初始化
`service postgresql-9.4 initdb`
4. 启动服务
`service postgresql-9.4 start`
5. 连接
`psql -d postgres`
6. 可能的问题
`Error: Cannot retrieve repository metadata (repomd.xml) for repository: pgdg94. Please verify its path and try again`
**解决**：
a. `vi /etc/yum.repos.d/pgdg-94-centos.repo`，设置`enabled=0`
b. `yum install ca-certificates`
c. `vi /etc/yum.repos.d/pgdg-94-centos.repo`，设置`enabled=1`


## 二进制文件安装
1.  下载地址
`https://www.enterprisedb.com/download-postgresql-binaries`
2. 检查有无安装，有则卸载
```
rpm -qa|grep postgresql
yum remove postgresql*
```
3. 安装前准备（非root用户跳过）
如果拥有root用户权限，建议新增一个psotgres用户，建议不要起别的名字，因为这是pg默认的超级管理员用户
`useradd postgres`
4. 解压并创建pg的数据存储目录
```
tar xzf postgresql-9.4.9-1-linux-x64-binaries.tar.gz 
makedir data
```
5. 初始化
`pgsql/bin/initdb -E utf8 -D data`
6. 启动
```pgsql/bin/pg_ctl -D data -l logfile start```
7. 连接
`pgsql/bin/psql -d postgres`
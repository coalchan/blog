---
title: 实时错误日志收集平台sentry安装
date: 2017-03-30 22:50:30
categories: [技术]
tags: [sentry]
description: 本文的撰写参照官网的安装文档，旨在自我的安装问题记录与回顾
feature: /images/sentry_index.jpg
---

# 什么是sentry
这里说的不是[Apache Sentry](https://sentry.incubator.apache.org/)，而是一个实时的事件日志和聚合平台，基于 Django 构建。Sentry 可以帮助你将程序的所有exception自动记录下来，然后在一个好用的 UI 上呈现和搜索。处理exception是每个程序的必要部分，所以 Sentry 也几乎可以说是所有项目的必备组件。最早仅仅支持python的exception，而今已经支持市面上常见的各种开发语言，具体请参照[官网](https://docs.sentry.io/clients/)。下面是安装步骤，当时安装的版本为8.14.1，要想安装最新版本最好还是参照[官网](https://docs.sentry.io/server/installation/)。<!--more-->先抢先看下结果画面——![](/images/sentry_view.jpg)

# 安装环境
1. CentOS release 6.5
2. postgres 9.4
    这个安装可以参照[postgresql安装、非root用户配置、登录](/2017/03/19/install_postgresql/ )
3. redis 2.6.10
4. Python 2.7.13
5. pip 8.1

# 安装
这里推荐本地用户安装：
`pip install --user sentry`
如果安装较慢建议使用豆瓣源，最方便的设置如下：
```
[sentry@my_centos ~]$cat .pip/pip.conf 
[global]
index-url = http://pypi.douban.com/simple/
trusted-host = pypi.douban.com
```

# 初始化
## 设置配置文件路径
```
mkdir $HOME/sentry/
echo "export SENTRY_CONF=$HOME/sentry/" >> ~/.bash_profile
source ~/.bash_profile
```

## 初始化
`sentry init $SENTRY_CONF`
完了会生成两个文件：`sentry.conf.py`和`config.yml`

## 修改配置文件
主要是配置pg, redis, email
`vi $SENTRY_CONF/sentry.conf.py`
```
DATABASES = {
    'default': {
        'ENGINE': 'sentry.db.postgres',
        'NAME': 'sentry',
        'USER': 'postgres',
        'PASSWORD': '',
        'HOST': '',
        'PORT': '',
    }
}

BROKER_URL = 'redis://:password@redis_host:6379/1'
```

`vi config.yml `
```
mail.backend: 'smtp'  # Use dummy if you want to disable email entirely
mail.host: 
mail.port: 25
mail.username: 
mail.password:
mail.use-tls: false
# The email address to send on behalf of
mail.from: 

redis.clusters:
  default:
    hosts:
      0:
        host: redis_host
        port: 6379
        password: "password"
```

## 在pg中创建数据库，用于存放sentry数据
`pgsql/bin/createdb -E utf-8 sentry` 

## 初始化数据库脚步
`sentry upgrade`

## 创建sentry用户（如果上一步没有创建的话）
`sentry createuser`

# 启动服务
## 启动Web Service（用于登录账户）
`sentry run web`

## 启动后台workers进程（用来发邮件等）
`sentry run worker`

## 启动定时脚步（猜测是用于发周报）
`sentry run cron`

## 实际生产
考虑到服务比较多，为了便于管理，采用[supervisord](!http://www.supervisord.org/)，管理这3个进程。
### 生成配置文件
`echo_supervisord_conf > supervisord.conf`
### 加入以下配置
```
[program:sentry-web]
command=sentry start
autostart=true
autorestart=true
redirect_stderr=true
stdout_logfile=syslog
stderr_logfile=syslog

[program:sentry-worker]
command=sentry run worker
autostart=true
autorestart=true
redirect_stderr=true
stdout_logfile=syslog
stderr_logfile=syslog

[program:sentry-cron]
command=sentry run cron
autostart=true
autorestart=true
redirect_stderr=true
stdout_logfile=syslog
stderr_logfile=syslog
```

### 更改配置(取消注释即可)：
```
[inet_http_server]         ; inet (TCP) server disabled by defaultport=127.0.0.1:9001        ; (ip_address:port specifier, *:port for all iface)
username=user              ; (default is no username (open server))
password=123               ; (default is no password (open server))
```

### 启动
`supervisord  -c supervisord.conf`
### 停止
`supervisorctl -c supervisord.conf shutdown`
### 查看日志
浏览器打开 http://ip:9001
![](/images/sentry_supervisor.jpg)

# 登录使用
浏览器键入 ip:9000，如果看到如下界面，恭喜成功了！
![](/images/sentry_login.jpg)


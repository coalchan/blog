title: 在amazon ec2上搭建pptp vpn
date: 2014-12-13 20:10:30
categories: [技术]
tags: [amazon,vpn]
description: 
---
### 前提
由于amazon的aws有一年的免费服务，只需要有一个双币的信用卡绑定即可，开始注册会刷你1美元的预授权但不会真的扣。aws下的服务很多，具体的你去[aws的官网](http://aws.amazon.com/)看吧，这里介绍用aws的ec2来搭建vpn来合理上网，你懂的（注意在ec2的控制台开启1723端口）。

### 准备
检测是否符合pptp的搭建环境的要求
服务器版本：CentOs 6.4    
如果检查结果没有这些支持的话，是不能安装pptp的。执行指令：
`modprobe ppp-compress-18 && echo ok`
这条执行执行后，显示“ok”则表明通过。不过接下来还需要做另一个检查，输入指令：
`cat /dev/net/tun`
如果这条指令显示结果为下面的文本，则表明通过：
`cat: /dev/net/tun: File descriptor in bad state`
上述两条均通过，才能安装pptp。否则就只能考虑openvpn，或者请你的服务商来解决这个问题。

### 步骤	
{% label 1 primary %} 安装pptpd
``` shell
sudo yum install pptpd
```
64位的也可以
```
wget cache.ali.dagaiba.com/pptpd/pptpd-1.3.4-2.el6.x86_64.rpm
rpm -ivh pptpd-1.3.4-2.el6.x86_64.rpm
```

{% label 2 primary %} 配置pptpd
<!--more-->
    (1). 修改`/etc/pptpd.conf`文件, 在最先面添加以下2行
```
localip 192.168.9.1
remoteip 192.168.9.11-30
```

   (2). 修改`/etc/ppp/options.pptpd`文件, 加上谷歌的dns
```
ms-dns 8.8.8.8
ms-dns 8.8.4.4
```

   (3). 修改`/etc/ppp/chap-secrets`文件, 配置你自己VPN的用户名/密码, 格式如下:
```
<username> pptpd <passwd> *
```

   (4). 修改`/etc/sysctl.conf`文件, 添加以下内容(默认里面有这行, 把0改为1即可)
```
net.ipv4.ip_forward=1
```

   (5). 执行`sudo /sbin/sysctl -p`重新加载配置

   (6). 启用iptables的NAT configuration
```
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

   (7). 为了保证每次EC2实例重启后NAT configuration能启动, 还要修改`/etc/rc.local`文件, 在`exit 0`这行上面加上
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

{% label 3 primary %} 重启pptpd服务
```
sudo /etc/init.d/pptpd restart
```

### 连接
我用的是windows，它自带就可以创建vpn连接了，很方便的，然后就很开心啦啦啦。
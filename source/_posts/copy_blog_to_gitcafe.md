title: 将hexo构建的博客同时放在github和gitcafe上
date: 2014-12-14 20:10:30
categories: [技术]
tags: [hexo,gitcafe]
description: 
---
### 写在前面
之前就有看到有人将博客从github迁移到gitcafe上了，因为github在国内的访问速度真的不够快，有时甚至还上不了，而[gitcafe](https://gitcafe.com)在国内的访问真心很快，谁用谁知道。

### 开始吧
考虑到两个因素，保证网站能够访问（*github和gitcafe同时挂掉的可能性应该是很低的啦*），然后是国内国外分别走不同的线路，达到优化。
<!--more-->
{% label primary @1 %} 在gitcafe上创建一个与用户名相同名称的项目,并且在自定义域名里添加上你的域名。

{% label primary @2 %} 修改_config.yml，hexo已经可以支持多个repo部署了，所以很方便。
```
deploy:
  type: git
  message: new
  repo:
    github: ssh://git@github.com/yourname/yourname.github.io.git,master
    gitcafe: git@gitcafe.com:yourname/yourname.git,gitcafe-pages
```
{% label warning @最好严格按照上面的格式来进行修改，否则可能有问题，另外如果部署的时候还是报错的话，就删除`.deploy`文件夹（失败了好多次，才发现都是这个文件夹惹的祸）。 %}


{% label primary @3 %} 修改dnspod
管理域名，最好还是[dnspod](https://www.dnspod.cn)啦。
以前注册的dnspod可以通过设置“国外”或“国内”来定义线路，后来注册的需要“自定义线路”的话是要付费用户的，但是也可以通过多添加几条记录来达到相同目的，即设置“电信”“联通”“教育网”都指向gitcafe提供的ip，“默认”的指向github即可。设置完了，一般是几秒钟就生效了，你可以在本地ping一下你的网址，然后在用vpn或其他代理工具登录到国外网站，然后再ping一下你的网址，就可以得到验证啦。



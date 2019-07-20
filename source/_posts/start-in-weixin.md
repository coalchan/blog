title: 利用heroku,nodejs进行微信公众平台的接入
date: 2014-11-12 23:10:30
categories: [技术]
tags: [nodejs,微信]
description: 微信公众平台开发的第一步
feature: images/weixin.png 
toc: true
---
## 写在前面
我的微信公众平台已经申请好久了，但是一直没拿来用干啥的，最近捣鼓了下nodejs，除了仿照着网上的例子做了一个豆瓣电影新片榜的爬虫就没干啥了。毕竟要学以致用吗，就想着可以做一个爬虫来爬自己的网站最近更新的文章然后推送给微信公众平台。于是开始了，下面是成为开发者要做的工作。参照[微信公众平台接入指南
](http://mp.weixin.qq.com/wiki/index.php?title=%E6%8E%A5%E5%85%A5%E6%8C%87%E5%8D%97)。
<!--more-->
## heroku
[heroku](http://heroku.com/)简单来说是一个支持多种编程语言的PAAS，详细请参考[heroku的维基百科](http://zh.wikipedia.org/wiki/Heroku)。这里用来作为我们微信公众平台的服务器端代码的部署。

## nodejs
[nodejs](http://nodejs.org/)简单来说是一个服务器端js语言，详细请参考[nodejs的维基百科](http://zh.wikipedia.org/wiki/Node.js)。这里用来作为我们微信公众平台的开发语言。

## 废话少说，开始吧

### 开始heroku的nodejs之旅
注册一个heroku账号，`new`一个app,名称为appname(下面会用到)，参照[heroku的官方教程](https://devcenter.heroku.com/articles/getting-started-with-nodejs#introduction)一步步地进行，前提是熟悉git的基本操作就没啥问题。

### 关键代码
下面附上我的代码——
``` javascript
var http = require('http');
var url = require('url');
var sha1 = require('sha1');
var token = '你要在公众平台服务器配置填写的token';

http.createServer(function(req, res) {
	var gurl = url.parse(req.url, true);
	var signature = gurl.query.signature;
	var timestamp = gurl.query.timestamp;
	var nonce = gurl.query.nonce;
	var echostr = gurl.query.echostr;	
	var array = [token, timestamp, nonce];
	array.sort();
	
	var str = sha1(array.join(""));

  if(str == signature) {
		res.end(echostr);
	}
}).listen(process.env.PORT || 80);
```

### 部署测试
此时访问`http://appname.herokuapp.com?signature=f60e14a9ca350972c5134540d573f33ac8a51604&timestamp=timestamp&nonce=nonce&echostr=OK`，如果你有看到浏览器返回了`OK`,应该就代码部署OK了。

### 公众平台的服务器配置
在`URL`中填写`appname.herokuapp.com`,`token`和代码中保持一致，`EncodingAESKey`随意，然后提交，一般在页面会通知你认证通过了。
接下来就是你的微信公众平台真正的开发啦啦啦。。。

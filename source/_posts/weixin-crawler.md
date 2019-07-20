title: 微信公众平台爬取网页内容
date: 2014-11-27 22:10:30
categories: [技术]
tags: [微信,js]
description: 这里利用nodejs爬取本博客最新文章，然后返回给用户
toc: true
feature: images/weixinkf.jpg
---
本来想着可以每天定时群发消息来推送本博客的最新文章，可是发现需要微信认证才能获得群发接口的权限，但是个人订阅号暂时又不能认证。无奈，只得利用已有的接口，利用自动回复指定内容来完成推送。

不多说，这里仍然是使用nodejs的[wechat接口](https://github.com/node-webot/wechat)来实现的，非常easy,直接上代码：
<!--more-->
``` javascript
var wechat = require('wechat');
var express = require('express');

var superagent = require('superagent');
var cheerio = require('cheerio');
var urlhelp = require('url');

var app = express();//var app = express.Router();
app.use(express.query());
app.use('/', wechat('你的token', function (req, res, next) {
  // 微信输入信息都在req.weixin上
  var message = req.weixin;
	if (message.Content === 'test') {
		//文本回复
    
  } else if (message.Content === 'doc') {
		//图文回复
    
  }else if (message.Content === 'mp3') {
		// 回复榜首音乐
		
	}else if (message.MsgType === 'voice') {
    // 回复一段音乐
    
  } else if (message.MsgType === 'event') {
		//关注回复
     
	} else {
		//文本回复
     
  }
}));

app.listen(process.env.PORT || 3000);
```

其中*图文回复*和*回复榜首音乐*都是使用了superagent来完成爬虫的工作，直接上小案例，谁跑谁知道：
``` javascript
var superagent = require('superagent');
var cheerio = require('cheerio');
var urlhelp = require('url');
var express = require('express');

var app = express();
app.listen(process.env.PORT || 5000);
app.get('/', function(req, ares, next) {
    var myUrl = 'http://luckypeng.com/';
    superagent.get(myUrl).end(function(err, res1){
      if(err) {
         
      } 
      var $ = cheerio.load(res1.text);
      var $article = $('.title').eq(0).children('a').first(); 
      var title = $article.text();
      var description = $article.attr('title');
      var url = urlhelp.resolve(myUrl,$article.attr('href'));
      var picurl = urlhelp.resolve(myUrl, $('.nofancybox').eq(0).attr('src')); 
      
      var latest = {
      title: title,
      description: description,
      url: url,
			picurl: picurl
      } 
			ares.send(latest);
      console.log(latest);     
      console.log('已经获取最新文章!')
    });
});

console.log('-----------------------------');
```

怎么样，是不是so easy，有空请关注我的公众号哦。
![](http://luckypeng.qiniudn.com/qrcode_for_gh_57a59748aef9_258.jpg)

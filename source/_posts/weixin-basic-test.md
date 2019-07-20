title: 微信公众平台开发基础接口测试
date: 2014-11-13 22:10:30
categories: [技术]
tags: [微信,js]
description: 这里以接收用户消息(包括文本、语音)，回复消息（包括文本、音乐、图文）为例测试了微信公众平台开发基础接口。
toc: true
feature: images/basicTest.jpg
---
## 介绍
这里使用了[nodejs版本的微信公共平台消息接口服务中间件](https://github.com/node-webot/wechat)来进行测试，非常简单。

## 测试步骤

### 准备
安装`wechat`和`express`
- `npm install wechat`
- `npm install expreess`

接着在你的项目目录下运行`npm init`，将依赖的npm库的信息写进package.json，为的是push到heroku上之后，能帮你自动安装依赖。
<!--more-->
### 上代码
改写app.js
``` javascript
var wechat = require('wechat');
var express = require('express');

var app = express();//var app = express.Router();
app.use(express.query());
app.use('/', wechat('luckypeng', function (req, res, next) {
  // 微信输入信息都在req.weixin上
  var message = req.weixin;
	if (message.Content === 'text') {
		//文本回复
    res.reply({
      content: '文本信息',
      type: 'text'
    });
  } else if (message.MsgType === 'voice') {
    // 回复一段音乐
    res.reply({
      type: "music",
      content: {
        title: "来点music",
        description: "一无所有",
//        musicUrl: "http://luckypeng.qiniudn.com/一无所有.mp3",
        hqMusicUrl: "http://luckypeng.qiniudn.com/一无所有.mp3"
      }
    });
  } else {
    // 图文回复
    res.reply([
      {
        title: '微信公众平台开发基础接口测试',
        description: '点击详情查看',
        picurl: 'http://luckypeng.com/images/basicTest.jpg',
        url: 'http://luckypeng.com/2014/11/13/weixin-basic-test/'
      }
    ]);
  }
}));

app.listen(process.env.PORT || 3000);
```

### 上传代码
```
git add .
git commit -m "weixin basic test"
git push heroku master
```

## 测试吧
![](http://luckypeng.qiniudn.com/基础接口测试.png)

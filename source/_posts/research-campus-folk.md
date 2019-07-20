title: 统计一些民谣的歌词的押韵情况（一）
date: 2014-12-17 22:10:30
categories: [技术]
tags: [民谣,nodejs]
description: 离开校园后才开始真正喜欢校园民谣。。。
feature: images/compus-folk.jpg
---
### 写在前面
由于个人比较喜欢民谣，特别是[校园民谣](http://baike.baidu.com/link?url=XT6hfXuVqF9p1OtN5owkXfYIHVet3vzLtT_DBzI8kIeJlMX8JsahFVD2BzVFlnKOKpI0lRuRJta7_I85RUY8Ea)，结果听的多了，发现这些个歌词好像有不少相同的地方，特别是押韵的字上面，于是想着统计下，看看能够得到什么结果。

### 思路
1. 找出所有歌词的每一句的最后一个字，进行单字次数、和其韵母次数的统计。
2. 对于每一篇歌词，根据韵母出现最多的，定为韵脚，同时记录该韵脚对应的每句最后一个字出现的次数，然后再总的进行韵脚和字的次数的统计。
显然，第1步很容易，那么这篇文章就从这个开始做起吧，下次再说第2步。<!--more-->

### 技术点
1. 爬取中文歌词（要断句）
解决：我发现在百度音乐中的歌曲详情页是有歌词的下载地址的，那就很方便了;
2. 获取汉字的拼音 -> [nodejs版本的汉字拼音转换工具](https://github.com/hotoo/pinyin)，进而区别拼音的声母和韵母
解决：[nodejs版本的汉字拼音转换工具](https://github.com/hotoo/pinyin)提供了获取声母的方法，却没有提供获取韵母的方法，稍微看了一下代码，可读性很高，于是自己很容易就实现了（[传送门](https://github.com/coalchan/pinyin)）。

### 步骤
1. 例如想知道老狼的歌曲的情况，找到[老狼的专辑页面](http://music.baidu.com/artist/1314/album),上面就有他的所有专辑情况（*可能不止这些，只不过百度音乐上只有这些，不过也应该差不多了吧*），得到这些专辑的链接后，再去找收录的歌曲，然后便可以下载歌词了。
2. 得到歌词后，读取歌词正文，得到每一行的最后一个字，然后用上面的方法得到韵母，再进行统计排序即可。

代码就不贴了，请见github上的项目：[lyricAnalysis](https://github.com/coalchan/lyricAnalysis)

### 小问题
1. 歌词中有重复的段落导致重复的字，如何处理，distinct，或许无关大碍?
2. 获取歌词正文的方式不够好，优化？
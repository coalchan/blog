title: 博客文章的单词统计
date: 2014-12-04 17:00:00
categories: [技术]
tags: [nodejs, js]
description: 对自己博客的文章进行中文分词处理，然后统计词频
feature: images/blog-wordcount.jpg
---
闲来无事，想着自己也捣鼓了不少文字了，就用所学的知识统计一下词频，看看我的文字倾向。

统计词频的关键，肯定是分词，尤其对于中文来说不容易，一般需要建立词库才能分好中文的词汇，我上网找了半天，终于发现一个不错的nodejs版本的中文分词工具——[node-segment](https://github.com/leizongmin/node-segment)，这个使用起来非常方便，谁用谁知道。接下来就是合并相同词语并排序了，这里的排序使用的是[lodash](https://github.com/lodash/lodash)。

说起来，其实这个分词工具最大的好处在于它对中文语法的词性进行了考虑，<!--more-->比如有形容词、区别词、连词、副词、叹词 、方位词、成语、习语等等。我这里就先对我博客中的所有文章的名词进行了统计。

照样直接上代码：
``` javascript
/**
 * 指定目录下文章单词统计
 */
 
var Segment = require('./index').Segment;
var POSTAG = require('./index').POSTAG; 
var fs = require('fs');
var lodash = require('lodash');

//深度优先遍历指定目录下的所有文件路径
var fileList = [];
function getFileList(path){
    var fileList = [];
    var dirList = fs.readdirSync(path);
    dirList.forEach(function(item){
        if(fs.statSync(path + '/' + item).isDirectory()){
            getFileList(path + '/' + item);
        }else{
            fileList.push(path + '/' + item);
        }
    });
    return fileList;
}

//单词数统计
function wordCount(retList) {
   var wc = {};
   for (var k in retList) {
      var ret = retList[k];
      for (var i in ret) {
         var word = ret[i].w;
         var type = ret[i].p;
         if(!(type === 0x00100000)) {// 如果只统计[名词 名语素]
           continue;
         }
         if(!wc.hasOwnProperty(word)) {
            wc[word] = 1;
         }else {
            wc[word]++;
         }
      }
   }
   return wc;
}

//var fileList = getFileList('D:/programme/git/node-segment/testFile');
var fileList = getFileList('D:/programme/git/MyHexo/source/_posts');
var segment = new Segment();
segment.useDefault();// 使用默认的识别模块及字典

//存储分词结果
var retList = [];
//对所有文件进行分词
for(var i in fileList) {
   var text = fs.readFileSync(fileList[i], 'utf8');
   var ret = segment.doSegment(text);
   fs.appendFile('./log.txt',JSON.stringify(ret),'utf8',function(err){  
      if(err){  
         console.log(err);  
      }  
   });
   retList.push(ret);
}
//console.log(retList);

//对所有文件进行单词统计
var wc = wordCount(retList);
retList = [];
//console.log(wc);
//console.log(wc);

//将单词统计结果写入文件
fs.writeFile('./log1.txt',JSON.stringify(wc),'utf8',function(err){  
   if(err){  
     console.log(err);  
   }  
}); 

//排序
var keys = Object.keys(wc);
var characters = [];
for(var i in keys){
   var key = keys[i];
   var value = wc[key];
   characters.push({
      'word' : key,
      'count' : value
   });
}
wc = {};
var res = lodash.map(lodash.sortBy(characters, 'count'), lodash.values).reverse();

//将排序后的结果写入文件
fs.writeFile('./log2.txt',JSON.stringify(res),'utf8',function(err){  
   if(err){  
     console.log(err);  
   }  
});
```

对于前15的词语，请见下图：
![](http://luckypeng.qiniudn.com/blog_wordcount.png)

由此可见嘛，我还是最爱电影啦，但是对于社会、政府、国家层面的各种问题我也是很关心的，此外我还重视个人自由、文化等方面，貌似很准嘛，呵呵！

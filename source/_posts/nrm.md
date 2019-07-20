title: nrm——快速切换NPM源
date: 2014-12-16 20:10:30
categories: [技术]
tags: [nodejs]
description: nrm是一个 NPM 源管理器，允许你快速地在如下 NPM 源间切换：
---
## 安装
`npm install -g nrm`

## 使用
### 列出可选的源
```
nrm ls                                                                                                                                 

* npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
  taobao - http://registry.npm.taobao.org/
  eu ----- http://registry.npmjs.eu/
  au ----- http://registry.npmjs.org.au/
  sl ----- http://npm.strongloop.com/
  nj ----- https://registry.nodejitsu.com/
```<!--more-->
{% alert warning %}带 * 的是当前使用的源，上面的输出表明当前源是官方源。{% endalert %}

### 切换
切换到taobao
```
nrm use taobao 

 Registry has been set to: http://registry.npm.taobao.org/
```

### 增加源
你可以增加定制的源，特别适用于添加企业内部的私有源。[私有源可以使用cnpmjs架设](http://segmentfault.com/a/1190000000368906)。
`nrm add  <registry> <url> [home]`

### 删除源
`nrm del <registry>`

### 测试速度
你还可以通过 nrm test 测试相应源的响应时间。
例如，测试官方源的响应时间：
```
nrm test npm                                                                                                                               

  npm ---- 1328ms
```
测试所有源的响应时间：
```
nrm test

  npm ---- 891ms
  cnpm --- 1213ms
* taobao - 460ms
  eu ----- 3859ms
  au ----- 1073ms
  sl ----- 4150ms
  nj ----- 8008ms
```
- - - 
*转自[nrm —— 快速切换 NPM 源 （附带测速功能）](http://segmentfault.com/a/1190000000473869)*












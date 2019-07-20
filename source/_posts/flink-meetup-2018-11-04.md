---

title: 2018-11-04参加Flink meetup总结
date: 2018-11-05 22:45:00
categories: [技术]
tags: [flink]

---

## Flink在有赞的实践

1. 实时框架从Storm到Spark Structed Streaming，到今年才开始的Flink尝试
2. Spark Structed Streaming对SQL支持不够（如不支持多个聚合、count distinct等）
3. Flink的实际踩坑经历：
   - container超过配置的数量，解决：[FLINK-9567](https://issues.apache.org/jira/browse/FLINK-9567)
   - 开启延迟监控后收到太多报警，解决1：[FLINK-10243](https://issues.apache.org/jira/browse/FLINK-10243)；解决2：[FLINK-10246](https://issues.apache.org/jira/browse/FLINK-10246)
4. Flink结合spring：如何获取SpringContext（单例，在算子的open方法中获取bean）
5. Flink异步不支持KeyedState（没搞懂。。）
6. Flink CEP简介

小结：刚开始尝试flink，坑比较多，需要紧跟开源社区。<!--more-->

## Flink在袋鼠云一站式大数据平台中的使用

专题演讲：Flink SQL的扩展

开源地址：https://github.com/DTStack/flinkStreamSQL

另外推荐他们开源的离线同步工具，也是用Flink实现的，叫做[flinkx](https://github.com/DTStack/flinkx)，基本上是参考了[datax](https://github.com/alibaba/DataX)的设计思路。

1. 为什么要扩展Flink SQL
   * 实际需要，有些数据就是在外部，而非在流中。
2. 如何实现流与维表的JOIN
   - 尽量少修改源码，只在SQL解析时转化成流
   - 两种方式：
     1. LRU维表：继承`RichAsyncFunction`，相关：Async I/O
     2. ALL维表：继承`RichFlatMapFunction`

小结：推崇SQL的开发效率和学习成本，相信SQL可以解决大部分问题。

## 汇智在Flink上的实践

写在前面：号称做的都是国家安全部门的项目，下面都是虚拟案例。。。

专题演讲：Flink CEP的一些实践案例，主要就是利用flink的实时处理能力，搭配自研的规则引擎模块，做一些规则匹配与计算。

1. 案例：商品的实时监控
   - UDF实现：在open方法中，将规则加载到内存
2. 案例：实时在线统计
   - 利用flink算子实现
3. 案例：定时器功能
   - 使用DataStream API中的process function实现

小结：逻辑简单，但是号称每秒数据量能有百万，估计集群规模也不小。

## Stream Processing with Apache RocketMQ and Apache Flink

小结：没听进去，干货不多，讲了好多RocketMQ的东西。。。

## 提高Flink易用性

小结：讲了很多阿里内部对于Flink使用上的一些努力，听下来觉得很牛逼，但是基本没怎么说怎么实现的。如：资源自动优化、一键诊断等等。

扩展阅读：[Flink已经足够强大了吗？阿里巴巴说：还不够](https://mp.weixin.qq.com/s/hike1xQcykFyXpNb6E11tw)

## 总结

本次大会干活第一：袋鼠云案例

## 附

如要下载PPT，请加入钉钉群（扫描下方二维码，或者搜索钉钉群号21789141）

![钉钉群](/images/flink-dingtalk.png)
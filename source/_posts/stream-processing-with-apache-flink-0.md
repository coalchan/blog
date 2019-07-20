---
title: Flink流计算——前言和目录
date: 2018-11-27 21:08:39
categories: [翻译]
tags: [flink]
description: 原文链接：https://www.safaribooksonline.com/library/view/stream-processing-with/9781491974285/
---

## 本书概要

对于提前发布的电子书来说，你得到的是最原始的内容，甚至没有经过作者的编辑，因此你可以早在官方正式发布之前就利用其中的技术。此外，在本书有重大修改、新章节更新以及最终的电子书发布的时候，都会收到更新提示。

开始学习Apache Flink吧，这套开源框架让你驾驭流式数据处理，诸如用户的交互行为、传感器数据和机器日志等。通过这套实用指南，你将学会使用Apache Flink的流处理API来实现、持续运行和维护实际的程序。

作者之一Fabian Hueske是Flink的创建者，另外一位作者Vasia Kalavri是Flink图处理API (Gelly)的核心贡献者。他们解释了并行处理的基本概念，告诉你流式分析与传统批处理数据分析的区别。通过此书，软件工程师、数据工程师以及系统管理员将会学到Flink的DataStream API的基础知识，包括它的架构和一个基本的Flink流处理程序的组件。<!--more-->

* 使用Apache Flink的DataStream API来解决现实世界的问题
* 为开发一个Flink的流处理程序创建一个环境
* 设计流式程序并且将周期的批处理工作迁移到持续的流处理工作上
* 学习处理一组记录的窗口操作
* 将数据流提取到一个DataStream程序，并将结果流发到不同的存储系统中
* 实现流处理应用中创建有状态以及自定义运算
* 操作、维护和更新持续运行的Flink流处理程序
* 研究几个部署选项，包括高可用安装的配置

## 出版社资源

勘误页：[http://oreilly.com/catalog/0636920057321/errata](http://oreilly.com/catalog/0636920057321/errata)

## 关于出版社

O Reilly Media通过在线和面对面的培训、书籍、视频、研究和会议传播创新者的知识。自1978年以来，O Reilly一直是前沿技术发展的记录者和催化剂，瞄准了……

[更多关于O'Reilly Media, Inc.](https://www.safaribooksonline.com/library/publisher/oreilly-media-inc/)

## 目录

1. 有状态的流处理介绍
   * 传统的数据结构
   * 有状态的流处理
     * 事件驱动的应用
     * 数据管道与实时ETL
     * 流式分析
   * 开源流处理的演进
   * Flink尝鲜
   * 你将在这本书中学到什么
2. 流处理的基本原理
   * Dataflow编程模型介绍
     * Dataflow图
     * 数据并行与任务并行
     * 数据交换策略
   * 并行处理无限流
     * 延迟与吞吐量
     * 数据量的操作
   * 时间语义
     * 一分钟是什么意思
     * 处理时间
     * 事件时间
     * 水印
     * 处理时间 VS 事件时间
   * 状态与一致性模型
     * 任务失败
     * 结果保证
   * 小结
3. Apache Flink的架构
   * 系统架构
     * Flink组件配置
     * 应用部署
     * 任务执行
     * 高可用配置
   * Flink中的数据传输
     * 高吞吐和低延迟
     * 背压流量控制
   * 事件时间的处理
     * 时间戳
     * 水印
     * 水印与事件时间
     * 时间戳分配和水印生成
   * 状态管理
     * 操作符状态
     * 带键的（keyed）状态 
     * 后端状态
     * 扩展状态操作符
   * 检查点、保存点以及状态恢复
     * 一致性检查点
     * 从一致性检查点恢复
     * Flink的轻量级检查点算法
     * 保存点
   * 小结
4. 搭建Apache Flink的开发环境
   * 软件要求
   * 在IDE中运行和调试Flink程序
     * 在你的IDE中导入书中的案例
     * 在IDE中运行Flink程序
     * 在IDE中调试Flink程序
   * Flink的maven项目开发向导
5. DataStream的API（v1.4.0）
   * 你好，Flink！
     * 设置运行时环境
     * 读取输入流
     * 应用转换
     * 输出结果
     * 执行
   * 类型
     * 支持的数据类型
     * 类型提示
     * TypeInformation介绍
   * 转换
     * 基本的转换
     * 带键流的转换
     * 多流转换
     * 分区转换
   * 并行设置
   * 引用字段（field）和定义键（key）
   * 定义UDF
   * 添加Flink以及外部的依赖
   * 小结
6. 基于时间和窗口的操作符
   * 时间特征配置
     * 程序中事件时间的时间戳和水印
     * 水印、延迟和完整性
   * 处理函数
     * 计时服务和计时器
     * Emitting to Side Outputs
     * CoProcessFunction介绍
   * 窗口操作符
     * 定义窗口操作符
     * 内置的Window Assigners
     * 在窗口上应用函数
     * 自定义窗口操作符
   * 实时的流Join
   * 处理迟到的数据
     * 丢弃迟到的事件
     * 重定向迟到的事件
     * 使用迟到的事件来更新结果
   * 小结
7. 状态操作符和用户函数
   * 实现有状态的函数
     * 在RuntimeContext中声明带键状态
     * 使用ListCheckpointed接口实现操作符列表状态（Operator List State）
     * 使用连接的广播状态（Connected Broadcast State）
     * 使用CheckpointedFunction接口
     * 接收已完成的检查点的通知
   * 有状态程序的鲁棒性和性能
     * 选择一个状态后端（State Backend）
     * 启用检查点
     * 更新状态操作符
     * 优化有状态程序的性能
     * 防止状态泄露
   * 可查询的状态
     * 架构和启用可查询的状态
     * 暴露可查询的状态
     * 从外部程序中查询状态
   * 小结
8. 外部系统的读写
   * 应用一致性保证
   * 提供的连接器
     * Apache Kafka的source连接器
     * Apache Kafka的sink连接器
     * 文件系统的source连接器
     * 文件系统的sink连接器
     * Apache Cassandra的sink连接器
   * 实现一个自定义source函数
     * 可重置的source函数
     * source函数、时间戳和水印
   * 实现一个自定义sink函数
     * 幂等的sink连接器
     * 事务性的sink连接器
   * 异步访问外部系统
   * 小结
9. 为流处理程序搭建Flink
   * 部署模式
     * 独立集群
     * Docker
     * Apache Hadoop YARN
     * Kubernetes
   * 高可用配置
     * 高可用的独立部署配置
     * 高可用的YARN部署配置
     * 高可用的Kubernetes配置
   * 与Hadoop组件的集成
   * 文件系统配置
   * 系统配置
     * Java和类加载
     * CPU
     * 主存
     * 磁盘存储器
     * 状态后端、检查点以及恢复
     * 安全
   * 小结
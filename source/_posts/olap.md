---
title: 大数据之OLAP系列一——介绍篇
date: 2017-04-30 19:30:30
categories: [技术]
tags: [OLAP]
---

# 历史
其实OLAP的历史很早，只不过在今天的大数据时代，有了更加高级的工具，赋予了这个老的概念以更新的活力。据说最早的OLAP产品出现在1970年，叫做*Express*，但是“OLAP”这个名词直到1993年才被关系数据库之父Edgar F. Codd提出，在90年代的后期OLAP的市场经历了更好的发展，也出现了不少的产品。1998年微软推出了Microsoft Analysis Services，推动了广泛采用OLAP技术。<!--more-->

# 定义
之所以有OLAP的提出，在于当时的OLTP技术不能满足用户对于数据库查询分析的要求，用户的决策需要对关系数据库进行大量计算才能得到结果，而查询的结果并不能满足决策者提出的需求。因此，Codd提出了多维数据库和多维分析的概念，即OLAP。OLAP委员会对联机分析处理的定义为：从原始数据中转化出来的、能够真正为用户所理解的、并真实反映企业多维特性的数据称为信息数据，使分析人员、管理人员或执行人员能够从多种角度对信息数据进行快速、一致、交互地存取，从而获得对数据的更深入了解的一类软件技术。OLAP的目标是满足决策支持或多维环境特定的查询和报表需求，它的技术核心是“维”这个概念，因此OLAP也可以说是多维数据分析工具的集合。
简单从我的理解来说就是，OLAP帮助我们更好的从多个角度去理解现有的数据。

# 分类
1. MOLAP
  Multidimensional OLAP：多维联机分析处理
2. ROLAP
  Relational OLAP：关系型联机分析处理
3. HOLAP
  Hybrid OLAP：混合型OLAP
4. WOLAP
  Web-based OLAP
5. DOLAP
  Desktop OLAP
6. ROLAP
  Real-Time OLAP

## MOLAP
这是一种传统的OLAP形式，它将数据存储在优化后的多维数组存储引擎中，形成数据立方体，即所谓的“cube”，基本原理就是将多维的数据**预先填好**。这种形式的最大优点是查询快，缺点是计算过程长，可能遇到维度爆炸。大部分商业的OLAP产品也是采用这种形式，如Cognos Powerplay, Oracle Database OLAP Option, MicroStrategy, Microsoft Analysis Services, Essbase, TM1, Jedox, 和icCube等等。

## ROLAP
将分析用的多维数据存储在关系数据库中并根据应用的需要有选择的定义一批实视图作为表也存储在关系数据库中。与MOLAP的主要区别是，它不会把所有维度的数据结果计算好，而是需要查询时进行计算。优点和缺点几乎就是MOLAP的对立面，优点在于计算快，维护简单，缺点在于查询慢。

## HOLAP
混合型的OLAP其实只是一种策略，它结合了上述了两种方式的优缺点，提供用户在不同适宜场景下的方案选择，以期望达到更快的查询且不错的稳定性。像Microsoft Analysis Services, Oracle Database OLAP Option, MicroStrategy and SAP AG BI Accelerator都用到HOLAP的技术策略。

## WOLAP,DOLAP,ROLAP
资料太少，以后查到的话再补上。

## 比较
ROLAP实现较容易，但是对于存储的优化、索引、查询性能的提升有很高的要求，而MOLAP主要的挑战在于多维数组的存储，需要维护复杂的结构，而且要注意维度爆炸。而HOLAP就是一种随机应变的策略。

# 相关概念
## 数据仓库
数据仓库的概念提出于20世纪80年代中期，20世纪90年代，数据仓库已从早期的探索阶段走向实用阶段。业界公认的数据仓库概念创始人W.H.Inmon在《BuildingtheDataWarehouse》一书中对数据仓库的定义是：“数据仓库是支持管理决策过程的、面向主题的、集成的、随时间变化的持久的数据集合”。构建数据仓库的过程就是根据预先设计好的逻辑模式从分布在企业内部各处的OLTP数据库中提取数据并对经过必要的变换最终形成全企业统一模式数据的过程。

## Hive
hive是基于Hadoop的一个数据仓库工具，可以将结构化的数据文件映射为一张数据库表，并提供简单的sql查询功能，可以将sql语句转换为MapReduce任务进行运行。

## 小结
数据仓库是存储数据的仓库，要构建这个仓库需要一些工具，Hive便是大数据下的一种选择，而OALP是这个仓库的具体应用，主要用来分析数据，支撑决策。

# 参考资料
1. [维基百科-Online analytical processing](https://en.wikipedia.org/wiki/Online_analytical_processing)

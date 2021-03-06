---
layout: post
title:  Spark系列文章（一）:Spark初识
category: spark 
tags: 大数据 Spark
keywords: bigdata,Spark,Spark学习之路,Spark系列文章,Spark初识
---

## Spark产生背景、简介、特点、现有使用场景等做简单的介绍解析。

### 什么是Spark
[官网地址：http://spark.apache.org/](http://spark.apache.org/)

spark 是高性能 DAG 计算引擎，一个实现快速通用的集群计算平台。它是由加州大学伯克利分校AMP 实验室开发的通用内存并行计算框架，用来构建大型的、低延迟的数据分析应用程序。它扩展了广泛使用的 MapReduce 计算模型。高效的支撑更多计算模式，包括交互式查询和流处理。spark 的一个主要特点是能够在内存中进行计算，及时依赖磁盘进行复杂的运算。Spark 依然比 MapReduce 更加高效。

发展历程:
- 2009年，Spark 诞生于伯克利大学 AMPLab，最开初属于伯克利大学的研究性项目
- 2010年，正式开源 
- 2013年，成为了 Apache 基金项目，同年，基于 spark 的开源商业公司 Databricks 成立
- 2014年，成为 Apache 基金的顶级项目

### Spark产生背景
Spark 是在 MapReduce 的基础上产生的，借鉴了大量 MapReduce 实践经验，引入多种新型涉及思想和优化策略。

针对MapReduce计算框架存在的局限性进行分析，能更好的了解到 Spark。

MapReduce 的局限性如下:

1、处理效率低效

- Map中间结果写磁盘，Reduce写HDFS，多个MR之间通过HDFS交换数据
- 任务调度和启动开销大
- 无法充分利用内存 
- Map 端和 Reduce 端均需要排序
- 复杂功能 Io 开销大，对于复杂 sql，需转换 MapReduce 计算，需要通过 HDFS 进行磁盘数据交换，而读写Hfds需消耗大量磁盘和网络 IO

2、 不适合迭代计算（如机器学习、图计算等），交互式处理（数据挖掘） 和流式处理（点击日志分析）

3、 MapReduce 编程不够灵活
- 仅支持 Map 和 Reduce 两种操作 
- 尝试函数式编程风格

4、计算框架多样化、无形中产生运维和管理成本

### Spark特点
Spark 是基与 MapReduce 基础产生了，克服了其存在的性能低下，变成不够灵活的缺点。

Spark 作为一种 DAG 计算框架，主要特点如下:<br>
1、高效性，性能高效，主要体现如下:

- 内存计算引擎。Spark 允许用户将数据放到内存中以加快数据读取，进而提高数据的处理性能。Spark提供了数据抽象RDD,支持多种存储级别。
- 通用 DAG 计算引擎。相较于 MapReduce 的简单两阶段计算引擎，Spark 则是一种更加通用的DAG引擎，它使得数据可通过本地磁盘或内存流向不同就按单元，而不用想 MapReduce 哪像借助抵消的HDFS.
- 性能高效。Spark 是在 MapReduce 基础上产生的，借鉴和重用了 MapReduce 众多已存在组件和涉及思想，同时引入了大量新颖的设计理念，包括允许资源重用、基于线程池的 Executor，无排序shuffle,通用DAG优化和调度引擎等。据有关测试，相关资源消耗情况下，20-100倍的提升。

2、易用性。<br>
Spark 支持 Java、Python 和 Scala 的 API，还支持超过 80 种高级算法，使用户可以快速构建不同的应用。而且 Spark 支持交互式的 Python 和 Scala 的 shell，可以非常方便地在这些 shell 中使用 Spark 集群来验证解决问题的方法。

3、通用性。<br>
Spark 提供了统一的解决方案。Spark 可以用于批处理、交互式查询（Spark SQL）、实时流处理（Spark Streaming）、机器学习（Spark MLlib）和图计算（GraphX）。这些不同类型的处理都可以在同一个应用中无缝使用。Spark统一的解决方案非常具有吸引力，毕竟任何公司都想用统一的平台去处理遇到的问题，减少开发和维护的人力成本和部署平台的物力成本。

4、兼容性。<br>
Spark可以非常方便地与其他的开源产品进行融合。比如，Spark可以使用Hadoop的YARN和Apache Mesos作为它的资源管理和调度器，器，并且可以处理所有Hadoop支持的数据，包括HDFS、HBase和Cassandra等。这对于已经部署Hadoop集群的用户特别重要，因为不需要做任何数据迁移就可以使用Spark的强大处理能力。

总之，Spark以上有别于MapReduce的特点，使得它在数据分析、数据挖掘和机器学习等方面得到广泛的应用，Spark已取代MapReduce成为应用最为广泛的数据计算引擎，同时基于MapReduce实现的机器学习库Mahout也已经迁移到Spark或Flink.

### Spark的组成
Spark组成(BDAS)：全称伯克利数据分析栈，通过大规模集成算法、机器、人之间展现大数据应用的一个平台。也是处理大数据、云计算、通信的技术解决方案。

它的主要组件有：
- SparkCore：将分布式数据抽象为弹性分布式数据集（RDD），实现了应用任务调度、RPC、序列化和压缩，并为运行在其上的上层组件提供API。
- SparkSQL：Spark Sql 是Spark来操作结构化数据的程序包，可以让我使用SQL语句的方式来查询数据，Spark支持 多种数据源，包含Hive表，parquest以及JSON等内容。
- SparkStreaming： 是Spark提供的实时数据进行流式计算的组件。
- MLlib：提供常用机器学习算法的实现库。
- GraphX：提供一个分布式图计算框架，能高效进行图计算。
- BlinkDB：用于在海量数据上进行交互式SQL的近似查询引擎。
- Tachyon：以内存为中心高容错的的分布式文件系统。

### Spark应用场景

1、Yahoo将Spark用在Audience Expansion中的应用，进行点击预测和即席查询等 <br>
2、淘宝技术团队使用了Spark来解决多次迭代的机器学习算法、高计算复杂度的算法等。应用于内容推荐、社区发现等 <br>
3、腾讯大数据精准推荐借助Spark快速迭代的优势，实现了在“数据实时采集、算法实时训练、系统实时预测”的全流程实时并行高维算法，最终成功应用于广点通pCTR投放系统上 <br>
4、优酷土豆将Spark应用于视频推荐(图计算)、广告业务，主要实现机器学习、图计算等迭代计算 <br>

### 问题讨论
1、 Spark 在任何情况下均比 MapReduce 高效吗？

> 回答: 答案是否定的，在某些数据量非常大的情况下，MapReduce要比Spark快。例如: WordCount计算的数据量到1PB级别时，就会出现。原因为:目前Spark处理shufflw实现相对较差，但之后可能会改观。

2、Spark 号称“内存计算框架”，它将所有数据写到内存吗？

> 回答: 答案是不一定的，存在多种级别类型，可以自行选择。相对建议选择内存，但是内存不够最终还是会写入磁盘中。

3、 当前存在很多 DAG 引擎，包括 Spark Flink ，为何大家都在讨论 Spark ？
> 回答: 从设计架构上来看，Spark机构不一定是最优的，甚至某些方面还不如Flink和Tez.但我觉得相面两个方面可以说明部分问题,社区支持，推广宣传较好，活跃度高，故而关注度也更高。同时spark具有通用性，也是一大原因。





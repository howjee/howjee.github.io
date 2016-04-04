---

layout: post

title: what is hadoop

---


1.到底什么是Hadoop？
-----

-----

要理解什么是Hadoop，[Hadoop官网](http://hadoop.apache.org/)对Hadoop介绍如下摘要所示：

>The Apache Hadoop software library is a framework that allows for the distributed processing of large data sets across clusters of computers using simple programming models. It is designed to scale up from single servers to thousands of machines, each offering local computation and storage. Rather than rely on hardware to deliver high-availability, the library itself is designed to detect and handle failures at the application layer, so delivering a highly-available service on top of a cluster of computers, each of which may be prone to failures.

从以上描述，可以提取出对Hadoop以几点的理解：

- 采用简单的编程模型结构（MapReduce编程模型）就可以在集群服务器上对大数据集进行分布式处理
- Hadoop集群的逻辑结构是支持横向扩展(scale out)的，可以很方便的扩展服务器集群数量
- 集群中的每台既是存储节点也是计算节点，也就是说，Hadoop分布式计算思想可以简单理解成：移动计算比移动数据更有效
- 高可靠性：具有能够检测和处理应用层错误的能力

2.Hadoop包含的主要模块
-----

-----
- Hadoop Common: The common utilities that support the other Hadoop modules.
- Hadoop Distributed File System (HDFS™): A distributed file system that provides high-throughput access to application data.
- Hadoop YARN: A framework for job scheduling and cluster resource management.
- Hadoop MapReduce: A YARN-based system for parallel processing of large data sets.

3.Hadoop生态系统
-----

-----
    用于存储海量数据并从中获取洞察力的开源框架


大量 Apache Software Foundation 项目构成了企业部署、集成和使用 Hadoop 所需的服务。这些项目中的每一个都经过部署，提供明确的功能，都有自己的开发人员群体和各自的发布周期。
![Hadoop Ecosystem](/images/hadoop/Hadoop-Ecosystem.jpg)

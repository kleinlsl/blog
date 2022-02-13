---
title: 基于MapReduce编程模型的内连接算法设计与实现                    #文章页面上的显示名称，一般是中文
date: 2020-10-07 12:14:16 #文章生成时间，一般不改，当然也可以任意修改
tags: [MapReduce,内连接,Hadoop]
categories: [Hadoop]
copyright: true
comments: false
description: 基于MapReduce编程模型的内连接算法设计与实现
top: false
photos: 
	- "/images/Title-photos/photos.png"
---



<h2 align=center>摘  要</h2>
<p>
    信息技术和互联网的发展使得每天都会产生海量的数据，这些数据具有结构复杂、数据量大的特性。连接操作是大数据中常用的且耗时的操作。Map Reduce编程模型的提出，使得大数据处理有了基本的思路。本文通过Map Reduce编程模型设计算法实现内连接操作，并通过TPC-H基准程序生成的数据集，进行算法的正确性检验和算法的性能测试。实验结果表明，本文所设计算法是正确可行的，并且在Map Reduce编程模型下其处理大量数据时有明显的优势。
</p>

> 关键词：MapReduce；Hadoop；内连接；HDFS；TPC-H

<!--more-->


![](/images/blog-images/基于MapReduce编程模型的内连接算法设计与实现/MapReduce-00.png)
## 摘要

![](/images/blog-images/基于MapReduce编程模型的内连接算法设计与实现/MapReduce-01.png)
## 目录

![](/images/blog-images/基于MapReduce编程模型的内连接算法设计与实现/MapReduce-02.png)
## 正文

![](/images/blog-images/基于MapReduce编程模型的内连接算法设计与实现/MapReduce-03.png)

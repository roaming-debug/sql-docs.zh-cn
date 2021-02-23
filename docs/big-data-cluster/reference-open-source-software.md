---
title: 开源软件参考
titleSuffix: SQL Server Big Data Clusters
description: 确定 SQL Server 大数据群集中使用的开源软件和版本。
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 210787512ea3ded5aaab660c8973482bb13c2ff4
ms.sourcegitcommit: 6c93282cce1216dac327cb28848a3ab4d51b776e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2021
ms.locfileid: "100646359"
---
# <a name="open-source-software-reference"></a>开源软件参考

[!INCLUDE [sqlserver2019](../includes/applies-to-version/sqlserver2019.md)]

SQL Server 大数据群集包括由开源项目开发的一些容器。 本文列出了这些容器的特定项目和版本。

## <a name="project-list"></a>项目列表

下表显示了正在使用的开源项目：[!INCLUDE [sssql19-md](../includes/sssql19-md.md)] CU8 和更低版本以及 CU9 和更高版本。 

| Project | CU8 和更低版本 | 从 CU9 开始 |
|--|--|--|
| [collectd](https://collectd.org/) | 5.8.1 | 5.12 |
| [InfluxDB](https://www.influxdata.com) | 1.7.6 | 1.8.3 |
| [Elasticsearch](https://www.elastic.co/) | 7.0.1 | 7.9.1 |
| [Fluent Bit](https://docs.fluentbit.io/manual/about/what-is-fluent-bit) | 1.1.1 | 1.6.3 |
| [Grafana](https://grafana.com/) | 6.3.6 | 7.3.1 |
| Hadoop <br/>[HDFS DataNode](concept-storage-pool.md)<br/>[HDFS NameNode](https://cwiki.apache.org/confluence/display/HADOOP2/NameNode) |3.1.3+|3.3.0|
| [Hive（元存储）](https://hive.apache.org/) |2.3.7|2.3.7<br/>3.0.0（独立）<br/>3.1.2（配置单元）|
| [Kibana](https://www.elastic.co/kibana) | 7.0.1 | 7.9.1 |
| [Knox](https://knox.apache.org/) |1.2.0|1.4.0|
| [Livy](https://livy.apache.org/) |0.6.0|0.7.0|
| [opendistro-elasticsearch-security](https://www.elastic.co/what-is/elastic-stack-security) | 1.0.0.1 | 1.10.1.0 |
| [Openresty (Nginx)](https://openresty.org/) | 1.15.8 | 1.17.8.2 |
| [Spark](configure-spark-hdfs.md) |2.4.6+|2.4.10|
| [Telegraf](https://docs.influxdata.com/telegraf/) | 1.10.3 | 1.16.1 |
| [ZooKeeper](https://cwiki.apache.org/confluence/display/zookeeper) |3.5.8|3.6.2

## <a name="next-steps"></a>后续步骤

查看[使用大数据群集部署的资源](concept-architecture-pods.md)。
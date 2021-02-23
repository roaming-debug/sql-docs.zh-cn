---
title: SQL Server 大数据群集配置属性
titleSuffix: SQL Server big data clusters
description: 大数据群集配置属性的参考文章
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ecaba704d9c08619f42c5cdf8d726917ccc61b9c
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343546"
---
# <a name="sql-server-big-data-clusters-configuration-properties"></a>SQL Server 大数据群集配置属性

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

大数据群集配置设置可在以下范围定义：`cluster`、`service` 和 `resource`。 这些设置的层次结构也遵循此顺序，即从最高到最低。 BDC 组件将使用在最低范围定义的设置的值。 如果未在给定范围定义设置，则它将继承其更高的父范围中的值。 下面是不同范围内每个 BDC 组件的可用设置列表。 你还可以使用 azdata 查看 BDC 的可配置设置。

## <a name="bdc-cluster-scope-settings"></a>BDC 群集范围设置
你可以在群集范围配置以下设置。

|属性|选项|
| --- | --- |
|`mssql.telemetry`|`customerfeedback = { true | false }` |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="sql-service-scope-settings"></a>SQL 服务范围设置
你可以在 SQL 服务范围配置以下设置。

|属性|选项|
| --- | --- |
|`mssql.language`|`lcid = <language_identifier>` |

## <a name="spark-service-scope-settings"></a>Spark 服务范围设置
若要查看所有支持和不支持的设置，请访问 [Apache Spark 和 Apache Hadoop 配置项目](reference-config-spark-hadoop.md)。

## <a name="hdfs-service-scope-settings"></a>HDFS 服务范围设置
若要查看所有支持和不支持的设置，请访问 [Apache Spark 和 Apache Hadoop 配置项目](reference-config-spark-hadoop.md)。

## <a name="gateway-service-scope-settings"></a>网关服务范围设置
没有可配置的网关服务范围设置。 配置网关资源范围内的设置。

## <a name="app-service-scope-settings"></a>应用服务范围设置
无可用项

## <a name="master-pool-resource-scope-settings"></a>主池资源范围设置
|属性|选项|
| --- | --- |
|`mssql.sqlagent`|`enabled = { true | false }` |
|`mssql.licensing`|`pid = { Enterprise | Developer }` |
<!-- |`mssql.collation`|`x = <language_identifier>` | -->

> [!NOTE]
> 更改 SQL Server 实例的默认排序规则是一个复杂的操作。 除了更改 `mssql.collation` 设置外，可能还需要重新创建用户数据库和其中的所有对象。 有关如何执行此操作的说明，请参阅[设置或更改服务器排序规则](../relational-databases/collations/set-or-change-the-server-collation.md#changing-the-server-collation-in-sql-server)

## <a name="storage-pool-resource-scope-settings"></a>存储池资源范围设置
存储池由 SQL、Spark 和 HDFS 组件构成。

### <a name="available-sql-configurations"></a>可用的 SQL 配置
|属性|选项|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.storagePoolCacheSize`| |
|`mssql.storagePoolMaxCacheSize`| |
|`mssql.storagePoolCacheAutogrowth`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |


### <a name="available-apache-spark-and-hadoop-configurations"></a>可用的 Apache Spark 和 Hadoop 配置
若要查看所有支持和不支持的设置，请访问 [Apache Spark 和 Apache Hadoop 配置项目](reference-config-spark-hadoop.md)。

## <a name="data-pool-resource-scope-settings"></a>数据池资源范围设置
|属性|选项|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="compute-pool-resource-scope-settings"></a>计算池资源范围设置
|属性|选项|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="spark-pool-resource-scope-settings"></a>Spark 池资源范围设置
若要查看所有支持和不支持的设置，请访问 [Apache Spark 和 Apache Hadoop 配置项目](reference-config-spark-hadoop.md)。

## <a name="gateway-resource-scope-settings"></a>网关资源范围设置
若要查看所有支持和不支持的设置，请访问 [Apache Spark 和 Apache Hadoop 配置项目](reference-config-spark-hadoop.md)。

## <a name="sparkhead-resource-scope-settings"></a>`Sparkhead` 资源范围设置
若要查看所有支持和不支持的设置，请访问 [Apache Spark 和 Apache Hadoop 配置项目](reference-config-spark-hadoop.md)。

## <a name="zookeeper-resource-scope-settings"></a>Zookeeper 资源范围设置
若要查看所有支持和不支持的设置，请访问 [Apache Spark 和 Apache Hadoop 配置项目](reference-config-spark-hadoop.md)。

## <a name="namenode-resource-scope-settings"></a>Namenode 资源范围设置
若要查看所有支持和不支持的设置，请访问 [Apache Spark 和 Apache Hadoop 配置项目](reference-config-spark-hadoop.md)。

## <a name="app-proxy-resource-scope-settings"></a>应用代理资源范围设置
无可用项

## <a name="next-steps"></a>后续步骤

[配置 SQL Server 大数据群集](configure-bdc-overview.md)
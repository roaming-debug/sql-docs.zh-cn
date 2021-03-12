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
ms.openlocfilehash: c7cfe84e9b301ee75d1a031231824e50b0de6b9c
ms.sourcegitcommit: 765262cdc6352a5325148afc22fa4f1499fe1aa3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514874"
---
# <a name="sql-server-big-data-clusters-configuration-properties"></a>SQL Server 大数据群集配置属性

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

大数据群集配置设置可在以下范围定义：`cluster`、`service` 和 `resource`。 这些设置的层次结构也遵循此顺序，即从最高到最低。 BDC 组件将使用在最低范围定义的设置的值。 如果未在给定范围定义设置，则它将继承其更高的父范围中的值。 下面是不同范围内每个 BDC 组件的可用设置列表。 你还可以使用 azdata 查看 BDC 的可配置设置。

## <a name="bdc-cluster-scope-settings"></a>BDC 群集范围设置
你可以在群集范围配置以下设置。

| 设置名称                                             | 说明                                                                                                                                                                                                                                                                                                             | 类型   | 默认值          | 仅部署时间 | 
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ---------------------------- | --------- | 
| bdc.telemetry.customerFeedback                              | 控制此群集是否参与客户体验改善计划 (CEIP) 并向 Microsoft 发送产品使用情况和诊断数据。 | boolean | 是                    |           | 

## <a name="sql-service-scope-settings"></a>SQL 服务范围设置
你可以在 SQL 服务范围配置以下设置。

| 设置名称                                             | 说明                                                                                                                                                                                                                                                                                                             | 类型   | 默认值          | 仅部署时间 | 
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ---------------------------- | --------- | 
| mssql.language.lcid                              | 将 SQL Server 区域设置更改为任何支持的语言标识符 (LCID)。                                                                                                                                                                                                                                              | int    | 2052                         |           | 

## <a name="spark-service-scope-settings"></a>Spark 服务范围设置
若要查看所有支持和不支持的设置，请访问 [Apache Spark 和 Apache Hadoop 配置项目](reference-config-spark-hadoop.md)。

## <a name="hdfs-service-scope-settings"></a>HDFS 服务范围设置
若要查看所有支持和不支持的设置，请访问 [Apache Spark 和 Apache Hadoop 配置项目](reference-config-spark-hadoop.md)。

## <a name="gateway-service-scope-settings"></a>网关服务范围设置
没有可配置的网关服务范围设置。 配置网关资源范围内的设置。

## <a name="app-service-scope-settings"></a>应用服务范围设置
无可用项

## <a name="master-pool-resource-scope-settings"></a>主池资源范围设置
| 设置名称                                             | 说明                                                                                                                                                                                                                                                                                                             | 类型   | 默认值          | 仅部署时间 | 
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ---------------------------- | --------- | 
| mssql.licensing.pid                              | SQL Server 版本。                                                                                                                                                                                                                                                                                                     | 字符串 | 开发人员                    |           | 
| mssql.sqlagent.enabled                           | 启用 SQL Server 代理。                                                                                                                                                                                                                                                                                               | bool   | false                        |           | 
| mssql.collation                                  | 将 SQL Server 排序规则更改为任何支持的排序规则。                                                                                                                                                                                                                                                    | 字符串 | SQL_Latin1_General_CP1_CI_AS | 是      | 
| hadr.enabled                                     | 用于为 SQL Server 主池启用可用性组的布尔值。                                                                                                                                                                                                                                                    | bool   | false                        | true      | 
| hadr.leaseDurationInSeconds                      | HA 代理的租用过期超时时间。                                                                                                                                                                                                                                                                                  | int    | 30                           |           | 
| hadr.externalLeasePollingEnabled                 | 用于启用外部租用轮询 API 的布尔值。                                                                                                                                                                                                                                                                        | bool   | true                         | true      | 
| mssql.telemetry.userRequestedLocalAuditDirectory | 启用 SQL Server 本地审核，并允许用户设置在其中创建“本地审核”日志的目录。 此目录必须位于“/var/opt/mssql/audit”下。                                                                                                                                                            | 字符串 |                              |           | 

## <a name="storage-pool-resource-scope-settings"></a>存储池资源范围设置
存储池由 SQL、Spark 和 HDFS 组件构成。

### <a name="available-sql-configurations"></a>可用的 SQL 配置
| 设置名称                                             | 说明                                                                                                                                                                                                                                                                                                             | 类型   | 默认值          | 仅部署时间 | 
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ---------------------------- | --------- | 
| mssql.degreeOfParallelism                        | 每个 SQL 实例在每次并行计划执行中用于运行单个语句时要使用的处理器数。                                                                                                                                                                                                        | int    | 0                            |           | 
| mssql.maxServerMemory                            | SQL Server 实例使用的 SQL Server 进程的最大内存量（以 MB 为单位）。                                                                                                                                                                                                                 | int    | 2147483647                   |           | 
| mssql.minServerMemory                            | SQL Server 实例使用的 SQL Server 进程的最小内存量（以 MB 为单位）。                                                                                                                                                                                                                 | int    | 0                            |           | 
| mssql.numberOfCpus                    | 将 SQL Server 工作线程分发到指定范围内的各 CPU。 超出了指定范围的 CPU 将不会分配线程。 对于“自动”，请指定 0。 | 字符串 | AUTO                         |           | 
| mssql.storagePoolCacheSize                       | 存储池中每个 SQL 实例的缓存大小（以 MB 为单位）。                                                                                                                                                                                                                                             | int    | 8                            |           | 
| mssql.storagePoolMaxCacheSize                    | 存储池中每个 SQL 实例的缓存的最大大小（以 MB 为单位）。                                                                                                                                                                                                                                     | int    | 16384                        |           | 
| mssql.storagePoolCacheAutogrowth                 | 存储池缓存的自动增长系数（以 MB 为单位）。                                                                                                                                                                                                                                                                  | int    | 256                          |           | 
| mssql.tempdb.autogrowthPerDataFile               | 每个 TempDB 数据文件的自动增长量（以 MB 为单位）。                                                                                                                                                                                                                                                                          | int    | 64                           |           | 
| mssql.tempdb.autogrowthPerLogFile                | 每个 TempDB 日志文件的自动增长量（以 MB 为单位）。                                                                                                                                                                                                                                                                           | int    | 64                           |           | 
| mssql.tempdb.dataFileSize                        | 每个 TempDB 数据文件的文件大小（以 MB 为单位）。                                                                                                                                                                                                                                                                           | int    | 8                            |           | 
| mssql.tempdb.dataFileMaxSize                     | 每个 TempDB 数据文件的文件大小上限（以 MB 为单位）。                                                                                                                                                                                                                                                                   | int    | 16777215                     |           | 
| mssql.tempdb.logFileSize                         | 每个 TempDB 日志文件的文件大小（以 MB 为单位）。                                                                                                                                                                                                                                                                            | int    | 8                            |           | 
| mssql.tempdb.logFileMaxSize                      | 每个 TempDB 日志文件的文件大小上限（以 MB 为单位）。                                                                                                                                                                                                                                                                    | int    | 2097151                      |           | 
| mssql.tempdb.numberOfDataFiles                   | TempDB 的数据文件数。                                                                                                                                                                                                                                                                                        | int    | 8                            |           | 
| mssql.traceflags                                 | 启用或禁用 SQL Server 服务启动的跟踪标志。 提供要应用的跟踪标志的列表（以空格分隔）。                                                                                                                                                                                        | 字符串 | 3614                         |           | 


### <a name="available-apache-spark-and-hadoop-configurations"></a>可用的 Apache Spark 和 Hadoop 配置
若要查看所有支持和不支持的设置，请访问 [Apache Spark 和 Apache Hadoop 配置项目](reference-config-spark-hadoop.md)。

## <a name="data-pool-resource-scope-settings"></a>数据池资源范围设置
| 设置名称                                             | 说明                                                                                                                                                                                                                                                                                                             | 类型   | 默认值          | 仅部署时间 | 
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ---------------------------- | --------- | 
| mssql.degreeOfParallelism                        | 每个 SQL 实例在每次并行计划执行中用于运行单个语句时要使用的处理器数。                                                                                                                                                                                                        | int    | 0                            |           | 
| mssql.maxServerMemory                            | SQL Server 实例使用的 SQL Server 进程的最大内存量（以 MB 为单位）。                                                                                                                                                                                                                 | int    | 2147483647                   |           | 
| mssql.minServerMemory                            | SQL Server 实例使用的 SQL Server 进程的最小内存量（以 MB 为单位）。                                                                                                                                                                                                                 | int    | 0                            |           | 
| mssql.numberOfCpus                    | 将 SQL Server 工作线程分发到指定范围内的各 CPU。 超出了指定范围的 CPU 将不会分配线程。 对于“自动”，请指定 0。 | 字符串 | AUTO                         |           | 
| mssql.tempdb.autogrowthPerDataFile               | 每个 TempDB 数据文件的自动增长量（以 MB 为单位）。                                                                                                                                                                                                                                                                          | int    | 64                           |           | 
| mssql.tempdb.autogrowthPerLogFile                | 每个 TempDB 日志文件的自动增长量（以 MB 为单位）。                                                                                                                                                                                                                                                                           | int    | 64                           |           | 
| mssql.tempdb.dataFileSize                        | 每个 TempDB 数据文件的文件大小（以 MB 为单位）。                                                                                                                                                                                                                                                                           | int    | 8                            |           | 
| mssql.tempdb.dataFileMaxSize                     | 每个 TempDB 数据文件的文件大小上限（以 MB 为单位）。                                                                                                                                                                                                                                                                   | int    | 16777215                     |           | 
| mssql.tempdb.logFileSize                         | 每个 TempDB 日志文件的文件大小（以 MB 为单位）。                                                                                                                                                                                                                                                                            | int    | 8                            |           | 
| mssql.tempdb.logFileMaxSize                      | 每个 TempDB 日志文件的文件大小上限（以 MB 为单位）。                                                                                                                                                                                                                                                                    | int    | 2097151                      |           | 
| mssql.tempdb.numberOfDataFiles                   | TempDB 的数据文件数。                                                                                                                                                                                                                                                                                        | int    | 8                            |           | 
| mssql.traceflags                                 | 启用或禁用 SQL Server 服务启动的跟踪标志。 提供要应用的跟踪标志的列表（以空格分隔）。                                                                                                                                                                                        | 字符串 | 3614                         |           | 

## <a name="compute-pool-resource-scope-settings"></a>计算池资源范围设置
| 设置名称                                             | 说明                                                                                                                                                                                                                                                                                                             | 类型   | 默认值          | 仅部署时间 | 
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ---------------------------- | --------- | 
| mssql.degreeOfParallelism                        | 每个 SQL 实例在每次并行计划执行中用于运行单个语句时要使用的处理器数。                                                                                                                                                                                                        | int    | 0                            |           | 
| mssql.maxServerMemory                            | SQL Server 实例使用的 SQL Server 进程的最大内存量（以 MB 为单位）。                                                                                                                                                                                                                 | int    | 2147483647                   |           | 
| mssql.minServerMemory                            | SQL Server 实例使用的 SQL Server 进程的最小内存量（以 MB 为单位）。                                                                                                                                                                                                                 | int    | 0                            |           | 
| mssql.numberOfCpus                    | 将 SQL Server 工作线程分发到指定范围内的各 CPU。 超出了指定范围的 CPU 将不会分配线程。 对于“自动”，请指定 0。 | 字符串 | AUTO                         |           | 
| mssql.tempdb.autogrowthPerDataFile               | 每个 TempDB 数据文件的自动增长量（以 MB 为单位）。                                                                                                                                                                                                                                                                          | int    | 64                           |           | 
| mssql.tempdb.autogrowthPerLogFile                | 每个 TempDB 日志文件的自动增长量（以 MB 为单位）。                                                                                                                                                                                                                                                                           | int    | 64                           |           | 
| mssql.tempdb.dataFileSize                        | 每个 TempDB 数据文件的文件大小（以 MB 为单位）。                                                                                                                                                                                                                                                                           | int    | 8                            |           | 
| mssql.tempdb.dataFileMaxSize                     | 每个 TempDB 数据文件的文件大小上限（以 MB 为单位）。                                                                                                                                                                                                                                                                   | int    | 16777215                     |           | 
| mssql.tempdb.logFileSize                         | 每个 TempDB 日志文件的文件大小（以 MB 为单位）。                                                                                                                                                                                                                                                                            | int    | 8                            |           | 
| mssql.tempdb.logFileMaxSize                      | 每个 TempDB 日志文件的文件大小上限（以 MB 为单位）。                                                                                                                                                                                                                                                                    | int    | 2097151                      |           | 
| mssql.tempdb.numberOfDataFiles                   | TempDB 的数据文件数。                                                                                                                                                                                                                                                                                        | int    | 8                            |           | 
| mssql.traceflags                                 | 启用或禁用 SQL Server 服务启动的跟踪标志。 提供要应用的跟踪标志的列表（以空格分隔）。                                                                                                                                                                                        | 字符串 | 3614                         |           | 

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
---
title: 使用 Jupyter 笔记本和 Azure Data Studio 收集和分析日志
titleSuffix: SQL Server Big Data Clusters
description: 在 SQL Server 2019 大数据群集上，使用 Jupyter 笔记本和 Azure Data Studio 记录群集。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a8609fbe12e6218d328528dd5577bf1a40167162
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047777"
---
# <a name="gathering-and-analyzing-logs-in-the-cluster-with-notebooks"></a>使用笔记本收集和分析群集中的日志

此页是适用于 SQL Server 大数据群集的笔记本的索引。 这些可执行的笔记本 (.ipynb) 旨在用于 SQL Server 2019，以帮助记录大数据群集。

每个笔记本均可检查其自身的依赖项。 “运行所有单元格”要么成功完成，要么抛出异常，并显示指向另一个笔记本的超链接提示，以解决缺少依赖项的问题。 单击提示超链接转到后续的笔记本，按“运行所有单元格”，成功后返回到原始笔记本，再按“运行所有单元格”。

一旦安装了所有依赖项，但“运行所有单元格”失败，每个笔记本就会分析结果，并尽可能生成指向另一个笔记本的超链接提示，以进一步帮助解决问题。

## <a name="gathering-logs-from-big-data-cluster-bdc"></a>从大数据群集 (BDC) 收集日志

此部分包含一组笔记本，用于从 SQL Server 大数据群集 (BDC) 获取日志。

| 名称 | 说明 |
|--|--|
| TSG001 - 运行 azdata copy-logs | 使用 azdata 命令行接口复制 BDC 群集中的数据。 |
| TSG061 - 获取 BDC 命名空间中 Pod 所有容器日志的尾部 | 从命名空间中的 BDC 群集获取 Pod 的所有容器日志。 |
| TSG062 - 获取 BDC 命名空间中 Pod 所有以前的容器日志的尾部 | 从命名空间中的 BDC 群集获取 Pod 的所有以前的容器日志。 |
| TSG083 - 运行 kubectl 群集信息转储 | 使用 kubetl 命令行接口转储与 BDC 群集相关的信息。 |
| TSG084 - 内部查询处理器错误 | 使用 DMV 查询来获取内部查询处理器错误的详细信息。 |
| TSG091 - 获取 azdata CLI 日志 | 从本地计算机中获取 azdata 日志。 |



## <a name="analyse-logs-from-big-data-clusters-bdc"></a>从大数据群集 (BDC) 分析日志

一组用于从 SQL Server 大数据群集收集和分析日志的笔记本。  分析进程会建议后续笔记本运行，以解决日志中发现的已知问题。

|名称|说明 |
|---|---|
|TSG030 - SQL Server 错误日志文件|获取 SQL Server 错误日志文件，分析日志项目，并进一步建议相关故障排除指南。 |
|TSG031 - SQL Server PolyBase 日志|获取 SQL Server PolyBase 日志，分析日志项目，并进一步建议相关故障排除指南。|
|TSG034 - Livy 日志|获取 Livy 日志，分析日志项目，并进一步建议相关故障排除指南。|
|TSG035 - Spark 历史记录日志|获取 Spark History 日志，分析日志项目，并进一步建议相关故障排除指南。|
|TSG036 - 控制器日志|获取过去“n”小时的控制器日志，分析日志项目，并进一步建议相关故障排除指南。|
|TSG046 - Knox 网关日志|Knox 向客户端发出 500 错误，并删除指出潜在问题原因的详细信息（堆栈）。 因此，使用此笔记本从群集获取 Knox 日志。 获取 Knox Gateway 日志，分析日志项目，并进一步建议相关故障排除指南。|
|TSG073 - InfluxDB 日志|获取 InfluxDB 日志，分析日志项目，并进一步建议相关故障排除指南。|
|TSG076 - Elastic Search 日志|获取 Elastic Search 日志，分析日志项目，并进一步建议相关故障排除指南。|
|TSG077 - Kibana 日志|获取 Kibana 日志，分析日志项目，并进一步建议相关故障排除指南。|
|TSG088 - Hadoop datanode 日志|获取 Hadoop datanode 日志，分析日志项目，并进一步建议相关故障排除指南。|
|TSG090 - Yarn nodemanager 日志|获取 Yarn nodemanager 日志，分析日志项目，并进一步建议相关故障排除指南。|
|TSG092 - BDC 中所有容器的 Supervisord 日志结尾|获取 BDC 中所有容器的 Supervisord 日志结尾，分析日志项目，并进一步建议相关故障排除指南。|
|TSG093 - BDC 中所有容器的代理日志结尾|获取 BDC 中所有容器的 Agent 日志结尾，分析日志项目，并进一步建议相关故障排除指南。|
|TSG094 - Grafana 日志|获取 Grafana 日志，分析日志项目，并进一步建议相关故障排除指南。|
|TSG095 - Hadoop namenode 日志|获取 Hadoop namenode 日志，分析日志项目，并进一步建议相关故障排除指南。|
|TSG096 - Zookeeper 日志|获取 Zookeeper 日志，分析日志项目，并进一步建议相关故障排除指南。|

## <a name="next-steps"></a>后续步骤

有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)。

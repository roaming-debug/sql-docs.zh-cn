---
title: 使用 Jupyter 笔记本和 Azure Data Studio 处理大数据群集 (BDC) 的常见方案
titleSuffix: SQL Server Big Data Clusters
description: 在 SQL Server 2019 大数据群集上，使用 Jupyter 笔记本和 Azure Data Studio 处理 BDC 的常见方案。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8cbf034873b690262ff6acb395ed8eca5d09ae79
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039507"
---
# <a name="common-notebooks-for-sql-server-big-data-clusters"></a>适用于 SQL Server 大数据群集的常见笔记本

本文列出了适用于 SQL Server 大数据群集的笔记本。 这些可执行的笔记本 (.ipynb) 旨在用于 SQL Server 2019，以帮助显示有关大数据群集的常见方案。

每个笔记本均可检查其自身的依赖项。 “运行所有单元格”选项要么成功完成，要么抛出异常，并显示指向另一个笔记本的超链接提示，以解决缺少依赖项的问题。 单击提示超链接转到后续的笔记本，按“运行所有单元格”，成功后返回到原始笔记本，再按“运行所有单元格”。

一旦安装了所有依赖项，但“运行所有单元格”失败，每个笔记本就会分析结果，并尽可能生成指向另一个笔记本的超链接提示，以进一步帮助解决问题。

## <a name="gathering-logs-from-big-data-cluster-bdc"></a>从大数据群集 (BDC) 收集日志

此部分中的笔记本用作其他笔记本的先决条件，如登录和注销群集。

|名称 |说明 |
|---|---|
|SOP005 - az 登录|使用 az 命令行界面登录 Azure。 |
|SOP006 - az 注销|使用 az 命令行界面注销 Azure。|
|SOP007 - 版本信息（azdata、bdc、kubernetes）|定义在关于版本控制信息的笔记本中使用的帮助程序函数。|
|SOP011 - 设置 kubernetes 配置上下文|设置要使用的 kubernetes 配置。 |
|SOP028 - azdata 登录|使用 azdata 命令行接口登录到大数据群集。 |
|SOP033 - azdata 注销|使用 azdata 命令行接口注销大数据群集。 |
|SOP034 - 等待 BDC 恢复正常状态|一直阻止到大数据群集恢复正常状态或指定的超时到期。min_pod_count 参数指明除非群集中至少有此数量的 Pod，否则运行状况检查将不会通过。 如果任何超过此限制的现有 Pod 运行不正常，则群集运行不正常。|
|OPR001 - 创建 app-deploy|使用此笔记本在大数据群集中创建应用。 |
|OPR002 - 运行 app-deploy|使用此笔记本在大数据群集中运行应用。 |
|OPR003 - 创建 cronjob|使用此笔记本在大数据群集中创建 cronjob。 |
|OPR004 - 暂停 cronjob|使用此笔记本在大数据群集中暂停 cronjob。 |
|OPR005 - 恢复 cronjob|使用此笔记本在大数据群集中恢复 cronjob。 |
|OPR006 - 删除 cronjob|使用此笔记本在大数据群集中删除 cronjob。 |
|OPR007 - 删除 app-deploy|使用此笔记本在大数据群集中删除应用。 |
|OPR100 - 部署和计划笔记本|使用此笔记本将笔记本列表部署到 App-Deploy Pod，使用 Kubernetes CronJob 按计划运行 App-Deploy，然后安装 Grafana Dashboard 来查看结果。|
|OPR600 - 监视基础结构 (Kubernetes)|使用此笔记本来监视基础结构。|
|OPR700 - 为 App-Deploy 应用程序创建 Grafana Dashboard|使用此笔记本在 Grafana 中创建仪表板来监视 App-Deploy 结果，并在 Canary 或应用程序启动失败时生成警报。|
|OPR900 - 对运行 app-deploy 进行故障排除|使用此笔记本直接在容器中运行 app-deploy 脚本（使用 kubectl exec）。 这可以为故障排除提供更多调试信息，特别是与脚本启动阶段的问题相关的信息。|
|OPR901 - 对 cronjob 进行故障排除|使用此笔记本直接在容器中运行 cronjob 脚本（使用 kubectl exec）。 这可以为问题排查提供更多调试信息。|


## <a name="analyze-logs-from-big-data-clusters-bdc"></a>从大数据群集 (BDC) 分析日志

一组用于演示 SQL Server 大数据群集方案的示例笔记本。

|名称 |说明 |
|---|---|
|SAM001a - 从 SQL Server 主池中查询存储池（第 1 部分，共 3 部分）- 加载示例数据|在由 3 部分组成的本教程中，使用 azdata 将数据加载到存储池 (HDFS)，将数据转换为 Parquet（使用 Spark），在第 3 部分中使用主池 (SQL Server) 查询数据。 |
|SAM001b - 从 SQL Server 主池中查询存储池（第 2 部分，共 3 部分）- 将数据转换为 parquet|在由 3 部分组成的本教程的第 2 部分中，使用 Spark 将 .csv 文件转换为 parquet 文件。|
|SAM001c - 从 SQL Server 主池中查询存储池（第 3 部分，共 3 部分）- 从 SQL Server 查询 HDFS|在这个存储池教程的第 3 部分中，你将了解如何创建指向大数据群集中 HDFS 数据的外部表，以及如何将此数据与主实例中的高值数据联接起来。|
|SAM002 - 存储池（第 2 部分，共 2 部分）- 查询 HDFS|在这个存储池教程的第 2 部分中，你将了解如何创建指向大数据群集中 HDFS 数据的外部表，以及如何将此数据与主实例中的高值数据联接起来|
|SAM003 - 数据池示例|在本教程中，你将了解如何在数据池中创建数据池源和外部表，然后在数据池表中插入数据，并将数据从一个数据池表加载到另一个数据池表。 将数据池表中的数据与其他数据池表联接起来，这也会截断表并进行清理。 |
|SAM004 - 从 MongoDB 虚拟化数据|若要查询 MongoDB 外部数据源中的数据，必须创建外部表来引用该外部数据。 本节提供用于创建这些外部表的示例代码。|
|SAM005 - 来自 Oracle 的虚拟化数据|若要查询 Oracle 外部数据源中的数据，必须创建外部表以引用外部数据。 本节提供用于创建这些外部表的示例代码。|
|SAM006 - 来自 SQL Server 的虚拟化数据|若要从另一 SQL Server 数据源虚拟查询数据，必须创建外部表以引用外部数据。 本节提供用于创建这些外部表的示例代码。|
|SAM007 - 虚拟化来自 Teradata 的数据|若要查询来自 Teradata 外部数据源的数据，必须创建外部表以引用外部数据。 本节提供用于创建这些外部表的示例代码。|
|SAM008 - 使用 azdata 的 Spark|用于 Spark 会话的 azdata 和 kubectl 命令。|
|SAM009 - 使用 azdata 的 HDFS|用于 HDFS 的 azdata 和 kubectl 命令。|
|SAM010 - 使用 azdata 的应用|用于 App Deploy 的 azdata 和 kubectl 命令。 |

## <a name="next-steps"></a>后续步骤

有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)。

---
title: 适用于大数据群集 (BDC) 的管理资源
titleSuffix: SQL Server
description: 本文介绍了如何使用 Azure Data Studio、笔记本和 Azure Data CLI (azdata) 命令来查看大数据群集的状态。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 10/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 194f74fa175ad3f1c08ceab542b6db7812987157
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045630"
---
# <a name="administration-resources-for-big-data-clusters-bdc"></a>适用于大数据群集 (BDC) 的管理资源 

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文介绍了如何从外部管理 SQL Server 大数据群集。

## <a name="know-your-architecture"></a>了解体系结构

自 SQL Server 2019 (15.x) 起，使用 SQL Server 大数据群集，可以部署运行在 Kubernetes 上的 SQL Server、Spark 和 HDFS 容器的可缩放群集。 下面的文章对大数据群集进行了概述：
- [什么是 SQL Server 大数据群集](big-data-cluster-overview.md)

SQL Server 大数据群集提供连贯且一致的授权和身份验证。 下面的文章对大数据群集安全性进行了概述：
- [SQL Server 大数据群集的安全性概念](concept-security.md)

## <a name="manage-and-operate-with-tools"></a>使用工具进行管理和操作

下面的文章介绍了如何通过以下方式管理和操作大数据群集： 

- [使用 Azure Data Studio 连接到 SQL Server 大数据群集](connect-to-big-data-cluster.md)
- [管理 SQL Server 控制器仪表板的大数据群集](manage-with-controller-dashboard.md)
- [使用 Azure Data Studio 笔记本管理 SQL Server 大数据群集](notebooks-manage-bdc.md)
- [使用笔记本管理大数据群集 (BDC)](cluster-manage-notebooks.md)
- [使用 Spark 运行示例笔记本](notebooks-tutorial-spark.md)

## <a name="monitor-with-tools"></a>使用工具进行监视

下面的文章介绍了如何通过以下方式监视大数据群集： 

- [使用 Azure Data Studio 监视 BDC 群集](cluster-monitor-ads.md)
- [使用 Azdata 实用工具监视 BDC 群集](cluster-monitor-cmdlet.md)
- [使用 Grafana Dashboard 监视 BDC 群集](cluster-monitor-grafana.md)
- [使用 Jupyter 笔记本和 Azure Data Studio 监视 BDC 群集](cluster-monitor-notebooks.md)

## <a name="monitor-and-inspect-logs-with-notebooks"></a>使用笔记本监视和检查日志

下面的文章列出了 Azure Data Studio 中提供的许多 Jupyter 笔记本。

- [使用笔记本监视群集](cluster-monitor-notebooks.md)
- [使用笔记本收集和分析群集中的日志](cluster-logging-notebooks.md)

## <a name="big-data-clusters-troubleshooting-resources"></a>大数据群集故障排除资源

下面的文章介绍了如何对大数据群集进行故障排除：

- [使用 kubectl 实用工具对 BDC 群集进行故障排除](cluster-troubleshooting-commands.md) 
- [排查 pyspark 笔记本故障](troubleshoot-pyspark-notebook.md)
- [使用 Jupyter 笔记本和 Azure Data Studio (ADS) 对 BDC 群集进行故障排除](cluster-troubleshooter-notebooks.md)
- [还原 HDFS 权限](troubleshoot-hdfs-restore-admin.md)

下面的文章介绍了如何对在 Active Directory 模式下部署的大数据群集进行故障排除：
- [对在 Active Directory 模式下部署的 BDC 群集进行故障排除](troubleshoot-active-directory.md) 
- [对 AD 模式登录失败进行故障排除](troubleshoot-ad-login-failed-untrusted-domain.md)
- [对 BDC AD 模式部署停止进行故障排除](troubleshoot-ad-reverse-lookup-zone.md)

## <a name="next-steps"></a>后续步骤

有关大数据群集的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md)。
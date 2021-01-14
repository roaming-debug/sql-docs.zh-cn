---
title: 使用 Grafana Dashboard 监视群集
titleSuffix: SQL Server Big Data Clusters
description: 在 SQL Server 2019 大数据群集上，使用 Grafana Dashboard 监视群集。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ee0ceb3a9f86fc2a7fabe44e9279b7e63dabfb8b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091682"
---
# <a name="monitor-cluster-with-azdata-and-grafana-dashboard"></a>使用 azdata 和 Grafana Dashboard 监视群集

本文介绍如何监视 SQL Server 大数据群集内的应用程序。

## <a name="prerequisites"></a>先决条件

- [SQL Server 2019 大数据群集](deployment-guidance.md)
- [azdata 命令行工具](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>功能

在 SQL Server 2019 中，可以创建、删除、描述、初始化、列出运行和更新应用程序。 下表介绍了可以与 azdata 一起使用的应用程序部署命令。

|Command |说明 |
|:---|:---|
|`azdata bdc endpoint list` | 列出大数据群集的终结点。 |


可以使用以下示例列出 Grafana 仪表板的终结点：

```bash
azdata bdc endpoint list --endpoint-name metricsui 
```

输出会为你显示终结点，你可以使用群集用户名和密码进行登录。 

![Grafana 仪表板](media/big-data-cluster-monitor-apps/grafana-dashboard-endpoint.png)

`nodeMetricsUrl` 和 `sqlMetricsUrl` 值链接到用于监视 Kubernetes 节点指标和大数据群集服务指标的 Grafana 仪表板：

![Grafana 仪表板](./media/view-cluster-status/grafana-dashboard.png)

![SQL](./media/view-cluster-status/grafana-sql-status.png)



## <a name="next-steps"></a>后续步骤

有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)。
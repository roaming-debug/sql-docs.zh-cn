---
title: 使用 Kibana Dashboard 签出群集日志
titleSuffix: SQL Server Big Data Clusters
description: 在 SQL Server 2019 大数据群集上，使用 Kibana Dashboard 监视群集。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 10/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 83e9d79f0316646d6ee25233e604e6751167b5f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039517"
---
# <a name="check-out-cluster-logs--with-kibana-dashboard"></a>使用 Kibana Dashboard 签出群集日志

本文介绍如何监视 SQL Server 大数据群集内的应用程序。

## <a name="prerequisites"></a>先决条件

- [SQL Server 2019 大数据群集](deployment-guidance.md)
- [azdata 命令行工具](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>功能

在 SQL Server 2019 中，可以创建、删除、描述、初始化、列出运行和更新应用程序。 下表介绍了可以与 azdata 一起使用的应用程序部署命令。

|Command |说明 |
|:---|:---|
|`azdata bdc endpoint list` | 列出大数据群集的终结点。 |


可以使用下面的示例来列出 Kibana Dashboard 的终结点：

```bash
azdata bdc endpoint list --endpoint-name logsui 
```

输出会为你显示终结点，你可以使用群集用户名和密码进行登录。 

![Kibana Dashboard](media/big-data-cluster-monitor-cluster/kibana-dashboard-endpoint.png)


指向 Kibana Dashboard 的链接：

![Kibana 仪表板](./media/view-cluster-status/kibana-dashboard.png)

> [!NOTE]
> （旧）Microsoft Edge 浏览器与 Kibana 不兼容，因此必须使用基于 chromium 的浏览器，才能正确显示仪表板。 使用不受支持的浏览器加载仪表板时，会看到一个空白页。 请参阅此处，了解 Kibana 支持的浏览器。

## <a name="next-steps"></a>后续步骤

有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)。
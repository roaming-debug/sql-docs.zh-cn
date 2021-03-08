---
title: 使用 Kibana Dashboard 签出群集日志
titleSuffix: SQL Server Big Data Clusters
description: 在 SQL Server 2019 大数据群集上，使用 Kibana Dashboard 监视群集。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 02/25/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7c26dc7e193d3dd5ad688af4d4dc79e2dd3eeea7
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835964"
---
# <a name="check-out-cluster-logs--with-kibana-dashboard"></a>使用 Kibana Dashboard 签出群集日志

本文介绍如何监视 [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] 内的应用程序。

## <a name="prerequisites"></a>先决条件

- [[!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md)
- [azdata 命令行工具](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>功能

在 [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] 中，可以创建、删除、描述、初始化、列出运行和更新应用程序。 下表介绍了可以与 azdata 一起使用的应用程序部署命令。

|Command |说明 |
|:---|:---|
|`azdata bdc endpoint list` | 列出 [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] 的终结点。 |


可以使用下面的示例来列出 Kibana Dashboard 的终结点：

```bash
azdata bdc endpoint list --endpoint-name logsui 
```

输出会为你显示终结点，你可以使用群集用户名和密码进行登录。 

![Kibana Dashboard](media/big-data-cluster-monitor-cluster/kibana-dashboard-endpoint.png)


指向 Kibana Dashboard 的链接：

![Kibana 仪表板](./media/view-cluster-status/kibana-dashboard.png)

> [!NOTE]
> 旧版 Microsoft Edge 浏览器与 Kibana 不兼容，因此必须使用基于 chromium 的 Edge 浏览器，才能正确显示仪表板。 使用不受支持的浏览器加载仪表板时，会看到一个空白页，请参阅 [Kibana 支持的浏览器](https://www.elastic.co/support/matrix#matrix_browsers)。

## <a name="next-steps"></a>后续步骤

有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)。



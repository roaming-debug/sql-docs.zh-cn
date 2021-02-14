---
title: 使用 azdata 和 Grafana 仪表板监视应用程序
titleSuffix: SQL Server Big Data Clusters
description: 在 SQL Server 2019 大数据群集上使用 azdata 和 Grafana 监视应用程序。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 08/16/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 380a6c8ab8812ee6a8aae132cbc12125777e2960
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047787"
---
# <a name="monitor-applications-with-azdata-and-grafana-dashboard"></a>使用 azdata 和 Grafana 仪表板监视应用程序

Grafana 是最佳云本机虚拟化工具之一，可用于提供在 Kubernetes 中运行的应用程序的各种监视指标。  

本文介绍如何监视 SQL Server 大数据群集内的应用程序。

## <a name="prerequisites"></a>先决条件

- [SQL Server 2019 大数据群集](deployment-guidance.md)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)

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


打开仪表板时，转到“主机应用指标” **** ，你可以在其中获取有关应用程序的更多见解并进行跟踪。  

![主机应用指标](media/big-data-cluster-monitor-apps/host-apps-metrics.png)


若要获取有关应用程序的单个 pod（在某些情况下，你有应用程序的多个副本）的更多见解，请转到“主机 Pod 标准” ****  并选择相应的 pod，你将获得指标视图，如下所示：  

![主机 pod 标准](media/big-data-cluster-monitor-apps/host-pods-metrics.png) 


## <a name="next-steps"></a>后续步骤

有关其他示例，请参阅[应用部署示例](https://aka.ms/sql-app-deploy)。

有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)。
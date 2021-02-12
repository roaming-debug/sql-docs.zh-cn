---
title: 通过控制器仪表板管理 SQL Server 大数据群集
titleSuffix: Manage SQL Server big data cluster with controller dashboard
description: 使用 Azure Data Studio 中的笔记本管理大数据群集和排除其故障。
author: yualan
ms.author: alanyu
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: e7b71318f55b15df6a758237c26eedb9be5646c4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046254"
---
# <a name="manage-big-data-clusters-for-sql-server-controller-dashboard"></a>管理 SQL Server 控制器仪表板的大数据群集

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

除了 azdata 和群集状态笔记本以外，还可以通过另一种方法查看 SQL Server 大数据群集的状态  。 现在可以通过“连接”viewlet 添加 SQL Server 大数据群集控制器  。 这样，就可以使用仪表板查看群集运行状况。

![仪表板](media/manage-with-controller-dashboard/controller-dashboard.png)
## <a name="prerequisites"></a>必备条件

若要启动笔记本，需要满足以下必备条件：

* [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) 的最新版本
* [已在 Azure Data Studio 中安装 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 扩展](../azure-data-studio/data-virtualization-extension.md)

除上述内容外，SQL Server 2019 大数据群集还需要：

* **azdata**
    - [Windows Installer](../azdata/install/deploy-install-azdata-installer.md)
    - [Linux 包管理器](../azdata/install/deploy-install-azdata-linux-package.md)
* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-native-package-management)
* [Azure CLI](/cli/azure/install-azure-cli)

## <a name="add-sql-server-big-data-cluster-controller"></a>添加 SQL Server 大数据群集控制器

1. 单击左窗格上的“连接”视图。
2. 在“连接”视图的底部，单击“SQL Server 大数据群集”以将其展开  。
3. 单击“添加 SQL Server 大数据群集控制器”，将启动一个新的对话框  。

## <a name="add-new-controller"></a>添加新控制器

1. 添加群集名称。
2. 添加群集管理服务 URL。 这可以在 BDC 仪表板的“服务终结点”表中找到，也可以询问群集管理员。
3. 添加用户名和密码。

## <a name="launch-controller-dashboard"></a>启动控制器仪表板

1. 成功添加控制器后，可以在 SQL Server 大数据群集下查看它。
2. 若要启动该仪表板，请右键单击该控制器，然后单击“管理”  。

## <a name="controller-dashboard"></a>控制器仪表板

1. 控制器仪表板上有 3 个主要组件：

    - **工具栏**，位于顶部，其中包含仪表板的操作。
    - **导航窗格**，位于左侧，可在仪表板上更改为不同视图。
    - **视图** 覆盖大部分页面。

2. 在“大数据群集概述”页上，可以看到  ：

    - **群集属性**，可查看有关群集的信息。
    - **群集概述**，可查看所有群集组件的高级概述，以及哪些组件运行不正常
    - **服务终结点**，可以复制或访问 SQL Server 大数据群集内的不同服务。

3. 在“导航窗格”中，可查看服务列表，并可单击一个以查看更多群集详细信息  。 单击“群集概述”中某个服务时，都是相同的视图  。

4. “群集详细信息”下的每个视图都包含相同的 UI 组件  ：

    - **运行状况状态详细信息**，共享组件的状态和运行状况状态。
    - **指标和日志**，通过 Grafana 和 Kibana 查看其他指标和日志。

1. 如果查看运行不正常的组件，请单击工具栏上的“故障排除”，启动包含笔记本的 Jupyter Book，以帮助诊断问题  。

## <a name="next-steps"></a>后续步骤

有关控制器的详细信息，请参阅[我们的控制器文档](concept-controller.md)。
---
title: 使用 Azure Data Studio 监视群集
titleSuffix: SQL Server Big Data Clusters
description: 在 SQL Server 2019 大数据群集上，使用 Azure Data Studio 监视群集。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d03dc9c26f33fd512e035ab3cc9e167e1d9ef7cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047737"
---
# <a name="monitor-cluster-status-with-azure-data-studio"></a>使用 Azure Data Studio 监视群集状态

本文介绍了如何使用 Azure Data Studio 查看大数据群集的状态。

## <a name="use-azure-data-studio"></a><a id="datastudio"></a> 使用 Azure Data Studio

下载 [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) 的最新 **预览体验内部版本** 后，可以使用 SQL Server 大数据群集仪表板查看服务终结点以及大数据群集的状态。 下面的某些功能仅在 Azure Data Studio 的预览体验内部版本中可用。

1. 首先，在 Azure Data Studio 中创建到大数据群集的连接。 有关详细信息，请参阅[使用 Azure Data Studio 连接到 SQL Server 大数据群集](connect-to-big-data-cluster.md)。

1. 右键单击大数据群集终结点，然后单击“管理”  。

   ![右键单击管理](media/view-cluster-status/right-click-manage.png)

1. 选择“SQL Server 大数据群集”选项卡访问大数据群集仪表板  。

   ![大数据群集仪表板](media/view-cluster-status/bdc-dashboard.png)

### <a name="service-endpoints"></a>服务终结点

能够轻松访问大数据群集中的各项服务很重要。 大数据群集仪表板提供了一个服务终结点表，可用于查看和复制服务终结点。

![服务终结点](media/view-cluster-status/service-endpoints.png)

当需要可用于连接这些服务的终结点时，这些服务会列出可以复制和粘贴的终结点。 例如，可以单击终结点右侧的复制图标，然后将其粘贴到请求该终结点的文本窗口中。 要运行[群集状态笔记本](#notebook)，群集管理服务终结点是必需的。

### <a name="dashboards"></a>仪表板

服务终结点表还公开多个用于监视的仪表板：

- 度量值 (Grafana)
- 日志 (Kibana)
- Spark 作业监视
- Spark 资源管理

可以直接单击这些链接。 在访问这些仪表板时，将需要进行身份验证。 对于指标和日志仪表板，请提供在部署时使用环境变量设置的控制器管理员凭据 AZDATA_USERNAME 和 AZDATA_PASSWORD   。 Spark 仪表板将使用网关 (Knox) 凭据：与 AD 集成的群集中的 AD 标识，或如果在群集中使用基本身份验证，则为 AZDATA_USERNAME 和 AZDATA_PASSWORD 。

[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

### <a name="cluster-status-notebook"></a><a id="notebook"></a> 群集状态笔记本

1. 还可以通过启动群集状态笔记本来查看大数据群集的群集状态。 若要启动笔记本，请单击“群集状态”任务  。

    ![启动](media/view-cluster-status/cluster-status-launch.png)

2. 在开始之前，需要以下内容：

    - 大数据群集名称
    - 控制器用户名
    - 控制器密码
    - 控制器终结点

    除非在部署过程中进行了自定义，否则大数据群集名称默认为 mssql-cluster  。 可以在服务终结点表的大数据群集仪表板中找到控制器终结点。 此终结点作为“群集管理服务”  列出。 如果不知道凭据，请向部署群集的管理员询问。

3. 单击顶部工具栏中的“运行单元”  。

4. 按照提示输入凭据。 分别为大数据群集名称、控制器用户名和控制器密码键入凭据后，按 ENTER。

    > [!Note]
    > 如果没有通过大数据设置的配置文件，系统会要求提供控制器终结点。 键入或粘贴它，然后按 ENTER 继续。

5. 如果成功连接，则笔记本的其余部分将显示大数据群集的每个组件的输出。 如果要重新运行某个代码单元，请将鼠标悬停在代码单元上，并单击“运行”图标  。


## <a name="next-steps"></a>后续步骤

有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)。
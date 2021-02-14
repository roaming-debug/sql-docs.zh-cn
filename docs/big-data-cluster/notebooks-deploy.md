---
title: 部署：Azure Data Studio 笔记本
titleSuffix: SQL Server Big Data Clusters
description: 了解如何使用 Azure Data Studio 笔记本中的代码和文档部署 SQL Server 大数据群集。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 027da3c742508ff962fcf10b084b638a7eeee708
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044205"
---
# <a name="deploy-sql-server-big-data-cluster-with-azure-data-studio-notebook"></a>使用 Azure Data Studio 笔记本部署 SQL Server 大数据群集

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 为包含部署笔记本的 Azure Data Studio 提供扩展。 部署笔记本包含可在 Azure Data Studio 中用于创建 SQL Server 大数据群集的文档和代码。

[笔记本](../azure-data-studio/notebooks/notebooks-guidance.md)最初是作为开放源代码项目实现的，现已在 [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) 中实现。 可以在文本单元格中使用 markdown 作为文本，并使用其中一个可用核心在代码单元格中编写代码。

可以使用笔记本为 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 部署大数据群集。

## <a name="prerequisites"></a>先决条件

若要启动笔记本，还需要满足以下必备条件：

* 已安装 [Azure Data Studio 预览体验内部版本](https://github.com/microsoft/azuredatastudio#try-out-the-latest-insiders-build-from-master)的最新版本

除上述内容外，部署 SQL Server 2019 大数据群集还需要：

* [azdata](../azdata/install/deploy-install-azdata.md)
* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-native-package-management)
* [Azure CLI（如果是部署在 Azure 中）](/cli/azure/install-azure-cli)

## <a name="launch-the-notebook"></a>启动笔记本

1. 启动 Azure Data Studio。

2. 在“连接”选项卡上，选择省略号 (...)，然后选择“部署 SQL Server...”    。

   ![部署 SQL Server](media/notebooks-deploy/deploy-notebooks.png)

3. 从部署选项中选择“SQL Server 大数据群集”  。

4. 在“选项”下的“部署目标”中，选择“新建 Azure Kubernetes 群集”或“现有 Azure Kubernetes 服务群集”     。

5. 接受隐私和许可条款

6. 此对话框还会检查主机上是否存在部署所选类型的 SQL 所需的工具。 工具检查成功后才会启用“选择”按钮  。

7. 选择“选择”按钮  。 此操作将启动部署体验。

## <a name="set-deployment-configuration-template"></a>设置部署配置模板

可以按照以下说明自定义部署配置文件的设置。

### <a name="target-configuration-template"></a>目标配置模板

从可用模板中选择目标配置模板。 可用的配置文件是根据前一对话框中选择的部署目标类型来筛选的。

   ![部署配置模板 步骤 1](media/notebooks-deploy/deployment-configuration-template.png)

### <a name="azure-settings"></a>Azure 设置

如果部署目标是新的 AKS，则需要额外的信息（例如 Azure 订阅 ID、资源组、AKS 群集名称、VM 计数、大小和其他附加信息）来创建 AKS 群集。

   ![Azure 设置](media/notebooks-deploy/azure-settings.png)

如果部署目标是现有的 Kubernetes 群集，向导则会提示你输入 kube 配置文件的路径以导入 Kubernetes 群集设置  。 请确保选择适当的群集上下文，可在其中部署 SQL Server 2019 大数据群集。

   ![目标群集上下文](media/notebooks-deploy/target-cluster-context.png)

### <a name="cluster-docker-and-ad-settings"></a>群集、docker 和 AD 设置

1. 输入 SQL Server 2019 BDC 的群集名称、管理员用户名和密码。
注意：用于控制器和 SQL Server 的是同一个帐户。

   ![群集设置](media/notebooks-deploy/cluster-settings.png)

2. 输入适当的 Docker 设置

   ![Docker 设置](media/notebooks-deploy/docker-settings.png)

3. 如果 AD 身份验证可用，请输入 AD 设置

   ![Active Directory 设置](media/notebooks-deploy/active-directory-settings.png)

### <a name="service-settings"></a>服务设置

此屏幕中包含各种设置的输入，例如“缩放”、“终结点”、“存储”和其他的“高级存储设置”     。 请输入适当的值，然后选择“下一步”  。

#### <a name="scale-settings"></a>缩放设置

输入大数据群集中每个组件的实例数量。

Spark 实例可以随 HDFS 一起提供。 它包含在存储池中，或者单独位于 Spark 池中。

   ![服务设置](media/notebooks-deploy/service-settings.png)

有关这些组件的其他信息，可以参考[主实例](concept-master-instance.md)、[数据池](concept-data-pool.md)、[存储池](concept-storage-pool.md)或[计算池](concept-compute-pool.md)。

#### <a name="endpoint-settings"></a>终结点设置

已预填充了默认的终结点。 但可以对它们进行适当的更改。

   ![终结点设置](media/notebooks-deploy/endpoint-settings.png)

#### <a name="storage-settings"></a>存储设置

存储设置包括数据和日志的存储类和声明大小。 这些设置可应用于存储、数据和 SQL Server 主池。

   ![存储设置](media/notebooks-deploy/storage-settings.png)

#### <a name="advanced-storage-settings"></a>高级存储设置

可以在“高级存储设置”下添加其他存储设置 

* 存储池 (HDFS)
* 数据池
* SQL Server Master

   ![高级存储设置](media/notebooks-deploy/advanced-storage-settings.png)

### <a name="summary"></a>总结

此屏幕中汇总了用于部署 SQL Server 2019 大数据群集所提供的所有输入。 可以通过“保存配置文件”按钮下载配置文件  。 选择“将脚本编写到笔记本”来将整个部署配置的脚本编写到笔记本  。 打开笔记本后，选择“运行单元格”开始将 SQL Server 2019 BDC 部署到所选目标  。

   ![总结](media/notebooks-deploy/deploy-sql-server-big-data-cluster-on-a-new-AKS-cluster.png)

## <a name="next-steps"></a>后续步骤

有关部署的详细信息，请参阅 [SQL Server 大数据群集部署指南](deployment-guidance.md)。
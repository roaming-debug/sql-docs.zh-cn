---
title: 使用 python 脚本进行部署
titleSuffix: SQL Server Big Data Clusters
description: 了解如何使用部署脚本在 Azure Kubernetes 服务 (AKS) 上部署 SQL Server 大数据群集。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9c0ccb64a67d547b2027423dbc3571e44b3b21ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046143"
---
# <a name="use-a-python-script-to-deploy-a-sql-server-big-data-cluster-on-azure-kubernetes-service-aks"></a>使用 python 脚本在 Azure Kubernetes 服务 (AKS) 上部署 SQL Server 大数据群集

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

在本教程中，将使用示例 python 部署脚本将 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]部署到 Azure Kubernetes Service (AKS)。

> [!TIP]
> AKS 只是为大数据群集托管 Kubernetes 的一种选择。 若要了解其他部署选项以及如何自定义部署选项，请参阅[如何在 Kubernetes 上部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md)。

此处使用的默认大数据群集部署包括一个 SQL Master 实例、一个计算池实例、两个数据池实例和两个存储池实例。 通过 AKS 默认存储类使用 Kubernetes 永久性卷来永久保存数据。 本教程中使用的默认配置适用于开发/测试环境。

## <a name="prerequisites"></a>先决条件

- Azure 订阅。
- [大数据工具](deploy-big-data-tools.md)：
   - **azdata**
   - **kubectl**
   - **Azure Data Studio**
   - **SQL Server 2019 扩展**
   - **Azure CLI**

## <a name="log-in-to-your-azure-account"></a>登录 Azure 帐户

该脚本使用 Azure CLI 自动创建 AKS 群集。 在运行脚本之前，必须至少使用 Azure CLI 登录 Azure 帐户一次。 从命令提示符运行以下命令。

```
az login
```

## <a name="download-the-deployment-script"></a>下载部署脚本

本教程使用 python 脚本“deploy-sql-big-data-aks.py”自动在 AKS 上创建大数据群集  。 如果已经为“azdata”安装了 python，则应能够在本教程中成功运行该脚本  。 

在 Windows PowerShell 或 Linux bash 提示符下，运行以下命令以从 GitHub 下载部署脚本。

```
curl -o deploy-sql-big-data-aks.py "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/aks/deploy-sql-big-data-aks.py"
```

## <a name="run-the-deployment-script"></a>运行部署脚本

在 Windows PowerShell 或 Linux bash 提示符下，使用以下步骤运行部署脚本。 此脚本将在 Azure 中创建 AKS 服务，然后会将 SQL Server 2019 大数据群集部署到 AKS。 还可以使用其他[环境变量](deployment-guidance.md#configfile)修改脚本以创建自定义部署。

1. 使用以下命令运行该脚本：

   ```
   python deploy-sql-big-data-aks.py
   ```

   > [!NOTE]
   > 如果客户端计算机和路径中都有 python3 和 python2，则必须使用 python3: `python3 deploy-sql-big-data-aks.py` 运行命令。

1. 出现提示时，输入以下信息：

   | 值 | 说明 |
   |---|---|
   | **Azure 订阅 ID** | 用于 AKS 的 Azure 订阅 ID。 可以通过从另一个命令行运行 `az account list` 来列出所有订阅及其 ID。 |
   | **Azure 资源组** | 要为 AKS 群集创建的 Azure 资源组名称。 |
   | **Azure 区域** | 新 AKS 群集的 Azure 区域（默认为“westus”）  。 |
   | **计算机大小** | 用于 AKS 群集中节点的[计算机大小](/azure/virtual-machines/windows/sizes)（默认为“Standard_D16s_v3”）。 |
   | **辅助角色节点** | AKS 群集中的工作器节点数（默认值为“1”）  。 |
   | **群集名称** | AKS 群集和大数据群集的名称。 大数据群集的名称只能为小写字母数字字符，不能有空格。 （默认为“sqlbigdata”）  。 |
   | **密码** | 控制器、HDFS/Spark 网关和主实例的密码（默认为“MySQLBigData2019”）  。 |
   | **用户名** | 控制器用户的用户名（默认为“admin”）  。 |

   > [!IMPORTANT]
   > 默认的“Standard_D16s_v3”计算机大小可能在每个 Azure 区域都不可用。 如果确实选择了不同的计算机大小，请确保可以跨群集中的节点连接的磁盘总数大于或等于 24。 群集中的每个永久性卷声明都需要附加磁盘。 目前，大数据群集需要 24 个永久性卷声明。
   >
   >运行以下命令以标识可用的 VM 类型。
   >
   >```azurecli-interactive
   >az vm list-sizes --query "sort_by(@,&name)[?contains(name,'Standard_D16s')]" -l westus2 -o table
   >```

   > [!NOTE]
   > 在大数据群集部署期间，SQL Server `sa` 帐户处于禁用状态。 SQL Server 主实例中预配了一个新的 sysadmin 登录名，其名称与为“用户名”输入指定的名称相同，密码与“密码”输入相对应 。 相同的“用户名”和“密码”值用于预配控制器管理员用户   。 在 SQL Server 2019 CU5 之前部署的群集上，网关 (Knox) 仅支持根用户，密码与上面相同。
   >[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

1. 该脚本将首先使用指定的参数创建 AKS 群集。 此步骤需要花费几分钟时间。

## <a name="monitor-the-status"></a>监视状态

该脚本创建 AKS 群集后，将继续使用之前指定的设置设置必需的环境变量。 然后，它调用“azdata”在 AKS 上部署大数据群集  。

客户端命令窗口将输出部署状态。 在部署过程中，应该看到一系列消息正在等待控制器 Pod：

```output
2018-11-15 15:42:02.0209 UTC | INFO | Waiting for controller pod to be up...
```

10 到 20 分钟后，你应该会收到控制器 Pod 正在运行的消息：

```output
2018-11-15 15:50:50.0300 UTC | INFO | Controller pod is running.
2018-11-15 15:50:50.0585 UTC | INFO | Controller Endpoint: https://111.111.111.111:30080
```

> [!IMPORTANT]
> 由于下载大数据群集组件的容器映像需要一定的时间，整个部署可能需要很长时间。 但是，这个过程应不会长达几个小时。 有关部署问题，请参阅[监视 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]并对其进行故障排除](cluster-troubleshooting-commands.md)。

## <a name="inspect-the-cluster"></a>检查群集

在部署期间的任何时候，你都可以使用“kubectl”或“azdata”来检查有关正在运行的大数据群集的状态和详细信息   。

### <a name="use-kubectl"></a>使用 kubectl

在部署过程中打开新的命令窗口以使用“kubectl”  。

1. 运行以下命令以获取整个群集的状态摘要：

   ```
   kubectl get all -n <your-big-data-cluster-name>
   ```

   > [!TIP]
   > 如果未更改大数据群集名称，则脚本默认为“sqlbigdata”  。

1. 使用以下“kubectl”命令检查 kubernetes 服务及其内部和外部终结点  ：

   ```
   kubectl get svc -n <your-big-data-cluster-name>
   ```

1. 还可以使用以下命令检查 kubernetes pod 的状态：

   ```
   kubectl get pods -n <your-big-data-cluster-name>
   ```

1. 使用以下命令查找有关特定 pod 的更多信息：

   ```
   kubectl describe pod <pod name> -n <your-big-data-cluster-name>
   ```

> [!TIP]
> 有关如何监视部署以及对其进行故障排除的更多详细信息，请参阅[监视 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]并对其进行故障排除](cluster-troubleshooting-commands.md)。

## <a name="connect-to-the-cluster"></a>连接到群集

部署脚本完成后，输出会通知你成功：

```output
2018-11-15 16:10:25.0583 UTC | INFO | Cluster state: Ready
2018-11-15 16:10:25.0583 UTC | INFO | Cluster deployed successfully.
```

SQL Server 大数据群集现在部署在 AKS 上。 你现在可以使用 Azure Data Studio 连接到该群集。 有关详细信息，请参阅[使用 Azure Data Studio 连接到 SQL Server 大数据群集](connect-to-big-data-cluster.md)。

## <a name="clean-up"></a>清除

如果在 Azure 中测试 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]，则应在完成后删除 AKS 群集以避免意外收费。 如果你打算继续使用它，请不要删除该群集。

> [!WARNING]
> 以下步骤将删除 AKS 群集，删除该群集也将删除 SQL Server 大数据群集。 如果要保留任何数据库或 HDFS 数据，请在删除群集之前对该数据进行备份。

运行以下 Azure CLI 命令以删除 Azure 中的大数据群集和 AKS 服务（将 `<resource group name>` 替换为你在部署脚本中指定的“Azure 资源组”）  ：

```azurecli
az group delete -n <resource group name>
```

## <a name="next-steps"></a>后续步骤

部署脚本配置了 Azure Kubernetes Service，并部署了 SQL Server 2019 大数据群集。 你还可以选择通过手动安装来自定义未来的部署。 若要了解有关如何部署大数据群集以及如何自定义部署的详细信息，请参阅[如何在 Kubernetes 上部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md)。

现在已部署 SQL Server 大数据群集，你可以加载示例数据并浏览教程：

> [!div class="nextstepaction"]
> [教程：将示例数据加载到 SQL Server 2019 大数据群集中](tutorial-load-sample-data.md)

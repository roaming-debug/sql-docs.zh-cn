---
title: 升级到新版本
titleSuffix: SQL Server Big Data Clusters
description: 了解如何将 SQL Server 大数据群集升级到新版本。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 09/02/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 48edd5e76a87a866a2090329f144c5a7dc0934a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046857"
---
# <a name="how-to-upgrade-big-data-clusters-2019"></a>如何升级 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

升级路径取决于 SQL Server 大数据群集 (BDC) 的当前版本。 若要从支持的版本（包括常规分发版本 (GDR)、累积更新 (CU) 或快速修补工程 (QFE) 更新）升级，可以就地升级。 不支持从 BDC 的客户技术预览版 (CTP) 或候选发布版本进行就地升级。 需要删除并重新创建群集。 以下部分介绍了每个场景的步骤：

- [从支持的版本升级](#upgrade-from-supported-release)
- [从 CTP 或候选发布更新 BDC 部署](#update-a-bdc-deployment-from-ctp-or-release-candidate)

>[!NOTE]
>大数据群集的第一个受支持版本为 SQL Server 2019 GDR1。

## <a name="upgrade-release-notes"></a>升级发行说明

在继续操作之前，请查看[升级发行说明已知问题](release-notes-big-data-cluster.md#known-issues)。

## <a name="upgrade-from-supported-release"></a>从支持的版本升级

本部分介绍如何将 SQL Server BDC 从支持的版本（从 SQL Server 2019 GDR1 开始）升级到新的支持版本。

1. 验证是否没有活动的 Livy 会话。

   请确保没有活动的 Livy 会话或批处理作业正在 Azure Data Studio 中运行。 若要确认这一点，一种简单的方法是通过 `curl` 命令或浏览器来请求这些 URL：

    - `<your-gateway-endpoint>/gateway/default/livy/v1/sessions`
    - `<your-gateway-endpoint>/gateway/default/livy/v1/batches`

1. 备份 SQL Server 主实例。

1. 备份 HDFS。

   ```
   azdata bdc hdfs cp --from-path <path> --to-path <path>
   ```
   
   例如： 

   ```
   azdata bdc hdfs cp --from-path hdfs://user/hive/warehouse/%%D --to-path ./%%D
   ```

1. 更新 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]。

   按照说明安装 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]。 
   - [Windows Installer](../azdata/install/deploy-install-azdata-installer.md)
   - [带有 apt 的 Linux](../azdata/install/deploy-install-azdata-linux-package.md)
   - [带有 yum 的 Linux](../azdata/install/deploy-install-azdata-yum.md)
   - [带有 zypper 的 Linux](../azdata/install/deploy-install-azdata-zypper.md)

   >[!NOTE]
   >如果 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 随 `pip` 一起安装，则需要在安装 Windows 安装程序或 Linux 包管理器之前将其手动删除。

1. 更新大数据群集。

   ```
   azdata bdc upgrade -n <clusterName> -t <imageTag> -r <containerRegistry>/<containerRepository>
   ```

   例如，以下脚本使用 `2019-CU6-ubuntu-16.04` 图像标记：

   ```
   azdata bdc upgrade -n bdc -t 2019-CU6-ubuntu-16.04 -r mcr.microsoft.com/mssql/bdc
   ```

>[!NOTE]
>最新的图像标记可以在 [SQL Server 2019 大数据群集发行说明](release-notes-big-data-cluster.md)中获得。

>[!IMPORTANT]
>如果使用专用存储库来预提取用于部署或升级 BDC 的映像，请确保当前版本映像和目标版本映像位于专用存储库中。 这样，在必要时可以成功回退。 此外，如果在原始部署后更改了专用存储库的凭据，请更新相应的环境变量 DOCKER_PASSWORD 和 DOCKER_USERNAME。 不支持对当前版本和目标版本使用不同的专用存储库进行升级。

### <a name="increase-the-timeout-for-the-upgrade"></a>增加升级的超时时间值

如果在分配的时间内未升级某些组件，则可能会发生超时。 下面的代码显示了提示失败的消息：

   ```
   >azdata.EXE bdc upgrade --name <mssql-cluster>
   Upgrading cluster to version 15.0.4003

   NOTE: Cluster upgrade can take a significant amount of time depending on
   configuration, network speed, and the number of nodes in the cluster.

   Upgrading Control Plane.
   Control plane upgrade failed. Failed to upgrade controller.
   ```

若要增加升级的超时时间，请在进行升级时使用 --controller-timeout 和 --component-timeout 参数指定较高的值 。 此选项仅自 SQL Server 2019 CU2 版本起开始提供。 例如：

   ```bash
   azdata bdc upgrade -t 2019-CU6-ubuntu-16.04 --controller-timeout=40 --component-timeout=40 --stability-threshold=3
   ```
--controller-timeout 指定等待控制器或控制器 db 完成升级所需的分钟数。
--component-timeout 指定升级的每个后续阶段必须完成的时间量。

若要增加低于 SQL Server 2019 CU2 版本的版本升级的超时时间值，请编辑升级配置映射。 编辑升级配置映射：

运行以下命令：

   ```bash
   kubectl edit configmap controller-upgrade-configmap
   ```

编辑以下字段：

   controllerUpgradeTimeoutInMinutes 指定等待控制器或控制器 db 完成升级所需的分钟数。 默认值为 5。 至少更新为 20。
   **totalUpgradeTimeoutInMinutes**：指定控制器和控制器 db 完成升级所需的总时间（控制器 + 控制器 db 升级）。默认值为 10。 至少更新为 40。
   **componentUpgradeTimeoutInMinutes**：指定升级的每个后续阶段必须完成的时间量。 默认值为 30。 更新为 45。

保存并退出。

## <a name="update-a-bdc-deployment-from-ctp-or-release-candidate"></a>从 CTP 或候选发布更新 BDC 部署

不支持从 SQL Server 大数据群集的 CTP 或候选发布版本就地升级。 以下部分介绍如何手动删除并重新创建群集。

### <a name="backup-and-delete-the-old-cluster"></a>备份和删除旧群集

在 SQL Server 2019 GDR1 版本之前，不会对部署的大数据群集进行就地升级。 升级到新版本的唯一方法是手动删除并重新创建群集。 每个版本都有唯一的 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 版本，该版本与以前的版本不兼容。 此外，如果在部署了不同旧版本的群集上下载了新的容器映像，则最新映像可能与群集上的旧映像不兼容。 当在容器设置的部署配置文件中使用 `latest` 映像标记时，将拉取较新的映像。 默认情况下，每个版本都具有与 SQL Server 发行版本相对应的特定映像标记。 若要升级到最新版本，请使用以下步骤：

1. 在删除旧群集之前，备份 SQL Server 主实例和 HDFS 上的数据。 对于 SQL Server 主实例，可以使用 [SQL Server 备份和还原](data-ingestion-restore-database.md)。 对于 HDFS，[可以使用 `curl` 复制数据](data-ingestion-curl.md)。

1. 使用 `azdata delete cluster` 命令删除旧群集。

   ```bash
    azdata bdc delete --name <old-cluster-name>
   ```

   > [!Important]
   > 使用与群集匹配的 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 版本。 请勿删除具有较新 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 版本的旧群集。

   > [!Note]
   > 发出 `azdata bdc delete` 命令将导致删除以大数据群集名称标识的命名空间中所创建的所有对象，而不会删除命名空间本身。 只要命名空间为空且未在其中创建其他应用程序，就可以在后续部署中重复使用该命名空间。

1. 卸载旧版本的 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]。

   ```powershell
   pip3 uninstall -r https://azdatacli.blob.core.windows.net/python/azdata/2019-rc1/requirements.txt
   ```

1. 安装最新版本的 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]。 通过以下命令安装最新版本的 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]：

   **Windows：**

   ```powershell
   pip3 install -r https://aka.ms/azdata
   ```

   **Linux：**

   ```bash
   pip3 install -r https://aka.ms/azdata --user
   ```

   > [!IMPORTANT]
   > 对于每个版本，[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 的 `n-1` 的路径都有变化。 即使以前安装了 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]，也必须在创建新群集前从最新路径重新安装。

### <a name="verify-the-azdata-version"></a><a id="azdataversion"></a> 验证 azdata 版本

在部署新的大数据群集之前，请使用 `--version` 参数验证你是否使用最新版本的 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]：

```bash
azdata --version
```

### <a name="install-the-new-release"></a>安装新版本

删除以前的大数据群集并安装最新的 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 后，使用当前的部署说明部署新的大数据群集。 有关详细信息，请参阅[如何在 Kubernetes 上部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md)。 然后，还原所有必需的数据库或文件。

## <a name="next-steps"></a>后续步骤

有关大数据群集的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md)。
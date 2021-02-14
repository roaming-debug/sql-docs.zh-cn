---
title: 为 HDFS 分层装入 ADLS Gen2
titleSuffix: How to mount ADLS Gen2
description: 本文提供了如何使用 Azure Data Lake Storage Gen2 数据源配置 HDFS 分层的示例。
author: nelgson
ms.author: negust
ms.reviewer: mikeray
ms.date: 06/29/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b41ad6a56da30fb30677d781630e30ad5e07d333
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046547"
---
# <a name="how-to-mount-adls-gen2-for-hdfs-tiering-in-a-big-data-cluster"></a>如何在大数据群集中装载 ADLS Gen2 以实现 HDFS 分层

以下部分展示了如何使用 Azure Data Lake Storage Gen2 数据源配置 HDFS 分层的示例。

## <a name="prerequisites"></a>先决条件

- [部署大数据群集](deployment-guidance.md)
- [大数据工具](deploy-big-data-tools.md)
  - **azdata**
  - **kubectl**

## <a name="load-data-into-azure-data-lake-storage"></a><a id="load"></a> 将数据加载到 Azure Data Lake Storage 中

以下部分介绍如何设置 Azure Data Lake Storage Gen2，用于测试 HDFS 分层。 如果已将数据存储在 Azure Data Lake Storage 中，则可以跳过此部分，使用自己的数据。

1. [使用 Data Lake Storage Gen2 功能创建存储帐户](/azure/storage/blobs/data-lake-storage-quickstart-create-account)。

1. 在此存储帐户中为数据[创建文件系统](/azure/storage/blobs/data-lake-storage-explorer)。

1. 将 CSV 或 Parquet 文件上传到容器中。 这是外部 HDFS 数据，该数据将被装载到大数据群集中的 HDFS。

## <a name="credentials-for-mounting"></a>装载凭据

### <a name="use-oauth-credentials-to-mount"></a>使用 OAuth 凭据进行装载

要使用 OAuth 凭据进行装载，需要执行以下步骤：

1. 转到 [Azure 门户](https://portal.azure.com)
1. 导航到“Azure Active Directory”。 可在左侧导航栏上看到此服务。
1. 在右侧导航栏中选择“应用注册”，并创建新的注册
1. 创建 Web 应用程序，然后按照向导操作。 记住在此处创建的应用的名称。 稍后需要以授权用户的身份将此名称添加到 ADLS 帐户。 在选择应用程序时，另请注意概述中的应用程序客户端 ID。
1. 创建 web 应用程序后，请转到“证书和密码”，创建“新客户端密码”并选择密钥持续时间。 “添加”密码。
1. 返回“应用注册”页面，然后单击顶部的“终结点”。 记下“OAuth 令牌终结点(v2)”URL
1. 现在应为 OAuth 记下以下内容：

    - Web 应用程序的“应用程序客户端 ID”
    - 客户端密码
    - 令牌终结点

### <a name="adding-the-service-principal-to-your-adls-account"></a>将服务主体添加到 ADLS 帐户

1. 再次转到门户，并导航到 ADLS 存储帐户文件系统，并在左侧菜单中选择“访问控制(IAM)”。
1. 选择“添加角色分配” 
1. 选择角色“存储 Blob 数据参与者”
1. 搜索之前创建的名称（请注意，它不会显示在列表中，但可通过全名搜索发现）。
1. 保存该角色。

在使用凭据进行装载之前，请等待 5-10 分钟

### <a name="set-environment-variable-for-oauth-credentials"></a>为 OAuth 凭据设置环境变量

在可以访问大数据群集的客户端计算机上打开命令提示符。 使用以下格式设置环境变量。凭据需要包含在逗号分隔列表中。 在 Windows 上需使用“set”命令。 如果使用的是 Linux，请改用“export”。

请注意，在提供凭据时，需要删除逗号“,”之间的所有换行符或空格。 下面的格式设置只是为了便于阅读。

```console
   set MOUNT_CREDENTIALS=fs.azure.account.auth.type=OAuth,
   fs.azure.account.oauth.provider.type=org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider,
   fs.azure.account.oauth2.client.endpoint=[token endpoint],
   fs.azure.account.oauth2.client.id=[Application client ID],
   fs.azure.account.oauth2.client.secret=[client secret]
```

## <a name="use-access-keys-to-mount"></a>使用访问密钥进行装载

还可使用可在 Azure 门户上为 ADLS 帐户获取的访问密钥进行装载。

 > [!TIP]
   > 有关如何查找存储帐户的访问密钥 (`<storage-account-access-key>`) 的详细信息，请参阅[查看帐户密钥和连接字符串](/azure/storage/common/storage-account-keys-manage#view-access-keys-and-connection-string)。

### <a name="set-environment-variable-for-access-key-credentials"></a>为访问密钥凭据设置环境变量

1. 在可以访问大数据群集的客户端计算机上打开命令提示符。

1. 在可以访问大数据群集的客户端计算机上打开命令提示符。 使用以下格式设置环境变量。 凭据需要包含在逗号分隔列表中。 在 Windows 上需使用“set”命令。 如果使用的是 Linux，请改用“export”。

请注意，在提供凭据时，需要删除逗号“,”之间的所有换行符或空格。 下面的格式设置只是为了便于阅读。

```console
set MOUNT_CREDENTIALS=fs.azure.abfs.account.name=<your-storage-account-name>.dfs.core.windows.net,
fs.azure.account.key.<your-storage-account-name>.dfs.core.windows.net=<storage-account-access-key>
```

## <a name="mount-the-remote-hdfs-storage"></a><a id="mount"></a>装载远程 HDFS 存储

现在已为访问密钥或为使用 OAuth 设置了 MOUNT_CREDENTIALS 环境变量，可以开始装载了。 以下步骤将 Azure Data Lake 中的远程 HDFS 存储装载到大数据群集的本地 HDFS 存储中。

1. 使用 kubectl 查找大数据群集中终结点 controller-svc-external 服务的 IP 地址 。 查找“外部 IP”。

   ```bash
   kubectl get svc controller-svc-external -n <your-big-data-cluster-name>
   ```

1. 使用 azdata，同时使用控制器终结点的外部 IP 地址和群集用户名及密码登录：

   ```bash
   azdata login -e https://<IP-of-controller-svc-external>:30080
   ```
1. 设置环境变量 MOUNT_CREDENTIALS（向上滚动获取说明）

1. 使用 azdata bdc hdfs mount create 在 Azure 中装载远程 HDFS 存储。 在运行以下命令之前替换占位符值：

   ```bash
   azdata bdc hdfs mount create --remote-uri abfs://<blob-container-name>@<storage-account-name>.dfs.core.windows.net/ --mount-path /mounts/<mount-name>
   ```

   > [!NOTE]
   > 装载 create 命令是异步的。 此时，没有消息指示装载是否成功。 请查看[状态](#status)部分，检查装载的状态。

如果成功装载，应该能够查询 HDFS 数据并针对它运行 Spark 作业。 它显示在大数据群集上 HDFS 中由 `--mount-path` 指定的位置处。

## <a name="get-the-status-of-mounts"></a><a id="status"></a> 获取装载状态

要列出大数据群集中的全部装载状态，可使用以下命令：

```bash
azdata bdc hdfs mount status
```

要列出 HDFS 中特定路径处的装载状态，可使用以下命令：

```bash
azdata bdc hdfs mount status --mount-path <mount-path-in-hdfs>
```

## <a name="refresh-a-mount"></a>刷新装载

以下示例刷新装载。 此刷新还会清除装载缓存。

```bash
azdata bdc hdfs mount refresh --mount-path <mount-path-in-hdfs>
```

## <a name="delete-the-mount"></a><a id="delete"></a> 删除装载

要删除装载，请使用 azdata bdc hdfs mount delete 命令，并在 HDFS 中指定装载路径：

```bash
azdata bdc hdfs mount delete --mount-path <mount-path-in-hdfs>
```

## <a name="next-steps"></a>后续步骤

有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] 的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)。

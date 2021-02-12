---
title: 为 HDFS 分层装入 S3
titleSuffix: SQL Server big data clusters
description: 本文介绍如何配置 HDFS 分层，以将外部 S3 文件系统装载到 SQL Server 2019 大数据群集上的 HDFS 中。
author: nelgson
ms.author: negust
ms.reviewer: mikeray
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6fce2d87f618fec23f204d14cd813beea4ac74bd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046507"
---
# <a name="how-to-mount-s3-for-hdfs-tiering-in-a-big-data-cluster"></a>如何在大数据群集中装载 S3 以实现 HDFS 分层

以下部分提供了如何使用 S3 Storage 数据源配置 HDFS 分层的示例。

## <a name="prerequisites"></a>先决条件

- [部署大数据群集](deployment-guidance.md)
- [大数据工具](deploy-big-data-tools.md)
  - **azdata**
  - **kubectl**
- 创建数据并将其上传到 S3 Bucket 
  - 将 CSV 或 Parquet 文件上传到 S3 Bucket。 这是外部 HDFS 数据，该数据将被装载到大数据群集中的 HDFS。

## <a name="access-keys"></a>访问密钥

### <a name="set-environment-variable-for-access-key-credentials"></a>为访问密钥凭据设置环境变量

在可以访问大数据群集的客户端计算机上打开命令提示符。 使用以下格式设置环境变量。 请注意，凭据需要位于以逗号分隔的列表中。 在 Windows 上需使用“set”命令。 如果使用的是 Linux，请改用“export”。

   ```text
    set MOUNT_CREDENTIALS=fs.s3a.access.key=<Access Key ID of the key>,
    fs.s3a.secret.key=<Secret Access Key of the key>
   ```

   > [!TIP]
   > 有关如何创建 S3 访问密钥的详细信息，请参阅 [S3 访问密钥](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys)。

## <a name="mount-the-remote-hdfs-storage"></a><a id="mount"></a>装载远程 HDFS 存储

准备好带有访问密钥的凭据文件后，接下来即可开始装载。 以下步骤说明了如何将 S3 中的远程 HDFS 存储装载到大数据群集的本地 HDFS 存储中。

1. 使用 kubectl 查找大数据群集中终结点 controller-svc-external 服务的 IP 地址 。 查找“外部 IP”。

   ```bash
   kubectl get svc controller-svc-external -n <your-big-data-cluster-name>
   ```

1. 使用 azdata，同时使用控制器终结点的外部 IP 地址和群集用户名及密码登录：

   ```bash
   azdata login -e https://<IP-of-controller-svc-external>:30080/
   ```
   
1. 按照上面的说明设置环境变量 MOUNT_CREDENTIALS

1. 使用 azdata bdc hdfs mount create 在 Azure 中装载远程 HDFS 存储。 在运行以下命令之前替换占位符值：

   ```bash
   azdata bdc hdfs mount create --remote-uri s3a://<S3 bucket name> --mount-path /mounts/<mount-name>
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

以下示例刷新装载。

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

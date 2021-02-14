---
title: 还原数据库
titleSuffix: SQL Server big data clusters
description: 本文介绍如何将数据库还原到 SQL Server 2019 大数据群集的主实例中。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3082c970c648fddf67b635ba7cbae5b19bfc0170
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049017"
---
# <a name="restore-a-database-into-the-sql-server-big-data-cluster-master-instance"></a>将数据库还原到 SQL Server 大数据群集主实例

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文介绍如何将现有数据库还原到 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] 的主实例中。 建议使用备份、复制和还原。

## <a name="backup-your-existing-database"></a>备份现有数据库

首先，从 Windows 或 Linux 上的 SQL Server 备份现有 SQL Server 数据库。 将标准备份技术与 Transact-SQL 或 SQL Server Management Studio (SSMS) 等工具结合使用。

本文演示如何还原 AdventureWorks 数据库，但可以使用任何数据库备份。 

> [!TIP]
> 下载 [AdventureWorks 备份](../samples/adventureworks-install-configure.md)。

## <a name="copy-the-backup-file"></a>复制备份文件

将备份文件复制到 Kubernetes 群集的主实例 Pod 中的 SQL Server 容器。

```bash
kubectl cp <path to .bak file> master-0:/var/tmp/<.bak filename> -c mssql-server -n <name of your big data cluster>
```

示例：

```bash
kubectl cp ~/Downloads/AdventureWorks2016CTP3.bak master-0:/var/tmp/AdventureWorks2016CTP3.bak -c mssql-server -n clustertest
```

然后，验证是否已将备份文件复制到 Pod 容器。

```bash
kubectl exec -it master-0 -n <name of your big data cluster> -c mssql-server -- bin/bash
cd /var/
ls /tmp
exit
```

示例：

```bash
kubectl exec -it master-0 -n clustertest -c mssql-server -- bin/bash
cd /var/
ls /tmp
exit
```

## <a name="restore-the-backup-file"></a>还原备份文件

接下来，将数据库备份还原到主实例 SQL Server。  如果要还原在 Windows 上创建的数据库备份，则需要获取文件的名称。  在 Azure Data Studio 中，连接到主实例并运行以下 SQL 脚本：

```sql
RESTORE FILELISTONLY FROM DISK='/tmp/<db file name>.bak'
```

示例：

```sql
RESTORE FILELISTONLY FROM DISK='/tmp/AdventureWorks2016CTP3.bak'
```

![备份文件列表](media/restore-database/database-restore-file-list.png)

现在，还原数据库。 以下脚本是一个示例。 根据需要替换名称/路径，具体取决于数据库备份。

```sql
RESTORE DATABASE AdventureWorks2016CTP3
FROM DISK='/tmp/AdventureWorks2016CTP3.bak'
WITH MOVE 'AdventureWorks2016CTP3_Data' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_Data.mdf',
        MOVE 'AdventureWorks2016CTP3_Log' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_Log.ldf',
        MOVE 'AdventureWorks2016CTP3_mod' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_mod'
```

## <a name="configure-data-pool-and-hdfs-access"></a>配置数据池和 HDFS 访问

现在，针对访问数据池和 HDFS 的 SQL Server 主实例，请运行数据池和存储池存储过程。 针对新还原的数据库运行以下 Transact-SQL 脚本：

```sql
USE AdventureWorks2016CTP3
GO
-- Create the SqlDataPool data source:
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlDataPool')
  CREATE EXTERNAL DATA SOURCE SqlDataPool
  WITH (LOCATION = 'sqldatapool://controller-svc/default');

-- Create the SqlStoragePool data source:
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlStoragePool')
   CREATE EXTERNAL DATA SOURCE SqlStoragePool
   WITH (LOCATION = 'sqlhdfs://controller-svc/default');
GO
```

> [!NOTE]
> 只有针对从旧版 SQL Server 还原的数据库才必须运行这些安装脚本。 如果在 SQL Server 主实例中创建新数据库，则系统已为你配置数据池和存储池存储过程。

## <a name="next-steps"></a>后续步骤

若要了解有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅以下概述：

- [什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)

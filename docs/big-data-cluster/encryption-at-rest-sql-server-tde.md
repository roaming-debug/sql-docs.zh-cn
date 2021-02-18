---
title: SQL Server 大数据群集静态透明数据加密 (TDE) 使用指南
titleSuffix: SQL Server Big Data Clusters
description: 本文介绍了如何使用 BDC 的 SQL Server TDE 静态加密功能
author: DaniBunny
ms.author: dacoelho
ms.reviewer: mihaelab
ms.date: 10/19/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7d249f9b70479603ad66e6b68cec784b1476324a
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489311"
---
# <a name="sql-server-big-data-clusters-transparent-data-encryption-tde-at-rest-usage-guide"></a>SQL Server 大数据群集静态透明数据加密 (TDE) 使用指南

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本指南演示了如何使用 SQL Server 大数据群集的静态加密功能来加密数据库。

体验通常与 Linux 上的 SQL Server 相同，除了特别注明的地方，[标准 TDE 文档](../relational-databases/security/encryption/transparent-data-encryption.md)也适用。 若要监视主实例上的加密状态，请遵循在 [`sys.dm_database_encryption_keys`](../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md) 和 [`sys.certificates`](../relational-databases/system-catalog-views/sys-certificates-transact-sql.md) 基础之上生成的标准 DMV 查询模式。

不受支持的功能：
* 数据池加密

## <a name="prerequisites"></a><a id="prereqs"></a>先决条件

- [SQL Server 大数据群集 CU8 及更高版本](release-notes-big-data-cluster.md)
- [大数据工具](deploy-big-data-tools.md)
   - **Azure Data Studio**
- 拥有管理权限的 SQL Server 用户。

## <a name="query-the-installed-certificates"></a>查询已安装的证书

1. 在 Azure Data Studio 中，连接到大数据群集的 SQL Server 主实例。 有关详细信息，请参阅[连接到 SQL Server 主实例](connect-to-big-data-cluster.md#master)。

1. 双击“服务器”窗口中的连接，以显示 SQL Server 主实例的服务器仪表板  。 选择“新建查询”  。

   ![SQL Server 主实例查询](./media/tutorial-data-pool-ingest-sql/sql-server-master-instance-query.png)

1. 运行下面的 Transact-SQL 命令，以将上下文更改为主实例中的 master 数据库。

   ```sql
   USE master
   GO
   ```

1. 查询已安装的系统管理的证书。 

   ```sql
    SELECT TOP 1 name FROM sys.certificates WHERE name LIKE 'TDECertificate%' ORDER BY name DESC
   ```

    根据需要，使用不同的查询条件。

    证书名称将列为“TDECertificate{timestamp}”。 当你看到前缀 TDECertificate 且后面是时间戳时，表示这是由系统管理的功能提供的证书。

## <a name="encrypt-a-database-using-the-system-provided-certificate"></a>使用系统提供的证书对数据库进行加密

在以下示例中，根据上一部分的输出，将名为 userdb 的数据库视为加密目标，并将系统提供的证书命名为 TDECertificate2020_09_15_22_46_27。

1. 使用以下模式通过所提供的系统证书来生成数据库加密密钥。

   ```sql
    USE userdb; 
    GO
    CREATE DATABASE ENCRYPTION KEY WITH ALGORITHM = AES_256 ENCRYPTION BY SERVER CERTIFICATE TDECertificate2020_09_15_22_46_27;
    GO
   ```

1. 运行以下命令来加密数据库 userdb。

   ```sql
    ALTER DATABASE userdb SET ENCRYPTION ON;
    GO
   ```

## <a name="next-steps"></a>后续步骤

了解 HDFS 的静态加密：
> [!div class="nextstepaction"]
> [HDFS 加密区域](encryption-at-rest-hdfs-encryption-zones.md)

---
title: 访问外部数据：SQL Server - PolyBase
description: 了解如何使用 SQL Server 实例上的 PolyBase 来查询另一个 SQL Server 实例中的外部数据。 创建外部表以引用外部数据。
ms.date: 10/06/2020
ms.custom: seo-lt-2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: dbd62f695128965e1b60f9b027ddc06c003a8b71
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351772"
---
# <a name="configure-polybase-to-access-external-data-in-sql-server"></a>配置 PolyBase 以访问 SQL Server 中的外部数据

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本文介绍如何使用 SQL Server 实例上的 PolyBase 来查询另一个 SQL Server 实例中的外部数据。

## <a name="prerequisites"></a>先决条件

如果尚未安装 PolyBase，请参阅 [PolyBase 安装](polybase-installation.md)。 这篇安装文章介绍了安装的先决条件。 安装后，还应确保[启用 PolyBase](polybase-installation.md#enable)。

SQL Server 外部数据源使用 SQL 身份验证。

在创建数据库范围凭据之前，必须先创建[主密钥](../../t-sql/statements/create-master-key-transact-sql.md)。 

## <a name="configure-a-sql-server-external-data-source"></a>配置 SQL Server 外部数据源

若要查询 SQL Server 数据源中的数据，必须创建外部表以引用外部数据。 本节提供用于创建这些外部表的示例代码。
 
为了获得最佳查询性能，请在外部表列上创建统计信息，尤其是用于联接、筛选和聚合的表列。

此部分中使用了以下 Transact-SQL 命令：

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
- [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md) 
- [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)

1. 创建数据库范围凭据以访问 SQL Server 源。 下面的示例使用 `IDENTITY = 'username'` 和 `SECRET = 'password'` 创建外部数据源的凭据。

    ```sql
    CREATE DATABASE SCOPED CREDENTIAL SqlServerCredentials
    WITH IDENTITY = 'username', SECRET = 'password';
    ```
   >[!IMPORTANT]
   >用于 PolyBase 的 SQL ODBC 连接器仅支持基本身份验证，不支持 Kerberos 身份验证。

1. 使用 [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) 创建外部数据源。 如下示例中：

   - 创建名为 `SQLServerInstance` 的外部数据源。
   - 标识外部数据源 (`LOCATION = '<vendor>://<server>[:<port>]'`)。 在示例中，它指向 SQL Server 的默认实例。
   - 标识是否应将计算推送到源 (`PUSHDOWN`)。 `PUSHDOWN` 默认设置为 `ON`。

   最后，该示例使用先前创建的凭据。

    ```sql
    CREATE EXTERNAL DATA SOURCE SQLServerInstance
        WITH ( LOCATION = 'sqlserver://SqlServer',
        PUSHDOWN = ON,
        CREDENTIAL = SQLServerCredentials);
    ```

1. 也可在外部表上创建统计信息。

  为了获得最佳查询性能，请在外部表列上创建统计信息，尤其是用于联接、筛选和聚合的表列。

  ```sql
    CREATE STATISTICS statistics_name ON customer (C_CUSTKEY)
    WITH FULLSCAN;
  ```

>[!IMPORTANT]
>创建外部数据源后，可以使用 [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md) 命令在该数据源上创建可查询的表。

## <a name="sql-server-connector-compatible-types"></a>SQL Server 连接器兼容类型

可以连接到可识别 SQL Server 连接的其他数据源。 使用 SQL Server PolyBase 连接器创建 Azure Synapse Analytics 和 Azure SQL 数据库的外部表。 若要完成此任务，请执行前面列出的相同步骤。 确保数据库作用域凭据、服务器地址、端口和位置字符串与要连接的兼容数据源的相应内容相关联。

## <a name="next-steps"></a>后续步骤

若要了解有关 PolyBase 的详细信息，请参阅 [SQL Server PolyBase 的概述](polybase-guide.md)。

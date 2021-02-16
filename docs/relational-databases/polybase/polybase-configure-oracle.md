---
title: 访问外部数据：Oracle - PolyBase
description: 本文演示如何使用 PolyBase 创建外部数据源以访问 Oracle 数据。
ms.date: 12/13/2019
ms.custom: seo-lt-2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: 77478f132083417b20f3a722804f497ecf373d73
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351786"
---
# <a name="configure-polybase-to-access-external-data-in-oracle"></a>配置 PolyBase 以访问 Oracle 中的外部数据

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本文介绍如何使用 SQL Server 实例上的 PolyBase 来查询 Oracle 中的外部数据。

## <a name="prerequisites"></a>先决条件

如果尚未安装 PolyBase，请参阅 [PolyBase 安装](polybase-installation.md)。

  在创建数据库范围凭据之前，必须先创建[主密钥](../../t-sql/statements/create-master-key-transact-sql.md)。 

## <a name="configure-an-oracle-external-data-source"></a>配置 Oracle 外部数据源

若要查询 Oracle 数据源中的数据，必须创建外部表以引用外部数据。 本节提供用于创建这些外部表的示例代码。

此部分中使用了以下 Transact-SQL 命令：

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
- [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md) 
- [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)


1. 创建数据库范围凭据以访问 Oracle 数据源。

    ```sql
    /*  specify credentials to external data source
    *  IDENTITY: user name for external source. 
    *  SECRET: password for external source.
    */
    CREATE DATABASE SCOPED CREDENTIAL credential_name WITH IDENTITY = 'username', Secret = 'password';
    ```
    
   > [!IMPORTANT] 
   > 用于 PolyBase 的 Oracle ODBC 连接器仅支持基本身份验证，不支持 Kerberos 身份验证。 

1. 使用 [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) 创建外部数据源。

    ```sql
    /* 
    * LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CONNECTION_OPTIONS: Specify driver location
    * CREDENTIAL: the database scoped credential, created above.
    */  
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH ( LOCATION = 'oracle://<server address>[:<port>]',
    -- PUSHDOWN = ON | OFF,
    CREDENTIAL = credential_name)
    ```

1. **可选：** 在外部表上创建统计信息。

    为了获得最佳查询性能，我们建议在外部表列上创建统计信息，尤其是用于联接、筛选和聚合的统计信息。

    ```sql
    CREATE STATISTICS statistics_name ON customer (C_CUSTKEY) WITH FULLSCAN; 
    ```

>[!IMPORTANT] 
>创建外部数据源后，可以使用 [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md) 命令在该数据源上创建可查询的表。 

有关详细信息和示例，请参阅以下文章：

- [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md)。
- [SQL Server PolyBase 概述](polybase-guide.md)。

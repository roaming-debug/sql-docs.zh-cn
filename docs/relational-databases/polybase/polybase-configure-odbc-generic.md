---
title: 访问外部数据：ODBC 泛型类型 - PolyBase
description: 借助 SQL Server 中的 PolyBase，可通过连接器连接到兼容 ODBC 的数据源。 安装 ODBC 驱动程序并创建外部表。
ms.date: 07/16/2020
ms.custom: seo-lt-2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: ac4fa22e2d0aea57f25aaa9ef2d8c570f8bb130b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475928"
---
# <a name="configure-polybase-to-access-external-data-with-odbc-generic-types"></a>配置 PolyBase 以使用 ODBC 泛型类型访问外部数据

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

借助 SQL Server 2019 中的 PolyBase，可使用 ODBC 连接器连接到兼容 ODBC 的数据源。

本文演示如何使用 ODBC 数据源创建配置连接。 提供的指导使用一个特定 ODBC 驱动程序作为示例。 请向 ODBC 提供程序获取具体的示例。 参考数据源的 ODBC 驱动程序文档来确定适当的连接字符串选项。 本文中的示例可能不适用于任何特定的 ODBC 驱动程序。

## <a name="prerequisites"></a>先决条件

>[!NOTE]
>此功能需要 Windows 上的 SQL Server。

* 必须为 SQL Server 实例 [PolyBase 安装](polybase-installation.md)安装和启用 PolyBase。

* 在创建数据库范围凭据之前，必须先创建[主密钥](../../t-sql/statements/create-master-key-transact-sql.md)。

## <a name="install-the-odbc-driver"></a>安装 ODBC 驱动程序

在每个 PolyBase 节点上下载并安装要连接的数据源的 ODBC 驱动程序。 正确安装驱动程序后，从“ODBC 数据源管理器”查看和测试该驱动程序  。

![PolyBase 横向扩展组](../../relational-databases/polybase/media/polybase-odbc-admin.png) 

在以上的示例中，驱动程序的名称用红圈圈出。 创建外部数据源时，请使用此名称。

> [!IMPORTANT]
> 若要提高查询性能，请启用连接池。 可通过“ODBC 数据源管理器”完成此操作  。

## <a name="create-dependent-objects-in-sql-server"></a>在 SQL Server 中创建从属对象

若要使用 ODBC 数据源，必须先创建几个对象才能完成配置。

此部分中使用了以下 Transact-SQL 命令：

* [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
* [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md) 

1. 创建数据库范围凭据以访问 ODBC 数据源。

    ```sql
    CREATE DATABASE SCOPED CREDENTIAL <credential_name> WITH IDENTITY = '<username>', Secret = '<password>';
    ```

    例如，以下示例创建一个名为 `credential_name` 的凭据，具有一个 `username` 标识和一个复杂密码。

    ```sql
    CREATE DATABASE SCOPED CREDENTIAL credential_name WITH IDENTITY = 'username', Secret = 'BycA4ZjrE#*2W%!';
    ```

1. 使用 [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) 创建外部数据源。

    ```sql
    CREATE EXTERNAL DATA SOURCE <external_data_source_name>
    WITH ( LOCATION = 'odbc://<ODBC server address>[:<port>]',
    CONNECTION_OPTIONS = 'Driver={<Name of Installed Driver>};
    ServerNode = <name of server  address>:<Port>',
    -- PUSHDOWN = [ON] | OFF,
    CREDENTIAL = <credential_name> );
    ```

    以下的示例创建一个外部数据源：
    * 命名为 `external_data_source_name`
    * 位于 ODBC `SERVERNAME` 和端口 `4444`
    * 与 `CData ODBC Driver For SAP 2015` 连接 - 这是在[安装 ODBC 驱动程序](#install-the-odbc-driver)下创建的驱动程序
    * 在 `ServerNode` `sap_server_node` 端口 `5555` 上
    * 配置进行下推到服务器的处理（`PUSHDOWN = ON`）
    * 使用 `credential_name` 凭据

    ```sql
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH ( LOCATION = 'odbc://SERVERNAME:4444',
    CONNECTION_OPTIONS = 'Driver={CData ODBC Driver For SAP 2015};
    ServerNode = sap_server_node:5555',
    PUSHDOWN = ON,
    CREDENTIAL = credential_name );
    ```
    
## <a name="create-an-external-table"></a>创建外部表

一旦创建了从属对象，就可以使用 T-SQL 创建外部表。 

此部分中使用了以下 Transact-SQL 命令：
* [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md)
* [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)

1. 创建一个或多个外部表。

   创建外部表。 需要使用 `DATA_SOURCE` 参数引用前面创建的外部数据源，并将源表指定为 `LOCATION`。 不需要引用所有列，但需要确保正确映射类型。  

   ```sql
     CREATE EXTERNAL TABLE <your_table_name>
     (
     <col1_name>     DECIMAL(38) NOT NULL,
     <col2_name>     DECIMAL(38) NOT NULL,
     <col3_name>     CHAR COLLATE Latin1_General_BIN NOT NULL
     )
     WITH (
     LOCATION='<sap_table_name>',
     DATA_SOURCE= <external_data_source_name>
     )
     ;
   ```

   > [!NOTE]
   > 请注意，可以使用此外部数据源对所有外部表重复使用从属对象。

1. **可选：** 在外部表上创建统计信息。

    为了获得最佳查询性能，建议在外部表列上创建统计信息，尤其是用于联接、筛选和聚合的统计信息。

    ```sql
    CREATE STATISTICS statistics_name ON contact (FirstName) WITH FULLSCAN; 
    ```
    
## <a name="next-steps"></a>后续步骤

若要了解有关 PolyBase 的详细信息，请参阅 [SQL Server PolyBase 的概述](polybase-guide.md)。

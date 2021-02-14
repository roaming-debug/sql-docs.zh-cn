---
title: 查询 Oracle 中的外部数据
titleSuffix: SQL Server big data clusters
description: 本教程演示如何从 SQL Server 2019 大数据群集中查询 Oracle 数据。 为 Oracle 中的数据创建外部表，然后运行查询。
author: MikeRayMSFT
ms.author: dacoelho
ms.reviewer: mikeray
ms.date: 10/01/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 670ab5046c1904fee16f6c0271c49e2a4ecd72fe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045627"
---
# <a name="tutorial-query-oracle-from-sql-server-big-data-cluster"></a>教程：从 SQL Server 大数据群集查询 Oracle

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本教程演示如何从 SQL Server 2019 大数据群集中查询 Oracle 数据。 若要运行本教程，需要有 Oracle 服务器的访问权限。 需要拥有对外部对象的读取权限的 Oracle 用户帐户。 支持 Oracle 代理用户身份验证。 如果没有访问权限，可通过本教程了解数据虚拟化如何适用于 SQL Server 大数据群集中的外部数据源。

在本教程中，你将了解如何执行以下操作：

> [!div class="checklist"]
> * 为外部 Oracle 数据库中的数据创建外部表。
> * 将此数据与主实例中的高值数据联接起来。

> [!TIP]
> 如果需要，可以下载并运行本教程中的命令脚本。 有关说明，请参阅 GitHub 上的[数据虚拟化示例](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/data-virtualization)。

## <a name="prerequisites"></a><a id="prereqs"></a>先决条件

- [大数据工具](deploy-big-data-tools.md)
   - **kubectl**
   - **Azure Data Studio**
   - **SQL Server 2019 扩展**
- [将示例数据加载到大数据群集中](tutorial-load-sample-data.md)

## <a name="create-an-oracle-table"></a>创建 Oracle 表

以下步骤在 Oracle 中创建一个名为 `INVENTORY` 的示例表。

1. 连接到要用于本教程的 Oracle 实例和数据库。

1. 若要创建 `INVENTORY` 表，请运行下列语句：

   ```sql
    CREATE TABLE "INVENTORY"
    (
        "INV_DATE" NUMBER(10,0) NOT NULL,
        "INV_ITEM" NUMBER(10,0) NOT NULL,
        "INV_WAREHOUSE" NUMBER(10,0) NOT NULL,
        "INV_QUANTITY_ON_HAND" NUMBER(10,0)
    );

    CREATE INDEX INV_ITEM ON HR.INVENTORY(INV_ITEM);
    ```

1. 将 inventory.csv 文件的内容导入到此表中。 此文件由[先决条件](#prereqs)部分中的示例创建脚本创建。

## <a name="create-an-external-data-source"></a>创建外部数据源

第一步是创建可访问 Oracle 服务器的外部数据源。

1. 在 Azure Data Studio 中，连接到大数据群集的 SQL Server 主实例。 有关详细信息，请参阅[连接到 SQL Server 主实例](connect-to-big-data-cluster.md#master)。

1. 双击“服务器”窗口中的连接，以显示 SQL Server 主实例的服务器仪表板  。 选择“新建查询”  。

   ![SQL Server 主实例查询](./media/tutorial-query-oracle/sql-server-master-instance-query.png)

1. 运行以下 Transact-SQL 命令，将上下文更改为主实例中的 Sales 数据库。

   ```sql
   USE Sales
   GO
   ```

1. 创建数据库范围凭据以连接到 Oracle 服务器。 在以下语句中向 Oracle 服务器提供适当的凭据。

   ```sql
   CREATE DATABASE SCOPED CREDENTIAL [OracleCredential]
   WITH IDENTITY = '<oracle_user,nvarchar(100),SYSTEM>', SECRET = '<oracle_user_password,nvarchar(100),manager>';
   ```

1. 创建指向 Oracle 服务器的外部数据源。

   ```sql
   CREATE EXTERNAL DATA SOURCE [OracleSalesSrvr]
   WITH (LOCATION = 'oracle://<oracle_server,nvarchar(100)>',CREDENTIAL = [OracleCredential]);
   ```

### <a name="optional-oracle-proxy-authentication"></a>可选：Oracle 代理身份验证

Oracle 支持代理身份验证，以提供精细的访问控制。 代理用户使用其凭据连接到 Oracle 数据库，并在数据库中模拟另一个用户。 

可以将代理用户配置为，与被模拟的用户相比，具有有限的访问权限。 例如，可以允许代理用户使用被模拟用户的特定数据库角色进行连接。 即使多个用户使用代理身份验证进行连接，通过代理用户连接到 Oracle 数据库的用户的标识也会保留在连接中。 这样，Oracle 可以强制执行访问控制，并审核代表实际用户执行的操作。

如果你的方案需要使用 Oracle 代理用户，请将前面的第 4 步和第 5 步替换为以下内容。

4. 创建数据库范围凭据以连接到 Oracle 服务器。 在下面的语句中，向 Oracle 服务器提供适当的 Oracle 代理用户凭据。

   ```sql
   CREATE DATABASE SCOPED CREDENTIAL [OracleProxyCredential]
   WITH IDENTITY = '<oracle_proxy_user,nvarchar(100),SYSTEM>', SECRET = '<oracle_proxy_user_password,nvarchar(100),manager>';
   ```

5. 创建指向 Oracle 服务器的外部数据源。

   ```sql
   CREATE EXTERNAL DATA SOURCE [OracleSalesSrvr]
   WITH (LOCATION = 'oracle://<oracle_server,nvarchar(100)>',
   CONNECTION_OPTIONS = 'ImpersonateUser=% CURRENT_USER',
   CREDENTIAL = [OracleProxyCredential]);
   ```

## <a name="create-an-external-table"></a>创建外部表

接下来，在 Oracle 服务器上的 `INVENTORY` 表上创建名为 iventory_ora 的外部表。

```sql
CREATE EXTERNAL TABLE [inventory_ora]
    ([inv_date] DECIMAL(10,0) NOT NULL, [inv_item] DECIMAL(10,0) NOT NULL,
    [inv_warehouse] DECIMAL(10,0) NOT NULL, [inv_quantity_on_hand] DECIMAL(10,0))
WITH (DATA_SOURCE=[OracleSalesSrvr],
        LOCATION='<oracle_service_name,nvarchar(30),xe>.<oracle_schema,nvarchar(128),HR>.<oracle_table,nvarchar(128),INVENTORY>');
```

> [!NOTE]
> 针对 Oracle 进行查询时，表名称和列名称将使用 ANSI SQL 带引号的标识符。 因此，名称区分大小写。 务必在外部表定义中指定与 Oracle 元数据中的表和列名称完全匹配的名称。

## <a name="query-the-data"></a>查询数据

运行以下查询，将 `iventory_ora` 外部表中的数据与本地 `Sales` 数据库中的表联接起来。

```sql
SELECT TOP(100) w.w_warehouse_name, i.inv_item, SUM(i.inv_quantity_on_hand) as total_quantity
  FROM [inventory_ora] as i
  JOIN item as it
    ON it.i_item_sk = i.inv_item
  JOIN warehouse as w
    ON w.w_warehouse_sk = i.inv_warehouse
 WHERE it.i_category = 'Books' and i.inv_item BETWEEN 1 and 18000 --> get items within specific range
 GROUP BY w.w_warehouse_name, i.inv_item;
```

## <a name="clean-up"></a>清除

使用以下命令删除本教程中创建的数据库对象。

```sql
DROP EXTERNAL TABLE [inventory_ora];
DROP EXTERNAL DATA SOURCE [OracleSalesSrvr] ;
DROP DATABASE SCOPED CREDENTIAL [OracleCredential];
```

## <a name="next-steps"></a>后续步骤

了解如何将数据引入到数据池中：
> [!div class="nextstepaction"]
> [将数据加载到数据池中](tutorial-data-pool-ingest-sql.md)

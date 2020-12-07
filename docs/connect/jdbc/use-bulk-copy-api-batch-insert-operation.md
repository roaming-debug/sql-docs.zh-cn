---
title: 用于在 JDBC 中执行批量插入的大容量复制 API
description: Microsoft JDBC Driver for SQL Server 支持使用大容量复制对 Azure 数据仓库执行批量插入操作，以便更快地将数据加载到数据库中。
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 14074b0136baf800b038e4b113325e81d65dc3e7
ms.sourcegitcommit: 0c0e4ab90655dde3e34ebc08487493e621f25dda
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96442601"
---
# <a name="using-bulk-copy-api-for-batch-insert-operation"></a>将大容量复制 API 用于批量插入操作

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Microsoft JDBC Driver 7.0 for SQL Server 支持使用大容量复制 API 对 Azure 数据仓库执行批量插入操作。 使用此功能，用户可以将驱动程序启用为，在执行批量插入操作时，在底层执行大容量复制操作。 驱动程序旨在实现性能提升，同时插入驱动程序通过常规批量插入操作插入的相同数据。 驱动程序分析用户的 SQL 查询，同时利用大容量复制 API 代替常规批量插入操作。 下面介绍了为大容量复制 API 启用批量插入功能的各种方法，并列出了此功能的限制。 此页还包含一个展示使用情况和性能提升的小型示例代码。

此功能仅适用于 PreparedStatement 和 CallableStatement 的 `executeBatch()` 和 `executeLargeBatch()` API。

## <a name="prerequisites"></a>先决条件

为大容量复制 API 启用批量插入功能有两个先决条件。

* 服务器必须是 Azure 数据仓库。
* 查询必须是插入查询（查询可以包含注释，但要使此功能生效，查询必须以 INSERT 关键字开头）。

## <a name="enabling-bulk-copy-api-for-batch-insert"></a>为大容量复制 API 启用批量插入功能

为大容量复制 API 启用批量插入功能的方法有三种。

### <a name="1-enabling-with-connection-property"></a>1.使用连接属性启用

将 useBulkCopyForBatchInsert=true;  添加到连接字符串可启用此功能。

```java
Connection connection = DriverManager.getConnection("jdbc:sqlserver://<server>:<port>;userName=<user>;password=<password>;database=<database>;useBulkCopyForBatchInsert=true;");
```

### <a name="2-enabling-with-setusebulkcopyforbatchinsert-method-from-sqlserverconnection-object"></a>2.从 SQLServerConnection 对象使用 setUseBulkCopyForBatchInsert() 方法启用

调用 SQLServerConnection.setUseBulkCopyForBatchInsert(true)  可启用此功能。

SQLServerConnection.getUseBulkCopyForBatchInsert()  用于检索 useBulkCopyForBatchInsert  连接属性的当前值。

对于每个 PreparedStatement，useBulkCopyForBatchInsert  值在其初始化时保持不变。 对 SQLServerConnection.setUseBulkCopyForBatchInsert()  的任何后续调用都不会影响已创建的 PreparedStatement 的值。

### <a name="3-enabling-with-setusebulkcopyforbatchinsert-method-from-sqlserverdatasource-object"></a>3.从 SQLServerDataSource 对象使用 setUseBulkCopyForBatchInsert() 方法启用

与上面的方法类似，不同之处在于使用 SQLServerDataSource 创建 SQLServerConnection 对象。 这两种方法可以得到相同的结果。

## <a name="known-limitations"></a>已知的限制

目前，这些限制适用于此功能。

* 不支持包含非参数化值的插入查询（例如，`INSERT INTO TABLE VALUES (?, 2`）。 通配符 (?) 是此函数唯一支持的参数。
* 不支持包含 INSERT-SELECT 表达式的插入查询（例如，`INSERT INTO TABLE SELECT * FROM TABLE2`）。
* 不支持包含多个 VALUE 表达式的插入查询（例如，`INSERT INTO TABLE VALUES (1, 2) (3, 4)`）。
* 不支持后跟 OPTION 子句、与多个表联接或后跟另一个查询的插入查询。
* 由于大容量复制 API 的限制，此功能暂不支持 `MONEY`、`SMALLMONEY`、`DATE`、`DATETIME`、`DATETIMEOFFSET`、`SMALLDATETIME`、`TIME`、`GEOMETRY` 和 `GEOGRAPHY` 数据类型。

如果查询因非“SQL Server”相关错误而失败，驱动程序会记录错误消息，并回退到批量插入的原始逻辑。

## <a name="example"></a>示例

下面的示例代码展示了在两种方案（常规与大容量复制 API）中对包含一千行的 Azure Synapse Analytics 执行批量插入操作的用例。

```java
    public static void main(String[] args) throws Exception
    {
        String tableName = "batchTest";
        String tableNameBulkCopyAPI = "batchTestBulk";

        String azureDWconnectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<database>;user=<user>;password=<password>";

        try (Connection con = DriverManager.getConnection(azureDWconnectionUrl); // connects to an Azure Data Warehouse.
                Statement stmt = con.createStatement();
                PreparedStatement pstmt = con.prepareStatement("insert into " + tableName + " values (?, ?)");) {

            String dropSql = "if exists (select * from dbo.sysobjects where id = object_id(N'[dbo].[" + tableName + "]') and OBJECTPROPERTY(id, N'IsUserTable') = 1) DROP TABLE [" + tableName + "]";
            stmt.execute(dropSql);

            String createSql = "create table " + tableName + " (c1 int, c2 varchar(20))";
            stmt.execute(createSql);

            System.out.println("Starting batch operation using regular batch insert operation.");
            long start = System.currentTimeMillis();
            for (int i = 0; i < 1000; i++) {
                pstmt.setInt(1, i);
                pstmt.setString(2, "test" + i);
                pstmt.addBatch();
            }
            pstmt.executeBatch();

            long end = System.currentTimeMillis();

            System.out.println("Finished. Time taken : " + (end - start) + " milliseconds.");
        }

        try (Connection con = DriverManager.getConnection(azureDWconnectionUrl + ";useBulkCopyForBatchInsert=true"); // connects to an Azure Data Warehouse, with useBulkCopyForBatchInsert connection property set to true.
                Statement stmt = con.createStatement();
                PreparedStatement pstmt = con.prepareStatement("insert into " + tableNameBulkCopyAPI + " values (?, ?)");) {

            String dropSql = "if exists (select * from dbo.sysobjects where id = object_id(N'[dbo].[" + tableNameBulkCopyAPI + "]') and OBJECTPROPERTY(id, N'IsUserTable') = 1) DROP TABLE [" + tableNameBulkCopyAPI + "]";
            stmt.execute(dropSql);

            String createSql = "create table " + tableNameBulkCopyAPI + " (c1 int, c2 varchar(20))";
            stmt.execute(createSql);

            System.out.println("Starting batch operation using Bulk Copy API.");
            long start = System.currentTimeMillis();
            for (int i = 0; i < 1000; i++) {
                pstmt.setInt(1, i);
                pstmt.setString(2, "test" + i);
                pstmt.addBatch();
            }
            pstmt.executeBatch();

            long end = System.currentTimeMillis();

            System.out.println("Finished. Time taken : " + (end - start) + " milliseconds.");
        }
    }
```

结果：

```bash
Starting batch operation using regular batch insert operation.
Finished. Time taken : 104132 milliseconds.
Starting batch operation using Bulk Copy API.
Finished. Time taken : 1058 milliseconds.
```

## <a name="see-also"></a>另请参阅

[通过 JDBC 驱动程序提升性能和可靠性](improving-performance-and-reliability-with-the-jdbc-driver.md)

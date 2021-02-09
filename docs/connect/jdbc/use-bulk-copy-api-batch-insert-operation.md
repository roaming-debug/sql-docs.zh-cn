---
title: 用于在 JDBC 中执行批量插入的大容量复制 API
description: Microsoft JDBC Driver for SQL Server 支持使用大容量复制进行批量插入，以便更快地将数据加载到数据库中。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4a769d73f799b8ca0b4b806a3e656517377e23ad
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161564"
---
# <a name="using-bulk-copy-api-for-batch-insert-operation"></a>将大容量复制 API 用于批量插入操作

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Microsoft JDBC Driver for SQL Server 版本 9.2 及更高版本支持使用大容量复制 API 执行批量插入操作。 使用此功能，用户可以将驱动程序启用为在执行批量插入操作时，在底层执行大容量复制操作。 驱动程序旨在实现性能提升，同时插入驱动程序通过常规批量插入操作插入的相同数据。 驱动程序使用大容量复制 API（而不是常规批量插入操作）分析用户的 SQL 查询。 下面介绍了启用大容量复制 API 来执行批量插入功能的各种方法，并列出了此功能的限制。 此页还包含一个展示使用情况和性能提升的小型示例代码。

此功能仅适用于 PreparedStatement 和 CallableStatement 的 `executeBatch()` 和 `executeLargeBatch()` API。

## <a name="prerequisites"></a>先决条件

启用大容量复制 API 来执行批量插入功能的先决条件。

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

对于每个 PreparedStatement，useBulkCopyForBatchInsert  值在其初始化时保持不变。 对 SQLServerConnection.setUseBulkCopyForBatchInsert() 的任何后续调用都不会影响已创建的 PreparedStatement 值。

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

此示例展示了在常规和大容量复制 API 方案中，执行上千行的批量插入操作的用例。

```java
    public static void main(String[] args) throws Exception
    {
        String tableName = "batchTest";
        String tableNameBulkCopyAPI = "batchTestBulk";

        String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<database>;user=<user>;password=<password>";

        try (Connection con = DriverManager.getConnection(connectionUrl);
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

        try (Connection con = DriverManager.getConnection(connectionUrl + ";useBulkCopyForBatchInsert=true");
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

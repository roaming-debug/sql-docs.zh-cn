---
title: 使用用于 SQL Server 和 Azure SQL 的 Apache Spark 连接器
titleSuffix: SQL Server big data clusters
description: 了解如何使用适用于 SQL Server 和 Azure SQL 的 Apache Spark 连接器读取和写入 SQL Server。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: 29a660452841ee0057485c1a44c8ff30abd6f230
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045587"
---
# <a name="use-the-apache-spark-connector-for-sql-server-and-azure-sql"></a>使用用于 SQL Server 和 Azure SQL 的 Apache Spark 连接器

[用于 SQL Server 和 Azure SQL 的 Apache Spark 连接器](https://github.com/microsoft/sql-spark-connector)是一种高性能连接器，可便于在大数据分析中使用事务数据，并暂留结果以用于即席查询或报告。 借助此连接器，可以使用任何 SQL 数据库（无论是在本地，还是在云中）作为 Spark 作业的输入数据源或输出数据接收器。 此连接器使用 SQL Server 大容量写入 API。 任何批量写入参数都可由用户作为可选参数进行传递，并可由连接器按原样传递到基础 API。 若要详细了解大容量写入操作，请参阅[结合使用大容量复制与 JDBC 驱动程序]( ../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md#sqlserverbulkcopyoptions)。

默认情况下，此连接器包含在 SQL Server 大数据群集中。

若要详细了解此连接器，请参阅[开放源代码存储库](https://github.com/microsoft/sql-spark-connector)。 有关示例，请参阅[示例](https://github.com/microsoft/sql-spark-connector/tree/master/samples)。

## <a name="write-to-a-new-sql-table"></a>写入到新 SQL 表

>[!CAUTION]
> 在 `overwrite` 模式下，此连接器先删除默认情况下数据库中已存在的表。 请谨慎使用此选项，以免发生意外数据丢失。
> 
> 如果在使用模式 `overwrite` 时没有使用选项 `truncate`，则会在重新创建表后丢失索引。 例如，列存储表会成为堆。 若要保留现有索引，请同时指定值为 `true` 的选项 `truncate`。 例如 `.option("truncate",true)`

```python
server_name = "jdbc:sqlserver://{SERVER_ADDR}"
database_name = "database_name"
url = server_name + ";" + "databaseName=" + database_name + ";"

table_name = "table_name"
username = "username"
password = "password123!#" # Please specify password here

try:
  df.write \
    .format("com.microsoft.sqlserver.jdbc.spark") \
    .mode("overwrite") \
    .option("url", url) \
    .option("dbtable", table_name) \
    .option("user", username) \
    .option("password", password) \
    .save()
except ValueError as error :
    print("Connector write failed", error)
```

## <a name="append-to-sql-table"></a>追加到 SQL 表
```python
try:
  df.write \
    .format("com.microsoft.sqlserver.jdbc.spark") \
    .mode("append") \
    .option("url", url) \
    .option("dbtable", table_name) \
    .option("user", username) \
    .option("password", password) \
    .save()
except ValueError as error :
    print("Connector write failed", error)
```

## <a name="specify-the-isolation-level"></a>指定隔离级别

在对数据库执行大容量插入时，此连接器默认使用 READ_COMMITTED 隔离级别。 若要将此隔离级别替代为另一个隔离级别，请使用 `mssqlIsolationLevel` 选项，如下所示。
```python
    .option("mssqlIsolationLevel", "READ_UNCOMMITTED") \
```

## <a name="read-from-sql-table"></a>从 SQL 表中读取数据

```python
jdbcDF = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("user", username) \
        .option("password", password).load()
```

## <a name="non-active-directory-mode"></a>非 Active Directory 模式

在非 Active Directory 模式安全性中，每个用户都有一个用户名和密码，需要在连接器实例化期间将它们作为参数提供，以执行读取和/或写入。

下面是用于非 Active Directory 模式的连接器实例化示例。 在运行此脚本之前，请先将 `?` 替换为你帐户对应的值。

```python
# Note: '?' is a placeholder for a necessary user-specified value
connector_type = "com.microsoft.sqlserver.jdbc.spark" 

url = "jdbc:sqlserver://master-p-svc;databaseName=?;"
writer = df.write \ 
   .format(connector_type)\ 
   .mode("overwrite") 
   .option("url", url) \ 
   .option("user", ?) \ 
   .option("password",?) 
writer.save() 
```

## <a name="active-directory-mode"></a>Active Directory 模式

在 Active Directory 模式安全性中，在用户生成 keytab 文件后，用户需要在连接器实例化期间提供 `principal` 和 `keytab` 作为参数。

在此模式下，驱动程序将 keytab 文件加载到相应的执行器容器。 然后，执行器使用主体名称和 keytab 生成一个令牌，该令牌可用于创建用于读取/写入的 JDBC 连接器。

下面是用于 Active Directory 模式的连接器实例化示例。 在运行此脚本之前，请先将 `?` 替换为你帐户对应的值。

```python
# Note: '?' is a placeholder for a necessary user-specified value
connector_type = "com.microsoft.sqlserver.jdbc.spark"

url = "jdbc:sqlserver://master-p-svc;databaseName=?;integratedSecurity=true;authenticationScheme=JavaKerberos;" 
writer = df.write \ 
   .format(connector_type)\ 
   .mode("overwrite") 
   .option("url", url) \ 
   .option("principal", ?) \ 
   .option("keytab", ?)   

writer.save() 
```

## <a name="next-steps"></a>后续步骤

有关大数据群集的详细信息，请参阅[如何在 Kubernetes 部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md)

需要对 SQL Server 大数据群集提供反馈或功能建议？ [请在 SQL Server 大数据群集反馈上给我们留言](https://aka.ms/sql-server-bdc-feedback)。

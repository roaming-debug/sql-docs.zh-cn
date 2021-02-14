---
title: 用于 SQL Server 的 Apache Spark 连接器
description: 了解如何使用用于 SQL Server 的 Apache Spark 连接器。
ms.custom: ''
ms.date: 08/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: rajmera3
ms.author: raajmera
ms.reviewer: mikeray
ms.openlocfilehash: 15a4bdb66ea87f85337540adfec4b5e175b1c55b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058122"
---
# <a name="apache-spark-connector-sql-server--azure-sql"></a>Apache Spark 连接器：SQL Server 和 Azure SQL

用于 SQL Server 和 Azure SQL 的 Apache Spark 连接器是一种高性能连接器，可便于在大数据分析中使用事务数据，并暂留结果以用于即席查询或报告。 借助此连接器，可以使用任何 SQL 数据库（无论是在本地，还是在云中）作为 Spark 作业的输入数据源或输出数据接收器。

此库包含用于 SQL Server 和 Azure SQL 的 Apache Spark 连接器的源代码。

[Apache Spark](https://spark.apache.org/) 是用于大规模数据处理的统一分析引擎。

可以从 [Maven](https://search.maven.org/search?q=spark-mssql-connector) 获取连接器。 若要导入连接器，请使用坐标 `com.microsoft.azure:spark-mssql-connector:<latest version>`。 也可以从源构建连接器，或从 GitHub 的“发布”部分下载 jar。 有关连接器的最新信息，请参阅 [SQL Spark 连接器 GitHub 存储库](https://github.com/microsoft/sql-spark-connector)。

## <a name="supported-features"></a>支持的功能

* 支持所有 Spark 绑定（Scala、Python、R）
* 基本身份验证和 Active Directory (AD) 密钥选项卡支持
* 重新排序的 `dataframe` 写入支持
* 支持在 SQL Server 大数据群集中写入 SQL Server 单实例和数据池
* Sql Server 单实例的可靠连接器支持

| 组件                            | 支持的版本              |
|--------------------------------------|---------------------------------|
| Apache Spark                         | 2.4.5（不支持 Spark 3.0） |
| Scala                                | 2.11                            |
| Microsoft JDBC Driver for SQL Server | 8.2                             |
| Microsoft SQL Server                 | SQL Server 2008 或更高版本        |
| Azure SQL 数据库                  | 支持                       |

> [!NOTE]
> Azure Synapse Analytics 的使用未通过此连接器进行测试。 虽然可以工作，但可能会产生意外后果。

### <a name="supported-options"></a>支持的选项
用于 SQL Server 和 Azure SQL 的 Apache Spark 连接器支持此处定义的选项：[SQL DataSource JDBC](https://spark.apache.org/docs/latest/sql-data-sources-jdbc.html)

此外，还支持以下选项

| 选项 | 默认 | 说明 |
| --------- | ------------------ | ------------------------------------------ |
| `reliabilityLevel` | `BEST_EFFORT` | `BEST_EFFORT` 或 `NO_DUPLICATES`。 `NO_DUPLICATES` 在执行器重启方案中实现可靠插入 |
| `dataPoolDataSource` | `none` | `none` 表示未设置值，连接器应写入 SQL Server 单实例。 将此值设置为数据源名称，以在大数据群集中写入数据池表|
| `isolationLevel` | `READ_COMMITTED` | 指定隔离级别 |
| `tableLock` | `false` | 实现带有 `TABLOCK` 选项的插入以提高写入性能 |
| `schemaCheckEnabled` | `true` | 当设置为 false 时，禁用严格的数据帧和 SQL 表架构检查 |

其他[大容量复制选项](../jdbc/using-bulk-copy-with-the-jdbc-driver.md#sqlserverbulkcopyoptions)可以设置为 `dataframe` 上的选项，并将在写入时传递到 `bulkcopy` API

## <a name="performance-comparison"></a>性能比较

用于 SQL Server 和 Azure SQL 的 Apache Spark 连接器比用于写入 SQL Server 的通用 JDBC 连接器快 15 倍。 性能特征因类型、数据量、使用的选项而异，并可能显示运行时的变化。 以下性能结果是用 spark `dataframe` 中的 143.9M 行覆盖 SQL 表所用的时间。 spark `dataframe` 是通过读取使用 [spark TPCDS 基准](https://github.com/databricks/spark-sql-perf)生成的 `store_sales` HDFS 表构造的。 不包括 `store_sales` 到 `dataframe` 的读取时间。 结果是三次运行的平均时间。

| 连接器类型 | 选项 | 说明 |  写入时间 |
| --------- | ------------------ | -------------------------------------| ---------- |
| `JDBCConnector` | 默认 | 具有默认选项的通用 JDBC 连接器 |  1385 秒 |
| `sql-spark-connector` | `BEST_EFFORT` | 具有默认选项的最佳工作状态 `sql-spark-connector` |580 秒 |
| `sql-spark-connector` | `NO_DUPLICATES ` | 可靠 `sql-spark-connector` | 709 秒 |
| `sql-spark-connector` | `BEST_EFFORT` + tabLock=true | 启用表锁的最佳工作状态 `sql-spark-connector` | 72 秒 |
| `sql-spark-connector` | `NO_DUPLICATES ` + tabLock=true| 启用表锁的可靠 `sql-spark-connector`| 198 秒 |

配置

- Spark 配置：num_executors = 20、executor_memory = '1664 m'、executor_cores = 2
- 数据生成配置：scale_factor=50、partitioned_tables=true
- 数据文件 `store_sales` 包含行数 143,997,590

环境

- [SQL Server 大数据群集](../../big-data-cluster/release-notes-big-data-cluster.md) CU5
- `master` + 6 个节点
- 每个节点第 5 代服务器，512 GB Ram，每个节点 4 TB NVM，NIC 10 GB

## <a name="commonly-faced-issues"></a>常见问题

### `java.lang.NoClassDefFoundError: com/microsoft/aad/adal4j/AuthenticationException`

在 hadoop 环境中使用 mssql 驱动程序的较旧版本（现在包含在此连接器中）时，会出现此问题。 如果你使用的是旧版 Azure SQL 连接器，并且已将驱动程序手动安装到该群集上以确保 Azure Active Directory 兼容性，则需要删除这些驱动程序。

修复问题的步骤：

1. 如果使用的是通用 Hadoop 环境，请检查并删除 mssql jar：`rm $HADOOP_HOME/share/hadoop/yarn/lib/mssql-jdbc-6.2.1.jre7.jar`。 如果使用的是 Databricks，请添加全局或群集初始化脚本，以从 `/databricks/jars` 文件夹中删除旧版 mssql 驱动程序，或将此行添加到现有脚本：`rm /databricks/jars/*mssql*`
2. 添加 `adal4j` 和 `mssql` 包。 例如，可以使用 Maven，但任何方式都应有效。 

   > [!CAUTION]
   > 请勿以此方式安装 SQL spark 连接器。

1. 将驱动程序类添加到连接配置。 例如：

   ```csharp
   connectionProperties = {
     `Driver`: `com.microsoft.sqlserver.jdbc.SQLServerDriver`
   }`
   ```

有关详细信息和说明，请参阅 [https://github.com/microsoft/sql-spark-connector/issues/26](https://github.com/microsoft/sql-spark-connector/issues/26#issuecomment-672006339) 解决方案。

## <a name="get-started"></a>开始使用

用于 SQL Server 和 Azure SQL 的 Apache Spark 连接器基于 Spark DataSourceV1 API 和 SQL Server 批处理 API，并使用与内置 JDBC Spark-SQL 连接器相同的接口。 通过这种集成，你可以轻松集成连接器，并通过使用 `com.microsoft.sqlserver.jdbc.spark` 更新格式参数来迁移现有 Spark 作业。

若要在项目中包含连接器，请下载此存储库，并使用 SBT 生成 jar。

## <a name="write-to-a-new-sql-table"></a>写入到新 SQL 表

> [!WARNING]
> `overwrite` 模式先删除默认情况下数据库中已存在的表。 请谨慎使用此选项，以免发生意外数据丢失。

在使用 `overwrite` 模式时，如果在重新创建表时没有使用选项 `truncate`，则索引将会丢失。 ，列存储表现在为堆。 若要保留现有索引，请同时指定值为 true 的选项 `truncate`。 例如，`.option("truncate","true")`。

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

### <a name="append-to-sql-table"></a>追加到 SQL 表

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

在对数据库执行大容量插入时，此连接器默认使用 `READ_COMMITTED` 隔离级别。 若要替代此隔离级别，请使用 `mssqlIsolationLevel` 选项，如下所示。

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

## <a name="azure-active-directory-authentication"></a>Azure Active Directory 身份验证

### <a name="python-example-with-service-principal"></a>使用服务主体的 Python 示例

```python
context = adal.AuthenticationContext(authority)
token = context.acquire_token_with_client_credentials(resource_app_id_url, service_principal_id, service_principal_secret)
access_token = token["accessToken"]

jdbc_db = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("accessToken", access_token) \
        .option("encrypt", "true") \
        .option("hostNameInCertificate", "*.database.windows.net") \
        .load()
```

### <a name="python-example-with-active-directory-password"></a>使用 Active Directory 密码的 Python 示例

```python
jdbc_df = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("authentication", "ActiveDirectoryPassword") \
        .option("user", user_name) \
        .option("password", password) \
        .option("encrypt", "true") \
        .option("hostNameInCertificate", "*.database.windows.net") \
        .load()
```

必须安装必需的依赖项，才能使用 Active Directory 进行身份验证。

对于 Scala，将需要安装 `_com.microsoft.aad.adal4j_` 项目。

对于 Python，将需要安装 `_adal_` 库。  这可通过 pip 获得。

有关示例，请查看[示例笔记本](https://github.com/microsoft/sql-spark-connector/tree/master/samples)。

## <a name="support"></a>支持

用于 Azure SQL 和 SQL Server 的 Apache Spark 连接器是一个开源项目。 此连接器不提供任何 Microsoft 支持。 有关连接器的疑问或问题，请在此项目存储库中创建问题。 连接器社区处于活动状态，并对提交情况进行监视。

## <a name="next-steps"></a>后续步骤

请访问 [SQL Spark 连接器 GitHub 存储库](https://github.com/microsoft/sql-spark-connector)。

有关隔离级别的信息，请参阅 [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)。

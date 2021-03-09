---
title: 访问外部数据：MongoDB - PolyBase
description: 本文介绍如何使用 SQL Server 实例上的 PolyBase 来查询 MongoDB 中的外部数据。 创建外部表以引用外部数据。
ms.date: 03/05/2021
ms.metadata: seo-lt-2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: a9d975bf5a65ec8ece1aa2f3b1e957007046f4c8
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247514"
---
# <a name="configure-polybase-to-access-external-data-in-mongodb"></a>配置 PolyBase 以访问 MongoDB 中的外部数据

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本文介绍如何使用 SQL Server 实例上的 PolyBase 来查询 MongoDB 中的外部数据。

## <a name="prerequisites"></a>先决条件

如果尚未安装 PolyBase，请参阅 [PolyBase 安装](polybase-installation.md)。

在创建数据库范围凭据之前，必须先创建[主密钥](../../t-sql/statements/create-master-key-transact-sql.md)。 
    

## <a name="configure-a-mongodb-external-data-source"></a>配置 MongoDB 外部数据源

若要查询 MongoDB 数据源中的数据，必须创建外部表以引用外部数据。 本节提供用于创建这些外部表的示例代码。

此部分中使用了以下 Transact-SQL 命令：

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
- [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md) 
- [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)

1. 创建数据库范围凭据以访问 MongoDB 数据源。

   下面的脚本将创建数据库范围的凭据。 在运行脚本之前，请针对你的环境更新它：

    - 将 `<credential_name>` 替换为凭据的名称。
    - 将 `<username>` 替换为外部源的用户名。
    - 将 `<password>` 替换为适当的密码。 

    ```sql
    CREATE DATABASE SCOPED CREDENTIAL <credential_name> WITH IDENTITY = '<username>', Secret = '<password>';
    ```

   > [!IMPORTANT]
   > 用于 PolyBase 的 MongoDB ODBC 连接器仅支持基本身份验证，不支持 Kerberos 身份验证。

1. 创建外部数据源。

    以下脚本将创建外部数据源。 有关详细信息，请参阅 [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)。 在运行脚本之前，请针对你的环境更新它：

    - 更新位置。 为你的环境设置 `<server>` 和 `<port>`。
    - 将 `<credential_name>` 替换为在上一步中创建的凭据的名称。
    - （可选）如果想要指定外部源的下推计算，可以指定 `PUSHDOWN = ON` 或 `PUSHDOWN = OFF`。

    ```sql
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (LOCATION = '<mongodb://<server>[:<port>]>',
    -- PUSHDOWN = ON | OFF,
    CREDENTIAL = <credential_name>);
    ```

1. **可选：** 在外部表上创建统计信息。

    为了获得最佳查询性能，我们建议在外部表列上创建统计信息，尤其是用于联接、筛选和聚合的统计信息。

    ```sql
    CREATE STATISTICS statistics_name ON customer (C_CUSTKEY) WITH FULLSCAN; 
    ```

>[!IMPORTANT]
>创建外部数据源后，可以使用 [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md) 命令在该数据源上创建可查询的表。
>
>有关示例，请参阅[为 MongoDB 创建外部表](../../t-sql/statements/create-external-table-transact-sql.md#k-create-an-external-table-for-mongodb)。

## <a name="mongodb-connection-options"></a>MongoDB 连接选项

有关 MongoDB 连接选项的详细信息，请参阅 [MongoDB 文档：连接字符串 URI 格式](https://docs.mongodb.com/manual/reference/connection-string/#connection-string-options)。

## <a name="flattening"></a>平展

为 MongoDB 文档集合中的嵌套和重复数据启用平展。 要求用户启用 `create an external table` 并通过可能包含嵌套和/或重复数据的 MongoDB 文档集合显式指定关系架构。 JSON 嵌套/重复数据类型将按如下所示平展

* 对象：大括号括起来的无序键/值集合（嵌套）

   - SQL Server 为每个对象键创建表列

     * 列名称：objectname_keyname

* 数组：有序值，以逗号分隔，用方括号括起来（重复）

   - SQL Server 为每个数组项添加新表行

   - SQL Server 按每个数组创建一列，用于存储数组项索引

     * 列名称：arrayname_index

     * 数据类型：bigint

此技术存在几个潜在问题，其中包括两个问题：

* 空的重复字段将有效地屏蔽包含在相同记录的平面字段中的数据

* 存在多个重复字段可能导致生成的行数呈爆炸式增长

例如，SQL Server 评估以非关系 JSON 格式存储的 MongoDB 示例数据集餐馆集合。 每家餐馆都有一个嵌套的地址字段和按不同日期分配的一组等级。 下图显示了包含嵌套地址和嵌套重复等级的典型餐馆。

![MongoDB 平展](../../relational-databases/polybase/media/mongo-flattening.png "MongoDB 餐馆平展")

对象地址将按如下所示平展：

- 嵌套字段 `restaurant.address.building` 变为 `restaurant.address_building`
- 嵌套字段 `restaurant.address.coord` 变为 `restaurant.address_coord`
- 嵌套字段 `restaurant.address.street` 变为 `restaurant.address_street`
- 嵌套字段 `restaurant.address.zipcode` 变为 `restaurant.address_zipcode`

数组等级将按如下所示平展：

| grades_date | grades_grade  | games_score | 
| ------------- | ------------------------- | -------------- |
|1393804800000 |A |2|
|1378857600000|A |6|
|135898560000 |A |10|
|1322006400000|A |9|
|1299715200000 |B |14|

## <a name="cosmos-db-connection"></a>Cosmos DB 连接

使用 Cosmos DB Mongo API 和 Mongo DB PolyBase 连接器，可创建 Cosmos DB 实例的外部表。 可按照以上列出的相同步骤完成此操作。 确保数据库范围凭据、服务器地址、端口和位置字符串反映 Cosmos DB 服务器的相应内容。

## <a name="examples"></a>示例

下面的示例使用以下参数创建外部数据源：

| 参数 | 值|
|---|---|
| 名称 | `external_data_source_name`|
| 服务 | `mongodb0.example.com`|
| 实例 | `27017`|
| 副本集 | `myRepl`|
| TLS | `true`|
| 下推计算 | `On`|

```sql
CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (LOCATION = 'mongodb://mongodb0.example.com:27017',
    CONNECTION_OPTION = 'replicaSet=myRepl','tls=true',
    PUSHDOWN = ON ,
    CREDENTIAL = credential_name);
```

## <a name="next-steps"></a>后续步骤

若要了解有关 PolyBase 的详细信息，请参阅 [SQL Server PolyBase 的概述](polybase-guide.md)。

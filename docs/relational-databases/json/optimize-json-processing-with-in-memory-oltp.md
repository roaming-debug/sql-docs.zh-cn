---
description: 使用内存中 OLTP 优化 JSON 处理
title: 使用内存中 OLTP 优化 JSON 处理
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
ms.assetid: d9c5adb1-3209-4186-bc10-8e41a26f5e57
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1854ba39c5131484079cccfd3f38e04743254106
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483399"
---
# <a name="optimize-json-processing-with-in-memory-oltp"></a>使用内存中 OLTP 优化 JSON 处理
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2017-asdb.md)]

SQL Server 和 Azure SQL 数据库允许使用 JSON 格式的文本。 为了提高处理 JSON 数据的查询性能，可以使用标准字符串列（NVARCHAR 类型）将 JSON 文档存储在内存优化表中。 将 JSON 数据存储在内存优化表中可以利用无锁内存中数据访问来提高查询性能。

## <a name="store-json-in-memory-optimized-tables"></a>在内存优化表中存储 JSON
下面的示例演示了包含两个 JSON 列（`Tags` 和 `Data`）的 `Product` 内存优化表：

```sql
CREATE SCHEMA xtp;
GO
CREATE TABLE xtp.Product(
    ProductID int PRIMARY KEY NONCLUSTERED, --standard column
    Name nvarchar(400) NOT NULL, --standard column
    Price float, --standard column

    Tags nvarchar(400),--json stored in string column
    Data nvarchar(4000) --json stored in string column

) WITH (MEMORY_OPTIMIZED=ON);
```

## <a name="optimize-json-processing-with-additional-in-memory-features"></a>使用其他内存中功能优化 JSON 处理
使用 SQL Server 和 Azure SQL 数据库中的功能可将 JSON 功能与现有的内存中 OLTP 技术完全集成。 例如，可以执行以下操作：
 - 使用本机编译的 CHECK 约束[验证内存优化表中存储的 JSON 文档的结构](#validate)。
 - 使用计算列[公开和强类型化 JSON 文档中存储的值](#computedcol)。
 - 使用内存优化索引[为 JSON 文档中的值编制索引](#index)。
 - [本机编译使用 JSON 文档中的值或者将结果设置为 JSON 文本格式的 SQL 查询](#compile)。

## <a name="validate-json-columns"></a><a name="validate"></a>验证 JSON 列
SQL Server 和 Azure SQL 数据库允许添加本机编译的 CHECK 约束，用于验证字符串列中存储的 JSON 文档的内容。 使用本机编译的 JSON CHECK 约束，可以确保内存优化表中存储的 JSON 文本的格式正确。

下面的示例创建一个包含 JSON 列 `Tags` 的 `Product` 表。 `Tags` 列包含 CHECK 约束，其使用 `ISJSON` 函数验证列中的 JSON 文本。

```sql
DROP TABLE IF EXISTS xtp.Product;
GO
CREATE TABLE xtp.Product(
    ProductID int PRIMARY KEY NONCLUSTERED,
    Name nvarchar(400) NOT NULL,
    Price float,

    Tags nvarchar(400)
            CONSTRAINT [Tags should be formatted as JSON]
                CHECK (ISJSON(Tags)=1),
    Data nvarchar(4000)

) WITH (MEMORY_OPTIMIZED=ON);
```

也可将本机编译的 CHECK 约束添加到包含 JSON 列的现有表中。

```sql
ALTER TABLE xtp.Product
    ADD CONSTRAINT [Data should be JSON]
        CHECK (ISJSON(Data)=1)
```

## <a name="expose-json-values-using-computed-columns"></a><a name="computedcol"></a>使用计算列公开 JSON 值
使用计算列可以公开 JSON 文本中的值和访问这些值，而无需从 JSON 文本中重新提取值，且无需重新分析 JSON 结构。 使用此方式公开的值已强类型化并且以物理方式持久保存在计算列中。 使用持久化计算列访问 JSON 值的速度比直接访问 JSON 文档中的值要快。

下面的示例演示如何公开 JSON `Data` 列中的以下两个值：
-   制造产品的国家/地区。
-   产品制造成本。

在此示例中，每当 `Data` 列中存储的 JSON 文档发生更改时，计算列 `MadeIn` 和 `Cost` 就会更新。

```sql
DROP TABLE IF EXISTS xtp.Product;
GO
CREATE TABLE xtp.Product(
    ProductID int PRIMARY KEY NONCLUSTERED,
    Name nvarchar(400) NOT NULL,
    Price float,

    Data nvarchar(4000),

    MadeIn AS CAST(JSON_VALUE(Data, '$.MadeIn') as NVARCHAR(50)) PERSISTED,
    Cost   AS CAST(JSON_VALUE(Data, '$.ManufacturingCost') as float)

) WITH (MEMORY_OPTIMIZED=ON);
```

## <a name="index-values-in-json-columns"></a><a name="index"></a>为 JSON 列中的值编制索引
SQL Server 和 Azure SQL 数据库允许使用内存优化索引为 JSON 列中的值编制索引。 必须使用计算列公开和强类型化要编制索引的 JSON 值，如前面的示例中所述。

可以使用标准 NONCLUSTERED 和 HASH 索引为 JSON 列中的值编制索引。
-   NONCLUSTERED 索引可以优化按某个 JSON 值选择行范围或者按 JSON 值将结果排序的查询。
-   HASH 索引通过指定要查找的精确值来优化选择单列或多列的查询。

下面的示例使用两个计算列生成公开 JSON 值的表。 该示例将为一个 JSON 值创建 NONCLUSTERED 索引并为另一个 JSON 值创建 HASH 索引。

```sql
DROP TABLE IF EXISTS xtp.Product;
GO
CREATE TABLE xtp.Product(
    ProductID int PRIMARY KEY NONCLUSTERED,
    Name nvarchar(400) NOT NULL,
    Price float,

    Data nvarchar(4000),

    MadeIn AS CAST(JSON_VALUE(Data, '$.MadeIn') as NVARCHAR(50)) PERSISTED,
    Cost   AS CAST(JSON_VALUE(Data, '$.ManufacturingCost') as float) PERSISTED,

    INDEX [idx_Product_MadeIn] NONCLUSTERED (MadeIn)

) WITH (MEMORY_OPTIMIZED=ON)

ALTER TABLE Product
    ADD INDEX [idx_Product_Cost] NONCLUSTERED HASH(Cost)
        WITH (BUCKET_COUNT=20000)
```

## <a name="native-compilation-of-json-queries"></a><a name="compile"></a>本机编译 JSON 查询
如果过程、函数和触发器包含使用内置 JSON 函数的查询，本机编译可以提高这些查询的性能，并减少运行这些查询所需的 CPU 周期。

下面的示例演示使用多个 JSON 函数（JSON_VALUE、OPENJSON 和 JSON_MODIFY）的本机编译过程。

```sql
CREATE PROCEDURE xtp.ProductList(@ProductIds nvarchar(100))
WITH SCHEMABINDING, NATIVE_COMPILATION
AS BEGIN
    ATOMIC WITH (transaction isolation level = snapshot,  language = N'English')

    SELECT ProductID,Name,Price,Data,Tags, JSON_VALUE(data,'$.MadeIn') AS MadeIn
    FROM xtp.Product
        JOIN OPENJSON(@ProductIds)
            ON ProductID = value
END;

CREATE PROCEDURE xtp.UpdateProductData(@ProductId int, @Property nvarchar(100), @Value nvarchar(100))
WITH SCHEMABINDING, NATIVE_COMPILATION
AS BEGIN
    ATOMIC WITH (transaction isolation level = snapshot,  language = N'English')

    UPDATE xtp.Product
    SET Data = JSON_MODIFY(Data, @Property, @Value)
    WHERE ProductID = @ProductId;
END
```

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>详细了解 SQL Server 和 Azure SQL 数据库中的 JSON  
  
### <a name="microsoft-videos"></a>Microsoft 视频

有关 SQL Server 和 Azure SQL 数据库中内置 JSON 支持的视频介绍，请观看以下视频：

-   [SQL Server 2016 和 JSON 支持](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [在 SQL Server 2016 和 Azure SQL 数据库中使用 JSON](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON 充当 NoSQL 和关系环境之间的桥梁](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)

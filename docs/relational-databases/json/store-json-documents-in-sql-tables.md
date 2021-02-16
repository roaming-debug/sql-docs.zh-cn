---
title: 在 SQL Server 或 SQL 数据库中存储 JSON 文档
description: 本文介绍了在 SQL Server 或 SQL 数据库中存储 JSON 文档并编制其索引的原因和方法，以及如何优化对 JSON 文档的查询。
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 30d51162b36d787e8976b2c6aa48e533fd476f8e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015057"
---
# <a name="store-json-documents-in-sql-server-or-sql-database"></a>在 SQL Server 或 SQL 数据库中存储 JSON 文档
SQL Server 和 Azure SQL 数据库包含可使用标准 SQL 语言分析 JSON 文档的本机 JSON 函数。 可以将 JSON 文档存储在 SQL Server 或 SQL 数据库中，并像在 NoSQL 数据库中一样查询 JSON 数据。 本文介绍在 SQL Server 或 SQL 数据库中存储 JSON 文档的相关选项。

## <a name="json-storage-format"></a>JSON 存储格式

第一个存储设计决策是，如何在表中存储 JSON 文档。 有以下两个可用选项：
- **LOB 存储** - JSON 文档可按原样存储在 `NVARCHAR` 列中。 这是快速数据加载和引入的最佳方式，因为加载速度与字符串列的加载是匹配的。 如果没有为 JSON 值编制索引，这种方法可能会额外带来查询/分析时间方面的性能损失，因为必须在运行查询时分析原始 JSON 文档。 
- **关系存储** - 使用 `OPENJSON`、`JSON_VALUE` 或 `JSON_QUERY` 函数将 JSON 文档插入表中时，可以分析这些文档。 输入 JSON 文档中的片段可以存储在 SQL 数据类型列中，也可以存储在包含 JSON 子元素的 NVARCHAR 列中。 这种方法会延长加载时间，因为 JSON 分析是在加载过程中完成的；但查询与关系数据的经典查询的性能是匹配的。

## <a name="classic-tables"></a>经典表

在 SQL Server 或 SQL 数据库中存储 JSON 文档最简单的方法是创建一个只有两列的表，一列为文档 ID，一列为文档内容。 例如：

```sql
create table WebSite.Logs (
    _id bigint primary key identity,
    log nvarchar(max)
);
```

此结构等效于可在经典文档数据库中找到的集合。 主键 `_id` 是一个自动递增的值，该值为每个文档提供唯一标识符并可用于快速查找。 此结构最适合用于经典 NoSQL 方案，即通过 ID 检索文档或通过 ID 更新存储的文档。

nvarchar(max) 数据类型可存储最大 2GB 大小的 JSON 文档。 但是，如果确定 JSON 文档不超过 8KB，出于性能原因，建议使用 NVARCHAR(4000) 而不是 NVARCHAR(max)。

上例创建的示例表假定在 `log` 列中存储的 JSON 文档有效。 如需确保在 `log` 列中保存的 JSON 文档有效，可在该列添加 CHECK 约束。 例如：

```sql
ALTER TABLE WebSite.Logs
    ADD CONSTRAINT [Log record should be formatted as JSON]
                   CHECK (ISJSON(log)=1)
```

每次有人在该表中插入或更新文档时，此约束都会验证 JSON 文档的格式是否正确。 如果没有该约束，该表的插入功能会得到优化，因为任何 JSON 文档无需进行任何处理都可直接添加到该列。

在表中存储 JSON 文档时，可使用标准 Transact-SQL 语言查询文档。 例如：

```sql
SELECT TOP 100 JSON_VALUE(log, '$.severity'), AVG( CAST( JSON_VALUE(log,'$.duration') as float))
 FROM WebSite.Logs
 WHERE CAST( JSON_VALUE(log,'$.date') as datetime) > @datetime
 GROUP BY JSON_VALUE(log, '$.severity')
 HAVING AVG( CAST( JSON_VALUE(log,'$.duration') as float) ) > 100
 ORDER BY AVG( CAST( JSON_VALUE(log,'$.duration') as float) ) DESC
```

可使用任意 T-SQL 函数和查询子句来查询 JSON 文档。 SQL Server 和 SQL 数据库不会在可以用来分析 JSON 文档的查询中引入任意约束。 可通过 `JSON_VALUE` 函数从 JSON 文档中提取值，并在查询中使用该值，如同使用任何其他值一样。

能够使用大量 T-SQL 查询语法是 SQL Server 和 SQL 数据库与经典 NoSQL 数据库的主要区别 - 在 Transact-SQL 中，可能有需要处理 JSON 数据的任何函数。

## <a name="indexes"></a>索引

如果发现查询频繁按某一属性（例如 JSON 文档中的 `severity` 属性）搜索文档，则可在该属性上添加经典非聚集索引来加速查询。

可创建计算列，在指定的路径（即 `$.severity` 路径）上公开 JSON 列中的 JSON 值，并在此计算列上创建标准索引。 例如：

```sql
create table WebSite.Logs (
    _id bigint primary key identity,
    log nvarchar(max),

    severity AS JSON_VALUE(log, '$.severity'),
    index ix_severity (severity)
);
```

此示例中使用的计算列是非持久化列或虚拟列，不会向表添加额外空间。 索引 `ix_severity` 用它来提升查询性能，如下例所示：

```sql
SELECT log
FROM Website.Logs
WHERE JSON_VALUE(log, '$.severity') = 'P4'
```

此索引的一个重要特征是，它可用于识别排序规则。 如果原始 NVARCHAR 列包含 COLLATION 属性（例如，区分大小写或日语），则该索引按照与 NVARCHAR 列关联的语言规则或区分大小写规则进行组织。 如果正在开发适用于全球市场的应用程序且在处理 JSON 文档时需要使用自定义语言规则，那么此排序规则感知功能会非常有用。

## <a name="large-tables--columnstore-format"></a>大型表和列存储格式

如需在集合中包含大量 JSON 文档，建议在该集合上添加聚集列存储索引，如下例所示：

```sql
create sequence WebSite.LogID as bigint;
go
create table WebSite.Logs (
    _id bigint default(next value for WebSite.LogID),
    log nvarchar(max),

    INDEX cci CLUSTERED COLUMNSTORE
);
```

聚集列存储索引提供较高的数据压缩率（高达 25 倍），可以极大地减少存储空间需求、降低存储成本并提高工作负荷的 I/O 性能。 此外，由于聚集列存储索引针对表扫描和 JSON 文档上的分析进行了优化，因此这类索引最适合用于日志分析。

前面的示例使用序列对象向 `_id` 列分配值。 序列和标识均为 ID 列的有效选项。

## <a name="frequently-changing-documents--memory-optimized-tables"></a>频繁更改的文档和内存优化表

如果希望在集合中进行大量更新、插入和删除操作，可将 JSON 文档存储在内存优化表中。 内存优化 JSON 集合始终将数据保留在内存中，因此不会产生任何存储 I/O 开销。 此外，内存优化 JSON 集合是完全无锁的，这意味着文档上的操作不会锁定其他任何操作。

将经典集合转换为内存优化集合只需执行一个操作，即在表定义后指定 **with (memory_optimized=on)** 选项，如下例所示。 完成操作后，即可获得内存优化版的 JSON 集合。

```sql
create table WebSite.Logs (
  _id bigint identity primary key nonclustered,
  log nvarchar(4000)
) with (memory_optimized=on)
```

内存优化表最适合用于频繁更改的文档。 考虑内存优化表时，还要考虑性能。 如果可能，为内存优化集合中的 JSON 文档使用 NVARCHAR(4000) 而不是 NVARCHAR(max)，因为它可以极大地提升性能。

与经典表一样，可以使用计算列将索引添加到在内存优化表中公开的字段上。 例如：

```sql
create table WebSite.Logs (

  _id bigint identity primary key nonclustered,
  log nvarchar(4000),

  severity AS cast(JSON_VALUE(log, '$.severity') as tinyint) persisted,
  index ix_severity (severity)

) with (memory_optimized=on)
```

若要最大化性能，请将 JSON 值强制转换为用于保留属性值的最小可能类型。 在前面的示例中，使用的是 **tinyint**。

还可将更新 JSON 文档的 SQL 查询放置在存储过程中，以获得本机编译的优势。 例如：

```sql
CREATE PROCEDURE WebSite.UpdateData(@Id int, @Property nvarchar(100), @Value nvarchar(100))
WITH SCHEMABINDING, NATIVE_COMPILATION

AS BEGIN
    ATOMIC WITH (transaction isolation level = snapshot,  language = N'English')

    UPDATE WebSite.Logs
    SET log = JSON_MODIFY(log, @Property, @Value)
    WHERE _id = @Id;

END
```

此本机编译的过程生成查询并创建运行查询的 .DLL 代码。 本机编译的过程是查询和更新数据的一种更快的方法。

## <a name="conclusion"></a>结束语

可使用 SQL Server 和 SQL 数据库中的本机 JSON 函数处理 JSON 文档，如在 NoSQL 数据库中一样。 每种数据库（关系型或 NoSQL）在 JSON 数据处理方面都各有优缺点。 在 SQL Server 或 SQL 数据库中存储 JSON 文档的关键优势在于，可以获得完整的 SQL 语言支持。 可以使用丰富的 Transact-SQL 语言处理数据和配置各种存储选项（从列存储索引的高压缩比和快速分析到内存优化表的无锁处理方式）。 同时，还可获取成熟的安全性和国际化功能带来的优势，这些功能可在 NoSQL 方案中重复使用。 鉴于本文中所述的理由，建议考虑在 SQL Server 和 SQL 数据库中存储 JSON 文档。

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>详细了解 SQL Server 和 Azure SQL 数据库中的 JSON  
  
### <a name="microsoft-videos"></a>Microsoft 视频

有关 SQL Server 和 Azure SQL 数据库中内置 JSON 支持的视频介绍，请观看以下视频：

-   [SQL Server 2016 和 JSON 支持](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [在 SQL Server 2016 和 Azure SQL 数据库中使用 JSON](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON 充当 NoSQL 和关系环境之间的桥梁](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)

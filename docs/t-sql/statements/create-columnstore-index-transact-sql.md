---
description: CREATE COLUMNSTORE INDEX (Transact-SQL)
title: CREATE COLUMNSTORE INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_COLUMNSTORE_INDEX_TSQL
- COLUMNSTORE INDEX
- COLUMNSTORE_INDEX_TSQL
- CREATE CLUSTERED COLUMNSTORE INDEX
- COLUMNSTORE_TSQL
- CREATE NONCLUSTERED COLUMNSTORE INDEX
- CREATE_NONCLUSTERED_COLUMNSTORE_INDEX_TSQL
- CREATE COLUMNSTORE INDEX
- CREATE_CLUSTERED_COLUMNSTORE_INDEX_TSQL
- COLUMNSTORE
dev_langs:
- TSQL
helpviewer_keywords:
- index creation [SQL Server], columnstore indexes
- columnstore index, creating
- CREATE COLUMNSTORE INDEX statement
- CREATE INDEX statement
ms.assetid: 7e1793b3-5383-4e3d-8cef-027c0c8cb5b1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 57665811cd12b4c31effb82a91a722780a774874
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170449"
---
# <a name="create-columnstore-index-transact-sql"></a>CREATE COLUMNSTORE INDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

将行存储表转换为聚集列存储索引，或创建非聚集列存储索引。 使用列存储索引可对 OLTP 工作负载有效地运行实时运营分析，或提高数据仓库工作负载的数据压缩和查询性能。  
  
> [!NOTE]
> 从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 开始，可以将表创建为聚集列存储索引。   再也不需要先创建行存储表，然后将其转换为聚集列存储索引。  

> [!TIP]
> 有关索引设计指南的信息，请参阅 [SQL Server 索引设计指南](../../relational-databases/sql-server-index-design-guide.md)。

跳转到示例：  
-   [将行存储表转换为列存储的示例](../../t-sql/statements/create-columnstore-index-transact-sql.md#convert)  
-   [非聚集列存储索引示例](../../t-sql/statements/create-columnstore-index-transact-sql.md#nonclustered)  
  
跳转到方案：  
-   [用于实时运营分析的列存储索引](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)  
-   [用于数据仓库的列存储索引](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)  
  
了解详细信息：  
-   [列存储索引指南](../../relational-databases/indexes/columnstore-indexes-overview.md)  
-   [列存储索引功能摘要](../../relational-databases/indexes/columnstore-indexes-what-s-new.md)  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Create a clustered columnstore index on disk-based table.  
CREATE CLUSTERED COLUMNSTORE INDEX index_name  
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }  
    [ WITH ( < with_option> [ ,...n ] ) ]  
    [ ON <on_option> ] 
[ ; ]  
  
--Create a nonclustered columnstore index on a disk-based table.  
CREATE [NONCLUSTERED]  COLUMNSTORE INDEX index_name   
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }
        ( column  [ ,...n ] )  
    [ WHERE <filter_expression> [ AND <filter_expression> ] ]
    [ WITH ( < with_option> [ ,...n ] ) ]  
    [ ON <on_option> ]
[ ; ]  
  
<with_option> ::=  
      DROP_EXISTING = { ON | OFF } -- default is OFF  
    | MAXDOP = max_degree_of_parallelism 
    | ONLINE = { ON | OFF } 
    | COMPRESSION_DELAY  = { 0 | delay [ Minutes ] }  
    | DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }  
      [ ON PARTITIONS ( { partition_number_expression | range } [ ,...n ] ) ]  
  
<on_option>::=  
      partition_scheme_name ( column_name )
    | filegroup_name
    | "default"
  
<filter_expression> ::=  
      column_name IN ( constant [ ,...n ]  
    | column_name { IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !< } constant  
  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE CLUSTERED COLUMNSTORE INDEX index_name
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name } 
    [ORDER (column [,...n] ) ]  
    [ WITH ( DROP_EXISTING = { ON | OFF } ) ] --default is OFF  
[;]  

```
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

某些选项并非在所有数据库引擎版本中均可用。 下表显示了聚集列存储索引和非聚集列存储索引中引入选项时的版本：

|选项| CLUSTERED | NONCLUSTERED |
|---|---|---|
| COMPRESSION_DELAY | [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] | [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] |
| DATA_COMPRESSION | [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] | [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] | 
| ONLINE | [!INCLUDE[ssSQLv15_md](../../includes/sssqlv15-md.md)] | [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] |
| WHERE 子句 | 空值 | [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] |

所有选项在 Azure SQL 数据库中均可用。

### <a name="create-clustered-columnstore-index"></a>CREATE CLUSTERED COLUMNSTORE INDEX

创建一个聚集列存储索引，并按列压缩和存储其中的所有数据。 该索引包含表中的所有列，并且存储整个表。 如果现有表是堆或聚集索引，则该表会转换为聚集列存储索引。 如果该表已作为聚集列存储索引存储，则会删除并重新生成现有索引。  
  
index_name  
指定新索引的名称。  
  
如果该表已具有聚集列存储索引，则可以指定与现有索引相同的名称，也可以使用 DROP EXISTING 选项指定新名称。  
  
ON [database_name. [schema_name ] . | schema_name . ] *table_name*

指定要作为聚集列存储索引存储的由一部分、两部分或三部分构成的名称。 如果该表是堆或聚集索引，则会将其从行存储转换为列存储。 如果该表已经是列存储，则此语句会重新生成聚集列存储索引。 若要转换为有序聚集列存储索引，现有索引必须是聚集列存储索引。
  
#### <a name="with-options"></a>WITH 选项

##### <a name="drop_existing--off--on"></a>DROP_EXISTING = [OFF] | ON

   `DROP_EXISTING = ON` 指定删除现有的索引，并创建一个新的列存储索引。  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH (DROP_EXISTING = ON);
```
   DROP_EXISTING = OFF（默认值）要求索引名称与现有名称相同。 如果指定的索引名称已存在，则会出错。  
  
##### <a name="maxdop--max_degree_of_parallelism"></a>MAXDOP = max_degree_of_parallelism  
   在索引操作期间覆盖现有的最大并行度服务器配置。 使用 MAXDOP 可以限制在执行并行计划的过程中使用的处理器数量。 最大数量为 64 个处理器。  
  
   max_degree_of_parallelism 可为以下值  ：  
   - 1 - 取消生成并行计划。  
   - \>1 - 基于当前系统工作负载，将并行索引操作中使用的最大处理器数限制为指定数量或更少。 例如，当 MAXDOP = 4 时，使用的处理器数为 4 或更少。  
   - 0（默认值） - 根据当前系统工作负荷使用实际的处理器数量或更少数量的处理器。  
  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH (MAXDOP = 2);
```

   有关详细信息，请参阅[配置 max degree of parallelism 服务器配置选项](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)和[配置并行索引操作](../../relational-databases/indexes/configure-parallel-index-operations.md)。  
 
###### <a name="compression_delay--0--delay--minutes-"></a>COMPRESSION_DELAY = **0** | *delay* [ Minutes ]  
   对于基于磁盘的表，delay 指定处于关闭状态的增量行组在 SQL Server 可以将它压缩为压缩行组之前，必须保持为增量行组的最小分钟数  。 由于基于磁盘的表不对单个行跟踪插入和更新时间，因此 SQL Server 会将该延迟应用于处于关闭状态的增量行组。  
   默认为 0 分钟。  
   
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( COMPRESSION_DELAY = 10 Minutes );
```

   有关何时使用 COMPRESSION_DELAY 的建议，请参阅[开始使用列存储进行实时运行分析](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)。  
  
##### <a name="data_compression--columnstore--columnstore_archive"></a>DATA_COMPRESSION = COLUMNSTORE | COLUMNSTORE_ARCHIVE  
   为指定的表、分区号或分区范围指定数据压缩选项。 选项如下：   
- `COLUMNSTORE` 是默认值，它指定使用性能最高的列存储压缩进行压缩。 这是典型选择。  
- `COLUMNSTORE_ARCHIVE` 将表或分区进一步压缩为更小的大小。 可在许多情况下使用此选项，例如，用于要求存储更小并且可以付出更多时间来进行存储和检索的存档。  
  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( DATA_COMPRESSION = COLUMNSTORE_ARCHIVE );
```
   有关压缩的详细信息，请参阅[数据压缩](../../relational-databases/data-compression/data-compression.md)。  

###### <a name="online--on--off"></a>ONLINE = [ON | OFF]
- `ON` 指定列存储索引在生成新副本时保持联机并可用。
- `OFF` 指定索引在生成新副本时不可用。

```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( ONLINE = ON );
```

#### <a name="on-options"></a>ON 选项 
   使用 ON 选项，您可为数据存储指定选项，例如分区架构、特定的文件组或默认文件组。 如果未指定 ON 选项，索引会使用现有表的分区设置或文件组设置。  
  
   *partition_scheme_name* **(** _column_name_ **)**  
   指定表的分区方案。 分区方案必须已在数据库中存在。 若要创建分区方案，请参阅 [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md)。  
 
   *column_name* 指定对已分区索引进行分区所依据的列。 该列必须与 partition_scheme_name 使用的分区函数参数的数据类型、长度和精度相匹配  。  

   filegroup_name  
   指定用于存储聚集列存储索引的文件组。 如果未指定位置并且表未分区，则索引将与基础表或视图使用相同的文件组。 该文件组必须已存在。  

   "default"  
   若要对默认文件组创建索引，请使用 "default" 或 [ default ]。  
  
   如果指定了 "default"，则当前会话的 QUOTED_IDENTIFIER 选项必须为 ON。 QUOTED_IDENTIFIER 默认为 ON。 有关详细信息，请参阅 [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md)。  
  
### <a name="create-nonclustered-columnstore-index"></a>CREATE [NONCLUSTERED] COLUMNSTORE INDEX  
对存储为堆或聚集索引的行存储表创建内存中非聚集列存储索引。 该索引可以具有经过筛选的条件，并且不需要包含基础表的所有列。 列存储索引需要足够的空间来存储数据副本。 它是可更新的，在基础表发生更改时会进行更新。 聚集索引上的非聚集列存储索引可启用实时分析。  
  
index_name  
   指定索引的名称。 *index_name* 在表中必须唯一，但在数据库中不必唯一。 索引名称必须符合[标识符](../../relational-databases/databases/database-identifiers.md)的规则。  
  
 **(** _column_  [ **,** ...*n* ] **)**  
    指定要存储的列。 非聚集列存储索引限定为 1024 个列。  
   每个列都必须采用列存储索引支持的数据类型。 有关受支持数据类型的列表，请参阅[限制和局限](../../t-sql/statements/create-columnstore-index-transact-sql.md#LimitRest)。  

ON [database_name. [schema_name ] . | schema_name . ] *table_name*  
   指定包含该索引的由一部分、两部分或三部分名称组成的表。  

#### <a name="with-options"></a>WITH 选项
##### <a name="drop_existing--off--on"></a>DROP_EXISTING = [OFF] | ON  
   DROP_EXISTING = ON：删除并重新生成现有索引。 指定的索引名称必须与当前的现有索引相同；但可以修改索引定义。 例如，可以指定不同的列或索引选项。
  
   DROP_EXISTING = OFF：如果指定的索引名称已存在，则会显示一条错误。 使用 DROP_EXISTING 不能更改索引类型。 在向后兼容的语法中，WITH DROP_EXISTING 等效于 WITH DROP_EXISTING = ON。  

###### <a name="maxdop--max_degree_of_parallelism"></a>MAXDOP = max_degree_of_parallelism  
   在索引操作期间覆盖[配置 max degree of parallelism 服务器配置选项](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)配置选项。 使用 MAXDOP 可以限制在执行并行计划的过程中使用的处理器数量。 最大数量为 64 个处理器。  
  
   max_degree_of_parallelism 可为以下值  ：  
   - 1 - 取消生成并行计划。  
   - \>1 - 基于当前系统工作负载，将并行索引操作中使用的最大处理器数限制为指定数量或更少。 例如，当 MAXDOP = 4 时，使用的处理器数为 4 或更少。  
   - 0（默认值） - 根据当前系统工作负荷使用实际的处理器数量或更少数量的处理器。  
  
   有关详细信息，请参阅 [配置并行索引操作](../../relational-databases/indexes/configure-parallel-index-operations.md)。  
  
> [!NOTE]
>  并非在 [!INCLUDE[msC](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的每个版本中均支持并行索引操作。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 各版本支持的功能列表，请参阅 [SQL Server 2016 的版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)。  
  
###### <a name="online--on--off"></a>ONLINE = [ON | OFF]   
- `ON` 指定列存储索引在生成新副本时保持联机并可用。
- `OFF` 指定索引在生成新副本时不可用。 在非聚集索引中，基表仍然可用，只不过非聚集列存储索引在新索引完成前不能用于满足查询。 

```sql
CREATE COLUMNSTORE INDEX ncci ON Sales.OrderLines (StockItemID, Quantity, UnitPrice, TaxRate) WITH ( ONLINE = ON );
```

##### <a name="compression_delay--0--delayminutes"></a>COMPRESSION_DELAY = 0 | \<delay>[Minutes]  
   指定某一行在适合迁移到压缩行组之前，应在增量行组中保留的时间下限。 例如，客户可以说，如果某一行在 120 分钟内保持不变，则可以将其压缩为列存储格式。 对于基于磁盘的表中的列存储索引，我们不跟踪行的插入或更新时间，而是使用增量行组关闭时间作为行代理。 默认持续时间为 0 分钟。 一旦增量行组中累积了 100 万行，并且该行组标记为已关闭，就会将行迁移到列存储。  
  
###### <a name="data_compression"></a>DATA_COMPRESSION  
   为指定的表、分区号或分区范围指定数据压缩选项。 仅适用于列存储索引，包括非聚集列存储索引和聚集列存储索引。 选项如下：
   
- `COLUMNSTORE` - 默认值，它指定使用性能最高的列存储压缩进行压缩。 这是典型选择。  
- `COLUMNSTORE_ARCHIVE` - COLUMNSTORE_ARCHIVE 将表或分区进一步压缩为更小的大小。 这可用于存档，或者用于要求更小存储大小并且可以付出更多时间来进行存储和检索的其他情形。  
  
 有关压缩的详细信息，请参阅[数据压缩](../../relational-databases/data-compression/data-compression.md)。  
  
##### <a name="where-filter_expression--and-filter_expression-"></a>WHERE \<filter_expression> [ AND \<filter_expression> ]
  
   调用一个筛选器谓词，它指定哪些行包含在索引中。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对筛选索引中的数据行创建筛选统计信息。  
  
   该筛选器谓词使用简单的比较逻辑。 比较运算符不允许使用 NULL 文本的比较。 请改用 IS NULL 和 IS NOT NULL 运算符。  
  
   下面是一些 `Production.BillOfMaterials` 表筛选谓词示例：  
   `WHERE StartDate > '20000101' AND EndDate <= '20000630'`    
   `WHERE ComponentID IN (533, 324, 753)`  
   `WHERE StartDate IN ('20000404', '20000905') AND EndDate IS NOT NULL`  
   
   有关筛选索引的指南，请参阅[创建筛选索引](../../relational-databases/indexes/create-filtered-indexes.md)。  
  
#### <a name="on-options"></a>ON 选项  
   这些选项指定创建该索引时所在的文件组。  
  
*partition_scheme_name* **(** _column_name_ **)**  
   指定分区方案，该方案定义要将已分区索引的分区映射到的文件组。 必须通过执行 [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) 使数据库中存在该分区方案。 
   *column_name* 指定对已分区索引进行分区所依据的列。 该列必须与 partition_scheme_name 使用的分区函数参数的数据类型、长度和精度相匹配  。 column_name 不限于索引定义中的列  。 在对列存储索引进行分区时，如果尚未指定分区依据列，则[!INCLUDE[ssDE](../../includes/ssde-md.md)]会添加分区依据列作为索引列。  
   如果未指定 partition_scheme_name 或 filegroup 且该表已分区，则索引会与基础表使用相同分区依据列并被放入同一分区方案中   。  
   分区表的列存储索引必须实现分区对齐。  
   有关分区索引的详细信息，请参阅[已分区表和已分区索引](../../relational-databases/partitions/partitioned-tables-and-indexes.md)。  

filegroup_name  
   指定要对其创建索引的文件组名称。 如果未指定 *filegroup_name* 并且该表未分区，则索引与基础表使用相同的文件组。 该文件组必须已存在。  
 
"default"  
为默认文件组创建指定索引。  
  
在此上下文中，“default”一词不是关键字。 而是默认文件组的标识符，并且必须进行分隔，如 ON "default" 或 ON [default] 中所示     。 如果指定了 "default"，则当前会话的 QUOTED_IDENTIFIER 选项必须为 ON。 这是默认设置。 有关详细信息，请参阅 [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md)。  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对表的 ALTER 权限。  
  
##  <a name="general-remarks"></a><a name="GenRemarks"></a> 一般备注  
可以为临时表创建列存储索引。 在删除表或结束会话时，也将删除索引。  

可以在 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 支持的任何数据类型的列（字符串列除外）上创建有序的聚集列存储索引。  
 
## <a name="filtered-indexes"></a>筛选索引  
筛选索引是一种经过优化的非聚集索引，适用于从表中选择少数行的查询。 筛选索引使用筛选谓词对表中的部分数据进行索引。 设计良好的筛选索引可以提高查询性能，降低存储成本和维护成本。  
  
### <a name="required-set-options-for-filtered-indexes"></a>筛选索引所需的 SET 选项  
如果下列任何条件成立，则需要“必需的值”列中的 SET 选项：  
- 创建筛选索引。  
- INSERT、UPDATE、DELETE 或 MERGE 操作修改筛选索引中的数据。  
- 查询优化器使用该筛选索引生成查询计划。  
  
    |SET 选项|所需的值|默认服务器值|默认<br /><br /> OLE DB 和 ODBC 值|默认<br /><br /> DB-Library 值|  
    |-----------------|--------------------|--------------------------|---------------------------------------|-----------------------------------|  
    |ANSI_NULLS|ON|ON|ON|OFF|  
    |ANSI_PADDING|ON|ON|ON|OFF|  
    |ANSI_WARNINGS*|ON|ON|ON|OFF|  
    |ARITHABORT|ON|ON|OFF|OFF|  
    |CONCAT_NULL_YIELDS_NULL|ON|ON|ON|OFF|  
    |NUMERIC_ROUNDABORT|OFF|OFF|OFF|OFF|  
    |QUOTED_IDENTIFIER|ON|ON|ON|OFF|   
  
     *当数据库兼容性级别设置为 90 或更高时，如果将 ANSI_WARNINGS 设置为 ON，则将使 ARITHABORT 隐式设置为 ON。 如果数据库兼容性级别设置为 80 或更低，则必须将 ARITHABORT 选项显式设置为 ON。  
  
 如果 SET 选项不正确，则可能会出现以下情况：  
  
-   不会创建筛选索引。  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)]生成错误并回滚对索引中的数据进行更改的 INSERT、UPDATE、DELETE 或 MERGE 语句。  
  
-   查询优化器不考虑任何 Transact-SQL 语句的执行计划中的索引。  
  
 有关筛选索引的详细信息，请参阅[创建筛选索引](../../relational-databases/indexes/create-filtered-indexes.md)。 
  
##  <a name="limitations-and-restrictions"></a><a name="LimitRest"></a> 限制和局限  

**列存储索引中的每一列都必须是以下常见业务数据类型之一：** 
-   datetimeoffset [ ( n ) ]  
-   datetime2 [ ( n ) ]  
-   datetime  
-   smalldatetime  
-   date  
-   time [ ( n ) ]  
-   float [ ( n ) ]  
-   real [ ( n ) ]  
-   decimal [ ( *precision* [ *, scale* ] **)** ]
-   numeric [ ( *precision* [ *, scale* ] **)** ]    
-   money  
-   smallmoney  
-   bigint  
-   int  
-   smallint  
-   tinyint  
-   bit  
-   nvarchar [ ( n ) ] 
-   nvarchar(max) （适用于 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 和高级层、标准层（S3 及更高），以及所有 VCore 产品/服务层，仅限聚集列存储索引）   
-   nchar [ ( n ) ]  
-   varchar [ ( n ) ]  
-   varchar(max) （适用于 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 和高级层、标准层（S3 及更高），以及所有 VCore 产品/服务层，仅限聚集列存储索引）
-   char [ ( n ) ]  
-   varbinary [ ( n ) ] 
-   varbinary(max) （适用于 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 和高级层、标准层（S3 及更高）的 Azure SQL 数据库，以及所有 VCore 产品/服务层，仅限聚集列存储索引）
-   binary [ ( n ) ]  
-   uniqueidentifier（适用于 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本）
  
如果基础表中包含的某一列的数据类型不受列存储索引支持，则必须从非聚集列存储索引中省略该列。  
  
**使用以下任何数据类型的列都不能包括在列存储索引中：**
-   ntext、text 和 image  
-   nvarchar(max)、varchar(max) 和 varbinary(max)（适用于 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 和早期版本，以及非聚集列存储索引） 
-   rowversion（和 timestamp）  
-   sql_variant  
-   CLR 类型（hierarchyid 和空间类型）  
-   xml  
-   uniqueidentifier（适用于 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]）  

**非聚集列存储索引：**
-   包含的列数不能超过 1024。
-   无法创建为基于约束的索引。 对于具有列存储索引的表，可以具有唯一约束、主键约束和外键约束。 总是通过行存储索引强制执行约束。 无法使用列存储（群集或非群集）索引强制执行约束。
-   不能包含稀疏列。  
-   不能使用 **ALTER INDEX** 语句进行更改。 若要更改非聚集索引，必须先删除该列存储索引，然后重新创建它。 可以使用 **ALTER INDEX** 禁用并重新生成列存储索引。  
-   不能使用 **INCLUDE** 关键字创建。  
-   不能包括用来对索引排序的 **ASC** 或 **DESC** 关键字。 根据压缩算法对列存储索引排序。 排序将抵销许多性能优势。  
-   不能在非聚集列存储索引中包含 nvarchar(max)、varchar(max) 和 varbinary(max) 类型的大型对象 (LOB) 列。 仅从 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 版本开始以及在高级层、标准层（S3 及更高）以及所有 VCore 产品/服务层配置的 Azure SQL 数据库中，聚集列存储索引才支持 LOB 类型。 请注意，以前的版本不管是在聚集列存储索引还是非聚集列存储索引中都不支持 LOB 类型。


> [!NOTE]  
> 从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 开始，可以在索引视图上创建非聚集列存储索引。  


 **列存储索引不能与以下功能结合使用：**  
-   计算列。 从 SQL Server 2017 开始，聚集列存储索引可以包含非持久化计算列。 但是，在 SQL Server 2017 中，聚集列存储索引不能包含持久化计算列，并且你不能对计算列创建非聚集索引。 
-   页和行压缩以及 **vardecimal** 存储格式（列存储索引已采用不同格式压缩）。  
-   复制  
-   文件流

不能在具有聚集列存储索引的表中使用游标或触发器。 此限制不适用于非聚集列存储索引；可以在具有非聚集列存储索引的表中使用游标和触发器。

**[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 特定限制**  
这些限制仅适用于 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]。 在此版本中，我们引入了可更新的聚集列存储索引。 非聚集列存储索引仍为只读。  

-   更改跟踪。 不能将更改跟踪与列存储索引配合使用。  
-   变更数据捕获。 不能对具有聚集列存储索引的表启用变更数据捕获。 从 SQL Server 2016 开始，可以对具有非聚集列存储索引的表启用此功能。  
-   可读辅助副本。 不能通过 Always OnReadable 可用性组的可读辅助副本访问聚集列存储索引 (CCI)。  可以通过可读辅助副本访问非聚集列存储索引 (NCCI)。  
-   多重活动结果集 (MARS)。 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 使用 MARS 对包含列存储索引的表执行只读连接。 不过，[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 不支持使用 MARS 对包含列存储索引的表执行并发数据操作语言 (DML) 操作。 如果发生这种情况，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会终止连接，并中止事务。  
-  无法在视图或索引视图上创建非聚集列存储索引。
  
 有关列存储索引的性能优势和限制的信息，请参阅[列存储索引概述](../../relational-databases/indexes/columnstore-indexes-overview.md)。
  
##  <a name="metadata"></a><a name="Metadata"></a> 元数据  
 列存储索引中的所有列在元数据中作为包含性列存储。 列存储索引中没有任何键列。 这些系统视图提供有关列存储索引的信息。  
  
-   [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
-   [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
-   [sys.partitions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)  
-   [sys.column_store_segments (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
-   [sys.column_store_dictionaries (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)  
-   [sys.column_store_row_groups (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)  

##  <a name="examples-for-converting-a-rowstore-table-to-columnstore"></a><a name="convert"></a> 将行存储表转换为列存储的示例  
  
### <a name="a-convert-a-heap-to-a-clustered-columnstore-index"></a>A. 将堆转换为聚集列存储索引  
 此示例将一个表作为堆创建，然后将其转换为名为 cci_Simple 的聚集列存储索引。 这会将整个表的存储从行存储转换为列存储。  
  
```sql  
CREATE TABLE SimpleTable(  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED COLUMNSTORE INDEX cci_Simple ON SimpleTable;  
GO  
```  
  
### <a name="b-convert-a-clustered-index-to-a-clustered-columnstore-index-with-the-same-name"></a>B. 将聚集索引转换为具有相同名称的聚集列存储索引。  
 此示例创建一个具有聚集索引的表，然后演示将该聚集索引转换为聚集列存储索引的语法。 这会将整个表的存储从行存储转换为列存储。  
  
```sql  
CREATE TABLE SimpleTable (  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED INDEX cl_simple ON SimpleTable (ProductKey);  
GO  
CREATE CLUSTERED COLUMNSTORE INDEX cl_simple ON SimpleTable  
WITH (DROP_EXISTING = ON);  
GO  
```  
  
### <a name="c-handle-nonclustered-indexes-when-converting-a-rowstore-table-to-a-columnstore-index"></a>C. 将行存储表转换为列存储索引时处理非聚集索引。  
 此示例展示如何在将行存储表转换为列存储索引时处理非聚集索引。 实际上，从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 开始，无需执行特别的操作；[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会在新的聚集列存储索引上自动定义并重新生成非聚集索引。  
  
 如果要删除非聚集索引，请在创建列存储索引之前使用 DROP INDEX 语句。 DROP EXISTING 选项仅删除正在转换的聚集索引。 它不会删除非聚集索引。  
  
 在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 中，无法在列存储索引上创建非聚集索引。 此示例展示在早期版本中，如何在创建列存储索引之前删除非聚集索引。  
  
```sql  
--Create the table for use with this example.  
CREATE TABLE SimpleTable (  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
  
--Create two nonclustered indexes for use with this example  
CREATE INDEX nc1_simple ON SimpleTable (OrderDateKey);  
CREATE INDEX nc2_simple ON SimpleTable (DueDateKey);   
GO  
  
--SQL Server 2012 and SQL Server 2014: you need to drop the nonclustered indexes  
--in order to create the columnstore index.   
  
DROP INDEX SimpleTable.nc1_simple;  
DROP INDEX SimpleTable.nc2_simple;  
  
--Convert the rowstore table to a columnstore index.  
CREATE CLUSTERED COLUMNSTORE INDEX cci_simple ON SimpleTable;   
GO  
```  
  
### <a name="d-convert-a-large-fact-table-from-rowstore-to-columnstore"></a>D. 将大型事实表从行存储转换为列存储方式  
 此示例说明如何将大型事实表从行存储表转换为列存储表。  
  
 将行存储表转换为列存储表  
  
1.  首先，创建要在此示例中使用的一个较小的表。  
  
    ```sql  
    --Create a rowstore table with a clustered index and a nonclustered index.  
    CREATE TABLE MyFactTable (  
        ProductKey [INT] NOT NULL,  
        OrderDateKey [INT] NOT NULL,  
         DueDateKey [INT] NOT NULL,  
         ShipDateKey [INT] NOT NULL )  
    )  
    WITH (  
        CLUSTERED INDEX ( ProductKey )  
    );  
  
    --Add a nonclustered index.  
    CREATE INDEX my_index ON MyFactTable ( ProductKey, OrderDateKey );  
    ```  
  
2.  从行存储表中删除所有非聚集索引。  
  
    ```sql  
    --Drop all nonclustered indexes  
    DROP INDEX my_index ON MyFactTable;  
    ```  
  
3.  删除聚集索引。  
  
    -   仅在您要在将它转换为聚集列存储索引时指定索引的新名称的情况下才这样做。 如果不删除聚集索引，新的聚集列存储索引将具有相同的名称。  
  
        > [!NOTE]  
        > 如果您使用自己的名称，索引的名称可能更易于记住。 所有行存储格式的聚集索引均使用以下默认名称：“ClusteredIndex_\<GUID>”。  
  
    ```sql  
    --Process for dropping a clustered index.  
    --First, look up the name of the clustered rowstore index.  
    --Clustered rowstore indexes always use the DEFAULT name 'ClusteredIndex_<GUID>'.  
    SELECT i.name   
    FROM sys.indexes i   
    JOIN sys.tables t  
    ON ( i.type_desc = 'CLUSTERED' ) WHERE t.name = 'MyFactTable';  
  
    --Drop the clustered rowstore index.  
    DROP INDEX ClusteredIndex_d473567f7ea04d7aafcac5364c241e09 ON MyDimTable;  
    ```  
  
4.  将行存储表转换为具有聚集列存储索引的列存储表。  
  
    ```sql  
    --Option 1: Convert to columnstore and name the new clustered columnstore index MyCCI.  
    CREATE CLUSTERED COLUMNSTORE INDEX MyCCI ON MyFactTable;  
  
    --Option 2: Convert to columnstore and use the rowstore clustered   
    --index name for the columnstore clustered index name.  
    --First, look up the name of the clustered rowstore index.  
    SELECT i.name   
    FROM sys.indexes i  
    JOIN sys.tables t   
    ON ( i.type_desc = 'CLUSTERED' )  
    WHERE t.name = 'MyFactTable';  
  
    --Second, create the clustered columnstore index and   
    --Replace ClusteredIndex_d473567f7ea04d7aafcac5364c241e09  
    --with the name of your clustered index.  
    CREATE CLUSTERED COLUMNSTORE INDEX   
    ClusteredIndex_d473567f7ea04d7aafcac5364c241e09  
     ON MyFactTable  
    WITH DROP_EXISTING = ON;  
    ```  
  
### <a name="e-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index"></a>E. 将列存储表转换为具有聚集索引的行存储表  
 要将列存储表转换为具有聚集索引的行存储表，请使用带 DROP_EXISTING 选项的 CREATE INDEX 语句。  
  
```sql  
CREATE CLUSTERED INDEX ci_MyTable   
ON MyFactTable  
WITH ( DROP EXISTING = ON );  
```  
  
### <a name="f-convert-a-columnstore-table-to-a-rowstore-heap"></a>F. 将列存储表转换为行存储堆  
 要将列存储表转换为行存储堆，只需删除聚集列存储索引即可。  
  
```sql  
DROP INDEX MyCCI   
ON MyFactTable;  
```  
  

### <a name="g-defragment-by-rebuilding-the-entire-clustered-columnstore-index"></a>G. 重新生成整个聚集列存储索引以进行碎片整理  
   适用范围：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
 有两种方法可以重新生成完整的聚集列存储索引。 可以使用 CREATE CLUSTERED COLUMNSTORE INDEX，或使用 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md) 和 REBUILD 选项。 这两种方法可以得到相同的结果。  
  
> [!NOTE]  
> 自 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 起，改用 `ALTER INDEX...REORGANIZE`，而不使用此示例中所述的方法来重新生成。  
  
```sql  
--Determine the Clustered Columnstore Index name of MyDimTable.  
SELECT i.object_id, i.name, t.object_id, t.name   
FROM sys.indexes i   
JOIN sys.tables t  
ON (i.type_desc = 'CLUSTERED COLUMNSTORE')  
WHERE t.name = 'RowstoreDimTable';  
  
--Rebuild the entire index by using CREATE CLUSTERED INDEX.  
CREATE CLUSTERED COLUMNSTORE INDEX my_CCI   
ON MyFactTable  
WITH ( DROP_EXISTING = ON );  
  
--Rebuild the entire index by using ALTER INDEX and the REBUILD option.  
ALTER INDEX my_CCI  
ON MyFactTable  
REBUILD PARTITION = ALL  
WITH ( DROP_EXISTING = ON );  
```  
  
##  <a name="examples-for-nonclustered-columnstore-indexes"></a><a name="nonclustered"></a> 非聚集列存储索引示例  
  
### <a name="a-create-a-columnstore-index-as-a-secondary-index-on-a-rowstore-table"></a>A. 对行存储表创建列存储索引作为辅助索引  
 此示例会对行存储表创建非聚集列存储索引。 在这种情况下只能创建一个列存储索引。 列存储索引需要额外的存储空间，因为它包含行存储表中数据的副本。 此示例会创建一个简单表和聚集索引，然后演示创建非聚集列存储索引的语法。  
  
```sql  
CREATE TABLE SimpleTable  
(ProductKey [INT] NOT NULL,   
OrderDateKey [INT] NOT NULL,   
DueDateKey [INT] NOT NULL,   
ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED INDEX cl_simple ON SimpleTable (ProductKey);  
GO  
CREATE NONCLUSTERED COLUMNSTORE INDEX csindx_simple  
ON SimpleTable  
(OrderDateKey, DueDateKey, ShipDateKey);  
GO  
```  
  
### <a name="b-create-a-simple-nonclustered-columnstore-index-using-all-options"></a>B. 使用所有选项创建简单的非聚集列存储索引  
 下面的示例演示了通过使用所有选项创建非聚集列存储索引的语法。  
  
```sql  
CREATE NONCLUSTERED COLUMNSTORE INDEX csindx_simple  
ON SimpleTable  
(OrderDateKey, DueDateKey, ShipDateKey)  
WITH (DROP_EXISTING =  ON,   
    MAXDOP = 2)  
ON "default"  
GO  
```  
  
 有关使用已分区表的更复杂示例，请参阅[列存储索引概述](../../relational-databases/indexes/columnstore-indexes-overview.md)。  
  
### <a name="c-create-a-nonclustered-columnstore-index-with-a-filtered-predicate"></a>C. 使用筛选谓词创建非聚集列存储索引  
 以下示例会对 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的 Production.BillOfMaterials 表创建已筛选的非聚集列存储索引。 筛选谓词可包含那些不是筛选索引中的键列的列。 本示例中的谓词将仅选择其中的 EndDate 为非 NULL 的行。  
  
```sql  
IF EXISTS (SELECT name FROM sys.indexes  
    WHERE name = N'FIBillOfMaterialsWithEndDate'   
    AND object_id = OBJECT_ID(N'Production.BillOfMaterials'))  
DROP INDEX FIBillOfMaterialsWithEndDate  
    ON Production.BillOfMaterials;  
GO  
CREATE NONCLUSTERED COLUMNSTORE INDEX "FIBillOfMaterialsWithEndDate"  
    ON Production.BillOfMaterials (ComponentID, StartDate)  
    WHERE EndDate IS NOT NULL;  
```  
  
###  <a name="d-change-the-data-in-a-nonclustered-columnstore-index"></a><a name="ncDML"></a> D. 更改非聚集列存储索引中的数据  
   适用范围：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]。
  
 在您在表上创建非聚集列存储索引后，不能直接在该表中修改数据。 具有 INSERT、UPDATE、DELETE 或 MERGE 的查询会失败并且返回错误消息。 若要添加或修改表中的数据，可以执行以下操作之一：  
  
-   禁用或删除列存储索引。 然后可以更新表中的数据。 如果禁用列存储索引，则可以在完成数据更新后重新生成列存储索引。 例如，  
  
    ```sql  
    ALTER INDEX mycolumnstoreindex ON mytable DISABLE;  
    -- update mytable --  
    ALTER INDEX mycolumnstoreindex on mytable REBUILD  
    ```  
  
-   将数据加载到不含列存储索引的临时表中。 在临时表上生成列存储索引。 将临时表切换到主表的一个空分区中。  
  
-   将分区从具有列存储索引的表切换到一个空的临时表中。 如果在临时表上有某个列存储索引，则禁用该列存储索引。 执行任何更新。 生成（或重新生成）列存储索引。 将临时表切换回主表的（现在为空的）分区中。  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-change-a-clustered-index-to-a-clustered-columnstore-index"></a>A. 将聚集索引更改为聚集列存储索引  
 通过使用 DROP_EXISTING = ON 的 CREATE CLUSTERED COLUMNSTORE INDEX 语句，可以：  
  
-   将聚集索引更改为聚集列存储索引。  
  
-   重新生成聚集列存储索引。  
  
 此示例将 xDimProduct 表创建为具有聚集索引的行存储表，然后使用 CREATE CLUSTERED COLUMNSTORE INDEX 将该表从行存储表更改为列存储表。  
  
```sql  
-- Uses AdventureWorks  
  
IF EXISTS (SELECT name FROM sys.tables  
    WHERE name = N'xDimProduct'  
    AND object_id = OBJECT_ID (N'xDimProduct'))  
DROP TABLE xDimProduct;  
  
--Create a distributed table with a clustered index.  
CREATE TABLE xDimProduct (ProductKey, ProductAlternateKey, ProductSubcategoryKey)  
WITH ( DISTRIBUTION = HASH(ProductKey),  
    CLUSTERED INDEX (ProductKey) )  
AS SELECT ProductKey, ProductAlternateKey, ProductSubcategoryKey FROM DimProduct;  
  
--Change the existing clustered index   
--to a clustered columnstore index with the same name.  
--Look up the name of the index before running this statement.  
CREATE CLUSTERED COLUMNSTORE INDEX <index_name>   
ON xdimProduct   
WITH ( DROP_EXISTING = ON );  
```  
  
### <a name="b-rebuild-a-clustered-columnstore-index"></a>B. 重新生成聚集列存储索引  
 基于上一示例，此示例使用 CREATE CLUSTERED COLUMNSTORE INDEX 重新生成名为 cci_xDimProduct 的现有聚集列存储索引。  
  
```sql  
--Rebuild the existing clustered columnstore index.  
CREATE CLUSTERED COLUMNSTORE INDEX cci_xDimProduct   
ON xdimProduct   
WITH ( DROP_EXISTING = ON );  
```  
  
### <a name="c-change-the-name-of-a-clustered-columnstore-index"></a>C. 更改聚集列存储索引的名称  
 若要更改聚集列存储索引的名称，请删除现有的聚集列存储索引，然后使用新名称重新创建索引。  
  
 建议仅对小型表或空表执行此操作。 删除大型聚集列存储索引并使用其他名称重新生成需要很长时间。  
  
 通过使用上一示例中的 cci_xDimProduct 聚集列存储索引，此示例将删除 cci_xDimProduct 聚集列存储索引，然后使用名称 mycci_xDimProduct 重新创建聚集列存储索引。  
  
```sql  
--For illustration purposes, drop the clustered columnstore index.   
--The table continues to be distributed, but changes to a heap.  
DROP INDEX cci_xdimProduct ON xDimProduct;  
  
--Create a clustered index with a new name, mycci_xDimProduct.  
CREATE CLUSTERED COLUMNSTORE INDEX mycci_xDimProduct  
ON xdimProduct  
WITH ( DROP_EXISTING = OFF );  
```  
  
### <a name="d-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index"></a>D. 将列存储表转换为具有聚集索引的行存储表  
 可能会出现想删除聚集列存储索引并创建聚集索引的情况。 这会以行存储格式存储表。 此示例会将列存储表转换为具有同名聚集索引的行存储表。 不会丢失任何数据。 所有数据都转到行存储表中，列出的列成为聚集索引中的键列。  
  
```sql  
--Drop the clustered columnstore index and create a clustered rowstore index.   
--All of the columns are stored in the rowstore clustered index.   
--The columns listed are the included columns in the index.  
CREATE CLUSTERED INDEX cci_xDimProduct    
ON xdimProduct (ProductKey, ProductAlternateKey, ProductSubcategoryKey, WeightUnitMeasureCode)  
WITH ( DROP_EXISTING = ON);  
```  
  
### <a name="e-convert-a-columnstore-table-back-to-a-rowstore-heap"></a>E. 将列存储表转换回行存储堆  
 使用 [DROP INDEX (SQL Server PDW)](drop-index-transact-sql.md) 删除聚集列存储索引并将表转换为行存储堆。 此示例会将 cci_xDimProduct 表转换为行存储堆。 可继续分配该表，但将其存储为堆。  
  
```sql  
--Drop the clustered columnstore index. The table continues to be distributed, but changes to a heap.  
DROP INDEX cci_xdimProduct ON xdimProduct;  
```  

### <a name="f-create-an-ordered-clustered-columnstore-index-on-a-table-with-no-index"></a>F. 在不具有索引的表上创建有序聚集列存储索引

```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE );
```

### <a name="g-convert-a-clustered-columnstore-index-to-an-ordered-clustered-columnstore-index"></a>G. 将聚集列存储索引转换为有序聚集列存储索引

```sql  
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE );
WITH (DROP_EXISTING = ON)
```

### <a name="h-add-a-column-to-the-ordering-of-an-ordered-clustered-columnstore-index"></a>H. 向有序聚集列存储索引的排序添加列

```sql
-- The original ordered clustered columnstore index was ordered on SHIPDATE column only.  Add PRODUCTKEY column to the ordering.
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE, PRODUCTKEY );
WITH (DROP_EXISTING = ON)
```
### <a name="i-change-the-ordinal-of-ordered-columns"></a>I. 更改有序列的序号  
```sql
-- The original ordered clustered columnstore index was ordered on SHIPDATE, PRODUCTKEY.  Change the ordering to PRODUCTKEY, SHIPDATE.  
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( PRODUCTKEY,SHIPDATE );
WITH (DROP_EXISTING = ON)
```
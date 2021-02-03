---
description: CREATE EXTERNAL TABLE AS SELECT (Azure Synapse Analytics)
title: CREATE TABLE AS SELECT (Azure Synapse Analytics) | Microsoft Docs
ms.custom: ''
ms.date: 10/07/2016
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: d1e08f88-64ef-4001-8a66-372249df2533
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 9597182b2266ef67878a5d55b96ce731278d9d36
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204915"
---
# <a name="create-table-as-select-azure-synapse-analytics"></a>CREATE EXTERNAL TABLE AS SELECT (Azure Synapse Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

CREATE TABLE AS SELECT (CTAS) 是提供最重要的 T-SQL 功能之一。 它是一个完全并行化的操作，基于 SELECT 语句的输出创建新表。 CTAS 是创建表副本最便捷的方法。   
 
 例如，使用 CTAS：  
  
-   重新创建具有不同哈希分布列的表。
-   重新创建一个表作为复制表。   
-   只在表的某些列上创建列存储索引。  
-   查询或导入外部数据。  

> [!NOTE]  
> 由于 CTAS 增加了创建表的功能，因此本主题尽量不重复 CREATE TABLE 主题。 而是描述 CTAS 和 CREATE TABLE 语句之间的差异。 有关 CREATE TABLE 的详细信息，请参阅 [CREATE TABLE (Azure Synapse Analytics)](./create-table-azure-sql-data-warehouse.md) 语句。 
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
<a name="syntax-bk"></a>

## <a name="syntax"></a>语法   

```syntaxsql
CREATE TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    [ ( column_name [ ,...n ] ) ]  
    WITH ( 
      <distribution_option> -- required
      [ , <table_option> [ ,...n ] ]    
    )  
    AS <select_statement>  
    OPTION <query_hint> 
[;]  

<distribution_option> ::=
    { 
        DISTRIBUTION = HASH ( distribution_column_name ) 
      | DISTRIBUTION = ROUND_ROBIN 
      | DISTRIBUTION = REPLICATE
    }   

<table_option> ::= 
    {   
        CLUSTERED COLUMNSTORE INDEX --default for Synapse Analytics 
      | CLUSTERED COLUMNSTORE INDEX ORDER (column[,...n])
      | HEAP --default for Parallel Data Warehouse   
      | CLUSTERED INDEX ( { index_column_name [ ASC | DESC ] } [ ,...n ] ) --default is ASC 
    }  
      | PARTITION ( partition_column_name RANGE [ LEFT | RIGHT ] --default is LEFT  
        FOR VALUES ( [ boundary_value [,...n] ] ) ) 
  
<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT select_criteria  

<query_hint> ::=
    {
        MAXDOP 
    }
```  

<a name="arguments-bk"></a>
  
## <a name="arguments"></a>参数  
有关详细信息，请参阅 CREATE TABLE 中的[参数部分](./create-table-azure-sql-data-warehouse.md#Arguments)。  

<a name="column-options-bk"></a>

### <a name="column-options"></a>列选项
`column_name` [ ,...`n` ]   
 列名不允许 CREATE TABLE 中提及的[列选项](./create-table-azure-sql-data-warehouse.md#ColumnOptions)。  相反，可以为新表提供包含一个或多个列名的可选列表。 新表中的列将使用指定的名称。 指定列名时，列表中的列数必须与所选结果中的列数相匹配。 如果未指定任何列名，新的目标表将使用 select 语句结果中的列名。 
  
 不能指定任何其他列选项，例如数据类型、排序规则或为 Null 性。 每个属性都派生自 `SELECT` 语句的结果。 但可以使用 SELECT 语句来更改属性。 有关示例，请参阅[使用 CTAS 更改列属性](#ctas-change-column-attributes-bk)。   

<a name="table-distribution-options-bk"></a>

### <a name="table-distribution-options"></a>表分发选项

`DISTRIBUTION` = `HASH` ( distribution_column_name ) | ROUND_ROBIN | REPLICATE      
CTAS 语句需要分布选项，并且没有默认值。 这就不同于具有默认值的 CREATE TABLE。 

有关详细信息以及如何选择最佳分布列的信息，请参阅 CREATE TABLE 中的[表分发选项](./create-table-azure-sql-data-warehouse.md#TableDistributionOptions)部分。 

<a name="table-partition-options-bk"></a>

### <a name="table-partition-options"></a>表分区选项
即使已对源表进行分区，CTAS 语句也会默认创建一个非分区表。 若要使用 CTAS 语句创建已分区表，必须指定分区选项。 

有关详细信息，请参阅 CREATE TABLE 中的[表分区选项](./create-table-azure-sql-data-warehouse.md#TablePartitionOptions)部分。

<a name="select-options-bk"></a>

### <a name="select-statement"></a>Select 语句
select 语句是 CTAS 和 CREATE TABLE 之间的根本区别。  

 `WITH` *common_table_expression*  
 指定临时命名的结果集，这些结果集称为公用表表达式 (CTE)。 有关详细信息，请参阅 [WITH common_table_expression (Transact-SQL)](../../t-sql/queries/with-common-table-expression-transact-sql.md)。  
  
 `SELECT` *select_criteria*  
 使用 SELECT 语句的结果填充新表。 select_criteria 是 SELECT 语句的主体，用于确定将哪些数据复制到新表中。 有关 SELECT 语句的信息，请参阅 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)。  
 
### <a name="query-hint"></a>查询提示
用户可以将 MAXDOP 设置为一个整数值，以控制最大并行度。  当 MAXDOP 设置为 1 时，由单线程执行查询。

 
<a name="permissions-bk"></a>  
  
## <a name="permissions"></a>权限  
CTAS 需要 select_criteria 中引用的任何对象的 `SELECT` 权限。

有关创建表的权限，请参阅 CREATE TABLE 中的[权限](./create-table-azure-sql-data-warehouse.md#Permissions)。 
  
<a name="general-remarks-bk"></a>
  
## <a name="general-remarks"></a>一般备注
有关详细信息，请参阅 CREATE TABLE 中的[一般备注](./create-table-azure-sql-data-warehouse.md#GeneralRemarks)。

<a name="limitations-bk"></a>

## <a name="limitations-and-restrictions"></a>限制和局限  

可以在 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 支持的任何数据类型的列（字符串列除外）上创建有序的聚集列存储索引。  

[SET ROWCOUNT (Transact-SQL)](../../t-sql/statements/set-rowcount-transact-sql.md) 对 CTAS 没有影响。 要实现类似的行为，请使用 [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md)。  
 
有关详细信息，请参阅 CREATE TABLE 中的[限制和局限](./create-table-azure-sql-data-warehouse.md#LimitationsRestrictions)。

<a name="locking-behavior-bk"></a>
  
## <a name="locking-behavior"></a>锁定行为  
 有关详细信息，请参阅 CREATE TABLE 中的[锁定行为](./create-table-azure-sql-data-warehouse.md#LockingBehavior)。
 
<a name="performance-bk"></a>
 
 ## <a name="performance"></a>性能 

对于哈希分布式表，可使用 CTAS 选择其他分布列，提高联接和聚合性能。 如果目标不是选择其他分布列，那么指定相同的分布列可获得最佳 CTAS 性能，因为这样可以避免重新分布行。 

如果使用 CTAS 创建表，并且不考虑性能，则可以指定 `ROUND_ROBIN` 来避免选定分布列。

若要避免数据在后续查询中发生移动，可指定 `REPLICATE`，但代价是增加了在每个计算节点上加载表的完整副本所用的存储。  


<a name="examples-copy-table-bk"></a>

## <a name="examples-for-copying-a-table"></a>复制表的示例

<a name="ctas-copy-table-bk"></a>

### <a name="a-use-ctas-to-copy-a-table"></a>A. 使用 CTAS 复制表 
适用范围：[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

`CTAS` 最常见的用途之一就是创建表副本，使你可以更改 DDL。 例如，如果最初将表创建为 `ROUND_ROBIN`，现在希望将其更改为分布在列上的表，则可以使用 `CTAS` 更改分布列。 `CTAS` 还可用于更改分区、索引或列类型。

假设你使用默认分布类型 `ROUND_ROBIN` 创建了此表，因为 `CREATE TABLE` 中没有指定分布列。

```sql
CREATE TABLE FactInternetSales
(
    ProductKey INT NOT NULL,
    OrderDateKey INT NOT NULL,
    DueDateKey INT NOT NULL,
    ShipDateKey INT NOT NULL,
    CustomerKey INT NOT NULL,
    PromotionKey INT NOT NULL,
    CurrencyKey INT NOT NULL,
    SalesTerritoryKey INT NOT NULL,
    SalesOrderNumber NVARCHAR(20) NOT NULL,
    SalesOrderLineNumber TINYINT NOT NULL,
    RevisionNumber TINYINT NOT NULL,
    OrderQuantity SMALLINT NOT NULL,
    UnitPrice MONEY NOT NULL,
    ExtendedAmount MONEY NOT NULL,
    UnitPriceDiscountPct FLOAT NOT NULL,
    DiscountAmount FLOAT NOT NULL,
    ProductStandardCost MONEY NOT NULL,
    TotalProductCost MONEY NOT NULL,
    SalesAmount MONEY NOT NULL,
    TaxAmt MONEY NOT NULL,
    Freight MONEY NOT NULL,
    CarrierTrackingNumber NVARCHAR(25),
    CustomerPONumber NVARCHAR(25)
);
```

现在，你想使用聚集列存储索引创建此表的新副本，以便可以利用聚集列存储表的性能。 因为希望联接此列，避免在联接 ProductKey 期间数据发生移动，你还想在 ProductKey 上分布此表。 最后，你还想在 OrderDateKey 上添加分区，以便通过删除旧分区来快速删除旧数据。 下面是将旧表复制到新表的 CTAS 声明。

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

最后，可通过重新命名表来交换新表，然后删除旧表。

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

<a name="examples-column-bk"></a>

## <a name="examples-for-column-options"></a>列选项的示例

<a name="ctas-change-column-attributes-bk"></a>

### <a name="b-use-ctas-to-change-column-attributes"></a>B. 使用 CTAS 更改列属性 
适用范围：[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

本示例使用 CTAS 更改 DimCustomer2 表中多个列的数据类型、为 Null 性和排序规则。  
  
```sql  
-- Original table 
CREATE TABLE [dbo].[DimCustomer2] (  
    [CustomerKey] INT NOT NULL,  
    [GeographyKey] INT NULL,  
    [CustomerAlternateKey] nvarchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL  
)  
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = HASH([CustomerKey]));  
  
-- CTAS example to change data types, nullability, and column collations  
CREATE TABLE test  
WITH (HEAP, DISTRIBUTION = ROUND_ROBIN)  
AS  
SELECT  
    CustomerKey AS CustomerKeyNoChange,  
    CustomerKey*1 AS CustomerKeyChangeNullable,  
    CAST(CustomerKey AS DECIMAL(10,2)) AS CustomerKeyChangeDataTypeNullable,  
    ISNULL(CAST(CustomerKey AS DECIMAL(10,2)),0) AS CustomerKeyChangeDataTypeNotNullable,  
    GeographyKey AS GeographyKeyNoChange,  
    ISNULL(GeographyKey,0) AS GeographyKeyChangeNotNullable,  
    CustomerAlternateKey AS CustomerAlternateKeyNoChange,  
    CASE WHEN CustomerAlternateKey = CustomerAlternateKey 
        THEN CustomerAlternateKey END AS CustomerAlternateKeyNullable,  
    CustomerAlternateKey COLLATE Latin1_General_CS_AS_KS_WS AS CustomerAlternateKeyChangeCollation  
FROM [dbo].[DimCustomer2]  
  
-- Resulting table 
CREATE TABLE [dbo].[test] (
    [CustomerKeyNoChange] INT NOT NULL, 
    [CustomerKeyChangeNullable] INT NULL, 
    [CustomerKeyChangeDataTypeNullable] DECIMAL(10, 2) NULL, 
    [CustomerKeyChangeDataTypeNotNullable] DECIMAL(10, 2) NOT NULL, 
    [GeographyKeyNoChange] INT NULL, 
    [GeographyKeyChangeNotNullable] INT NOT NULL, 
    [CustomerAlternateKeyNoChange] NVARCHAR(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL, 
    [CustomerAlternateKeyNullable] NVARCHAR(15) COLLATE SQL_Latin1_General_CP1_CI_AS NULL, 
    [CustomerAlternateKeyChangeCollation] NVARCHAR(15) COLLATE Latin1_General_CS_AS_KS_WS NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN);
```
 
最后，可使用 [RENAME (Transact-SQL)](../../t-sql/statements/rename-transact-sql.md) 切换表名称。 这样就使得 DimCustomer2 成为新表。

```sql
RENAME OBJECT DimCustomer2 TO DimCustomer2_old;
RENAME OBJECT test TO DimCustomer2;

DROP TABLE DimCustomer2_old;
```

<a name="examples-table-distribution-bk"></a>

## <a name="examples-for-table-distribution"></a>表分发的示例

<a name="ctas-change-distribution-method-bk"></a>

### <a name="c-use-ctas-to-change-the-distribution-method-for-a-table"></a>C. 使用 CTAS 更改表的分布方法
适用范围：[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

此简单示例介绍如何改变表的分布方法。 为显示执行此操作的机制，它将哈希分布式表更改为轮循机制表，然后将轮循机制表更改回哈希分布式表。 最终的表与原始表相匹配。 

大多数情况下，无需将哈希分布式表更改为轮循机制表。 更常见的情况是，可能需要将轮循机制表更改为哈希分布式表。 例如，可能最初将新表加载为轮循机制表，然后将其移到哈希分布式表以提高联接性能。

本示例使用 AdventureWorksDW 示例数据库。 若要加载 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 版本，请参阅[将示例数据加载到 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-load-sample-databases/)
 
```sql
-- DimSalesTerritory is hash-distributed.
-- Copy it to a round-robin table.
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  
接下来，将其更改回哈希分布式表。

```sql
-- You just made DimSalesTerritory a round-robin table.
-- Change it back to the original hash-distributed table. 
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH(SalesTerritoryKey) 
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```
 
<a name="ctas-change-to-replicated-bk"></a>

### <a name="d-use-ctas-to-convert-a-table-to-a-replicated-table"></a>D. 使用 CTAS 将表转换为复制表  
适用范围：[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

此示例适用于将轮循机制表或哈希分布式表转换为复制表。 此特殊示例采用先前进一步改变分布类型的方法。  由于 DimSalesTerritory 是一个维度，而且可能是较小的表，因此可以选择重新创建该表作为复制表，从而避免在联接到其他表时数据发生移动。 

```sql
-- DimSalesTerritory is hash-distributed.
-- Copy it to a replicated table.
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE 
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```
 
### <a name="e-use-ctas-to-create-a-table-with-fewer-columns"></a>E. 使用 CTAS 创建列数较少的表
适用范围：[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

以下示例创建一个名为 `myTable (c, ln)` 的轮循机制分布式表。 新表仅包含两列。 它使用 SELECT 语句中的列别名作为列名。  
  
```sql  
CREATE TABLE myTable  
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN  
  )  
AS SELECT CustomerKey AS c, LastName AS ln  
    FROM dimCustomer; 
```  

<a name="examples-query-hints-bk"></a>

## <a name="examples-for-query-hints"></a>查询提示的示例

<a name="ctas-query-hint-bk"></a>

### <a name="f-use-a-query-hint-with-create-table-as-select-ctas"></a>F. 在 CREATE TABLE AS SELECT (CTAS) 中使用查询提示  
适用范围：[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]
  
此查询显示在 CTAS 语句中使用查询联接提示的基本语法。 提交查询后，[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 在为每个单独的分布生成查询计划时应用哈希联接策略。 有关哈希联接查询提示的详细信息，请参阅 [OPTION 子句 (Transact-SQL)](../../t-sql/queries/option-clause-transact-sql.md).  
  
```sql  
CREATE TABLE dbo.FactInternetSalesNew  
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN   
  )  
AS SELECT T1.* FROM dbo.FactInternetSales T1 JOIN dbo.DimCustomer T2  
ON ( T1.CustomerKey = T2.CustomerKey )  
OPTION ( HASH JOIN );  
```  

<a name="examples-external-tables"></a>

## <a name="examples-for-external-tables"></a>外部表的示例

<a name="ctas-azure-blob-storage-bk"></a>

### <a name="g-use-ctas-to-import-data-from-azure-blob-storage"></a>G. 使用 CTAS 从 Azure Blob 存储导入数据  
适用范围：[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

若要从外部表导入数据，只需使用 CREATE TABLE AS SELECT 从外部表进行选择。 从外部表选择数据到 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 中的语法与从常规表中选择数据的语法相同。  
  
 以下示例针对 Azure Blob 存储帐户中的数据定义外部表。 然后，它使用 CREATE TABLE AS SELECT 从外部表进行选择。 这样会从 Azure blob 存储文本分隔文件导入数据，并将数据存储到新的 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 表中。  
  
```sql  
--Use your own processes to create the text-delimited files on Azure blob storage.  
--Create the external table called ClickStream.  
CREATE EXTERNAL TABLE ClickStreamExt (   
    url VARCHAR(50),  
    event_date DATE,  
    user_IP VARCHAR(50)  
)  
WITH (  
    LOCATION='/logs/clickstream/2015/',  
    DATA_SOURCE = MyAzureStorage,  
    FILE_FORMAT = TextFileFormat)  
;  
  
--Use CREATE TABLE AS SELECT to import the Azure blob storage data into a new   
--Synapse Analytics table called ClickStreamData  
CREATE TABLE ClickStreamData   
WITH  
  (  
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH (user_IP)  
  )  
AS SELECT * FROM ClickStreamExt  
;  
```  

<a name="ctas-import-Hadoop-bk"></a>
  
### <a name="h-use-ctas-to-import-hadoop-data-from-an-external-table"></a>H. 使用 CTAS 从外部表导入 Hadoop 数据  
适用范围：[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
若要从外部表导入数据，只需使用 CREATE TABLE AS SELECT 从外部表进行选择。 从外部表选择数据到 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 中的语法与从常规表中选择数据的语法相同。  
  
 以下示例针对 Hadoop 群集定义外部表。 然后，它使用 CREATE TABLE AS SELECT 从外部表进行选择。 这样会从 Hadoop 文本分隔文件导入数据，并将数据存储到新的 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 表中。  
  
```sql  
-- Create the external table called ClickStream.  
CREATE EXTERNAL TABLE ClickStreamExt (   
    url VARCHAR(50),  
    event_date DATE,  
    user_IP VARCHAR(50)  
)  
WITH (  
    LOCATION = 'hdfs://MyHadoop:5000/tpch1GB/employee.tbl',  
    FORMAT_OPTIONS ( FIELD_TERMINATOR = '|')  
)  
;  
  
-- Use your own processes to create the Hadoop text-delimited files 
-- on the Hadoop Cluster.  
  
-- Use CREATE TABLE AS SELECT to import the Hadoop data into a new 
-- table called ClickStreamPDW  
CREATE TABLE ClickStreamPDW   
WITH  
  (  
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH (user_IP)  
  )  
AS SELECT * FROM ClickStreamExt  
;   
```  
 
<a name="examples-workarounds-bk"></a>
 
## <a name="examples-using-ctas-to-replace-sql-server-code"></a>使用 CTAS 替换 SQL Server 代码的示例

使用 CTAS 解决某些不支持的功能。 除了能够在数据仓库上运行代码之外，通过重写现有代码来使用 CTAS 通常还可以提高性能。 这是因为采用了完全并行化设计。 

> [!NOTE]
> 尽量考虑“CTAS 优先”。 如果你认为可以使用 `CTAS` 解决问题，那么这通常是解决问题的最佳方法（即使你会因此编写更多数据）。
>

<a name="ctas-replace-select-into-bk"></a>

### <a name="i-use-ctas-instead-of-selectinto"></a>I. 使用 CTAS 而不是 SELECT..INTO  
适用范围：[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

SQL Server 代码通常借助 SELECT..INTO 来使用 SELECT 语句的结果填充表。 这是 SQL Server SELECT..INTO 语句的一个例子。

```sql
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和并行数据仓库不支持此语法。 本示例显示如何将之前的 SELECT..INTO 语句重写为 CTAS 语句。 你可以选择 CTAS 语法中描述的任何 DISTRIBUTION 选项。 本示例使用 ROUND_ROBIN 分布方法。

```sql
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

<a name="ctas-replace-implicit-joins-bk"></a>

### <a name="j-use-ctas-and-implicit-joins-to-replace-ansi-joins-in-the-from-clause-of-an-update-statement"></a>J. 使用 CTAS 和隐式联接替换 `UPDATE` 语句中 `FROM` 子句的 ANSI 联接  
适用范围：[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

可能会发现更新很复杂，它使用 ANSI 联接语法将两个以上的表联接在一起，以执行 UPDATE 或 DELETE 操作。

假设必须更新此表：

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]          SMALLINT    NOT NULL
,   [TotalSalesAmount]      MONEY       NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

原始查询看起来可能如下：

```sql
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

由于 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 不支持 `UPDATE` 语句中 `FROM` 子句的 ANSI 联接，因此需要稍作更改才能使用此 SQL Server 代码。

可以将 `CTAS` 和隐式联接结合使用来替换此代码：

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

<a name="ctas-replace-ansi-joins-bk"></a>

### <a name="k-use-ctas-to-specify-which-data-to-keep-instead-of-using-ansi-joins-in-the-from-clause-of-a-delete-statement"></a>K. 使用 CTAS 指定要保留的数据而不是使用 DELETE 语句中 FROM 子句的 ANSI 联接  
适用范围：[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

有时，删除数据的最佳方法是使用 `CTAS`。 只需选择希望保留的数据，而不是删除数据。 这尤其适用于使用 ansi 联接语法的 `DELETE` 语句，因为 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 不支持 `DELETE` 语句中 `FROM` 子句中的 ANSI 联接。

转换后的 DELETE 语句示例如下：

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

<a name="ctas-simplify-merge-bk"></a>

### <a name="l-use-ctas-to-simplify-merge-statements"></a>L. 使用 CTAS 简化 merge 语句  
适用范围：[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

使用 `CTAS` 可以替换（至少部分替换）Merge 语句。 可以将 `INSERT` 和 `UPDATE` 合并为一个语句。 在第二个语句中需要关闭任何已删除的记录。

`UPSERT` 示例如下：

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimProduct_upsert]  TO [DimProduct];

```

<a name="ctas-data-type-and-nullability-bk"></a>

### <a name="m-explicitly-state-data-type-and-nullability-of-output"></a>M. 显式声明数据类型和输出是否可为 null  
适用范围：[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

将 SQL Server 代码迁移到 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 时，可能会遇到这种类型的编码模式：

```sql
DECLARE @d DECIMAL(7,2) = 85.455
,       @f FLOAT(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

你可能会本能地认为应将此代码迁移到 CTAS，这无疑是正确的。 但是，这里存在一个隐藏的问题。

以下代码不会生成相同的结果：

```sql
DECLARE @d DECIMAL(7,2) = 85.455
,       @f FLOAT(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

注意，“结果”列会沿用表达式的数据类型和为 Null 性值。 稍有不慎，可能就会导致值存在细微差异。

请尝试以下示例：

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

为结果存储的值不相同。 由于在其他表达式中使用了结果列中保留的值，因此差异会更大。

![CREATE TABLE AS SELECT 结果](../../t-sql/statements/media/create-table-as-select-results.png)

这一点对数据迁移来说尤其重要。 尽管第二个查询可能更为准确，但仍存在问题。 该数据与源系统相比有所不同，会导致迁移过程出现完整性问题。 这是一种“错误”答案实际上为正确答案的罕见情况！

我们发现这两种结果之间出现这种差异的原因是隐式类型转换。 在第一个示例中，表定义了列的定义。 插入行时会发生隐式类型转换。 在第二个示例中未发生隐式类型转换，因为表达式定义列的数据类型。 另请注意，第二个示例中的列已定义为可为 Null 的列，而第一个示例中并未如此。 在第一个示例中创建表时，显式定义了列的为 Null 性。 在第二个示例中，仅将它留给表达式，在默认情况下会产生 NULL 定义。  

若要解决这些问题，则必须在 `CTAS` 语句的 `SELECT` 部分中显式设置类型转换和为 Null 性。 在创建表的部分无法设置这些属性。

以下示例演示如何修复代码：

```sql
DECLARE @d DECIMAL(7,2) = 85.455
,       @f FLOAT(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

注意以下事项：
- 可能使用了 CAST 或 CONVERT
- 使用了 ISNULL 而不是 COALESCE 强制为 Null 性
- ISNULL 是最外面的函数
- ISNULL 的第二部分是一个常数（即 0）

> [!NOTE]
> 为正确设置为 Null 性，请务必使用 `ISNULL` 而不是 `COALESCE`。 `COALESCE` 不是确定性函数，因此表达式的结果将始终可以为 NULL。 `ISNULL` 则不同。 它是确定性的。 因此，当 `ISNULL` 函数的第二部分是常数或文本时，结果值将为 NOT NULL。

该提示不仅有助于确保计算的完整性。 它对于表格分区切换也很重要。 试想根据实际情况定义此表：

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

但值字段是一个计算表达式，不是源数据的一部分。

若要创建分区数据集，可能需要执行此操作：

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

该查询将会顺利运行。 尝试执行分区切换时，将出现问题。 表定义不匹配。 若要使表定义匹配，则需对 CTAS 进行修改。

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

因此，你会发现类型一致性和维持 CTAS 上的为 Null 性属性是优秀工程设计的最佳做法。 它有助于保持计算的完整性，而且还可确保实现分区切换。

### <a name="n-create-an-ordered-clustered-columnstore-index-with-maxdop-1"></a>N. 创建有序聚集列存储索引，同时将 MAXDOP 设为 1  
```sql
CREATE TABLE Table1 WITH (DISTRIBUTION = HASH(c1), CLUSTERED COLUMNSTORE INDEX ORDER(c1) )
AS SELECT * FROM ExampleTable
OPTION (MAXDOP 1);
```

 
## <a name="see-also"></a>另请参阅  
 [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md)   
 [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](../../t-sql/statements/create-external-file-format-transact-sql.md)   
 [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)   
 [CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)](../../t-sql/statements/create-external-table-as-select-transact-sql.md)   
 [CREATE TABLE &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-azure-sql-data-warehouse.md) [DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)   
 [DROP EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/drop-external-table-transact-sql.md)   
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER EXTERNAL TABLE (Transact-SQL)](./create-external-table-transact-sql.md)  

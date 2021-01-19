---
description: CREATE STATISTICS (Transact-SQL)
title: CREATE STATISTICS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/04/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STATISTICS
- STATISTICS_TSQL
- CREATE STATISTICS
- CREATE_STATISTICS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- query optimization statistics [SQL Server], creating
- indexed views [SQL Server], statistics
- FULLSCAN option
- CREATE STATISTICS statement
- filtered statistics [SQL Server]
- creating statistics [SQL Server]
- NORECOMPUTE clause
ms.assetid: b23e2f6b-076c-4e6d-9281-764bdb616ad2
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d0e799f28d327204707298dc63a4fb1a8470fa2f
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170519"
---
# <a name="create-statistics-transact-sql"></a>CREATE STATISTICS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在表格、索引视图或外部表格的一列或多列上创建查询优化统计信息。 对于大多数查询，查询优化器已为高质量查询计划生成必要的统计信息；在少数情况下，您需要使用 CREATE STATISTICS 创建附加的统计信息或修改查询设计以提高查询性能。  
  
 若要了解更多信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Create statistics on an external table  
CREATE STATISTICS statistics_name   
ON { table_or_indexed_view_name } ( column [ ,...n ] )   
    [ WITH FULLSCAN ] ;  
  
-- Create statistics on a regular table or indexed view  
CREATE STATISTICS statistics_name   
ON { table_or_indexed_view_name } ( column [ ,...n ] )   
    [ WHERE <filter_predicate> ]  
    [ WITH   
        [ [ FULLSCAN   
            [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
          | SAMPLE number { PERCENT | ROWS }   
            [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
          | <update_stats_stream_option> [ ,...n ]    
        [ [ , ] NORECOMPUTE ]   
        [ [ , ] INCREMENTAL = { ON | OFF } ] 
        [ [ , ] MAXDOP = max_degree_of_parallelism ]
    ] ;  
  
<filter_predicate> ::=   
    <conjunct> [AND <conjunct>]  
  
<conjunct> ::=  
    <disjunct> | <comparison>  
  
<disjunct> ::=  
        column_name IN (constant ,...)  
  
<comparison> ::=  
        column_name <comparison_op> constant  
  
<comparison_op> ::=  
    IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !<  
    
<update_stats_stream_option> ::=  
    [ STATS_STREAM = stats_stream ]  
    [ ROWCOUNT = numeric_constant ]  
    [ PAGECOUNT = numeric_contant ] 
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE STATISTICS statistics_name   
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( column_name  [ ,...n ] )   
    [ WHERE <filter_predicate> ]  
    [ WITH {  
           FULLSCAN   
           | SAMPLE number PERCENT   
      }  
    ]  
[;]  
  
<filter_predicate> ::=   
    <conjunct> [AND <conjunct>]  
  
<conjunct> ::=  
    <disjunct> | <comparison>  
  
<disjunct> ::=  
        column_name IN (constant ,...)  
  
<comparison> ::=  
        column_name <comparison_op> constant  
  
<comparison_op> ::=  
    IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !<  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *statistics_name*  
 是要创建的统计信息的名称。  
  
 table_or_indexed_view_name  
 在其上创建统计信息的表格、索引视图或外部表格的名称。 若要在另一个数据库中创建统计信息，请指定限定的表名称。  
  
 *column [ ,...n]*  
 统计信息中要包含的一列或多列。 这些列按照从左到右的优先顺序排列。 仅第一列用于创建直方图。 所有列都可用于调用密度的跨列相关性统计信息。  
  
 您可以指定任何可指定为索引键列的列，但下列情况除外：  
  
-   不能指定 Xml、全文和 FILESTREAM 列。  
  
-   只有当 ARITHABORT 和 QUOTED_IDENTIFIER 数据库设置为 ON 时，才能指定计算列。  
  
-   如果 CLR 用户定义类型支持二进制排序，则可以指定 CLR 用户定义类型列。 如果方法具有确定性标记，可以指定定义为用户定义类型的列的方法调用的计算列。  
  
 WHERE \<filter_predicate> 指定一个表达式，以选择在创建统计信息对象时要包括的行的子集。 使用筛选谓词创建的统计信息称作筛选统计信息。 筛选器谓词使用简单比较逻辑且不能引用计算列、UDT 列、空间数据类型列或 hierarchyID 数据类型列。 比较运算符不允许使用 NULL 文本的比较。 请改用 IS NULL 和 IS NOT NULL 运算符。  
  
 下面是 Production.BillOfMaterials 表的筛选谓词的一些示例：  
  
 * `WHERE StartDate > '20000101' AND EndDate <= '20000630'`  
  
 * `WHERE ComponentID IN (533, 324, 753)`  
  
 * `WHERE StartDate IN ('20000404', '20000905') AND EndDate IS NOT NULL`  
  
 有关筛选器谓词的详细信息，请参阅[创建筛选索引](../../relational-databases/indexes/create-filtered-indexes.md)。  
  
 FULLSCAN  
 扫描所有行，计算统计信息。 FULLSCAN 和 SAMPLE 100 PERCENT 的结果相同。 FULLSCAN 不能与 SAMPLE 选项一起使用。  
  
 如果省略，SQL Server 会使用采样创建统计信息，并确定创建高质量查询计划所需的示例大小  
  
 SAMPLE number { PERCENT | ROWS }  
 指定在查询优化器创建统计信息时所使用的表或索引视图中的近似行百分比或行数。 对于 PERCENT，number 可以介于 0 到 100 之间，对于 ROWS，number 可以介于 0 到总行数之间 。 查询优化器抽样的实际行百分比或行数可能与指定的行百分比或行数不匹配。 例如，查询优化器扫描数据页上的所有行。  
  
 对于基于默认抽样的查询计划并非最佳的特殊情况，SAMPLE 非常有用。 在大多数情况下，不必指定 SAMPLE，这是因为在默认情况下，查询优化器已根据需要采用抽样，并以统计方式确定大量样本的大小，以便创建高质量的查询计划。  
  
 SAMPLE 不能与 FULLSCAN 选项一起使用。 如果未指定 SAMPLE 和 FULLSCAN，查询优化器则默认使用抽样数据并计算样本大小。  
  
 我们建议不指定 0 PERCENT 或 0 ROWS。 如果指定 0 PERCENT 或 0 ROWS，则将创建统计信息对象，但该对象不包含任何统计信息数据。  
 
 PERSIST_SAMPLE_PERCENT = { ON | OFF }  
 为 ON 时，统计信息将保留创建采样百分比，用于未明确指定采样百分比的后续更新。 为 OFF 时，在未明确指定采样百分比的后续更新中，统计信息采样百分比将重置为默认采样。 默认为 **OFF**。 
 
 > [!NOTE]
 > 如果该表被截断，则截断的 HoBT 上生成的所有统计信息将恢复为使用默认采样百分比。

 **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 CU4 开始）及更高版本（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU1 开始）。    
  
 STATS_STREAM stats_stream **=**   
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 NORECOMPUTE  
 为 statistics_name 禁用自动统计信息更新选项 AUTO_STATISTICS_UPDATE。 如果指定了该选项，则查询优化器将完成 statistics_name 的任何正在进行中的统计信息更新并禁止在将来出现更新。  
  
 若要重新启用统计信息更新，请使用 [DROP STATISTICS](../../t-sql/statements/drop-statistics-transact-sql.md) 删除统计信息，然后运行 CREATE STATISTICS 但不使用 NORECOMPUTE 选项。  
  
> [!WARNING]  
> 使用此选项可能会产生并非最佳的查询计划。 建议您尽量少用此选项，并且此选项只能由有资格的系统管理员使用。  
  
 有关 AUTO_STATISTICS_UPDATE 选项的详细信息，请参阅 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)。 有关禁用和重新启用统计信息更新的详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)。  
  
 INCREMENTAL = { ON | OFF }  
 为 ON 时，根据分区统计信息创建统计信息。 为 OFF 时，为所有分区合并统计信息。 默认为 **OFF**。  
  
 如果不支持每个分区统计信息，将生成错误。 对于以下统计信息类型，不支持增量统计信息：  
  
-   使用未与基表的分区对齐的索引创建的统计信息。  
-   对 Always On 可读辅助数据库创建的统计信息。  
-   对只读数据库创建的统计信息。  
-   对筛选的索引创建的统计信息。  
-   对视图创建的统计信息。  
-   对内部表创建的统计信息。  
-   使用空间索引或 XML 索引创建的统计信息。  
  
**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。  
  
MAXDOP = max_degree_of_parallelism  
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始）。  
  
 在统计信息操作期间替代最大并行度配置选项。 有关详细信息，请参阅 [配置 max degree of parallelism 服务器配置选项](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。 使用 MAXDOP 可以限制在执行并行计划的过程中使用的处理器数量。 最大数量为 64 个处理器。  
  
 max_degree_of_parallelism 可以是：  
  
 1  
 取消生成并行计划。  
  
 \>1  
 基于当前系统工作负载，将并行统计信息操作中使用的最大处理器数限制为指定数量或更少。  
  
 0（默认值）  
 根据当前系统工作负荷使用实际的处理器数量或更少数量的处理器。  
  
 \<update_stats_stream_option> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  

## <a name="permissions"></a>权限  
 需要以下权限之一：  
  
-   ALTER TABLE  
-   用户是表所有者  
-   具有 db_ddladmin 固定数据库角色的成员身份  
  
## <a name="general-remarks"></a>一般备注  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可使用 tempdb 在生成统计信息之前对已抽样的行进行排序。  
  
### <a name="statistics-for-external-tables"></a>外部表的统计信息  
 创建外部表统计信息时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]先将外部表导入到临时[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]表中，然后再创建统计信息。 对于示例统计信息，仅导入已采样的行。 如果有一个大的外部表格，使用默认采样比使用完全扫描选项要快得多。  
  
### <a name="statistics-with-a-filtered-condition"></a>具有筛选条件的统计信息  
 筛选统计信息可以提高以下从定义完善的数据子集选择数据的查询的查询性能。 筛选的统计信息在 WHERE 子句中使用筛选谓词来选择统计信息中包含的数据子集。  
  
### <a name="when-to-use-create-statistics"></a>何时使用 CREATE STATISTICS  
 有关何时使用 CREATE STATISTICS 的详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)。  
  
### <a name="referencing-dependencies-for-filtered-statistics"></a>筛选统计信息的引用依赖项  
 [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) 目录视图将筛选统计信息谓词中的每一列作为一个引用依赖项，进行跟踪。 由于您无法删除、重命名或修改在筛选统计信息谓词中定义的表列的定义，因此在创建筛选统计信息之前应考虑清楚要对表列执行哪些操作。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
* 外部表格中不支持更新统计信息。 若要更新外部表格中的统计信息，请删除并重新创建统计信息。  
* 每个统计信息对象至多可列出 64 列。
* MAXDOP 选项与 STATS_STREAM、ROWCOUNT 和 PAGECOUNT 选项不兼容。
* 如果使用，MAXDOP 选项会受 Resource Governor 工作负载组 MAX_DOP 设置的限制。
* Azure SQL 数据库中不支持外部表上的 CREATE 和 DROP STATISTICS。
  
## <a name="examples"></a>示例  

### <a name="examples-use-the-adventureworks-database"></a>使用 AdventureWorks 数据库示例。  

### <a name="a-using-create-statistics-with-sample-number-percent"></a>A. 将 CREATE STATISTICS 与 SAMPLE number PERCENT 一起使用  
 下例使用 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中 `Person` 表的 `BusinessEntityID` 和 `EmailPromotion` 列的 5% 作为随机抽样来创建 `ContactMail1` 统计信息。  
  
```sql  
CREATE STATISTICS ContactMail1  
    ON Person.Person (BusinessEntityID, EmailPromotion)  
    WITH SAMPLE 5 PERCENT;  
```  
  
### <a name="b-using-create-statistics-with-fullscan-and-norecompute"></a>B. 将 CREATE STATISTICS 与 FULLSCAN 和 NORECOMPUTE 一起使用  
 以下示例对 `NamePurchase` 表的 `BusinessEntityID` 和 `EmailPromotion` 列中的所有行创建 `Person` 统计信息，并禁用自动重新计算统计信息。  
  
```sql  
CREATE STATISTICS NamePurchase  
    ON AdventureWorks2012.Person.Person (BusinessEntityID, EmailPromotion)  
    WITH FULLSCAN, NORECOMPUTE;  
```  
  
### <a name="c-using-create-statistics-to-create-filtered-statistics"></a>C. 使用 CREATE STATISTICS 创建筛选统计信息  
 以下示例创建筛选统计信息 `ContactPromotion1`。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]对 50% 的数据进行采样，然后选择 `EmailPromotion` 等于 2 的行。  
  
```sql  
CREATE STATISTICS ContactPromotion1  
    ON Person.Person (BusinessEntityID, LastName, EmailPromotion)  
WHERE EmailPromotion = 2  
WITH SAMPLE 50 PERCENT;  
GO  
```  
  
### <a name="d-create-statistics-on-an-external-table"></a>D. 在外部表上创建统计信息  
 在外部表上创建统计信息时，除了提供列的列表，唯一需要做的决定是通过对行采样创建统计数据，还是通过扫描所有行创建统计数据。 Azure SQL 数据库中不支持外部表上的 CREATE 和 DROP STATISTICS。
  
 由于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将外部表中的数据导入临时表，创建统计信息，所以完全扫描选项所需时间更长。 对于大型表格来说，通常情况下，使用默认采样方法就够了。  
  
```sql  
--Create statistics on an external table and use default sampling.  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress);  
  
--Create statistics on an external table and scan all the rows  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress) WITH FULLSCAN;  
```  

### <a name="e-using-create-statistics-with-fullscan-and-persist_sample_percent"></a>E. 将 CREATE STATISTICS 与 FULLSCAN 和 PERSIST_SAMPLE_PERCENT 一起使用  
 以下示例为 `Person` 表 `BusinessEntityID` 和 `EmailPromotion` 列中的所有行创建 `NamePurchase` 统计信息，并为所有未明确指定采样百分比的后续更新设置 100% 的采样百分比。  
  
```sql  
CREATE STATISTICS NamePurchase  
    ON AdventureWorks2012.Person.Person (BusinessEntityID, EmailPromotion)  
    WITH FULLSCAN, PERSIST_SAMPLE_PERCENT = ON;  
```  
  
### <a name="examples-using-adventureworksdw-database"></a>使用 AdventureWorksDW 数据库的示例。 
  
### <a name="f-create-statistics-on-two-columns"></a>F. 在两列中创建统计信息  
 以下示例基于 `DimCustomer` 表 `CustomerKey` 和 `EmailAddress` 列创建 `CustomerStats1` 统计信息。 此统计信息是基于 `Customer` 表的行中具有重大统计意义的采样而创建的。  
  
```sql  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress);  
```  
  
### <a name="g-create-statistics-by-using-a-full-scan"></a>G. 使用完全扫描创建统计信息  
 以下示例扫描 `DimCustomer` 表中的所有行，创建 `CustomerStatsFullScan` 统计信息。  
  
```sql  
CREATE STATISTICS CustomerStatsFullScan 
ON DimCustomer (CustomerKey, EmailAddress) WITH FULLSCAN;  
```  
  
### <a name="h-create-statistics-by-specifying-the-sample-percentage"></a>H. 指定采样百分比，创建统计信息  
 以下示例扫描 `DimCustomer` 表中 50% 的行，创建 `CustomerStatsSampleScan` 统计信息。  
  
```sql  
CREATE STATISTICS CustomerStatsSampleScan 
ON DimCustomer (CustomerKey, EmailAddress) WITH SAMPLE 50 PERCENT;  
```  
  
## <a name="see-also"></a>另请参阅  
 [统计信息](../../relational-databases/statistics/statistics.md)   
 [UPDATE STATISTICS (Transact-SQL)](../../t-sql/statements/update-statistics-transact-sql.md)   
 [sp_updatestats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md)   
 [DBCC SHOW_STATISTICS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS (Transact-SQL)](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [sys.stats (Transact-SQL)](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
 [sys.stats_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)  
  
  


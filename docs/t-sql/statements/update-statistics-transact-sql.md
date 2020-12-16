---
description: UPDATE STATISTICS (Transact-SQL)
title: UPDATE STATISTICS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/04/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- UPDATE STATISTICS
- UPDATE_STATISTICS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- updating statistics
- query optimization statistics [SQL Server], updating
- UPDATE STATISTICS statement
- statistical information [SQL Server], updating
ms.assetid: 919158f2-38d0-4f68-82ab-e1633bd0d308
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1992b8112bdb4d179f1b25471e9e34c42f2a4770
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483959"
---
# <a name="update-statistics-transact-sql"></a>UPDATE STATISTICS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

更新表或索引视图的查询优化统计信息。 默认情况下，查询优化器已根据需要更新统计信息以改进查询计划；但在某些情况下，可以通过使用 `UPDATE STATISTICS` 或存储过程 [sp_updatestats](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md) 来比默认更新更频繁地更新统计信息，提高查询性能。  
  
更新统计信息可确保查询使用最新的统计信息进行编译。 不过，更新统计信息会导致查询重新编译。 我们建议不要太频繁地更新统计信息，因为需要在改进查询计划和重新编译查询所用时间之间权衡性能。 具体的折衷方案取决于你的应用程序。 `UPDATE STATISTICS` 可以使用 tempdb 对行样本进行排序以生成统计信息。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
UPDATE STATISTICS table_or_indexed_view_name   
    [   
        {   
            { index_or_statistics__name }  
          | ( { index_or_statistics_name } [ ,...n ] )   
                }  
    ]   
    [    WITH   
        [  
            FULLSCAN   
              [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
            | SAMPLE number { PERCENT | ROWS }   
              [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
            | RESAMPLE   
              [ ON PARTITIONS ( { <partition_number> | <range> } [, ...n] ) ]  
            | <update_stats_stream_option> [ ,...n ]  
        ]   
        [ [ , ] [ ALL | COLUMNS | INDEX ]   
        [ [ , ] NORECOMPUTE ]   
        [ [ , ] INCREMENTAL = { ON | OFF } ] 
        [ [ , ] MAXDOP = max_degree_of_parallelism ] 
    ] ;  
  
<update_stats_stream_option> ::=  
    [ STATS_STREAM = stats_stream ]  
    [ ROWCOUNT = numeric_constant ]  
    [ PAGECOUNT = numeric_contant ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
UPDATE STATISTICS [ schema_name . ] table_name   
    [ ( { statistics_name | index_name } ) ]  
    [ WITH   
       {  
              FULLSCAN   
            | SAMPLE number PERCENT   
            | RESAMPLE   
        }  
    ]  
[;]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>参数
 table_or_indexed_view_name  
 包含统计信息对象的表或索引视图的名称。  
  
 index_or_statistics_name  
 要更新其统计信息的索引的名称，或要更新的统计信息的名称。 如果不指定 index_or_statistics_name，则查询优化器将更新表或索引视图的所有统计信息。 这包括使用 CREATE STATISTICS 语句创建的统计信息、在 AUTO_CREATE_STATISTICS 为 ON 时创建的单列统计信息以及为索引创建的统计信息。  
  
 有关 AUTO_CREATE_STATISTICS 的详细信息，请参阅 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)。 若要查看某一表或视图的所有索引，可以使用 [sp_helpindex](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md)。  
  
 FULLSCAN  
 通过扫描表或索引视图中的所有行来计算统计信息。 FULLSCAN 和 SAMPLE 100 PERCENT 的结果相同。 FULLSCAN 不能与 SAMPLE 选项一起使用。  
  
 SAMPLE number { PERCENT | ROWS }  
 指定当查询优化器更新统计信息时要为其使用的表或索引视图中近似的百分比或行数。 对于 PERCENT，number 可以介于 0 到 100 之间，对于 ROWS，number 可以介于 0 到总行数之间 。 查询优化器抽样的实际行百分比或行数可能与指定的行百分比或行数不匹配。 例如，查询优化器扫描数据页上的所有行。  
  
 对于基于默认抽样的查询计划并非最佳的特殊情况，SAMPLE 非常有用。 在大多数情况下，不必指定 SAMPLE，这是因为在默认情况下，查询优化器根据需要采用抽样，并以统计方式确定大量样本的大小，以便创建高质量的查询计划。 
 
从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始，在使用兼容性级别 130 时，并行执行数据采样以生成统计信息，从而提高统计信息收集的性能。 只要表大小超过某个阈值，查询优化器就会使用并行采样统计信息。 
   
 SAMPLE 不能与 FULLSCAN 选项一起使用。 如果未指定 SAMPLE 和 FULLSCAN，查询优化器则默认使用抽样数据并计算样本大小。  
  
 我们建议不指定 0 PERCENT 或 0 ROWS。 如果指定 0 PERCENT 或 0 ROWS，则将更新统计信息对象，但该对象不包含任何统计信息数据。  
  
 对于大多数工作负载，不需要完全扫描，默认采样已经足够。  
但是，对广泛变化的数据分布敏感的某些工作负载可能需要增加样本大小，或者甚至需要完全扫描。  
有关详细信息，请参阅 [CSS SQL 升级服务博客](/archive/blogs/psssql/sampling-can-produce-less-accurate-statistics-if-the-data-is-not-evenly-distributed)。  
  
 RESAMPLE  
 使用最近的采样速率更新每个统计信息。  
  
 使用 RESAMPLE 会导致全表扫描。 例如，索引的统计信息使用全表扫描来获取其采样速率。 如果未指定采样选项（SAMPLE、FULLSCAN、RESAMPLE），则查询优化器默认将对数据进行抽样并计算样本大小。  

PERSIST_SAMPLE_PERCENT = { ON | OFF }  
为 ON 时，统计信息将保留设定的采样百分比，以用于未明确指定采样百分比的后续更新。 为 OFF 时，在未明确指定采样百分比的后续更新中，统计信息采样百分比将重置为默认采样。 默认为 **OFF**。 
 
 > [!NOTE]
 > 如果执行 AUTO_UPDATE_STATISTICS，则在可用情况下使用持久采样百分比，否则使用默认采样百分比。
 > 此选项不影响 RESAMPLE 行为。
 
 > [!NOTE]
 > 如果该表被截断，则截断的 HoBT 上生成的所有统计信息将恢复为使用默认采样百分比。
 
 > [!TIP] 
 > [DBCC SHOW_STATISTICS](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md) 和 [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md) 公开选定统计信息的持久样本百分比值。
 
 **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1 CU4 开始）及更高版本（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU1 开始）。  
 
 ON PARTITIONS ( { \<partition_number> | \<range> } [, ...n] ) ] 强制重新计算涵盖在 ON PARTITIONS 子句中指定的分区的叶级统计信息，然后合并它们以生成全局统计信息。 需要 WITH RESAMPLE，因为使用不同抽样率生成的分区统计信息不能合并在一起。  
  
**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本
  
 ALL | COLUMNS | INDEX  
 更新所有现有统计信息、在一列或多列上创建的统计信息或为索引创建的统计信息。 如果未指定上述任何选项，则 UPDATE STATISTICS 语句将更新表或索引视图上的所有统计信息。  
  
 NORECOMPUTE  
 为指定统计信息禁用自动统计信息更新选项 AUTO_UPDATE_STATISTICS。 如果指定此选项，则查询优化器将完成此统计信息更新并禁用将来的更新。  
  
 若要重新启用 AUTO_UPDATE_STATISTICS 选项行为，请不使用 NORECOMPUTE 选项再次运行 UPDATE STATISTICS，或运行 sp_autostats。  
  
> [!WARNING]  
> 使用此选项可能会产生并非最佳的查询计划。 建议您尽量少用此选项，并且此选项只能由有资格的系统管理员使用。  
  
 有关 AUTO_STATISTICS_UPDATE 选项的详细信息，请参阅 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)。  
  
 INCREMENTAL = { ON | OFF }  
 为 ON 时，根据分区统计信息重新创建统计信息。 为 OFF 时，删除统计信息树并且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 重新计算统计信息。 默认为 **OFF**。  
  
 如果不支持每个分区统计信息，将生成错误。 对于以下统计信息类型，不支持增量统计信息：  
  
-   使用未与基表的分区对齐的索引创建的统计信息。  
-   对 Always On 可读辅助数据库创建的统计信息。  
-   对只读数据库创建的统计信息。  
-   对筛选的索引创建的统计信息。  
-   对视图创建的统计信息。  
-   对内部表创建的统计信息。  
-   使用空间索引或 XML 索引创建的统计信息。  
  
**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本

MAXDOP = max_degree_of_parallelism  
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始）。  
  
 在统计信息操作期间替代最大并行度配置选项。 有关详细信息，请参阅 [配置 max degree of parallelism 服务器配置选项](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。 使用 MAXDOP 可以限制在执行并行计划的过程中使用的处理器数量。 最大数量为 64 个处理器。  
  
 max_degree_of_parallelism 可以是：  
  
 1  
 取消生成并行计划。  
  
 \>1  
 基于当前系统工作负载，将并行统计信息操作中使用的最大处理器数限制为指定数量或更少。  
  
 0（默认值）  
 根据当前系统工作负荷使用实际的处理器数量或更少数量的处理器。  
  
 \<update_stats_stream_option> 
 
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  

## <a name="remarks"></a>备注  
  
### <a name="when-to-use-update-statistics"></a>何时使用 UPDATE STATISTICS  
 有关何时使用 `UPDATE STATISTICS` 的详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)。  

### <a name="limitations-and-restrictions"></a>限制和局限  
* 外部表格中不支持更新统计信息。 若要更新外部表格中的统计信息，请删除并重新创建统计信息。  
* `MAXDOP` 选项与 `STATS_STREAM`、`ROWCOUNT` 和 `PAGECOUNT` 选项不兼容。
* 如果使用的话，`MAXDOP` 选项会受资源调控器工作负载组 `MAX_DOP` 设置的限制。

### <a name="updating-all-statistics-with-sp_updatestats"></a>使用 sp_updatestats 更新所有统计信息  
有关如何为数据库中的所有用户定义表和内部表更新统计信息的信息，请参阅存储过程 [sp_updatestats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md)。 例如，以下命令调用 sp_updatestats 来更新数据库的所有统计信息。  
  
```sql  
EXEC sp_updatestats;  
```  

### <a name="automatic-index-and-statistics-management"></a>自动索引和统计信息管理
利用[自适应索引碎片整理](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag)等解决方案，自动管理一个或多个数据库的索引碎片整理和统计信息更新。 此过程根据碎片级别以及其他参数，自动选择是重新生成索引还是重新组织索引，并使用线性阈值更新统计信息。
  
### <a name="determining-the-last-statistics-update"></a>确定最近的统计信息更新  
 若要确定最近一次更新统计信息的时间，请使用 [STATS_DATE](../../t-sql/functions/stats-date-transact-sql.md) 函数。  
  
### <a name="pdw--azure-synapse-analytics"></a>PDW/Azure Synapse Analytics  
 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] / [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 不支持以下语法  
  
```sql
UPDATE STATISTICS t1 (a,b);   
```  
  
```sql  
UPDATE STATISTICS t1 (a) WITH SAMPLE 10 ROWS;  
```  
  
```sql  
UPDATE STATISTICS t1 (a) WITH NORECOMPUTE;  
```  
  
```sql  
UPDATE STATISTICS t1 (a) WITH INCREMENTAL = ON;  
```  
  
```sql  
UPDATE STATISTICS t1 (a) WITH stats_stream = 0x01;  
```  
  
## <a name="permissions"></a>权限  
 要求具有对表或视图的 `ALTER` 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-update-all-statistics-on-a-table"></a>A. 更新表的所有统计信息  
 以下示例更新 `SalesOrderDetail` 表上所有索引的统计信息。  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE STATISTICS Sales.SalesOrderDetail;  
GO  
```  
  
### <a name="b-update-the-statistics-for-an-index"></a>B. 更新索引的统计信息  
 以下示例更新 `AK_SalesOrderDetail_rowguid` 表的 `SalesOrderDetail` 索引的统计信息。  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE STATISTICS Sales.SalesOrderDetail AK_SalesOrderDetail_rowguid;  
GO  
```  
  
### <a name="c-update-statistics-by-using-50-percent-sampling"></a>C. 通过使用 50% 抽样更新统计信息  
 以下示例将创建并更新 `Name` 表中的 `ProductNumber` 和 `Product` 列的统计信息。  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE STATISTICS Products  
    ON Production.Product ([Name], ProductNumber)  
    WITH SAMPLE 50 PERCENT  
-- Time passes. The UPDATE STATISTICS statement is then executed.  
UPDATE STATISTICS Production.Product(Products)   
    WITH SAMPLE 50 PERCENT;  
```  
  
### <a name="d-update-statistics-by-using-fullscan-and-norecompute"></a>D. 通过使用 FULLSCAN 和 NORECOMPUTE 更新统计信息  
 以下示例更新 `Products` 表中的 `Product` 统计信息，强制对 `Product` 表中的所有行进行完全扫描，并关闭 `Products` 统计信息的自动统计信息功能。  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE STATISTICS Production.Product(Products)  
    WITH FULLSCAN, NORECOMPUTE;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-update-statistics-on-a-table"></a>E. 更新表的统计信息  
 以下示例更新 `Customer` 表的 `CustomerStats1` 统计信息。  
  
```sql  
UPDATE STATISTICS Customer ( CustomerStats1 );  
```  
  
### <a name="f-update-statistics-by-using-a-full-scan"></a>F. 使用完全扫描更新统计信息  
 以下示例基于扫描 `Customer` 表中的所有行来更新 `CustomerStats1` 统计信息。  
  
```sql  
UPDATE STATISTICS Customer (CustomerStats1) WITH FULLSCAN;  
```  
  
### <a name="g-update-all-statistics-on-a-table"></a>G. 更新表的所有统计信息  
 以下示例更新 `Customer` 表的所有统计信息。  
  
```sql  
UPDATE STATISTICS Customer;  
```  
  
## <a name="see-also"></a>另请参阅  
 [统计信息](../../relational-databases/statistics/statistics.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)   
 [DBCC SHOW_STATISTICS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS (Transact-SQL)](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [sp_autostats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)   
 [sp_updatestats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md)   
 [STATS_DATE (Transact-SQL)](../../t-sql/functions/stats-date-transact-sql.md)  
 [sys.dm_db_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)    
 [sys.dm_db_stats_histogram (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md)
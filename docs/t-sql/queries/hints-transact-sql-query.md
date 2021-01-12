---
description: 提示 (Transact-SQL) - 查询
title: 查询提示 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/27/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- Query_Hint_TSQL
- Query_TSQL
- Query
- Query Hint
- MAX_GRANT_PERCENT
- MAX_GRANT_PERCENT_TSQL
- MIN_GRANT_PERCENT
- MIN_GRANT_PERCENT_TSQL
- EXTERNALPUSHDOWN
- EXTERNALPUSHDOWN_TSQL
- NOLOCK_TSQL
- MAXDOP_TSQL
- USE_HINT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- REPORT PLAN query hint
- FORCE ORDER query hint
- HASH JOIN query hint
- query hints [SQL Server]
- OPTIMIZE FOR query hint
- FORCESCAN query hint
- RECOMPILE query hint
- MAXRECURSION query hint
- MERGE JOIN query hint
- HASH GROUP query hint
- KEEP PLAN query hint
- UNION query hint
- FORCESEEK query hint
- ORDER GROUP query hint
- LOOP JOIN query hint
- KEEPFIXED PLAN query hint
- FAST query hint
- MAXDOP query hint
- PARAMETERIZATION query hint
- hints [SQL Server], query
- JOIN query hint
- USE PLAN query hint
- EXPAND VIEWS query hint
- MAX_GRANT_PERCENT query hint
- MIN_GRANT_PERCENT query hint
- EXTERNALPUSHDOWN query hint
- USE HINT query hint
- QUERY_PLAN_PROFILE query hint
ms.assetid: 66fb1520-dcdf-4aab-9ff1-7de8f79e5b2d
author: pmasl
ms.author: pelopes
ms.openlocfilehash: eda240576ebb819e387e3fb93d55a06b6a1c1d9e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102371"
---
# <a name="hints-transact-sql---query"></a>提示 (Transact-SQL) - 查询
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

查询提示指定在查询范围内使用所指示的提示。 它们影响语句中的所有运算符。 如果主查询中涉及 UNION，则只有最后一个涉及 UNION 运算符的查询可以包含 OPTION 子句。 查询提示作为 [OPTION 子句](../../t-sql/queries/option-clause-transact-sql.md)的一部分指定。 如果一个或多个查询提示导致查询优化器生成无效计划，便会导致错误 8622 生成。  
  
> [!CAUTION]  
> 由于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询优化器通常会为查询选择最佳执行计划，我们建议资深开发人员和数据库管理员仅在不得已时使用提示。  
  
**适用于：**  
  
[DELETE](../../t-sql/statements/delete-transact-sql.md)  
  
[INSERT](../../t-sql/statements/insert-transact-sql.md)  
  
[SELECT](../../t-sql/queries/select-transact-sql.md)  
  
[UPDATE](../../t-sql/queries/update-transact-sql.md)  
  
[MERGE](../../t-sql/statements/merge-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
<query_hint> ::=   
{ { HASH | ORDER } GROUP   
  | { CONCAT | HASH | MERGE } UNION   
  | { LOOP | MERGE | HASH } JOIN   
  | EXPAND VIEWS   
  | FAST <integer_value>   
  | FORCE ORDER   
  | { FORCE | DISABLE } EXTERNALPUSHDOWN
  | { FORCE | DISABLE } SCALEOUTEXECUTION
  | IGNORE_NONCLUSTERED_COLUMNSTORE_INDEX  
  | KEEP PLAN   
  | KEEPFIXED PLAN  
  | MAX_GRANT_PERCENT = <numeric_value>  
  | MIN_GRANT_PERCENT = <numeric_value>  
  | MAXDOP <integer_value>   
  | MAXRECURSION <integer_value>   
  | NO_PERFORMANCE_SPOOL   
  | OPTIMIZE FOR ( @variable_name { UNKNOWN | = <literal_constant> } [ , ...n ] )  
  | OPTIMIZE FOR UNKNOWN  
  | PARAMETERIZATION { SIMPLE | FORCED }   
  | QUERYTRACEON <integer_value>   
  | RECOMPILE  
  | ROBUST PLAN   
  | USE HINT ( <use_hint_name> [ , ...n ] )
  | USE PLAN N'<xml_plan>'  
  | TABLE HINT ( <exposed_object_name> [ , <table_hint> [ [, ]...n ] ] )  
}  
  
<table_hint> ::=  
{ NOEXPAND [ , INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> ) ]  
  | INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> )
  | FORCESEEK [ ( <index_value> ( <index_column_name> [,... ] ) ) ]  
  | FORCESCAN  
  | HOLDLOCK   
  | NOLOCK   
  | NOWAIT  
  | PAGLOCK   
  | READCOMMITTED   
  | READCOMMITTEDLOCK   
  | READPAST   
  | READUNCOMMITTED   
  | REPEATABLEREAD   
  | ROWLOCK   
  | SERIALIZABLE   
  | SNAPSHOT  
  | SPATIAL_WINDOW_MAX_CELLS = <integer_value>  
  | TABLOCK   
  | TABLOCKX   
  | UPDLOCK   
  | XLOCK  
}  

<use_hint_name> ::=
{ 'ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS'
  | 'ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES'
  | 'DISABLE_BATCH_MODE_ADAPTIVE_JOINS'
  | 'DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK'
  | 'DISABLE_DEFERRED_COMPILATION_TV'
  | 'DISABLE_INTERLEAVED_EXECUTION_TVF'
  | 'DISABLE_OPTIMIZED_NESTED_LOOP'
  | 'DISABLE_OPTIMIZER_ROWGOAL'
  | 'DISABLE_PARAMETER_SNIFFING'
  | 'DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK'
  | 'DISABLE_TSQL_SCALAR_UDF_INLINING'
  | 'DISALLOW_BATCH_MODE'
  | 'ENABLE_HIST_AMENDMENT_FOR_ASC_KEYS'
  | 'ENABLE_QUERY_OPTIMIZER_HOTFIXES'
  | 'FORCE_DEFAULT_CARDINALITY_ESTIMATION'
  | 'FORCE_LEGACY_CARDINALITY_ESTIMATION'
  | 'QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n'
  | 'QUERY_PLAN_PROFILE' 
}
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
{ HASH | ORDER } GROUP  
指定查询的 GROUP BY 或 DISTINCT 子句描述的聚合应使用哈希或排序。  
  
{ MERGE | HASH | CONCAT } UNION  
指定所有 UNION 运算都通过合并、哈希或串联 UNION 集来运行。 如果指定了多个 UNION 提示，查询优化器就会从这些指定的提示中选择开销最少的策略。  
  
{ LOOP | MERGE | HASH } JOIN  
指定所有联接操作都通过整个查询中的 LOOP JOIN、MERGE JOIN 或 HASH JOIN 执行。 如果你指定了多个联接提示，优化器从可使用的联接策略中选择支出最低的。  
  
如果在同一查询对特定表对的 FROM 子句中指定联接提示，那么在联接两个表时优先使用此联接提示。 不过，仍必须遵循查询提示。 面向表对的联接提示可能只限制在查询提示中选择可用的联接方法。 有关详细信息，请参阅[联接提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-join.md)。  
  
EXPAND VIEWS  
指定展开索引视图。 此外，还指定查询优化器不将任何索引视图视为任何查询部分的替代。 当查询文本中的视图名称被视图定义替换时，视图展开。  
  
实际上，该查询提示不允许在查询计划中直接使用索引视图和直接在索引视图上使用索引。  
  
> [!NOTE]
> 如果在查询的 SELECT 部分中有对视图的直接引用，索引视图继续处于紧缩状态。 如果你指定 WITH (NOEXPAND) 或 WITH (NOEXPAND, INDEX( <index\_value> [ ,...n ] ) )，视图也继续处于紧缩状态。 有关查询提示 NOEXPAND 的详细信息，请参阅[使用 NOEXPAND](../../t-sql/queries/hints-transact-sql-table.md#using-noexpand)。  
  
提示只影响语句的 SELECT 部分中的视图（包括 INSERT、UPDATE、MERGE 和 DELETE 语句中的视图）。  
  
FAST <integer\_value>  
指定对查询进行优化，以便快速检索前 <integer\_value> 行。 此结果是非负整数。 在返回前 <integer\_value> 行后，查询继续执行并生成完整的结果集。  
  
FORCE ORDER  
指定在查询优化过程中保持由查询语法指示的联接顺序。 使用 FORCE ORDER 不影响查询优化器可能出现的角色逆转行为。  
  
> [!NOTE]  
> 在 MERGE 语句中，如果未指定 WHEN SOURCE NOT MATCHED 子句，则按照默认的联接次序，先访问源表再访问目标表。 如果指定 FORCE ORDER，则保留此默认行为。  
  
{ FORCE | DISABLE } EXTERNALPUSHDOWN  
强制或禁用向下推送 Hadoop 中符合条件的表达式的计算。 仅适用于使用 PolyBase 的查询。 不会向下推送到 Azure 存储。  

{ FORCE | DISABLE } SCALEOUTEXECUTION 强制或禁用 PolyBase 查询的横向扩展执行，这些查询使用 SQL Server 2019 大数据群集中的外部表。 仅使用 SQL 大数据群集的主实例的查询遵循此提示。 横向扩展将在大数据群集的计算池中进行。 

KEEP PLAN  
强制查询优化器对查询放宽估计的重新编译阈值。 通过运行以下语句之一对表进行了估计数目的索引列更改后，估计的重新编译阈值会开始自动重新编译查询：

* UPDATE
* DELETE
* MERGE
* INSERT

指定 KEEP PLAN 可确保不会像表有多个更新一样频繁地重新编译查询。  
  
KEEPFIXED PLAN  
强制查询优化器不因统计信息变化而重新编译查询。 指定 KEEPFIXED PLAN 可确保仅当更改基础表的架构或对这些表运行 sp_recompile 时才重新编译查询。  
  
IGNORE_NONCLUSTERED_COLUMNSTORE_INDEX       
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）  。  
  
防止查询使用非聚集内存优化列存储索引。 如果查询包含避免使用列存储索引的查询提示，而又包含支持使用列存储索引的索引提示，那么这两个提示相互冲突，导致查询返回错误。  
  
MAX_GRANT_PERCENT = <numeric_value>     
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。  

所配置的内存限制的最大内存授予大小（以百分比表示）。 查询保证不会超过此限制。 如果 Resource Governor 设置低于此提示指定的值，则实际限制可能更低。 有效值介于 0.0 和 100.0 之间。  
  
MIN_GRANT_PERCENT = <numeric_value>        
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。   

最小内存授予大小所占配置的内存限制的百分比。 查询保证会获取 `MAX(required memory, min grant)`，因为至少需要必需内存才能启动查询。 有效值介于 0.0 和 100.0 之间。  
 
MAXDOP <integer_value>      
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。  
  
替代 sp_configure 的“最大并行度”配置选项。 还会替代指定此选项的查询 Resource Governor。 MAXDOP 查询提示可以超出使用 sp_configure 配置的值。 如果 MAXDOP 超出使用 Resource Governor 配置的值，则[!INCLUDE[ssDE](../../includes/ssde-md.md)]会使用 Resource Governor MAXDOP 值（如 [ALTER WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/alter-workload-group-transact-sql.md) 中所述）。 使用 MAXDOP 查询提示时，所有和 max degree of parallelism 配置选项一起使用的语义规则均适用。 有关详细信息，请参阅 [配置 max degree of parallelism 服务器配置选项](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。  
  
> [!WARNING]     
> 如果 MAXDOP 设置为零，服务器将选择最大并行度。  
  
MAXRECURSION <integer_value>     
指定该查询允许的最大递归数。 number 是介于 0 至 32,767 之间的非负整数。 如果指定 0，则没有限制。 如果未指定此选项，服务器的默认限制为 100。  
  
如果在查询执行期间达到指定或默认的 MAXRECURSION 数量限制，查询结束并返回错误。  
  
由于此错误，该语句的所有结果都被回滚。 如果该语句为 SELECT 语句，则可能会返回部分结果或不返回结果。 所返回的任何部分结果都可能无法包括超过指定最大递归级别的递归级别上的所有行。  
  
有关详细信息，请参阅 [WITH common_table_expression (Transact-SQL)](../../t-sql/queries/with-common-table-expression-transact-sql.md)。     
  
NO_PERFORMANCE_SPOOL    
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。   
  
防止将 spool 运算符添加到查询计划（需要 spool 保证更新语义有效的计划除外）。 在某些情况下，spool 运算符可能会降低性能。 例如，如果有大量查询与 spool 操作并发运行，spool 将使用 tempdb 并会出现 tempdb 争用。  
  
OPTIMIZE FOR ( \@variable\_name { UNKNOWN | = <literal_constant> }_ [ , ...n ] )     
在编译和优化查询时指示查询优化器对局部变量使用特定值。 仅在查询优化期间使用该值，在查询执行期间不使用该值。  
  
_\@variable\_name_  
在查询中使用的局部变量的名称，可以为其分配用于 OPTIMIZE FOR 查询提示的值。  
  
UNKNOWN  
指定查询优化器在查询优化期间使用统计数据而不是初始值来确定局部变量的值。  
  
literal\_constant  
要分配给 _\@variable\_name_ 的文本常量值，以用于 OPTIMIZE FOR 查询提示。 literal\_constant 只在查询优化期间使用，在查询执行期间不用作 _\@variable\_name_ 的值。 literal\_constant 可以是任意可表达为文本常量的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型。 literal\_constant 的数据类型必须可隐式转换为 _\@variable\_name_ 在查询中引用的数据类型。  
  
OPTIMIZE FOR 可能会对优化器的默认参数检测行为起反作用。 创建计划指南时，也使用 OPTIMIZE FOR。 有关详细信息，请参阅[重新编译存储过程](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md)。  
  
OPTIMIZE FOR UNKNOWN  
在编译和优化查询时，指示查询优化器对所有列值使用谓词的平均选择性，而不使用运行时参数值。  
  
如果在同一查询提示中使用 OPTIMIZE FOR @variable_name = _literal\_constant_ 和 OPTIMIZE FOR UNKNOWN，查询优化器会使用为特定值指定的 _literal\_constant_。 查询优化器会对其余变量值使用 UNKNOWN。 这些值仅用于查询优化期间，而不会用于查询执行期间。  

PARAMETERIZATION { SIMPLE | FORCED }     
指定在编译查询时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询优化器应用于查询的参数化规则。  
  
> [!IMPORTANT]  
> 只能在计划指南中指定 PARAMETERIZATION 查询提示，用于覆盖 PARAMETERIZATION 数据库 SET 选项的当前设置。 无法直接在查询中指定它。    
> 有关详细信息，请参阅[使用计划指南指定查询参数化行为](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md)。
  
SIMPLE 用于指示查询优化器尝试进行简单参数化。 FORCED 用于指示查询优化器尝试进行强制参数化。 有关详细信息，请参阅[查询处理体系结构指南中的强制参数化](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)和[查询处理体系结构指南中的简单参数化](../../relational-databases/query-processing-architecture-guide.md#SimpleParam)。  

QUERYTRACEON <integer_value>    
使用此选项可以仅在单查询编译期间启用影响计划的跟踪标志。 与其他查询级别选项类似，你可以将选项与计划指南一起使用，以匹配从任何会话中执行的查询文本，并在编译此查询时自动应用影响计划的跟踪标志。 QUERYTRACEON 选项只能用于查询优化器跟踪标志。 有关更多信息，请参见[跟踪标记](../database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。 

> [!NOTE]  
> 如果使用了不支持的跟踪标志号，使用此选项将不会返回任何错误或警告。 如果指定的跟踪标志不是影响查询执行计划的跟踪标志，则将以无提示方式忽略该选项。

> [!NOTE]  
> 若要在查询中使用多个跟踪标志，请为每个不同的跟踪标志号指定一个 QUERYTRACEON 提示。

RECOMPILE  
指示 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 为查询生成新的临时计划，并在查询完成执行后立即放弃该计划。 如果在未指定 RECOMPILE 提示的情况下运行同一查询，生成的查询计划不会替换缓存中存储的计划。 如果未指定 RECOMPILE，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将缓存查询计划并重新使用它们。 编译查询计划时，RECOMPILE 查询提示使用查询中任何本地变量的当前值。 如果查询在存储过程内，当前值会传递给任意参数。  
  
RECOMPILE 是创建存储过程的实用替代方法。 如果必须仅重新编译存储过程中的一部分查询，而不是重新编译整个存储过程，RECOMPILE 使用 WITH RECOMPILE 子句。 有关详细信息，请参阅[重新编译存储过程](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md)。 在创建计划指南时，RECOMPILE 也很有用。  
  
ROBUST PLAN  
强制查询优化器尝试一个计划，该计划可能以性能为代价获得可能的最大行大小。 处理查询时，中间表和运算符可能需要存储和处理比任一输入行宽的行。 有时，行可能很宽，导致特定运算符无法处理行。 如果行这么宽，[!INCLUDE[ssDE](../../includes/ssde-md.md)]会在查询执行期间生成错误。 通过使用 ROBUST PLAN，可以指示查询优化器不考虑所有可能会遇到此问题的查询计划。  
  
如果此类计划不可行，则查询优化器会返回错误，而不是将错误检测延迟到查询执行阶段。 行可以包含可变长度列；[!INCLUDE[ssDE](../../includes/ssde-md.md)]允许将行大小定义为超过[!INCLUDE[ssDE](../../includes/ssde-md.md)]处理能力的最大可能的大小。 通常，应用程序存储实际大小在[!INCLUDE[ssDE](../../includes/ssde-md.md)]处理能力范围内的行，而不管最大可能大小。 如果[!INCLUDE[ssDE](../../includes/ssde-md.md)]遇到过长的行，便会返回执行错误。  
 
<a name="use_hint"></a> USE HINT ( 'hint\_name' )    
 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。
 
向查询处理器提供一个或多个附加提示。 附加提示由提示名称（放在单引号内）进行指定。   

支持以下提示名称：    
 
*  'ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS' <a name="use_hint_join_containment"></a>       
   在 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 或更新版本的查询优化器[基数估计](../../relational-databases/performance/cardinality-estimation-sql-server.md)模型下，导致 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用联接的简单包含假设而非默认的基本包含假设来生成查询计划。 此提示名与[跟踪标志](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9476 等效。 
*  'ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES' <a name="use_hint_correlation"></a>      
   会导致 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在为了进行关联而对筛选的 AND 谓词进行估值时使用最小选择性来生成计划。 与 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更早版本的基数估计模型一起使用时，此提示名等效于[跟踪标志](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4137；与 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 或更高版本的基数估计模型一起使用时，它等效于[跟踪标志](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9471。
*  'DISABLE_BATCH_MODE_ADAPTIVE_JOINS'       
   禁用批处理模式自适应联接。 有关详细信息，请参阅[批处理模式自适应联接](../../relational-databases/performance/intelligent-query-processing.md#batch-mode-adaptive-joins)。     
   适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。   
*  'DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK'       
   禁用批处理模式内存授予反馈。 有关详细信息，请参阅[批处理模式内存授予反馈](../../relational-databases/performance/intelligent-query-processing.md#batch-mode-memory-grant-feedback)。     
   适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。   
* 'DISABLE_DEFERRED_COMPILATION_TV'    
  禁用表变量延迟编译。 有关详细信息，请参阅[表变量延迟编译](../../relational-databases/performance/intelligent-query-processing.md#table-variable-deferred-compilation)。     
  适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。   
*  'DISABLE_INTERLEAVED_EXECUTION_TVF'      
   对多语句表值函数禁用交错执行。 有关详细信息，请参阅[多语句表值函数的交错执行](../../relational-databases/performance/intelligent-query-processing.md#interleaved-execution-for-mstvfs)。     
   适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。   
*  'DISABLE_OPTIMIZED_NESTED_LOOP'      
   指示查询处理器在生成查询计划时不对优化的嵌套循环联接使用排序操作（批处理排序）。 此提示名与[跟踪标志](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 2340 等效。
*  'DISABLE_OPTIMIZER_ROWGOAL' <a name="use_hint_rowgoal"></a>      
   让 SQL Server 生成计划，它不对包含以下关键字的查询使用行目标修改： 
   
   * TOP
   * OPTION (FAST N)
   * IN
   * EXISTS
   
   此提示名与[跟踪标志](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4138 等效。
*  'DISABLE_PARAMETER_SNIFFING'      
   指示查询优化器在使用一个或多个参数编译查询时，使用平均数据分布。 此指令让查询计划独立于编译查询时首次使用的参数值。 此提示名与[跟踪标志](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4136 或[数据库作用域配置](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)设置 `PARAMETER_SNIFFING = OFF` 等效。
* 'DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK'    
  禁用行模式内存授予反馈。 有关详细信息，请参阅[模式内存授予反馈](../../relational-databases/performance/intelligent-query-processing.md#row-mode-memory-grant-feedback)。      
  适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。     
* 'DISABLE_TSQL_SCALAR_UDF_INLINING'    
  禁用标量 UDF 内联。 有关详细信息，请参阅[标量 UDF 内联](../../relational-databases/user-defined-functions/scalar-udf-inlining.md)。     
  适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）  。    
* 'DISALLOW_BATCH_MODE'    
  禁用批处理模式执行。 有关详细信息，请参阅[执行模式](../../relational-databases/query-processing-architecture-guide.md#execution-modes)。     
  适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。     
*  'ENABLE_HIST_AMENDMENT_FOR_ASC_KEYS'      
   针对需要对其执行基数估计的任何前导索引列，启用自动生成的快速统计信息（直方图修正）。 用于评估基数的直方图将在查询编译时针对此列的实际最大值或最小值进行调整。 此提示名与[跟踪标志](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4139 等效。 
*  'ENABLE_QUERY_OPTIMIZER_HOTFIXES'     
   启用查询优化器修补程序（SQL Server 累积更新和服务包中发布的更改）。 此提示名与[跟踪标志](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4199 或[数据库作用域配置](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)设置 `QUERY_OPTIMIZER_HOTFIXES = ON` 等效。
*  'FORCE_DEFAULT_CARDINALITY_ESTIMATION'      
   强制查询优化器使用与当前数据库兼容级别相对应的[基数估计](../../relational-databases/performance/cardinality-estimation-sql-server.md)模型。 使用此提示替代[数据库作用域配置](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)设置 `LEGACY_CARDINALITY_ESTIMATION = ON` 或[跟踪标志](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9481。
*  'FORCE_LEGACY_CARDINALITY_ESTIMATION' <a name="use_hint_ce70"></a>      
   强制查询优化器使用 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更早版本的[基数估计](../../relational-databases/performance/cardinality-estimation-sql-server.md)模型。 此提示名与[跟踪标志](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9481 或[数据库作用域配置](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)设置 `LEGACY_CARDINALITY_ESTIMATION = ON` 等效。
*  'QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n'          
 强制查询优化器行为在查询级别发生。 此行为就像使用数据库兼容性级别 n 编译查询，其中，n 是受支持的数据库兼容性级别（例如，100、130 等）。 请参阅 [sys.dm_exec_valid_use_hints](../../relational-databases/system-dynamic-management-views/sys-dm-exec-valid-use-hints-transact-sql.md)，以了解目前对 n 支持的值。      
   **适用范围**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（自 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU10 起）。    

   > [!NOTE]
   > QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n 提示不替代默认或旧版基数估计设置（如果它是通过数据库范围内配置、跟踪标志或 QUERYTRACEON 等其他查询提示强制执行的话）。   
   > 此提示仅影响查询优化器的行为。 它不影响可能依赖[数据库兼容性级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)的其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能（如某些数据库功能的可用性）。  
   > 若要了解有关此提示的详细信息，请参阅[Developer's Choice:Hinting Query Execution model](/archive/blogs/sql_server_team/developers-choice-hinting-query-execution-model)（开发人员的选择：提示查询执行模型）。
    
*  'QUERY_PLAN_PROFILE'      
 启用用于查询的轻型分析。 当包含此新提示的查询完成时，会触发一个新扩展事件：query_plan_profile。 此扩展事件公开执行统计信息和实际执行计划 XML，类似于 query_post_execution_showplan 扩展事件，但仅针对包含新提示的查询。    
   适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 CU3 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU11 开始）。 

   > [!NOTE]
   > 如果启用收集 query_post_execution_showplan 扩展事件，会向在服务器上运行的每个查询添加标准分析基础结构，因此可能会影响服务器的总体性能。      
   > 如果启用 query_thread_profile 扩展事件集合以改为使用轻量分析基础结构，这将在很大程度上减少性能开销，但仍会影响服务器的总体性能。       
   > 如果启用 query_plan_profile 扩展事件，将只启用通过 QUERY_PLAN_PROFILE 执行的查询的轻量分析基础结构，因此不会影响服务器上的其他工作负载。 使用此提示来分析特定查询，而不会影响服务器工作负载的其他部分。
   > 若要了解有关此轻量分析的详细信息，请参阅[查询分析基础结构](../../relational-databases/performance/query-profiling-infrastructure.md)。
 
可以使用动态管理视图 [sys.dm_exec_valid_use_hints](../../relational-databases/system-dynamic-management-views/sys-dm-exec-valid-use-hints-transact-sql.md) 查询所有受支持的 USE HINT 名称的列表。    

> [!TIP]
> 提示名称不区分大小写。   
  
> [!IMPORTANT] 
> 某些 USE HINT 提示可能与在全局或会话级别启用的跟踪标志或与数据库作用域配置设置存在冲突。 在这种情况下，查询级别提示 (USE HINT) 将始终优先。 如果 USE HINT 与另一个查询提示或在查询级别启用（例如由 QUERYTRACEON 启用）的跟踪标志存在冲突，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在尝试执行查询时生成错误。 

<a name="use-plan"></a> USE PLAN N'<xml\_plan>'  
 强制查询优化器为查询使用由 **'** _xml\_plan_ **'** 指定的现有查询计划。 不能使用 INSERT、UPDATE、MERGE 或 DELETE 语句来指定 USE PLAN。  
  
TABLE HINT (<exposed\_object\_name> [ , \<table_hint> [ [, ]...n ] ] ) 将指定的表提示应用于与 exposed\_object\_name 对应的表或视图。 我们建议仅在[计划指南](../../relational-databases/performance/plan-guides.md)的上下文中将表提示用作查询提示。  
  
 <exposed\_object\_name> 可以是下面的引用之一：  
  
-   如果在查询的 [FROM](../../t-sql/queries/from-transact-sql.md) 子句中对表或视图使用别名，exposed\_object\_name 就是别名。  
  
-   如果不使用别名，exposed\_object\_name 与 FROM 子句中引用的表或视图完全匹配。 例如，如果使用包含两部分的名称引用表或视图，exposed\_object\_name 就是这个包含两部分的名称。  
  
 如果在指定 exposed\_object\_name 时未指定表提示，在查询中指定为属于对象的表提示的任何索引都会遭忽略。 然后，查询优化器确定索引使用情况。 如果无法修改原始查询，可以使用此方法来消除 INDEX 表提示的影响。 请参阅示例 J。  
  
\<table_hint> ::= { NOEXPAND [ , INDEX ( <index\_value> [ ,...n ] ) | INDEX = ( <index\_value> ) ] \| INDEX ( <index\_value> [ ,...n ] ) | INDEX = ( <index\_value> ) \| FORCESEEK [(<index\_value>(<index\_column\_name> [, ... ] )) ] \| FORCESCAN \| HOLDLOCK \| NOLOCK \| NOWAIT \| PAGLOCK \| READCOMMITTED \| READCOMMITTEDLOCK \| READPAST \| READUNCOMMITTED \| REPEATABLEREAD \| ROWLOCK \| SERIALIZABLE \| SNAPSHOT \| SPATIAL_WINDOW_MAX_CELLS = <integer\_value> \| TABLOCK \| TABLOCKX \| UPDLOCK \| XLOCK }    
要作为查询提示应用于与 exposed_object_name 对应的表或视图的表提示。 有关这些提示的说明，请参阅[表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)。  
  
 不允许将非 INDEX、FORCESCAN 和 FORCESEEK 的表提示用作查询提示，除非该查询已经具有一个指定该表提示的 WITH 子句。 有关详细信息，请参阅[“备注”部分](#remarks)。  
  
> [!CAUTION]
> 指定带参数的 FORCESEEK 限制查询优化器可以考虑的计划数大于指定不带参数的 FORCESEEK 时的计划数。 这可能导致在更多情况下出现“无法生成计划”错误。 在未来的版本中，对查询优化器进行内部修改后可允许考虑更多计划。  
  
## <a name="remarks"></a>备注  
 只有在 INSERT 语句中使用了 SELECT 子句时，才能在该语句中指定查询提示。  
  
 只能在顶级查询中指定查询提示，不能在子查询指定。 将表提示指定为查询提示时，可以在顶级查询或子查询中指定提示。 不过，在 TABLE HINT 子句中为 <exposed\_object\_name> 指定的值必须与查询或子查询中的公开名称完全匹配。  
  
## <a name="specifying-table-hints-as-query-hints"></a>将表提示指定为查询提示  
 我们建议仅在[计划指南](../../relational-databases/performance/plan-guides.md)的上下文中将 INDEX、FORCESCAN 或 FORCESEEK 表提示用作查询提示。 如果无法修改原始查询（例如，由于它是第三方应用程序），就会发现计划指南很有用。 计划指南中指定的查询提示在查询编译和优化前添加到查询中。 对于即席查询，仅在测试计划指南语句时才应使用 TABLE HINT 子句。 对于所有其他即席查询，建议仅将这些提示指定为表提示。  
  
 如果将 INDEX、FORCESCAN 和 FORCESEEK 表提示指定为查询提示，它们会对以下对象有效：  
  
-   表  
-   视图  
-   索引视图  
-   公用表表达式（必须在其结果集填充公用表表达式的 SELECT 语句中指定提示）  
-   动态管理视图 (DMV)
-   命名子查询  
  
可以将 INDEX、FORCESCAN 和 FORCESEEK 表提示指定为，没有任何现有表提示的查询的查询提示。 还可以使用它们分别替换查询中的现有 INDEX、FORCESCAN 或 FORCESEEK 提示。 

不允许将非 INDEX、FORCESCAN 和 FORCESEEK 的表提示用作查询提示，除非该查询已经具有一个指定该表提示的 WITH 子句。 在这种情况下，还必须将匹配提示指定为查询提示。 在 OPTION 子句中使用 TABLE HINT，将匹配提示指定为查询提示。 此规范保留了查询语义。 例如，如果查询包含表提示 NOLOCK，则计划指南的 \@hints 参数中的 OPTION 子句必须也包含 NOLOCK 提示。 请参见示例 K。 
  
## <a name="examples"></a>示例  
  
### <a name="a-using-merge-join"></a>A. 使用 MERGE JOIN  
 下面的示例指定，MERGE JOIN 在查询中运行 JOIN 操作。 该示例使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库。  
  
```sql  
SELECT *   
FROM Sales.Customer AS c  
INNER JOIN Sales.CustomerAddress AS ca ON c.CustomerID = ca.CustomerID  
WHERE TerritoryID = 5  
OPTION (MERGE JOIN);  
GO    
```  
  
### <a name="b-using-optimize-for"></a>B. 使用 OPTIMIZE FOR  
 以下示例指示查询优化器在优化查询时对 `@city_name` 使用值 `'Seattle'`，并对 `@postal_code` 的所有列值使用谓词的平均选择性。 该示例使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库。  
  
```sql  
CREATE PROCEDURE dbo.RetrievePersonAddress
@city_name NVARCHAR(30),  
 @postal_code NVARCHAR(15)
AS
SELECT * FROM Person.Address  
WHERE City = @city_name AND PostalCode = @postal_code  
OPTION ( OPTIMIZE FOR (@city_name = 'Seattle', @postal_code UNKNOWN) );  
GO
```  
  
### <a name="c-using-maxrecursion"></a>C. 使用 MAXRECURSION  
 可以使用 MAXRECURSION 来防止不合理的递归公用表表达式进入无限循环。 下面的示例特意创建了一个无限循环，然后使用 MAXRECURSION 提示将递归级别限制为两级。 该示例使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库。  
  
```sql  
--Creates an infinite loop  
WITH cte (CustomerID, PersonID, StoreID) AS  
(  
    SELECT CustomerID, PersonID, StoreID  
    FROM Sales.Customer  
    WHERE PersonID IS NOT NULL  
  UNION ALL  
    SELECT cte.CustomerID, cte.PersonID, cte.StoreID  
    FROM cte   
    JOIN  Sales.Customer AS e   
        ON cte.PersonID = e.CustomerID  
)  
--Uses MAXRECURSION to limit the recursive levels to 2  
SELECT CustomerID, PersonID, StoreID  
FROM cte  
OPTION (MAXRECURSION 2);  
GO  
```  
  
 在更正代码错误之后，就不再需要 MAXRECURSION。  
  
### <a name="d-using-merge-union"></a>D. 使用 MERGE UNION  
 以下示例使用 MERGE UNION 查询提示。 该示例使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库。  
  
```sql  
SELECT *  
FROM HumanResources.Employee AS e1  
UNION  
SELECT *  
FROM HumanResources.Employee AS e2  
OPTION (MERGE UNION);  
GO  
```  
  
### <a name="e-using-hash-group-and-fast"></a>E. 使用 HASH GROUP 和 FAST  
 以下示例使用 HASH GROUP 和 FAST 查询提示。 该示例使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库。  
  
```sql  
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total  
FROM Sales.SalesOrderDetail  
WHERE UnitPrice < $5.00  
GROUP BY ProductID, OrderQty  
ORDER BY ProductID, OrderQty  
OPTION (HASH GROUP, FAST 10);  
GO    
```  
  
### <a name="f-using-maxdop"></a>F. 使用 MAXDOP  
 以下示例使用 MAXDOP 查询提示。 该示例使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库。  
  
```sql  
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total  
FROM Sales.SalesOrderDetail  
WHERE UnitPrice < $5.00  
GROUP BY ProductID, OrderQty  
ORDER BY ProductID, OrderQty  
OPTION (MAXDOP 2);    
GO
```  
  
### <a name="g-using-index"></a>G. 使用 INDEX  
 以下示例使用 INDEX 提示。 第一个示例指定了一个索引。 第二个示例为单个表引用指定多个索引。 在这两个示例中，由于对使用别名的表应用了 INDEX 提示，因此 TABLE HINT 子句还必须将相同的别名指定为公开的对象名称。 该示例使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库。  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide1',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e, INDEX (IX_Employee_ManagerID)))';  
GO  
EXEC sp_create_plan_guide   
    @name = N'Guide2',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e, INDEX(PK_Employee_EmployeeID, IX_Employee_ManagerID)))';  
GO    
```  
  
### <a name="h-using-forceseek"></a>H. 使用 FORCESEEK  
 下面的示例使用 FORCESEEK 表提示。 TABLE HINT 子句还必须指定与公开的对象名称相同的名称（包含两部分）。 将 INDEX 提示应用于名称包含两部分的表时指定名称。 该示例使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库。  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide3',   
    @stmt = N'SELECT c.LastName, c.FirstName, HumanResources.Employee.Title  
              FROM HumanResources.Employee  
              JOIN Person.Contact AS c ON HumanResources.Employee.ContactID = c.ContactID  
              WHERE HumanResources.Employee.ManagerID = 3  
              ORDER BY c.LastName, c.FirstName;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT( HumanResources.Employee, FORCESEEK))';  
GO    
```  
  
### <a name="i-using-multiple-table-hints"></a>I. 使用多个表提示  
 下面的示例将 INDEX 提示应用到一个表，将 FORCESEEK 提示应用到另一个表。 该示例使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库。  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide4',   
    @stmt = N'SELECT e.ManagerID, c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, INDEX( IX_Employee_ManagerID))   
                       , TABLE HINT (c, FORCESEEK))';  
GO  
```  
  
### <a name="j-using-table-hint-to-override-an-existing-table-hint"></a>J. 使用 TABLE HINT 覆盖现有的表提示  
下面的示例展示了如何使用 TABLE HINT 提示。 使用提示时，可以不指定提示替代在查询的 FROM 子句中指定的 INDEX 表提示行为。 该示例使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库。  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide5',   
    @stmt = N'SELECT e.ManagerID, c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e WITH (INDEX (IX_Employee_ManagerID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e))';  
GO    
```  
  
### <a name="k-specifying-semantics-affecting-table-hints"></a>K. 指定语义影响的表提示  
以下示例在查询中包含了NOLOCK 和 INDEX 这两个表提示，其中前者会影响语义，后者不影响语义。 若要保留查询的语义，应在计划指南的 OPTIONS 子句中指定 NOLOCK 提示。 与 NOLOCK 提示一起，在语句编译和优化期间指定 INDEX 和 FORCESEEK 提示，并替换查询中不影响语义的 INDEX 提示。 该示例使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库。  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide6',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
                   WITH (NOLOCK, INDEX (PK_Employee_EmployeeID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',  
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, INDEX(IX_Employee_ManagerID), NOLOCK, FORCESEEK))';  
GO    
```  
  
下面的示例演示另一种保留查询语义并使优化器能够选择并非在表提示中指定的索引的方法。 允许优化器通过在 OPTIONS 子句中指定 NOLOCK 提示来进行选择。 指定提示是因为它会影响语义。 然后，指定只有表引用而无 INDEX 提示的 TABLE HINT 关键字。 该示例使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库。  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide7',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
                   WITH (NOLOCK, INDEX (PK_Employee_EmployeeID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',  
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, NOLOCK))';  
GO  
```  

### <a name="l-using-use-hint"></a>L. 使用 USE HINT  
 以下示例使用 RECOMPILE 和 USE HINT 查询提示。 该示例使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库。  
  
```sql  
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (RECOMPILE, USE HINT ('ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES', 'DISABLE_PARAMETER_SNIFFING')); 
GO  
```  

### <a name="m-using-querytraceon-hint"></a>M. 使用 QUERYTRACEON HINT  
 以下示例使用 QUERYTRACEON 查询提示。 该示例使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库。 可以使用以下查询，为特定查询启用跟踪标志 4199 控制的所有影响计划的修补程序：
  
```sql  
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (QUERYTRACEON 4199);
```  

 还可以使用多个跟踪标志，如以下查询中所示：

```sql
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION  (QUERYTRACEON 4199, QUERYTRACEON 4137);
```

## <a name="see-also"></a>另请参阅  
[提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql.md)   
[sp_create_plan_guide (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
[sp_control_plan_guide (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)  
[跟踪标志](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)       
[Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)      

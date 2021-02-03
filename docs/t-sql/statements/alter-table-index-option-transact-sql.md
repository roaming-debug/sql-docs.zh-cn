---
description: ALTER TABLE index_option (Transact-SQL)
title: " | Microsoft Docs"
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- index_option
ms.assetid: 8a14f12d-2fbf-4036-b8b2-8db3354e0eb7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1df9583fb18c87693074769b099151281c6136f8
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237056"
---
# <a name="alter-table-index_option-transact-sql"></a>ALTER TABLE index_option (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  指定一组可应用于某个索引的选项，该索引是使用 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) 创建的约束定义的一部分。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
{   
    PAD_INDEX = { ON | OFF }  
  | FILLFACTOR = fillfactor  
  | IGNORE_DUP_KEY = { ON | OFF }  
  | STATISTICS_NORECOMPUTE = { ON | OFF }  
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS = { ON | OFF } 
  | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF } 
  | SORT_IN_TEMPDB = { ON | OFF }   
  | ONLINE = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
  | DATA_COMPRESSION = { NONE |ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE }  
      [ ON PARTITIONS ( { <partition_number_expression> | <range> }   
      [ , ...n ] ) ]  
  | ONLINE = { ON [ ( <low_priority_lock_wait> ) ] | OFF }  
}  
  
<range> ::=   
<partition_number_expression> TO <partition_number_expression>  
  
<single_partition_rebuild__option> ::=  
{  
    SORT_IN_TEMPDB = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
  | DATA_COMPRESSION = {NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE } }  
  | ONLINE = { ON [ ( <low_priority_lock_wait> ) ] | OFF }  
}  
  
<low_priority_lock_wait>::=  
{  
    WAIT_AT_LOW_PRIORITY ( MAX_DURATION = <time> [ MINUTES ] ,   
                           ABORT_AFTER_WAIT = { NONE | SELF | BLOCKERS } )   
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 PAD_INDEX = { ON | OFF }   
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指定索引填充。 默认为 OFF。  
  
 ON  
 FILLFACTOR 指定的可用空间百分比应用于索引的中间级页。  
  
 OFF 或未指定 fillfactor  
 考虑到中间级页上的键集，可以将中间级页几乎填满，但至少要为最大索引行留出足够空间。  
  
 FILLFACTOR =fillfactor  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指定一个百分比，指示在[!INCLUDE[ssDE](../../includes/ssde-md.md)]创建或修改索引的过程中，应将每个索引页面的叶级填充到什么程度。 指定的值必须是 1 到 100 之间的整数。 默认值为 0。  
  
> [!NOTE]  
>  填充因子值 0 和 100 在所有方面都是相同的。  
  
 IGNORE_DUP_KEY = { ON | OFF }   
 指定在插入操作尝试向唯一索引插入重复键值时的响应类型。 IGNORE_DUP_KEY 选项仅适用于创建或重新生成索引后发生的插入操作。 当执行 [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)、[ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) 或 [UPDATE](../../t-sql/queries/update-transact-sql.md) 时，该选项无效。 默认为 OFF。  
  
 ON  
 向唯一索引插入重复键值时会出现警告消息。 只有违反唯一性约束的行才会失败。  
  
 OFF  
 向唯一索引插入重复键值时会出现错误消息。 整个 INSERT 操作将会回滚。  
  
 对于对视图创建的索引、非唯一索引、XML 索引、空间索引以及筛选的索引，IGNORE_DUP_KEY 不能设置为 ON。  
  
 若要查看 IGNORE_DUP_KEY，请使用 [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)。  
  
 在向后兼容的语法中，WITH IGNORE_DUP_KEY 等效于 WITH IGNORE_DUP_KEY = ON。  
  
 STATISTICS_NORECOMPUTE = { ON | OFF }   
 指定是否重新计算统计信息。 默认为 OFF。  
  
 ON  
 不会自动重新计算过时的统计信息。  
  
 OFF  
 启用统计信息自动更新功能。  
  
 ALLOW_ROW_LOCKS = { ON | OFF }   
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指定是否允许行锁。 默认值为 ON。  
  
 ON  
 在访问索引时允许使用行锁。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]确定何时使用行锁。  
  
 OFF  
 不使用行锁。  
  
 ALLOW_PAGE_LOCKS = { ON | OFF }   
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指定是否允许使用页锁。 默认值为 ON。  
  
 ON  
 在访问索引时允许使用页锁。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]确定何时使用页锁。  
  
 OFF  
 不使用页锁。  

 OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }

**适用于**：[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 及更高版本。

指定是否针对最后一页插入争用进行优化。 默认为 OFF。 有关详细信息，请参阅“CREATE INDEX”页的[顺序键](./create-index-transact-sql.md#sequential-keys)部分。
 
 SORT_IN_TEMPDB = { ON | OFF }   
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指定是否将排序结果存储在 tempdb 中。 默认为 OFF。  
  
 ON  
 在 tempdb 中存储用于生成索引的中间排序结果。 如果 tempdb 与用户数据库不在同一组磁盘上，就可缩短创建索引所需的时间。 但是，这会增加索引生成期间所使用的磁盘空间量。  
  
 OFF  
 中间排序结果与索引存储在同一数据库中。  
  
 ONLINE = { ON | OFF }   
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指定在索引操作期间基础表和关联的索引是否可用于查询和数据修改操作。 默认为 OFF。 REBUILD 可作为 ONLINE 操作执行。  
  
> [!NOTE]  
>  不能联机创建唯一的非聚集索引。 这包括由于 UNIQUE 或 PRIMARY KEY 约束而创建的索引。  
  
 ON  
 在索引操作期间不持有长期表锁。 在索引操作的主要阶段，源表上只使用意向共享 (IS) 锁。 这使得能够继续对基础表和索引进行查询或更新。 操作开始时，在很短的时间内对源对象持有共享 (S) 锁。 操作结束时，如果创建非聚集索引，将在短期内获取对源的 S（共享）锁；当联机创建或删除聚集索引时，以及重新生成聚集或非聚集索引时，将在短期内获取 SCH-M（架构修改）锁。 但联机索引锁是短的元数据锁，特别是 Sch-M 锁必须等待此表上的所有阻塞事务完成。 在等待期间，Sch-M 锁在访问同一表时阻止在此锁后等待的所有其他事务。 对本地临时表创建索引时，ONLINE 不能设置为 ON。  
  
> [!NOTE]  
>  联机索引重新生成可以设置本节稍后介绍的 low_priority_lock_wait 选项。 在联机索引重新生成期间，low_priority_lock_wait 管理 S 和 Sch-M 锁优先级。  
  
 OFF  
 在索引操作期间应用表锁。 这样可以防止所有用户在操作期间访问基础表。 创建、重新生成或删除聚集索引或者重新生成或删除非聚集索引的脱机索引操作将对表获取架构修改 (Sch-M) 锁。 这样可以防止所有用户在操作期间访问基础表。 创建非聚集索引的脱机索引操作将对表获取共享 (S) 锁。 这样可以防止更新基础表，但允许读操作（如 SELECT 语句）。  
  
 有关详细信息，请参阅[联机索引操作的工作方式](../../relational-databases/indexes/how-online-index-operations-work.md)。  
  
> [!NOTE]
>  在 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的各版本中均不提供联机索引操作。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
 MAXDOP =max_degree_of_parallelism  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 在索引操作期间替代 max degree of parallelism 配置选项。 有关详细信息，请参阅 [配置 max degree of parallelism 服务器配置选项](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。 使用 MAXDOP 可以限制在执行并行计划的过程中使用的处理器数量。 最大数量为 64 个处理器。  
  
 max_degree_of_parallelism 可以是：  
  
 - 1 - 取消生成并行计划。  
 - \>1 - 将并行索引操作中使用的最大处理器数量限制为指定数量。  
 - 0（默认值）- 根据当前系统工作负荷使用实际数量的处理器或更少数量的处理器。  
  
 有关详细信息，请参阅 [配置并行索引操作](../../relational-databases/indexes/configure-parallel-index-operations.md)。  
  
> [!NOTE]
>  并非在 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的每个版本中均支持并行索引操作。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
 DATA_COMPRESSION  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 为指定的表、分区号或分区范围指定数据压缩选项。 选项如下：  
  
 无  
 不压缩表或指定的分区。 仅适用于行存储表；不适用于列存储表。  
  
 ROW  
 使用行压缩来压缩表或指定的分区。 仅适用于行存储表；不适用于列存储表。  
  
 PAGE  
 使用页压缩来压缩表或指定的分区。 仅适用于行存储表；不适用于列存储表。  
  
 COLUMNSTORE  
 **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。  
  
 仅适用于列存储表。 COLUMNSTORE 指定对使用 COLUMNSTORE_ARCHIVE 选项压缩的分区进行解压缩。 还原数据时，将继续通过用于所有列存储表的列存储压缩对 COLUMNSTORE 索引进行压缩。  
  
 COLUMNSTORE_ARCHIVE  
 **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。  
  
 仅适用于列存储表，这是使用聚集列存储索引存储的表。 COLUMNSTORE_ARCHIVE 会进一步将指定分区压缩到更小。 这可用于存档，或者用于要求更少存储并且可以付出更多时间来进行存储和检索的其他情形  
  
 有关压缩的详细信息，请参阅[数据压缩](../../relational-databases/data-compression/data-compression.md)。  
  
ON PARTITIONS ( { \<partition_number_expression> | \<range> } [ ,...n ] ) 适用于：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本  。  
  
 指定对其应用 DATA_COMPRESSION 设置的分区。 如果表未分区，ON PARTITIONS 参数将生成错误。 如果不提供 ON PARTITIONS 子句，DATA_COMPRESSION 选项将应用于已分区表的所有分区。  
  
可以通过下列方式指定 \<partition_number_expression>：  
  
-   提供分区号，例如：ON PARTITIONS (2)。  
-   为多个单独分区提供分区号，用逗号分隔，例如：ON PARTITIONS (1, 5)。  
-   同时提供范围和单独分区，例如：ON PARTITIONS (2, 4, 6 TO 8)。  
  
\<range> 可指定为由单词 TO 隔开的分区号，例如：ON PARTITIONS (6 TO 8)。  
  
 若要为不同分区设置不同的数据压缩类型，请多次指定 DATA_COMPRESSION 选项，例如：  
  
```sql  
--For rowstore tables  
REBUILD WITH   
(  
DATA_COMPRESSION = NONE ON PARTITIONS (1),   
DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),   
DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)  
)  
  
--For columnstore tables  
REBUILD WITH   
(  
DATA_COMPRESSION = COLUMNSTORE ON PARTITIONS (1, 3, 5),   
DATA_COMPRESSION = COLUMNSTORE_ARCHIVE ON PARTITIONS (2, 4, 6 TO 8)  
)  
```  
  
**\<single_partition_rebuild__option>**  
 在大多数情况下，重新生成索引将重新生成已分区索引的所有分区。 下面的选项在应用于单个分区时不会重新生成所有分区。  
  
-   SORT_IN_TEMPDB  
-   MAXDOP  
-   DATA_COMPRESSION  
  
**low_priority_lock_wait**  
 **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。  
  
 当此表没有阻塞操作时，SWITCH 或联机索引重新生成便已完成。 WAIT_AT_LOW_PRIORITY 指示如果 SWITCH 或联机索引重新生成操作由于其他阻塞操作而无法立即完成，则进行等待。 该操作持有低优先级锁，允许持有与 DDL 语句冲突的锁的其他操作继续进行。 省略 WAIT AT LOW PRIORITY 选项与 `WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 0 minutes, ABORT_AFTER_WAIT = NONE)` 等效。  
  
MAX_DURATION = time [MINUTES ]  
 执行 DDL 命令时 SWITCH 或必须获取的联机索引重新生成锁将等待的时间（以分钟为单位指定的整数值）。 SWITCH 或联机索引重新生成操作试图立即完成。 如果操作被阻塞的时间达到 MAX_DURATION，则将执行某一 ABORT_AFTER_WAIT 操作 。 MAX_DURATION 时间始终以分钟为单位，MINUTES 一词可以省略 。  
  
ABORT_AFTER_WAIT = [NONE | SELF | BLOCKERS } ]    
 无  
 不更改锁优先级（使用常规优先级），继续 SWITCH 或联机索引重新生成操作。  
  
SELF  
 不采取任何操作，直接退出当前执行的 SWITCH 或联机索引重新生成 DDL 操作。  
  
BLOCKERS  
 终止阻塞当前 SWITCH 或联机索引重新生成 DDL 操作的所有用户事务以使操作可以继续。  
 BLOCKERS 要求具有 ALTER ANY CONNECTION 权限。  
  
## <a name="remarks"></a>备注  
 有关索引选项的完整说明，请参阅 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [column_constraint (Transact-SQL)](../../t-sql/statements/alter-table-column-constraint-transact-sql.md)   
 [computed_column_definition (Transact-SQL)](../../t-sql/statements/alter-table-computed-column-definition-transact-sql.md)   
 [table_constraint &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)  
  
 

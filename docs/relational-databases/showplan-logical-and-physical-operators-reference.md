---
description: Showplan 逻辑运算符和物理运算符参考
title: 显示计划逻辑运算符和物理运算符参考
ms.custom: seo-dt-2019
ms.date: 10/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.showplan.leftouterjoin.f1
- sql13.swb.showplan.remotedelete.f1
- sql13.swb.showplan.parallelism.f1
- sql13.swb.showplan.indexspool.f1
- sql13.swb.showplan.result.f1
- sql13.swb.showplan.bitmapcreate.f1
- sql13.swb.showplan.remotescan.f1
- sql13.swb.showplan.union.f1
- sql13.swb.showplan.bitmap.f1
- sql13.swb.showplan.RIDLookup
- sql13.swb.showplan.innerjoin.f1
- sql13.swb.showplan.dynamic.f1
- sql13.swb.showplan.distributestreams.f1
- sql13.swb.showplan.clusteredindexdelete.f1
- sql13.swb.showplan.keylookup.f1
- sql13.swb.showplan.partialaggregate.f1
- sql13.swb.showplan.distinctsort.f1
- sql13.swb.showplan.collapse.f1
- sql13.swb.showplan.print.f1
- sql13.swb.showplan.crossjoin.f1
- sql13.swb.showplan.convert.f1
- sql13.swb.showplan.split.f1
- sql13.swb.showplan.top.f1
- sql13.swb.showplan.update.f1
- sql13.swb.showplan.keyset.f1
- sql13.swb.showplan.fetchquery.f1
- sql13.swb.showplan.mergejoin.f1
- sql13.swb.showplan.branchrepartition.f1
- sql13.swb.showplan.tableinsert.f1
- sql13.swb.showplan.clusteredindexseek.f1
- sql13.swb.showplan.indexupdate.f1
- sql13.swb.showplan.indexinsert.f1
- sql13.swb.showplan.clusteredindexupdate.f1
- sql13.swb.showplan.streamaggregate.f1
- sql13.swb.showplan.columnstoreindexdelete.f1
- sql13.swb.showplan.snapshot.f1
- sql13.swb.showplan.remotequery.f1
- sql13.swb.showplan.constantscan.f1
- sql13.swb.showplan.rank.f1
- sql13.swb.showplan.rightsemijoin.f1
- sql13.swb.showplan.delete.f1
- sql13.swb.showplan.sequence.f1
- sql13.swb.showplan.locate.f1
- sql13.swb.showplan.aggregate.f1
- sql13.swb.showplan.rightouterjoin.f1
- sql13.swb.showplan.columnstoreindexupdate.f1
- sql13.swb.showplan.clusteredindexinsert.f1
- sql13.swb.showplan.rowcountspool.f1
- sql13.swb.showplan.columnstoreindexscan.f1
- sql13.swb.showplan.leftantisemijoin.f1
- sql13.swb.showplan.sort.f1
- sql13.swb.showplan.leftsemijoin.f1
- sql13.swb.showplan.columnstoreindexinsert.f1
- sql13.swb.showplan.indexscan.f1
- sql13.swb.showplan.columnstoreindexmerge.f1
- sql13.swb.showplan.lazyspool.f1
- sql13.swb.showplan.rightantisemijoin.f1
- sql13.swb.showplan.bookmarklookup.f1
- sql13.swb.showplan.remoteinsert.f1
- sql13.swb.showplan.intrinsic.f1
- sql13.swb.showplan.arithmeticexpression.f1
- sql13.swb.showplan.populationquery.f1
- sql13.swb.showplan.filter.f1
- sql13.swb.showplan.if.f1
- sql13.swb.showplan.hashmatchteam.f1
- sql13.swb.showplan.tablevaluedfunction.f1
- sql13.swb.showplan.assign.f1
- sql13.swb.showplan.nestedloops.f1
- sql13.swb.showplan.buildhash.f1
- sql13.swb.showplan.mergeinterval.f1
- sql13.swb.showplan.hashmatch.f1
- sql13.swb.showplan.parametertablescan.f1
- sql13.swb.showplan.tablemerge.f1
- sql13.swb.showplan.switch.f1
- sql13.swb.showplan.sql.f1
- sql13.swb.showplan.repartitionstreams.f1
- sql13.swb.showplan.logrowscan.f1
- sql13.swb.showplan.assert.f1
- sql13.swb.showplan.computescalar.f1
- sql13.swb.showplan.broadcast.f1
- sql13.swb.showplan.indexseek.f1
- sql13.swb.showplan.gatherstreams.f1
- sql13.swb.showplan.remoteindexscan.f1
- sql13.swb.showplan.segment.f1
- sql13.swb.showplan.tableupdate.f1
- sql13.swb.showplan.clusteredindexscan.f1
- sql13.swb.showplan.cache.f1
- sql13.swb.showplan.spool.f1
- sql13.swb.showplan.indexdelete.f1
- sql13.swb.showplan.distinct.f1
- sql13.swb.showplan.deletedscan.f1
- sql13.swb.showplan.eagerspool.f1
- sql13.swb.showplan.hashmatchroot.f1
- sql13.swb.showplan.setfunction.f1
- sql13.swb.showplan.clusteredindexmerge.f1
- sql13.swb.showplan.flowdistinct.f1
- sql13.swb.showplan.tabledelete.f1
- sql13.swb.showplan.tablescan.f1
- sql13.swb.showplan.refreshquery.f1
- sql13.swb.showplan.tablespool.f1
- sql13.swb.showplan.insertedscan.f1
- sql13.swb.showplan.insert.f1
- sql13.swb.showplan.remoteindexseek.f1
- sql13.swb.showplan.fullouterjoin.f1
- sql13.swb.showplan.declare.f1
- sql13.swb.showplan.udx.f1
- sql13.swb.showplan.while.f1
- sql13.swb.showplan.remoteupdate.f1
- sql13.swb.showplan.concatenation.f1
- sql13.swb.showplan.computescalar
- sql13.swb.showplan.foreignkeyreferencescheck
helpviewer_keywords:
- execution plans [SQL Server], operators
- ActualRows attribute
- reading execution plan output
- ActualRewinds attribute
- ActualEndOfScans attribute
- query tuning [SQL Server]
- mapping operators [SQL Server]
- operators [Database Engine query tuning]
- logical operators [SQL Server], execution plans
- logical operators [SQL Server], listed
- physical operators [SQL Server]
- ActualRebinds attribute
- execution plans [SQL Server], reading output
ms.assetid: e43fd0fe-5ea7-4ffe-8d52-759ef6a7c361
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7d5ef357580cf28d4173ebd945e5f683d2fd1911
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473248"
---
# <a name="showplan-logical-and-physical-operators-reference"></a>Showplan 逻辑运算符和物理运算符参考
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
  运算符说明了 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 如何执行查询或数据操作语言 (DML) 语句。 查询优化器使用运算符生成查询计划，以创建在查询中指定的结果或执行在 DML 语句中指定的操作。 查询计划是由物理运算符组成的一个树。 您可以使用 SET SHOWPLAN 语句、 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]中的图形执行计划选项或 SQL Server Profiler Showplan 事件类来查看查询计划。  
  
 运算符分为逻辑运算符和物理运算符。  
  
 **逻辑运算符**  
 逻辑运算符描述了用于处理语句的关系代数操作。 换言之，逻辑运算符从概念上描述了需要执行哪些操作。  
  
 **物理运算符**  
 物理运算符实施由逻辑运算符描述的操作。 每个物理运算符都是一个执行某项操作的对象或例程。 例如，某些物理运算符可访问表、索引或视图中的列或行。 其他物理运算符执行其他操作，如计算、聚合、数据完整性检查或联接。 物理运算符具有与其关联的开销。  
  
 物理运算符初始化、收集数据，然后关闭。 具体来讲，物理运算符可以响应下列三种方法调用：  
  
-   **Init()** ：Init() 方法使物理运算符初始化自身并设置所有需要的数据结构。 尽管一个物理运算符通常只接收一次 **Init()** 调用，但也可以接收许多次调用。  
  
-   **GetNext()** ：GetNext() 方法使物理运算符获得数据的第一行或后续行。 物理运算符可以不接收 **GetNext()** 调用，也可以接收许多次调用。  
  
-   **Close()** ：Close() 方法使物理运算符执行某些清除操作，然后关闭。 一个物理运算符只接收一个 **Close()** 调用。  
  
GetNext() 方法返回一个数据行，它的调用次数作为 ActualRows 显示在使用  `SET STATISTICS PROFILE ON` 或 `SET STATISTICS XML ON` 生成的显示计划输出中。 有关这些 SET 选项的详细信息，请参阅 [SET STATISTICS PROFILE (Transact-SQL)](../t-sql/statements/set-statistics-profile-transact-sql.md) 和 [SET STATISTICS XML (Transact-SQL)](../t-sql/statements/set-statistics-xml-transact-sql.md)。  
  
显示计划输出中显示的 **ActualRebinds** 和 **ActualRewinds** 计数是指 **Init()** 方法被调用的次数。 除非运算符位于嵌套循环联接的内侧，否则 ActualRebinds 等于一， ActualRewinds 等于零。  如果运算符位于循环联接的内侧，那么重新绑定次数和重绕次数之和应等于联接外侧所处理的行数。 重新绑定意味着联接的一个或多个相关参数发生更改后，必须重新计算联接的内侧。 重绕意味着任何相关参数都没有发生更改，可以重用之前的内侧结果集。  
  
**ActualRebinds** 和 **ActualRewinds** 显示在使用 SET STATISTICS XML ON 生成的 XML 显示计划输出中。 它们只为 **Nonclustered Index Spool**、 **Remote Query**、 **Row Count Spool**、 **Sort**、 **Table Spool** 和 **Table-valued Function** 运算符填充。 如果 **ActualRebinds** 属性设置为 TRUE，也会为 **ActualRebinds** 和 **ActualRebinds** 属性设置为 TRUE，也会为 **ActualRebinds** 和 **ActualRewinds** 。  
  
当 **ActualRebinds** 和 **ActualRewinds** 显示在 XML 显示计划中时，它们可以与 **EstimateRebinds** 和 **EstimateRewinds** 相比较。 如果它们没有显示，则预计的行数 (**EstimateRows**) 可以与实际的行数 (**ActualRows**) 相比较。 注意，如果它们没有显示，实际的图形显示计划输出中将实际的重新绑定次数和重绕次数均显示为零。  
  
只有在显示计划输出是使用 SET STATISTICS XML ON 生成的情况下，相关计数器 **ActualEndOfScans** 才可用。 只要物理运算符到达其数据流的结尾，此计数器就增加一。 物理运算符可以到达其数据流结尾零次、一次或多次。 对于重新绑定次数和重绕次数，只有在运算符位于循环联接的内侧时，扫描结束的次数才可以多于一次。 扫描结束的次数应少于或等于重新绑定次数与重绕次数之和。  
  
## <a name="mapping-physical-and-logical-operators"></a>映射物理运算符和逻辑运算符  
 查询优化器可以创建一个查询计划，该计划由逻辑运算符组成的树表示。 查询优化器创建计划后，将为每个逻辑运算符选择最有效的物理运算符。 查询优化器使用基于开销的方法确定将实施逻辑运算符的物理运算符。  
  
 通常，一个逻辑运算符可由多个物理运算符实施。 但是在少数情况下，一个物理运算符也可以实施多个逻辑操作符。  
  
## <a name="operator-descriptions"></a>操作说明  
 本节介绍了各个逻辑运算符和物理运算符。  

 > [!TIP]
 > 如果给定图形执行计划图标包含一个带有两个从右到左箭头的黄色圆圈，则表示运算符并行执行。 有关并行的详细信息，请参阅[线程和任务体系结构指南](../relational-databases/thread-and-task-architecture-guide.md#sql-server-task-scheduling)。
  
|图形执行计划图标|Showplan 运算符|说明|  
|-----------------------------------|-----------------------|-----------------|  
|![自适应联接运算符图标](../relational-databases/media/AdaptiveJoin.gif "自适应联接运算符图标")|**自适应联接**|通过“自适应联接”运算符，在扫描第一个输入后可延迟选择哈希联接或嵌套循环联接方法。 “自适应联接”运算符是一个物理运算符。 有关详细信息，请参阅[理解自适应联接](../relational-databases/performance/joins.md#adaptive)。 | 
|无|**聚合**|**Aggregate** 运算符计算包含 MIN、MAX、SUM、COUNT 或 AVG 的表达式。 **Aggregate** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Arithmetic Expression 运算符图标](../relational-databases/media/arithmetic-expression-32x-2.gif "Arithmetic Expression 运算符图标")|**Arithmetic Expression**|**Arithmetic Expression** 运算符根据行中的现有值计算新值。 **中未使用** 算术表达式 [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]| 
|无|**Async Concat**|Async Concat 运算符仅用于远程查询中（分布式查询）。 它有 *n* 个子节点和一个父节点。 通常，某些子节点是参与分布式查询的远程计算机。 Asnyc Concat 同时向所有子节点发出 `open()` 调用，然后将位图应用于每个子节点。 对于为 1 的每个位， **Async Concat** 按需向父节点发送输出行。| 
|![Assert 运算符图标](../relational-databases/media/assert-32x.gif "Assert 运算符图标")|**ActualRebinds**|**Assert** 运算符用于验证条件。 例如，验证引用完整性或确保标量子查询返回一行。 对于每个输入行， **Assert** 运算符都要计算执行计划的 **Argument** 列中的表达式。 如果此表达式的值为 NULL，则通过 **Assert** 运算符传递该行，并且查询执行将继续。 如果此表达式的值非 Null，则将产生相应的错误。 **Assert** 运算符是一个物理运算符。| 
|![Assign 语言元素图标](../relational-databases/media/assign-32.gif "Assign 语言元素图标")|**Assign**|**Assign** 运算符将表达式的值或常量分配给变量。 **Assign** 是一个语言元素。| 
|![Bitmap 运算符图标](../relational-databases/media/bitmap-32x.gif "Bitmap 运算符图标")|**Bitmap Create**|**Bitmap Create** 运算符出现在创建位图的显示计划输出中。 **Bitmap Create** 是一个逻辑运算符。| 
|![Bitmap 运算符图标](../relational-databases/media/bitmap-32x.gif "Bitmap 运算符图标")|**Bitmap**|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 使用 **Bitmap** 运算符来实现并行查询计划中的位图筛选。 在将行传递给另一个运算符（如 **Parallelism** 运算符）之前，通过消除无法生成任何联接记录的键值的行，位图筛选可提高查询的执行速度。 位图筛选器使用运算符树某部分的表中一组值的简洁表示形式来筛选位于该树另一部分的第二张表中的行。 通过在查询中预先删除不必要的行，后续运算符将处理较少的行，从而提高查询的整体性能。 优化器将确定位图的选择性何时可满足使用条件以及在哪些运算符上应用筛选器。 **Bitmap** 是一个物理运算符。| 
|![Bookmark Lookup 运算符图标](../relational-databases/media/bookmark-lookup-32x.gif "Bookmark Lookup 运算符图标")|**Bookmark Lookup**|**Bookmark Lookup** 运算符使用书签（行 ID 或聚集键）在表或聚集索引中查找相应的行。 **Argument** 列包含书签标签，用于在表或聚集索引内查找行。 **Argument** 列还包含要查找的行所在的表或聚集索引的名称。 如果 **Argument** 列中出现 WITH PREFETCH 子句，则表示查询处理器已决定在表或聚集索引中查找书签时将使用异步预提取（预读）作为最佳选择。<br /><br /> 从 [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] 开始，便不再使用 Bookmark Lookup。 而由 Key Lookup 和 RID Lookup 提供书签查找功能 。| 
|无|**Branch Repartition**|在并行查询计划中，有时存在迭代器的概念性区域。 此类区域中的所有迭代器都可通过并行线程执行。 这些区域本身必须串行执行。 单个区域内的某些 **Parallelism** 迭代器称为 **Branch Repartition**。 两个这样的区域边界上的 **Parallelism** 迭代器称为 **Segment Repartition**。 **Branch Repartition** 和 **Segment Repartition** 是逻辑运算符。| 
|无|**Broadcast**|**Broadcast** 有一个子节点和 *n* 个父节点。 **Broadcast** 根据使用者的请求将其输入行发送给多个使用者。 每个使用者都将获得所有行。 例如，如果所有使用者都是哈希联接的生成端，则将生成 *n* 份哈希表。| 
|![Build hash 运算符图标](../relational-databases/media/build-hash.gif "Build hash 运算符图标")|**Build Hash**|指示为 xVelocity 内存优化的列存储索引生成批处理哈希表。| 
|无|**缓存**|**Cache** 是一个专门的 **Spool** 运算符。 它仅存储一行数据。 **Cache** 是一个逻辑运算符。 **中不使用** Cache [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]。| 
|![Clustered Index Delete 运算符图标](../relational-databases/media/clustered-index-delete-32x.gif "Clustered Index Delete 运算符图标")|**Clustered Index Delete**|**Clustered Index Delete** 运算符从查询执行计划的 Argument 列中指定的聚集索引中删除行。 如果 Argument 列中存在 WHERE:() 谓词，则仅删除满足该谓词的行。**Clustered Index Delete** 是一个物理运算符。| 
|![Clustered Index Insert 运算符图标](../relational-databases/media/clustered-index-insert-32x.gif "Clustered Index Insert 运算符图标")|**Clustered Index Insert**|**Clustered Index Insert** Showplan 运算符可将行从其输入插入到 Argument 列中所指定的聚集索引中。 Argument 列还包含一个 SET:() 谓词，用于指示为每一列设置的值。 如果 **Clustered Index Insert** 的插入值没有子项，则插入的行来自 **Insert** 运算符本身。**Clustered Index Insert** 是一个物理运算符。| 
|![Clustered Index Merge 运算符](../relational-databases/media/clustered-index-merge-32x.gif "Clustered Index Delete 运算符图标")|**Clustered Index Merge**|**Clustered Index Merge** 运算符可将合并数据流应用于聚集索引。 该运算符可在其 **Argument** 列中所指定的聚集索引中删除、更新或插入行。 执行的实际操作取决于该运算符的 **Argument** 列中指定的 **ACTION** 列的运行时值。 **Clustered Index Merge** 是一个物理运算符。| 
|![Clustered Index Scan 运算符图标](../relational-databases/media/clustered-index-scan-32x.gif "Clustered Index Insert 运算符图标")|**Clustered Index Scan**|**Clustered Index Scan** 运算符会扫描查询执行计划的 Argument 列中指定的聚集索引。 存在可选 WHERE:() 谓词时，则只返回满足该谓词的那些行。 如果 Argument 列包含 ORDERED 子句，则表示查询处理器已请求按聚集索引排列行的顺序返回行输出。 如果没有 ORDERED 子句，存储引擎将以最佳方式扫描索引，而无需对输出进行排序。 **Clustered Index Scan** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Clustered Index Seek 运算符图标](../relational-databases/media/clustered-index-seek-32x.gif "Clustered Index Seek 运算符图标")|**Clustered Index Seek**|**Clustered Index Seek** 运算符可以利用索引的查找功能从聚集索引中检索行。 **Argument** 列包含所使用的聚集索引名称和 SEEK:() 谓词。 存储引擎仅使用索引来处理满足此 SEEK:() 谓词的行。 它还包括 WHERE:() 谓词，其中存储引擎对满足 SEEK:() 谓词的所有行进行计算，但此操作是可选的，并且不使用索引来完成此过程。<br /><br /> 如果 **Argument** 列包含 ORDERED 子句，则表示查询处理器已决定必须按聚集索引排序行的顺序返回行。 如果没有 ORDERED 子句，存储引擎将以最佳方式搜索索引，而不对输出进行必要的排序。 若允许输出保持顺序，则效率可能比生成非排序输出的效率低。 出现关键字 LOOKUP 时，将执行书签查找。 在 [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] 和更高版本中， **Key Lookup** 运算符提供书签查找功能。 **Clustered Index Seek** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Clustered Index Update 运算符图标](../relational-databases/media/clustered-index-update-32x.gif "Clustered Index Insert 运算符图标")|**Clustered Index Update**|**Clustered Index Update** 运算符更新 **Argument** 列中指定的聚集索引中的输入行。如果存在 WHERE:() 谓词，则只更新那些满足此谓词的行。 如果存在 SET:() 谓词，则将每个更新的列设置为该值。 如果存在 DEFINE:() 谓词，则列出此运算符定义的值。 可以在 SET 子句中、该运算符内的其他位置和该查询内的其他位置引用这些值。 **Clustered Index Update** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Collapse 运算符图标](../relational-databases/media/collapse-32x.gif "Collapse 运算符图标")|**折叠**|**Collapse** 运算符用于优化更新处理。 执行更新时，可以将该更新操作拆分（使用 **Split** 运算符）成为删除和插入操作。 **Argument** 列包含一个指定键列列表的 GROUP BY:() 子句。 如果查询处理器遇到删除和插入相同键值的相邻行，则会用一个更有效的更新操作替换这些单独的操作。 **Collapse** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Columnstore Index Scan](../relational-databases/media/columnstoreindexscan.gif "Columnstore Index Scan")|**Columnstore Index Scan**|**Columnstore Index Scan** 运算符会扫描查询执行计划的 **Argument** 列中指定的列存储索引。| 
|![Compute Scalar 运算符图标](../relational-databases/media/compute-scalar-32x.gif "Compute Scalar 运算符图标")|**Compute Scalar**|**Compute Scalar** 运算符通过对表达式求值来生成计算标量值。 该值可以返回给用户、在查询中的其他位置引用或二者皆可。 例如，在筛选谓词或联接谓词中就会出现二者皆可的情况。 **Compute Scalar** 既是一个逻辑运算符，也是一个物理运算符。<br /><br /> 在 SET STATISTICS XML 生成的显示计划中出现的 **Compute Scalar** 运算符可能不包含 **RunTimeInformation** 元素。 在图形显示计划中，当已在 **中选中**“包括实际的执行计划” **选项时，** “实际行” **、** “实际重新绑定次数” **和** “实际重绕次数” **可能不会出现在** “属性” [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]窗口中。 当出现这种情况时，意味着虽然编译过的查询计划中使用了这些运算符，但在运行时查询计划中，它们的作用是由其他运算符实现的。 另外，请注意，SET STATISTICS PROFILE 生成的显示计划输出中的执行数等于 SET STATISTICS XML 生成的显示计划中的重新绑定次数和重绕次数的总和。| 
|![Concatenation 运算符图标](../relational-databases/media/concatenation-32x.gif "Concatenation 运算符图标")|**Concatenation**|**Concatenation** 运算符扫描多个输入，并返回每个扫描的行。 **Concatenation** 通常用于实现 [!INCLUDE[tsql](../includes/tsql-md.md)] UNION ALL 结构。 **Concatenation** 物理运算符有两个或多个输入，有一个输出。 Concatenation 将行从第一个输入流复制到输出流，然后对其他输入流重复进行此操作。 **Concatenation** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Constant Scan 运算符图标](../relational-databases/media/constant-scan-32x.gif "Constant Scan 运算符图标")|**Constant Scan**|**Constant Scan** 运算符可将一个或多个常量行引入到查询中。 **Compute Scalar** 运算符通常在 **Constant Scan** 之后使用，以将列添加到 **Constant Scan** 运算符生成的行中。| 
|![Convert（数据库引擎）语言元素图标](../relational-databases/media/convert-32x.gif "Convert（数据库引擎）语言元素图标")|**转换**|**Convert** 运算符将标量数据类型转换为另一种类型。 **Convert** 是一个语言元素。| 
|无|**Cross Join**|**Cross Join** 运算符将第一个（顶端）输入中的每一行与第二个（底端）输入中的每一行联接在一起。 **Cross Join** 是一个逻辑运算符。| 
|无|**游标**|**Cursor** 逻辑运算符和物理运算符用于描述涉及游标操作的查询或更新的执行方式。 其中物理运算符描述用于处理游标（如使用键集驱动游标）的物理实现算法。 游标执行过程的每一步都涉及物理运算符。 而逻辑运算符描述游标的属性，如游标是只读。<br /><br /> 逻辑运算符包括 Asynchronous、Optimistic、Primary、Read Only、Scroll Locks、Secondary 和 Synchronous。<br /><br /> 物理运算符包括 Dynamic、Fetch Query、Keyset、Population Query、Refresh Query 和 Snapshot。| 
|![Cursor catchall cursor 运算符图标](../relational-databases/media/cursor-catch-all.gif "Cursor catchall cursor 运算符图标")|**catchall**|生成图形显示计划的逻辑找不到迭代器的合适图标时，将显示通用图标。 通用图标不一定指示存在错误。 有三种通用图标：蓝色（用于迭代器）、橙色（用于游标）和绿色（用于 [!INCLUDE[tsql](../includes/tsql-md.md)] 语言元素）。| 
|![Declare 语言元素图标](../relational-databases/media/declare-32x.gif "Declare 语言元素图标")|**Declare**|Declare 运算符用于分配查询计划中的局部变量。 **Declare** 是一个语言元素。| 
|![Delete（数据库引擎）运算符图标](../relational-databases/media/delete-32x.gif "Delete（数据库引擎）运算符图标")|**删除**|**Delete** 运算符将从对象中删除满足 **Argument** 列内的可选谓词的行。| 
|![Delete Scan 运算符图标](../relational-databases/media/delete-scan-32x.gif "Delete Scan 运算符图标")|**Deleted Scan**|**Deleted Scan** 运算符在触发器中扫描删除的表。| 
|无|**Distinct Sort**|**Distinct Sort** 逻辑运算符将对输入进行扫描，删除重复项并按 **Argument** 列的 DISTINCT ORDER BY:() 谓词中指定的列进行排序。 **Distinct Sort** 是一个逻辑运算符。| 
|无|**Distinct**|**Distinct** 运算符可以从行集或值集中删除重复项。 **Distinct** 是一个逻辑运算符。| 
|![Distribute Streams parallelism 运算符图标](../relational-databases/media/parallelism-distribute-stream.gif "Distribute Streams Parallelism 运算符图标")|**Distribute Streams**|**Distribute Streams** 运算符仅用于并行查询计划。 **Distribute Streams** 运算符接收记录的单个输入流，并生成多个输出流。 记录的内容和格式不会改变。 输入流中的每个记录都将在某个输出流中显示。 此运算符在输出流中自动保留输入记录的相对顺序。 通常情况下，使用哈希操作确定特定输入记录所属的输出流。<br /><br /> 如果将输出分区，那么 **Argument** 列会包含 PARTITION COLUMNS:() 谓词和分区依据列。 **Distribute Streams** 是一个逻辑运算符。| 
|![Dynamic Cursor 运算符图标](../relational-databases/media/dynamic-32x.gif "Dynamic Cursor 运算符图标")|**Dynamic**|**Dynamic** 运算符使用可以查看其他游标所做的任何更改的游标。| 
|![Fetch Query Cursor 运算符图标](../relational-databases/media/fetch-query-32x.gif "Fetch Query Cursor 运算符图标")|**Fetch Query**|当对游标发出提取命令时， **Fetch Query** 运算符将检索行。| 
|![Filter（数据库引擎）运算符图标](../relational-databases/media/filter-32x.gif "Filter（数据库引擎）运算符图标")|**筛选器**|**Filter** 运算符扫描输入，仅返回那些符合 **Argument** 列中的筛选表达式（谓词）的行。| 
|无|**Flow Distinct**|**Flow Distinct** 逻辑运算符用于通过扫描输入来删除重复项。 虽然 **Distinct** 运算符在生成任何输出前使用所有的输入，但 **FlowDistinct** 运算符在从输入获得行时会返回每行（除非该行是一个重复项，若是这样则删除该行）。| 
|![Foreign Key References Check 运算符图标](../relational-databases/media/fk-references-32x.gif "Foreign Key References Check 运算符图标")|**Foreign Key References Check**|Foreign Key References Check 运算符通过将修改行与引用表中的行进行比较，就地执行引用完整性检查，以便验证修改不会中断引用完整性。 当相同主键或唯一键上存在超过 253 个外键引用时，将使用 Foreign Key References Check 运算符。 Foreign Key References Check 是逻辑和物理运算符。| 
|无|**Full Outer Join**|**Full Outer Join** 逻辑运算符从第一个（顶端）输入与第二个（底端）输入相联接的行中返回每个满足联接谓词的行。 它还可以从下列输入返回行：<br /><br /> \- 在第二个输入中没有匹配项的第一个输入。<br /><br /> \- 在第一个输入中没有匹配项的第二个输入。<br /><br /> 不包含匹配值的输入将作为空值返回。 **Full Outer Join** 是一个逻辑运算符。| 
|![Gather Streams parallelism 运算符图标](../relational-databases/media/parallelism-32x.gif "Gather Streams Parallelism 运算符图标")|**Gather Streams**|**Gather Streams** 运算符仅用在并行查询计划中。 **Gather Streams** 运算符处理几个输入流并通过组合这几个输入流生成单个记录输出流。 记录的内容和格式不会改变。 如果此运算符保留顺序，则所有的输入流都必须有序。 如果输出已排序，则 **Argument** 列会包含一个 ORDER BY:() 谓词和正在排序的列名称。 **Gather Streams** 是一个逻辑运算符。| 
|![Hash Match 运算符图标](../relational-databases/media/hash-match-32x.gif "Hash Match 运算符图标")|**Hash Match**|**Hash Match** 运算符通过计算其生成输入中每行的哈希值生成哈希表。 HASH:() 谓词以及一个用于创建哈希值的列的列表会出现在 **Argument** 列中。 然后，该谓词为每个探测行（如果适用）计算哈希值（使用相同的哈希函数）并在哈希表内查找匹配项。 如果存在残留谓词（由 **Argument** 列中的 RESIDUAL:() 标识），则还须满足此残留谓词，只有这样行才能被视为是匹配项。 行为取决于所执行的逻辑操作：<br /><br /> \- 对于联接，使用第一个（顶端）输入生成哈希表，使用第二个（底端）输入探测哈希表。 按联接类型规定的模式输出匹配项（或不匹配项）。 如果多个联接使用相同的联接列，这些操作将分组为一个哈希组。<br /><br /> \- 对于非重复或聚合运算符，使用输入生成哈希表（删除重复项并计算聚合表达式）。 生成哈希表时，扫描该表并输出所有项。<br /><br /> \- 对于 union 运算符，使用第一个输入生成哈希表（删除重复项）。 使用第二个输入（它必须没有重复项）探测哈希表，返回所有没有匹配项的行，然后扫描该哈希表并返回所有项。<br />**Hash Match** 是一个物理运算符。 有关详细信息，请参阅[理解哈希联接](../relational-databases/performance/joins.md#hash)。| 
|![If 语言元素图标](../relational-databases/media/if-32x.gif "If 语言元素图标")|**If**|**If** 运算符执行基于表达式的有条件处理。 **If** 是一个语言元素。| 
|无|**Inner Join**|**Inner Join** 逻辑运算符返回满足联接第一个（顶端）输入与第二个（底端）输入的每一行。| 
|![Insert（数据库引擎）运算符图标](../relational-databases/media/insert-32x.gif "Insert（数据库引擎）运算符图标")|**插入**|**Insert** 逻辑运算符将每行从其输入插入 **Argument** 列内指定的对象中。 相应的物理运算符为 **Table Insert**、 **Index Insert** 或 **Clustered Index Insert** 运算符。| 
|![Inserted Scan 运算符图标](../relational-databases/media/inserted-scan-32x.gif "Inserted Scan 运算符图标")|**Inserted Scan**|**Inserted Scan** 运算符扫描 **插入的** 表。 **Inserted Scan** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Intrinsic 语言元素图标](../relational-databases/media/intrinsic-32x.gif "Intrinsic 语言元素图标")|**Intrinsic**|**Intrinsic** 运算符调用内部 [!INCLUDE[tsql](../includes/tsql-md.md)] 函数。 **Intrinsic** 是一个语言元素。| 
|![Iterator Catchall 运算符图标](../relational-databases/media/iterator-catch-all.gif "Iterator Catchall 运算符图标")|**迭代器**|生成图形显示计划的逻辑找不到 **Iterator** 的合适图标时，将显示通用图标。 通用图标不一定指示存在错误。 有三种通用图标：蓝色（用于迭代器）、橙色（用于游标）和绿色（用于 [!INCLUDE[tsql](../includes/tsql-md.md)] 语言构造）。| 
|![Bookmark Lookup 运算符图标](../relational-databases/media/bookmark-lookup-32x.gif "Bookmark Lookup 运算符图标")|**Key Lookup**|**Key Lookup** 运算符是在具有聚集索引的表上进行的书签查找。 **Argument** 列包含聚集索引的名称和用来在聚集索引中查找行的聚集键。 **Key Lookup** 通常带有 **Nested Loops** 运算符。 如果 **Argument** 列中出现 WITH PREFETCH 子句，则表示查询处理器已决定在聚集索引中查找书签时将使用异步预提取（预读）作为最佳选择。<br /><br /> 在查询计划中使用 **Key Lookup** 运算符表明该查询可能会从性能优化中获益。 例如，添加涵盖索引可能会提高查询性能。| 
|![Keyset Cursor 运算符图标](../relational-databases/media/keyset-32x.gif "Keyset Cursor 运算符图标")|**Keyset**|**Keyset** 运算符使用的游标可用于查看其他用户所做的更新，而不能查看其他用户所做的插入。| 
|![语言元素通用图标](../relational-databases/media/language-construct-catch-all.gif "语言元素通用图标")|**Language 元素**|生成图形显示计划的逻辑找不到 **Language Element** 的合适图标时，将显示通用图标。 通用图标不一定指示存在错误。 有三种通用图标：蓝色（用于迭代器）、橙色（用于游标）和绿色（用于 [!INCLUDE[tsql](../includes/tsql-md.md)] 语言构造）。| 
|无|**Left Anti Semi Join**|当第二个（底端）输入中没有匹配行时， **Left Anti Semi Join** 运算符则返回第一个（顶端）输入中的每一行。 如果 **Argument** 列内不存在任何联接谓词，则每行都是一个匹配行。 **Left Anti Semi Join** 是一个逻辑运算符。| 
|无|**Left Outer Join**|**Left Outer Join** 运算符返回满足联接第一个（顶端）输入与第二个（底端）输入的每一行。 它还返回任何在第二个输入中没有匹配行的第一个输入中的行。 第二个输入中的非匹配行作为空值返回。 如果 **Argument** 列内不存在任何联接谓词，则每行都是一个匹配行。 **Left Outer Join** 是一个逻辑运算符。| 
|无|**Left Semi Join**|当第二个（底端）输入中没有匹配行时， **Left Semi Join** 运算符则返回第一个（顶端）输入中的每一行。 如果 **Argument** 列内不存在任何联接谓词，则每行都是一个匹配行。 **Left Semi Join** 是一个逻辑运算符。| 
|![Log Row Scan 运算符图标](../relational-databases/media/log-row-scan-32x.gif "Log Row Scan 运算符图标")|**Log Row Scan**|**Log Row Scan** 运算符用于扫描事务日志。 **Log Row Scan** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Merge Interval 运算符图标](../relational-databases/media/merge-interval-32x.gif "Merge Interval 运算符图标")|**Merge Interval**|**Merge Interval** 运算符可合并多个（可能重叠的）间隔以得出最小的不重叠间隔，然后将其用于查找索引项。 此运算符通常出现在 **Constant Scan** 运算符中的一个或多个 **Compute Scalar** 运算符上方，Constant Scan 运算符构造了此运算符所合并的间隔（表示为一行中的列）。 **Merge Interval** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Merge Join 运算符图标](../relational-databases/media/merge-join-32x.gif "Merge Join 运算符图标")|**合并联接**|**Merge Join** 运算符执行内部联接、左外部联接、左半部联接、左反半部联接、右外部联接、右半部联接、右反半部联接和联合逻辑运算。<br /><br /> 在 **Argument** 列中，如果操作执行一对多联接，则 **Merge Join** 运算符将包含 MERGE:() 谓词；如果操作执行多对多联接，则该运算符将包含 MANY-TO-MANY MERGE:() 谓词。 **Argument** 列还包含一个用于执行操作的列的列表，该列表以逗号分隔。 **Merge Join** 运算符要求在各自的列上对两个输入进行排序，这可以通过在查询计划中插入显式排序操作来实现。 如果不需要显式排序（例如，如果数据库内有合适的 B 树索引或可以对多个操作（如合并联接和对汇总分组）使用排序顺序），则合并联接尤其有效。 **Merge Join** 是一个物理运算符。 有关详细信息，请参阅[理解合并联接](../relational-databases/performance/joins.md#merge)。| 
|![Nested Loops 运算符图标](../relational-databases/media/nested-loops-32x.gif "Nested Loops 运算符图标")|**Nested Loops**|**Nested Loops** 运算符执行内部联接、左外部联接、左半部联接和左反半部联接逻辑运算。 嵌套循环联接通常使用索引，针对外部表的每一行在内部表中执行搜索。 查询处理器根据预计的开销来决定是否对外部输入进行排序，以改进内部输入索引上的搜索定位。 将基于所执行的逻辑操作返回所有满足 **Argument** 列中的（可选）谓词的行。 如果 OPTIMIZED 特性设置为“True”，则表示使用了优化的嵌套循环（或批处理排序）。 **Nested Loops** 是一个物理运算符。 有关详细信息，请参阅[了解嵌套循环联接](../relational-databases/performance/joins.md#nested_loops)。| 
|![Nonclustered Index Delete 运算符图标](../relational-databases/media/nonclust-index-delete-32x.gif "Nonclustered Index Delete 运算符图标")|**Nonclustered Index Delete**|**Nonclustered Index Delete** 运算符通过 **Argument** 列中指定的非聚集索引删除输入行。 **Nonclustered Index Delete** 是一个物理运算符。| 
|![Nonclustered Index Insert 运算符图标](../relational-databases/media/nonclust-index-insert-32x.gif "Nonclustered Index Insert 运算符图标")|**Index Insert**|**Index Insert** 运算符用于将行从其输入插入到 **Argument** 列中指定的非聚集索引中。 **Argument** 列还包含一个 SET:() 谓词，用于指示为每一列设置的值。 **Index Insert** 是一个物理运算符。| 
|![Nonclustered Index Scan 运算符图标](../relational-databases/media/nonclustered-index-scan-32x.gif "Nonclustered Index Scan 运算符图标")|**Index Scan**|**Index Scan** 运算符从 **Argument** 列中指定的非聚集索引中检索所有行。 如果可选的 WHERE:() 谓词出现在 **Argument** 列中，则仅返回满足此谓词的那些行。 **Index Scan** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Nonclustered Index Seek 运算符图标](../relational-databases/media/index-seek-32x.gif "Nonclustered Index Seek 运算符图标")|**Index Seek**|**Index Seek** 运算符利用索引的查找功能从非聚集索引中检索行。 **Argument** 列包含所使用的非聚集索引的名称。 它还包括 SEEK:() 谓词。 存储引擎仅使用索引来处理满足 SEEK:() 谓词的行。 它可能还包含一个 WHERE:() 谓词，其中存储引擎对满足 SEEK:() 谓词的所有行进行计算（不使用索引来完成）。 如果 **Argument** 列包含 ORDERED 子句，则表示查询处理器已决定必须按非聚集索引排序行的顺序返回行。 如果没有 ORDERED 子句，则存储引擎将以最佳方式（不保证对输出排序）搜索索引。 如果让输出保持其顺序，则效率可能低于生成非排序输出。 **Index Seek** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Nonclustered Index Spool 运算符图标](../relational-databases/media/index-spool-32x.gif "Nonclustered Index Spool 运算符图标")|**Index Spool**|**Index Spool** 物理运算符在 **Argument** 列中包含 SEEK:() 谓词。 **Index Spool** 运算符扫描其输入行，将每行的副本放置在隐藏的假脱机文件（存储在 **tempdb** 数据库中且只在查询的生存期内存在）中，并在这些行上生成非聚集索引。 这样可以使用索引的查找功能来仅输出那些满足 SEEK:() 谓词的行。 如果重绕该运算符（例如通过 **Nested Loops** 运算符重绕），但不需要任何重新绑定，则将使用假脱机数据，而不用重新扫描输入。| 
|![Nonclustered Index Update 运算符图标](../relational-databases/media/nonclust-index-update-32x.gif "Nonclustered Index Update 运算符图标")|**Nonclustered Index Update**|**Nonclustered Index Update** 物理运算符用于更新 **Argument** 列内指定的非聚集索引中的输入行。 如果存在 SET:() 谓词，则将每个更新的列设置为该值。 **Nonclustered Index Update** 是一个物理运算符。| 
|![Online Index Insert 运算符图标](../relational-databases/media/online-index-32x.gif "Online Index Insert 运算符图标")|**Online Index Insert**|**Online Index Insert** 物理运算符指示索引创建、更改或删除操作是在线执行的。 也就是说，基础表数据在索引操作期间仍然对用户可用。| 
|无|**Parallelism**|<a name="exchange"></a> Parallelism 运算符（或交换迭代器）执行分发流、收集流和对流重新分区逻辑操作。 **Argument** 列可以包含一个 PARTITION COLUMNS:() 谓词和一个以逗号分隔的分区列的列表。 **Argument** 列还可以包含一个 ORDER BY:() 谓词，以列出分区过程中要保留排序顺序的列。 **Parallelism** 是物理运算符。 有关 Parallelism 运算符的详细信息，请参阅 [Craig Freedman 的博客系列](/archive/blogs/craigfr/the-parallelism-operator-aka-exchange)。<br /><br />**注意：** 如果查询已编译为并行查询，但在运行时作为串行查询运行，则通过 SET STATISTICS XML 或使用 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 中的“包括实际执行计划”选项生成的显示计划输出将不会包含 Parallelism 运算符的 RunTimeInformation 元素  。 在 SET STATISTICS PROFILE 输出中，为 **Parallelism** 运算符显示的实际行计数和实际执行数将为零。 出现任何一种情况时，都意味着 **Parallelism** 运算符只在查询编译时使用，未在运行时查询计划中使用。 请注意，如果服务器上的并发负荷很高，则并行查询计划有时会以串行方式运行。| 
|![Parameter Table Scan 运算符图标](../relational-databases/media/parameter-table-scan-32x.gif "Parameter Table Scan 运算符图标")|**Parameter Table Scan**|**Parameter Table Scan** 运算符扫描在当前查询中用作参数的表。 该运算符一般用于存储过程内的 INSERT 查询。 **Parameter Table Scan** 既是一个逻辑运算符，也是一个物理运算符。| 
|无|**Partial Aggregate**|**Partial Aggregate** 用于并行计划中。 它将聚合功能应用到尽可能多的输入行中，以便不必执行向磁盘写入数据的操作（称为“溢出”）。 **Hash Match** 是实现分区聚合的唯一一个物理运算符（迭代器）。 **Partial Aggregate** 是一个逻辑运算符。| 
|![Population Query Cursor 运算符图标](../relational-databases/media/poulation-query-32x.gif "Population Query Cursor 运算符图标")|**Population Query**|**Population Query** 运算符在打开游标时填充游标的工作表。| 
|![Refresh Query Cursor 运算符图标](../relational-databases/media/refresh-query-32x.gif "Refresh Query Cursor 运算符图标")|**Refresh Query**|**Refresh Query** 运算符为提取缓冲区中的行提取当前数据。| 
|![Remote Delete 运算符图标](../relational-databases/media/remote-delete-32x.gif "Remote Delete 运算符图标")|**Remote Delete**|**Remote Delete** 运算符用于从远程对象中删除输入行。 **Remote Delete** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Remote Index Scan Showplan 运算符](../relational-databases/media/remote-index-scan-32x.gif "Remote Index Scan Showplan 运算符")|**Remote Index Scan**|**Remote Index Scan** 运算符可以扫描在 Argument 列中指定的远程索引。 **Remote Index Scan** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Remote Index Seek Showplan 运算符](../relational-databases/media/remote-index-seek-32x.gif "Remote Index Seek Showplan 运算符")|**Remote Index Seek**|**Remote Index Seek** 运算符利用远程索引对象的查找功能来检索行。 **Argument** 列包含所使用的远程索引名称和 SEEK:() 谓词。 **Remote Index Seek** 是一个逻辑物理运算符。| 
|![Remote Insert 运算符图标](../relational-databases/media/remote-insert-32x.gif "Remote Insert 运算符图标")|**Remote Insert**|**Remote Insert** 运算符将输入行插入到远程对象。 **Remote Insert** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Remote Query 运算符图标](../relational-databases/media/remote-query-32x.gif "Remote Query 运算符图标")|**Remote Query**|**Remote Query** 运算符将查询提交给远程源。 发送给远程服务器的查询文本显示在 **Argument** 列中。 **Remote Query** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Remote Scan 运算符图标](../relational-databases/media/remote-scan-32x.gif "Remote Scan 运算符图标")|**Remote Scan**|**Remote Scan** 运算符扫描远程对象。 远程对象的名称显示在 **Argument** 列中。 **Remote Scan** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Remote Update 运算符图标](../relational-databases/media/remote-update-32x.gif "Remote Update 运算符图标")|**Remote Update**|**Remote Update** 运算符将更新远程对象中的输入行。 **Remote Update** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Repartition Streams Parallelism 运算符图标](../relational-databases/media/parallelism-repartition-stream.gif "Repartition Streams Parallelism 运算符图标")|**Repartition Streams**|Repartition Streams 运算符（或交换迭代器）使用多个流并生成多个记录流。 记录的内容和格式不会改变。 如果查询优化器使用位图筛选器，则输出流中行的数量将减少。 输入流中的每个记录都放入一个输出流中。 如果该运算符保留次序，则必须对所有输入流排序并将它们合并到几个有序的输出流中。 如果将输出分区，那么 **Argument** 列会包含 PARTITION COLUMNS:() 谓词和分区依据列。如果输出已经排序，则 **Argument** 列包含一个 ORDER BY:() 谓词和已经排序的列。 **Repartition Streams** 是一个逻辑运算符。 该运算符只用于并行查询计划中。| 
|![Result 语言元素图标](../relational-databases/media/result-32x.gif "Result 语言元素图标")|**结果**|**Result** 运算符是查询计划结束时返回的数据。 它通常是显示计划的根元素。 **Result** 是一个语言元素。| 
|![RID Lookup 运算符图标](../relational-databases/media/rid-nonclust-locate-32x.gif "RID Lookup 运算符图标")|**RID Lookup**|**RID Lookup** 是使用提供的行标识符 (RID) 在堆上进行的书签查找。 **Argument** 列包含用于查找表中的行的书签标签和从中查找行的表的名称。 **RID Lookup** 通常带有 NESTED LOOP JOIN。 **RID Lookup** 是一个物理运算符。 有关书签查找的详细信息，请参阅 MSDN SQL Server 博客中的[Bookmark Lookup](/archive/blogs/craigfr/)（书签查找）。| 
|![Row Count Spool 运算符图标](../relational-databases/media/remote-count-spool-32x.gif "Row Count Spool 运算符图标")|**Row Count Spool**|**Row Count Spool** 运算符扫描输入，计算现有的行数并返回相同数目的不包含任何数据的行。 必须检查现有行数（而非行中包含的数据）时，使用此运算符。 例如，如果 **Nested Loops** 运算符执行左半联接操作且联接谓词应用于内部输入，则可以在 **Nested Loops** 运算符内部输入的顶部放置行计数假脱机。 这样， **Nested Loops** 运算符就可以确定行计数假脱机输出的行数（因为不需要内侧的实际数据）以决定是否返回外部行。 **Row Count Spool** 是一个物理运算符。| 
|无|**Right Anti Semi Join**|当第一个（顶端）输入中不存在匹配行时， **Right Anti Semi Join** 运算符返回第二个（底端）输入中的每一行。 匹配行的定义是满足 **Argument** 列中的谓词的行（如果不存在谓词，则每行都是一个匹配行）。 **Right Anti Semi Join** 是一个逻辑运算符。| 
|无|**Right Outer Join**|**Right Outer Join** 运算符返回满足联接第二个（底端）输入与第一个（顶端）输入中每个匹配行的每一行。 此外，它还返回第二个输入中在第一个输入中没有匹配行的任何行，即与 NULL 联接。 如果 **Argument** 列内不存在任何联接谓词，则每行都是一个匹配行。 **Right Outer Join** 是一个逻辑运算符。| 
|无|**Right Semi Join**|当第一个（顶端）输入中存在匹配行时， **Right Semi Join** 运算符返回第二个（底端）输入中的每一行。 如果 **Argument** 列内不存在任何联接谓词，则每行都是一个匹配行。 **Right Semi Join** 是一个逻辑运算符。| 
|![Segment 运算符图标](../relational-databases/media/segment-32x.gif "Segment 运算符图标")|**段**|**Segment** 既是一个物理运算符，也是一个逻辑运算符。 它基于一个或多个列的值将输入集划分成多个段。 这些列显示为 **Segment** 运算符中的参数。 然后此运算符每次输出一个段。| 
|![Sequence 运算符图标](../relational-databases/media/sequence-32x.gif "Sequence 运算符图标")|**序列**|**Sequence** 运算符驱动大范围的更新计划。 就其功能而言，该运算符按顺序（从上到下）执行每个输入。 每个输入通常是不同对象的更新。 该运算符只返回其上一个（底端）输入中的行。 **Sequence** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Sequence Project 运算符图标](../relational-databases/media/sequence-project-32x.gif "Sequence Project 运算符图标")|**Sequence Project**|**Sequence Project** 运算符将添加列以便计算有序集。 它基于一个或多个列的值将输入集划分成多个段。 然后此运算符每次输出一个段。 这些列在 **Sequence Project** 运算符中作为参数显示。 **Sequence Project** 既是一个逻辑运算符，也是一个物理运算符。| 
|无|**Segment Repartition**|在并行查询计划中，有时存在迭代器的概念性区域。 此类区域中的所有迭代器都可通过并行线程执行。 这些区域本身必须串行执行。 单个区域内的某些 **Parallelism** 迭代器称为 **Branch Repartition**。 两个这样的区域边界上的 **Parallelism** 迭代器称为 **Segment Repartition**。 **Branch Repartition** 和 **Segment Repartition** 是逻辑运算符。| 
|![Snapshot Cursor 运算符图标](../relational-databases/media/snapshot-32x.gif "Snapshot Cursor 运算符图标")|**快照**|**Snapshot** 运算符创建一个看不到其他人所做更改的游标。| 
|![Sort 运算符图标](../relational-databases/media/sort-32x.gif "Sort 运算符图标")|**Sort**|**Sort** 运算符可对所有传入的行进行排序。 **Argument** 列包含 DISTINCT ORDER BY:() 谓词（如果此操作删除了重复项），或 ORDER BY:() 谓词（如果对以逗号分隔的列列表进行排序）。 如果按升序对列排序，则使用值 ASC 作为列的前缀；如果按降序对列排序，则使用值 DESC 作为列的前缀。 **Sort** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Split 运算符图标](../relational-databases/media/split-32x.gif "Split 运算符图标")|**Split**|**Split** 运算符用于优化更新处理。 它将每个更新操作拆分成删除和插入操作。 **Split** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Spool 运算符图标](../relational-databases/media/spool-32x.gif "Spool 运算符图标")|**Eager Spool**|**Eager Spool** 运算符获取整个输入，并将每行存储在 **tempdb** 数据库中存储的隐藏临时对象中。 如果重绕该运算符（例如通过 **Nested Loops** 运算符重绕），但不需要任何重新绑定，则将使用假脱机数据，而不用重新扫描输入。 如果需要重新绑定，则将放弃假脱机数据，并通过重新扫描（重新绑定的）输入重新生成假脱机对象。 **Eager Spool** 运算符按“急切”方式生成自己的假脱机文件：当假脱机的父运算符请求第一行时，假脱机运算符将获取所有来自其输入运算符的行并将其存储在假脱机中。 **Eager Spool** 是一个逻辑运算符。| 
|![Spool 运算符图标](../relational-databases/media/spool-32x.gif "Spool 运算符图标")|**Lazy Spool**|**Lazy Spool** 逻辑运算符将其输入中的每一行存储到 **tempdb** 数据库内存储的隐藏临时对象中。 如果重绕该运算符（例如通过 **Nested Loops** 运算符重绕），但不需要任何重新绑定，则将使用假脱机数据，而不用重新扫描输入。 如果需要重新绑定，则将放弃假脱机数据，并通过重新扫描（重新绑定的）输入重新生成假脱机对象。 **Lazy Spool** 运算符以“迟缓”方式生成其假脱机文件，即每当假脱机父运算符请求一行时，假脱机运算符便从其输入运算符获取一行，然后将该行存储在假脱机中，而不是一次处理所有行。 Lazy Spool 是一个逻辑运算符。| 
|![Spool 运算符图标](../relational-databases/media/spool-32x.gif "Spool 运算符图标")|**Spool**|**Spool** 运算符将中间查询结果保存到 **tempdb** 数据库中。| 
|![Stream Aggregate 运算符图标](../relational-databases/media/stream-aggregate-32x.gif "Stream Aggregate 运算符图标")|**Stream Aggregate**|**Stream Aggregate** 运算符按一列或多列对行分组，然后计算由查询返回的一个或多个聚合表达式。 此运算符的输出可供查询中的后续运算符引用和/或返回到客户端。 **Stream Aggregate** 运算符要求输入在组中按列进行排序。 如果由于前面的 **Sort** 运算符或已排序的索引查找或扫描导致数据尚未排序，则优化器将在此运算符前面使用一个 **Sort** 运算符。 在 SHOWPLAN_ALL 语句或 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]中的图形执行计划中，GROUP BY 谓词中的列会列在 **Argument** 列中，而聚合表达式则列在 **Defined Values** 列中。 **Stream Aggregate** 是一个物理运算符。| 
|![Switch 运算符图标](../relational-databases/media/switch-32x.gif "Switch 运算符图标")|**开关**|**Switch** 是一种特殊类型的串联迭代器，它具有 *n* 个输入。 有一个表达式与每个 **Switch** 运算符关联。 根据表达式的返回值（在 0 到 *n*-1 之间）， **Switch** 将适当的输入流复制到输出流中。 **Switch** 的一种用途是与某些运算符（如 **TOP** 运算符）一起实现涉及快进游标的查询计划。 **Switch** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Table Delete 运算符图标](../relational-databases/media/table-delete-32x.gif "Table Delete 运算符图标")|**Table Delete**|**Table Delete** 物理运算符删除查询执行计划的 **Argument** 列中所指定表中的行。| 
|![Table Insert 运算符图标](../relational-databases/media/table-insert-32x.gif "Table Insert 运算符图标")|**Table Insert**|**Table Insert** 运算符将输入的行插入到在查询执行计划的 **Argument** 列指定的表中。 **Argument** 列还包含一个 SET:() 谓词，用于指示为每一列设置的值。 如果 **Table Insert** 的插入值没有子项，插入的行则来自 Insert 运算符本身。 **Table Insert** 是一个物理运算符。| 
|![Table Merge 运算符](../relational-databases/media/table-merge-32x.gif "Table Merge 运算符")|**Table Merge**|**Table Merge** 运算符可将合并数据流应用到堆。 该运算符可在其 **Argument** 列中所指定的表中删除、更新或插入行。 执行的实际操作取决于该运算符的 **Argument** 列中指定的 **ACTION** 列的运行时值。 **Table Merge** 是一个物理运算符。| 
|![Table Scan 运算符图标](../relational-databases/media/table-scan-32x.gif "Table Scan 运算符图标")|**Table Scan**|**Table Scan** 运算符从查询执行计划的 **Argument** 列所指定的表中检索所有行。 如果 WHERE:() 谓词出现在 **Argument** 列中，则仅返回满足此谓词的那些行。 **Table Scan** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Table Spool 运算符图标](../relational-databases/media/table-spool-32x.gif "Table Spool 运算符图标")|**Table Spool**|**Table Spool** 运算符扫描输入，并将各行的一个副本放入隐藏的假脱机表中，此表存储在 [tempdb](../relational-databases/databases/tempdb-database.md) 数据库中并且仅在查询的生存期内存在。 如果重绕该运算符（例如通过 **Nested Loops** 运算符重绕），但不需要任何重新绑定，则将使用假脱机数据，而不用重新扫描输入。 **Table Spool** 是一个物理运算符。| 
|![Table Spool 运算符图标](../relational-databases/media/table-spool-32x.gif "Table Spool 运算符图标")|**Window Spool**|**Window Spool** 运算符将每个行扩展为表示与行关联的窗口的行集。 在查询中，OVER 子句定义查询结果集内的窗口和窗口函数，然后计算窗口中的每个行的值。 **Window Spool** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Table Update 运算符图标](../relational-databases/media/table-update-32x.gif "Table Update 运算符图标")|**Table Update**|**Table Update** 物理运算符更新查询执行计划的 **Argument** 列中所指定表中的输入行。 SET:() 谓词确定每个更新列的值。 可以在 SET 子句中、此运算符内的其他位置以及此查询内的其他位置引用这些值。| 
|![Table-valued Function 运算符图标](../relational-databases/media/table-valued-function-32x.gif "Table-valued Function 运算符图标")|**Table-valued Function**|**Table-valued Function** 运算符计算表值函数（ [!INCLUDE[tsql](../includes/tsql-md.md)] 或 CLR）并将结果行存储在 [tempdb](../relational-databases/databases/tempdb-database.md) 数据库中。 当父迭代器请求这些行时， **Table-valued Function** 将返回 **tempdb** 中的行。<br /><br /> 调用表值函数的查询生成具有 **Table-valued Function** 迭代器的查询计划。 可以使用不同的参数值计算 **Table-valued Function** ：<br /><br /> -<br /> **Table-valued Function XML Reader** 输入 XML BLOB 作为参数，并生成一个按 XML 文档顺序表示 XML 节点的行集。 其他输入参数可能会将返回的 XML 节点限于 XML 文档的子集。<br /><br /> -**Table Valued Function XML Reader with XPath filter** 是一种特殊类型的 **XML Reader Table-valued Function** ，它将输出限于满足 XPath 表达式的 XML 节点。<br /><br /> **Table-valued Function** 既是一个逻辑运算符，也是一个物理运算符。| 
|无|**Top N Sort**|**Top N Sort** 与 **Sort** 迭代器类似，差别仅在于前者需要前 *N* 行，而不是整个结果集。 如果 *N* 的值较小， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 查询执行引擎将尝试在内存中执行整个排序操作。 如果 *N* 的值较大，查询执行引擎将使用更通用的排序方法（该方法不采用 *N* 作为参数）重新排序。| 
|![Top 运算符图标](../relational-databases/media/top-32x.gif "Top 运算符图标")|顶部|**Top** 运算符扫描输入，但仅基于排序顺序返回最前面的指定行数或行百分比。 **Argument** 列可以包含要检查重复值的列的列表。 在更新计划中， **Top** 运算符用于强制实施行计数限制。 **Top** 既是一个逻辑运算符，也是一个物理运算符。| 
|![Extended 运算符 (UDX) 图标](../relational-databases/media/udx-32x.gif "Extended 运算符 (UDX) 图标")|**UDX**|扩展运算符 (UDX) 可以实现 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]中的一种 XQuery 或 XPath 操作。 所有 UDX 运算符既是逻辑运算符，又是物理运算符。<br /><br /> 扩展运算符 (UDX) **FOR XML** 用于将其输入的关系行集序列化为 XML 表示形式，并以单个输出行、单个 BLOB 列的形式存储。 它是区分顺序的 XML 聚合运算符。<br /><br /> 扩展运算符 (UDX) **XML SERIALIZER** 是区分顺序的一种 XML 聚合运算符。 它以 XML 文档顺序输入表示 XML 节点或 XQuery 标量的行，并在单个输出行、单个 XML 列中生成序列化的 XML BLOB。<br /><br /> 扩展运算符 (UDX) **XML FRAGMENT SERIALIZER** 是一种特殊类型的 **XML SERIALIZER** ，用于处理表示在 XQuery 插入数据修改扩展中插入的 XML 片断的输入行。<br /><br /> 扩展运算符 (UDX) **XQUERY STRING** 计算表示 XML 节点的输入行的 XQuery 字符串值。 它是一个区分顺序的字符串聚合运算符。 它输出一行多列，表示包含输入字符串值的 XQuery 标量。<br /><br /> 扩展运算符 (UDX) **XQUERY LIST DECOMPOSER** 是一个 XQuery 列表分解运算符。 对于表示 XML 节点的每个输入行，它至少生成表示 XQuery 标量的一个行，如果输入的是 XSD 列表类型的行，则每个行都包含一个列表元素值。<br /><br /> 扩展运算符 (UDX) **XQUERY DATA** 在表示 XML 节点的输入行上计算 XQuery fn:data() 函数的值。 它是一个区分顺序的字符串聚合运算符。 它输出一行多列，表示包含 **fn:data()** 结果的 XQuery 标量。<br /><br /> 扩展运算符 **XQUERY CONTAINS** 在表示 XML 节点的输入行上计算 XQuery fn:contains() 函数的值。 它是一个区分顺序的字符串聚合运算符。 它输出一行多列，表示包含 **fn:contains()** 结果的 XQuery 标量。<br /><br /> 扩展运算符 **UPDATE XML NODE** 更新 XML 类型上的 **modify()** 方法中 XQuery 替换数据修改扩展中的 XML 节点。| 
|无|**Union**|**Union** 运算符扫描多个输入，输出扫描的每一行并删除重复项。 **Union** 是一个逻辑运算符。| 
|![Update（数据库引擎）运算符图标](../relational-databases/media/update-32x.gif "Update（数据库引擎）运算符图标")|**Update**|**Update** 运算符更新在查询执行计划的 **Argument** 列中所指定对象中的每一输入行。 **Update** 是一个逻辑运算符。 物理运算符为 **Table Update**、 **Index Update** 或 **Clustered Index Update**。| 
|![While 语言元素图标](../relational-databases/media/while-32x.gif "While 语言元素图标")|**While**|**While** 运算符实现 [!INCLUDE[tsql](../includes/tsql-md.md)] while 循环。 **While** 是一个语言元素。| 

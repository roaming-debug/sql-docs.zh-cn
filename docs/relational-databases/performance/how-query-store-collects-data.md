---
title: 查询存储的数据收集方式 | Microsoft Docs
description: SQL Server 查询存储将与查询相关的数据永久保存在内部表中，并通过一组视图向用户显示。
ms.custom: ''
ms.date: 11/29/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Query Store, data collection
ms.assetid: 8d5eec36-0013-480a-9c11-183e162e4c8e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b534aefadb3e81e4ab889d04c1eebc1d9b2aef9e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464828"
---
# <a name="how-query-store-collects-data"></a>查询存储的数据收集方式
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

SQL Server 查询存储的工作原理类似于飞行数据记录器，不断地收集与查询和计划相关的编译和运行时信息。 与查询相关的数据将永久保存在内部表中，并通过一组视图向用户显示。
  
## <a name="views"></a>视图 
 下图显示了查询存储视图及其逻辑关系，以及显示为蓝色实体的编译时间信息：
  
 ![查询存储进程视图](../../relational-databases/performance/media/query-store-process-2views.png "query-store-process-2views")  
**查看描述**  
  
|查看|说明|  
|----------|-----------------|  
|**sys.query_store_query_text**|提供对数据库执行的唯一查询文本。 将忽略查询文本之前和之后的注释和空格。 不忽略文本内部的注释和空格。 批中每个语句都会生成单独的查询文本项。|  
|**sys.query_context_settings**|显示执行查询所依据的影响计划的设置的非重复组合。 由于 `context_settings_id` 是查询键的一部分，因此采用影响计划的不同设置执行的相同查询文本将在查询存储中生成单独的查询条目。|  
|**sys.query_store_query**|在查询存储中单独进行跟踪和强制执行的查询条目。 如果查询文本在不同的上下文设置下执行，或在不同的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 模块（例如，存储过程和触发器等）内外执行，则可以产生多个查询条目。|  
|**sys.query_store_plan**|显示具有编译时间统计信息的查询估计计划。 存储计划相当于一个可以通过使用 `SET SHOWPLAN_XML ON` 获取的计划。|  
|**sys.query_store_runtime_stats_interval**|查询存储将时间划分为自动生成的时间窗口（间隔），并根据每个执行计划的间隔存储聚合的统计信息。 间隔大小由（位于 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中）的配置选项“统计信息收集间隔”或由使用 [ALTER DATABASE SET Options &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) 的 `INTERVAL_LENGTH_MINUTES` 进行控制。|  
|**sys.query_store_runtime_stats**|执行计划的聚合运行时统计信息。 所有捕获的指标都以 4 种统计函数的形式表示：平均值、最小值、最大值、标准偏差。|  
  
 有关查询存储视图的其他详细信息，请参阅[使用查询存储来监视性能](monitoring-performance-by-using-the-query-store.md)中的“相关视图、函数和过程”部分。 
  
## <a name="query-processing"></a>查询处理
 查询存储在以下关键点与查询处理管道进行交互：
  
1.  第一次编译查询时，会将查询文本和初始计划发送到查询存储。
  
2.  重新编译查询时，将在查询存储中更新该计划。 如果创建新的计划，查询存储将为查询添加新计划条目，将之前的条目与其执行统计信息保存在一起。
  
3.  执行查询时，运行时统计信息将发送到查询存储。 查询存储为每个在当前活动间隔内执行的计划保存准确的聚合统计信息。 
  
4.  在编译和检查重新编译阶段期间，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 确定查询存储中是否存在某一计划应当应用于当前正在运行的查询。 如果存在强制执行的计划，而且过程缓存中的计划与强制执行的计划不同，则将重新编译查询。 这就像 PLAN HINT 应用于该查询的方式一样有效。 此过程的发生对用户应用程序是透明的。 
  
 下图描绘了上述步骤中所述的集成点：
  
 ![查询存储进程](../../relational-databases/performance/media/query-store-process-2processor.png "query-store-process-2processor") 

## <a name="remarks"></a>备注
 若要最大程度减少 I/O 开销，新的数据是在内存中捕获的。 写入操作排队，并稍后将其刷新到磁盘。 下图中显示为“计划存储”的查询和计划信息以最小的延迟时间进行刷新。 显示为 Runtime Stats 的运行时统计信息会在内存中保留一段时间，该时间用 `SET QUERY_STORE` 语句的 `DATA_FLUSH_INTERVAL_SECONDS` 选项定义。 可以使用“[!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] 查询存储”对话框为“数据刷新间隔（分钟）”输入一个值，该值在内部转换为秒。 
  
 ![查询存储进程计划](../../relational-databases/performance/media/query-store-process-3.png "query-store-process-3plan") 
  
 如果在使用[跟踪标志 7745](../../relational-databases/performance/best-practice-with-the-query-store.md#Recovery) 时系统故障或关闭，则查询存储可能会丢失已收集但尚未保存的运行时数据，直到使用 `DATA_FLUSH_INTERVAL_SECONDS` 定义的时间窗口为止。 我们建议使用默认值 900 秒（15 分钟），可在查询捕获性能和数据可用性之间实现平衡。
 
 > [!IMPORTANT] 
 > 没有严格执行“最大大小 (MB)”限制。 仅当查询存储将数据写入磁盘时才检查存储大小。 此间隔由“数据刷新间隔”值设置。 如果查询存储已违反存储大小检查之间的最大大小限制，则它将转换为只读模式。 如果启用了“基于大小的清理模式”，则也会触发强制实施最大大小限制的清理机制。
 
 > [!NOTE]
 > 如果系统出现内存压力，运行时统计信息会在按照 `DATA_FLUSH_INTERVAL_SECONDS` 所定义的时间之前刷新到磁盘。
 
 读取查询存储数据过程中，内存中数据和磁盘上的数据以透明方式统一。 
 
 如果会话终止或者客户端应用程序重启或崩溃，则不会记录查询统计信息。 
  
 ![查询存储进程计划信息](../../relational-databases/performance/media/query-store-process-4planinfo.png "query-store-process-4planinfo")    

## <a name="see-also"></a>另请参阅
 [使用查询数据存储来监视性能](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)  
 [查询存储最佳做法](../../relational-databases/performance/best-practice-with-the-query-store.md)  
 [查询存储目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md) 
  
  

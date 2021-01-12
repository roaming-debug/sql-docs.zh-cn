---
description: sys.dm_db_xtp_gc_cycle_stats (Transact-SQL)
title: sys.dm_db_xtp_gc_cycle_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_gc_cycle_stats_TSQL
- dm_db_xtp_gc_cycle_stats
- sys.dm_db_xtp_gc_cycle_stats_TSQL
- sys.dm_db_xtp_gc_cycle_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_gc_cycle_stats dynamic management view
ms.assetid: bbc9704e-158e-4d32-b693-f00dce31cd2f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bd6973004f499bfefc950b61382a1d0fe5c6495d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092870"
---
# <a name="sysdm_db_xtp_gc_cycle_stats-transact-sql"></a>sys.dm_db_xtp_gc_cycle_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  输出删除了一行或多行的已提交事务的当前状态。 空闲垃圾回收线程每分钟唤醒一次，或在自上次垃圾回收周期以来提交的 DML 事务数超过内部阈值时唤醒。 在垃圾回收周期中，它将已提交的事务移动到一个或多个与各代关联的队列中。 生成了已过时版本的事务会在 16 代间以 16 个事务为单位进行分组，如下所示：  
  
-   第 0 代：存储在最早的活动事务之前提交的所有事务。 这些事务生成的行版本立即可用于垃圾回收。  
  
-   第 1-14 代：存储时间戳比最早活动事务更大的事务。 行版本无法进行垃圾回收。 每代可以容纳最多 16 个事务。 这些代中总计可以存在 224 (14 * 16) 个事务。  
  
-   第 15 代：时间戳比最早活动事务更大的其余事务进入第 15 代。 类似于第 0 代，第 15 代中没有事务数限制。  
  
 当存在内存压力时，垃圾回收线程会积极更新最早活动事务提示（这会强制进行垃圾回收）。  
  
 有关详细信息，请参阅[内存中 OLTP&#40;内存中优化&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)。  
  
  
|列名称|类型|描述|  
|-----------------|----------|-----------------|  
|cycle_id|**bigint**|垃圾回收周期的唯一标识符。|  
|ticks_at_cycle_start|**bigint**|周期开始时的时钟。|  
|ticks_at_cycle_end|**bigint**|周期结束时的时钟。|  
|base_generation|**bigint**|数据库中的当前基础代值。 这表示用于标识事务进行垃圾回收的最早活动事务时间戳。 最早活动事务 id 以 16 为增量进行更新。 例如，如果事务 id 为124、125、126 .。。139，此值将是124。 再添加一个事务（例如 140）时，值为 140。|  
|xacts_copied_to_local|**bigint**|从事务管道复制到数据库的代阵列中的事务数。|  
|xacts_in_gen_0- xacts_in_gen_15|**bigint**|每代中的事务数。|  
  
## <a name="permissions"></a>权限  
 要求对服务器具有 VIEW DATABASE STATE 权限。  
  
## <a name="usage-scenario"></a>使用方案  
 下面是一个包含列子集的示例输出，显示了 27 代：  
  
```  
cycle_id   ticks_at_cycle_start ticks_at_cycle_end   base_generation  xacts_in_gen_0    xacts_in_gen_1  
  
1          123160509            123160509            1                    0                    0  
2          123176822            123176822            1                    0                    1  
3          123236826            123236826            1                    0                    1  
4          123296829            123296829            1                    0                    1  
5          123356832            123356941            129                  0                    0  
6          123357473            123357473            129                  0                    0  
7          123417486            123417486            129                  0                    0  
8          123477489            123477489            129                  0                    0  
9          123537492            123537492            129                  0                    0  
10         123597500            123597500            129                  0                    0  
11         123657504            123657504            129                  0                    0  
12         123717507            123717507            129                  0                    0  
13         123777510            123777510            129                  0                    0  
14         123837513            123837513            129                  0                    0  
15         123897516            123897516            129                  0                    0  
16         123957516            123957516            129                  0                    0  
17         124017516            124017516            129                  0                    0  
18         124077517            124077517            129                  0                    0  
19         124137517            124137517            129                  0                    0  
20         124197518            124197518            129                  0                    0  
21         124257518            124257518            129                  0                    0  
22         124317523            124317523            129                  0                    0  
23         124377526            124377526            129                  0                    0  
24         124437529            124437529            129                  0                    0  
25         124497533            124497533            129                  0                    0  
26         124557536            124557536            129                  0                    0  
27         124617539            124617539            129                  0                    0  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [内存优化表动态管理视图 &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  

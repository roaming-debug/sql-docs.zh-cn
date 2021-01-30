---
description: MSreplication_monitordata (Transact-SQL)
title: MSreplication_monitordata (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSreplication_monitordata_TSQL
- MSreplication_monitordata
dev_langs:
- TSQL
helpviewer_keywords:
- MSreplication_monitordata system table
ms.assetid: 843d3ffd-a1ef-4fd5-a744-c2252199793e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 56129415a5595bacc8c975297847d093144dde1b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199086"
---
# <a name="msreplication_monitordata-transact-sql"></a>MSreplication_monitordata (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSreplication_monitordata** 表包含复制监视器所使用的缓存数据，每个受监视的订阅占一行。 此表存储在分发数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**lastrefresh**|**datetime**|刷新监视数据的日期和时间。|  
|**computetime**|**int**|计算监视数据所花的时间（以秒为单位）。|  
|**publication_id**|**int**|发布 ID。|  
|**publisher**|**sysname**|发布服务器的名称。|  
|**publisher_srvid**|**int**|发布服务器的服务器 ID。|  
|**publisher_db**|**sysname**|发布数据库的名称。|  
|**发布**|**sysname**|发布的名称。|  
|**publication_type**|**int**|发布的类型，可以是以下值之一：<br /><br /> **0** = 事务发布<br /><br /> **1** = 快照发布<br /><br /> **2** = 合并发布|  
|**agent_type**|**int**|复制代理的类型，可以是下列值之一。<br /><br /> **1** = 快照代理<br /><br /> **2** = 日志读取器代理<br /><br /> **3** = 分发代理<br /><br /> **4** = 合并代理<br /><br /> **9** = 队列读取器代理|  
|**agent_id**|**int**|复制代理的 ID。|  
|**agent_name**|**sysname**|复制代理作业的名称。|  
|**job_id**|**uniqueidentifier**|复制代理作业的 GUID 名称。|  
|**status**|**int**|复制代理的状态，可以是下列值之一：<br /><br /> **1** = 已启动<br /><br /> **2** = 成功<br /><br /> **3** = 正在进行<br /><br /> **4** = 空闲<br /><br /> **5** = 正在重试<br /><br /> **6** = 失败|  
|**isagentrunningnow**|**bit**|一个标志，用于指示代理作业当前是否正在运行，如果值为 **1** ，则表示作业正在运行。|  
|**warning**|**int**|由订阅生成的阈值警告，可以是以下值中一个或多个值的逻辑或结果。<br /><br /> **1** = 过期-对事务发布的订阅已超出保持期超过允许的阈值（以保持期的百分比形式表示）。<br /><br /> **2** = 滞后时间-将数据从事务发布服务器复制到订阅服务器所用的时间超过了阈值（以秒为单位）。<br /><br /> **4** = mergeexpiration-针对合并发布的订阅已超出保持期超过允许的阈值（以保持期的百分比表示）。 8 = mergefastrunduration - 通过快速网络连接完成合并订阅同步所用的时间超出阈值（以秒为单位）。<br /><br /> **16** = mergeslowrunduration-完成合并订阅同步所用的时间超过了慢速或拨号网络连接的阈值（以秒为单位）。<br /><br /> **32** = mergefastrunspeed-合并订阅的同步过程中的行传递速率未能维持快速网络连接上的阈值速率（以每秒行数为单位）。<br /><br /> **64** = mergeslowrunspeed-合并订阅的同步过程中的行传递速率未能维持慢速或拨号网络连接的阈值速率（以每秒行数为单位）。|  
|**last_distsync**|**datetime**|分发代理的最后日期和时间。|  
|**agentstoptime**|**datetime**|代理停止的日期和时间。|  
|**distdb**|**sysname**|用于订阅的分发数据库的名称。|  
|**保留**|**int**|发布的保持期。|  
|**time_stamp**|**datetime**|仅供内部使用。|  
|**worst_latency**|**int**|在事务发布中，由日志读取器代理或分发代理传播的数据更改的最长滞后时间（以秒为单位）。|  
|**best_latency**|**int**|在事务发布中，由日志读取器代理或分发代理传播的数据更改的最短滞后时间（以秒为单位）。|  
|**avg_latency**|**int**|在事务发布中，由日志读取器代理或分发代理传播的数据更改的平均滞后时间（以秒为单位）。|  
|**cur_latency**|**int**|由日志读取器或分发代理在当前运行过程中传播的数据变更滞后时间（以秒为单位）。|  
|**worst_runspeedPerf**|**int**|合并发布的最长同步时间|  
|**best_runspeedPerf**|**int**|合并发布的最短同步时间。|  
|**average_runspeedPerf**|**int**|合并发布的平均同步时间|  
|**mergePerformance**|**int**|订阅的上次同步相对于其所有同步而言的性能，用上次同步的传递速率除以之前所有传递速率的平均值。|  
|**mergelatestsessionrunduration**|**int**|合并代理最近运行的持续时间。|  
|**mergelatestsessionrunspeed**|**float(53)**|合并代理最近运行的传送速率。|  
|**mergelatestsessionconnectiontype**|**int**|最近的合并代理会话所用的连接，可以是下列值之一：<br /><br /> **1** = 局域网 (LAN) <br /><br /> **2** = 拨号网络连接|  
|**retention_period_unit**|**tinyint**|定义在定义保持期时使用的单位，可以为下列值之一：<br /><br /> **1** = 周<br /><br /> **2** = 月<br /><br /> **3** = 年|  
  
## <a name="see-also"></a>另请参阅  
 [以编程方式监视复制](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)   
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Transact-sql&#41;&#40;复制视图 ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_replmonitorhelpsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelpsubscription-transact-sql.md)   
 [sp_replmonitorhelppublication &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublication-transact-sql.md)   
 [sp_replmonitorhelppublisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublisher-transact-sql.md)   
 [sp_replmonitorhelpmergesession &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelpmergesession-transact-sql.md)   
 [sp_replmonitorhelppublicationthresholds &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublicationthresholds-transact-sql.md)   
 [sp_replmonitorhelpmergesessiondetail &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelpmergesessiondetail-transact-sql.md)  
  
  

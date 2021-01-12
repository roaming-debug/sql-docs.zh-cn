---
description: MSsubscription_agents (Transact-SQL)
title: MSsubscription_agents (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSsubscription_agents
- MSsubscription_agents_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscription_agents system table
ms.assetid: 86ad5891-0bef-4963-9381-7d5b45245a0c
author: cawrites
ms.author: chadam
ms.openlocfilehash: e8569eba1925879f2f757953f36e4ae632292100
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094709"
---
# <a name="mssubscription_agents-transact-sql"></a>MSsubscription_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  可更新订阅的分发代理和触发器使用 **MSsubscription_agents** 表来跟踪订阅属性。 该表存储在订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**id**|**int**|行的 ID。|  
|**publisher**|**sysname**|发布服务器的名称。|  
|**publisher_db**|**sysname**|发布数据库的名称。|  
|**发布**|**sysname**|发布的名称。|  
|**subscription_type**|**int**|订阅类型：<br /><br /> 0 = 推送。<br /><br /> 1 = 请求<br /><br /> 2 = 匿名请求。|  
|**queue_id**|**sysname**|[!INCLUDE[msCoName](../../includes/msconame-md.md)]发布服务器上的消息队列的 ID。 对于基于 SQL 的排队更新，将 *queue_id* 设置为 **sql** 。|  
|**update_mode**|**tinyint**|更新的类型：<br /><br /> **0** = 只读。<br /><br /> **1** = 立即更新。<br /><br /> **2** = 使用消息队列的排队更新。<br /><br /> **3** = 使用消息队列作为故障转移的立即更新和排队更新。<br /><br /> **4** = 使用 SQL Server 队列的排队更新。<br /><br /> **5** = 使用 SQL Server 队列进行排队更新故障转移的立即更新。|  
|**failover_mode**|**bit**|如果选择了更新的故障转移类型，则此参数值为选择的故障转移类型：<br /><br /> **0** = 正在使用即时更新。 不启用故障转移。<br /><br /> **1** = 正在使用排队更新。 已启用故障转移。 用于故障转移的队列在 *update_mode* 值中指定。|  
|spid|**int**|当前正在运行或刚运行过的分发代理使用的连接的系统进程 ID。|  
|**login_time**|**datetime**|当前正在运行或刚运行过的分发代理连接的日期和时间。|  
|**allow_subscription_copy**|**bit**|指定是否允许使用复制订阅数据库的功能。|  
|**attach_state**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**attach_version**|**binary(16)**|代表附加订阅版本的唯一标识符。|  
|**last_sync_status**|**int**|当前正在运行或刚运行过的分发代理的最后运行状态。 状态可以为：<br /><br /> **1** = 已启动。<br /><br /> **2** = 成功。<br /><br /> **3** = 正在进行。<br /><br /> **4** = 空闲。<br /><br /> **5** = 重试。<br /><br /> **6** = 失败。|  
|**last_sync_summary**|**sysname**|当前正在运行或刚运行过的分发代理的上一条消息。 状态可以为：<br /><br /> **首先.**<br /><br /> **成功.**<br /><br /> **正在进行。**<br /><br /> **时间.**<br /><br /> **后.**<br /><br /> **失败.**|  
|**last_sync_time**|**datetime**|更新 *last_sync_summary* 和 *last_sync_status* 列的日期和时间。 作为 SqlServer 代理服务作业运行的请求分发代理或匿名分发代理不更新这些列。 在这种情况下，会将历史记录信息记录到作业历史表中。|  
|**queue_server**|**sysname**|仅限内部使用。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Transact-sql&#41;&#40;复制视图 ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helppullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)  
  
  

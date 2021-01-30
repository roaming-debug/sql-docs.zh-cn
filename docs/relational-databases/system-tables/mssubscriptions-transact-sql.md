---
description: MSsubscriptions (Transact-SQL)
title: MSsubscriptions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSsubscriptions_TSQL
- MSsubscriptions
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscriptions system table
ms.assetid: b7e8301d-d115-41f6-8d4f-e0d25f453b25
author: cawrites
ms.author: chadam
ms.openlocfilehash: c03bbca418d75cfc3dcb2df0f476646f5f5dffcb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211584"
---
# <a name="mssubscriptions-transact-sql"></a>MSsubscriptions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在本地分发服务器提供服务的订阅中，每个已发布项目在 **MSsubscriptions** 表中各占一行。 此表存储在分发数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**publisher_database_id**|**int**|发布服务器数据库的 ID。|  
|**publisher_id**|**smallint**|发布服务器的 ID。|  
|**publisher_db**|**sysname**|发布服务器数据库的名称。|  
|**publication_id**|**int**|发布 ID。|  
|**article_id**|**int**|项目的 ID。|  
|**subscriber_id**|**smallint**|订阅服务器 ID。|  
|**subscriber_db**|**sysname**|订阅数据库的名称。|  
|**subscription_type**|**int**|订阅的类型：<br /><br /> **0** = 推送。<br /><br /> **1** = 请求。<br /><br /> **2** = 匿名。|  
|**sync_type**|**tinyint**|同步类型：<br /><br /> **1** = 自动。<br /><br /> **2** = 不同步。|  
|**status**|**tinyint**|订阅的状态：<br /><br /> **0** = 非活动。<br /><br /> **1** = 已订阅。<br /><br /> **2** = 活动。|  
|**subscription_seqno**|**varbinary(16)**|快照事务序列号。|  
|**snapshot_seqno_flag**|**bit**|指示快照事务序列号的源，其中，值为 **1** 表示 **subscription_seqno** 是快照序列号。|  
|**independent_agent**|**bit**|表明该发布是否有独立的分发代理。|  
|**subscription_time**|**datetime**|仅限内部使用。|  
|**loopback_detection**|**bit**|适用于作为双向事务复制拓扑的一部分的订阅。 环回检测将确定分发代理是否将在订阅服务器上发起的事务发送回订阅服务器：<br /><br /> **1** = 不发送回。<br /><br /> **0** = 发送回。<br /><br />|  
|**agent_id**|**int**|代理的 ID。|  
|**update_mode**|**tinyint**|更新的类型。|  
|**publisher_seqno**|**varbinary(16)**|该订阅在发布服务器上的事务序列号。|  
|**ss_cplt_seqno**|**varbinary(16)**|用于表示并发快照处理已完成的序列号。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Transact-sql&#41;&#40;复制视图 ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)  
  
  

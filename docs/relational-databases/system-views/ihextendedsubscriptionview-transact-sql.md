---
description: IHextendedSubscriptionView (Transact-SQL)
title: IHextendedSubscriptionView (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- IHextendedSubscriptionView_TSQL
- IHextendedSubscriptionView
dev_langs:
- TSQL
helpviewer_keywords:
- IHextendedSubscriptionView view
ms.assetid: 124756a4-463a-4a81-bf5b-de7e8ffc7a62
author: stevestein
ms.author: sstein
ms.openlocfilehash: 75498189c413aa89a81d2c0e8086c0f5cc37e1a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208889"
---
# <a name="ihextendedsubscriptionview-transact-sql"></a>IHextendedSubscriptionView (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **IHextendedSubscriptionView** 视图显示有关非 SQL Server 发布订阅的信息。 此视图存储在 **分发** 数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**article_id**|**int**|项目的唯一标识符。|  
|**dest_db**|**sysname**|目标数据库的名称。|  
|**srvid**|**smallint**|订阅服务器的唯一标识符。|  
|login_name|**sysname**|用于连接到订阅服务器的登录名。|  
|**distribution_jobid**|**binary**|标识分发代理作业。|  
|**publisher_database_id**|**int**|标识发布数据库。|  
|**subscription_type**|**int**|订阅的类型：<br /><br /> **0** = 推送-分发代理在订阅服务器上运行。<br /><br /> **1** = 请求-分发代理在分发服务器上运行。|  
|**sync_type**|**tinyint**|初始同步的类型：<br /><br /> **1** = 自动<br /><br /> **2** = 无|  
|**status**|**tinyint**|订阅的状态：<br /><br /> **0** = 非活动<br /><br /> **1** = 已订阅<br /><br /> **2** = 活动|  
|**snapshot_seqno_flag**|**bit**|指示是否使用快照序列号。|  
|**independent_agent**|**bit**|指定此发布是否有独立的分发代理。<br /><br /> **0** = 发布使用共享分发代理，每个发布服务器数据库/订阅服务器数据库对都有一个共享代理。<br /><br /> **1** = 此发布有独立的分发代理。|  
|**subscription_time**|**datetime**|仅限内部使用。|  
|**loopback_detection**|**bit**|适用于作为双向事务复制拓扑的一部分的订阅。 环回检测将确定分发代理是否将在订阅服务器上发起的事务发送回订阅服务器：<br /><br /> **1** = 不发送回。<br /><br /> **0** = 发送回。|  
|**agent_id**|**int**|分发代理的唯一标识符。|  
|**update_mode**|**tinyint**|指示更新模式的类型，可以为下列值之一：<br /><br /> **0** = 只读。<br /><br /> **1** = 立即更新。<br /><br /> **2** = 使用消息队列的排队更新。<br /><br /> **3** = 使用消息队列作为故障转移的立即更新和排队更新。<br /><br /> **4** = 使用 SQL Server 队列的排队更新。<br /><br /> **5** = 使用 SQL Server 队列进行排队更新故障转移的立即更新。|  
|**publisher_seqno**|**varbinary(16)**|该订阅在发布服务器上的事务序列号。|  
|**ss_cplt_seqno**|**varbinary(16)**|用于表示并发快照处理已完成的序列号。|  
  
## <a name="see-also"></a>另请参阅  
 [异类数据库复制](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

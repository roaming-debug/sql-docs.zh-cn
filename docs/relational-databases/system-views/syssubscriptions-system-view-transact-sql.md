---
description: syssubscriptions（系统视图）(Transact-SQL)
title: syssubscriptions (系统视图)  (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- syssubscriptions_TSQL
- syssubscriptions
dev_langs:
- TSQL
helpviewer_keywords:
- syssubscriptions view
ms.assetid: c9613858-9512-43a9-aa53-7ee8064f064c
author: stevestein
ms.author: sstein
ms.openlocfilehash: 052d7d4c3fa302fbb5962f89f69f1446bd6e7295
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181551"
---
# <a name="syssubscriptions-system-view-transact-sql"></a>syssubscriptions（系统视图）(Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **Syssubscriptions** 视图将公开订阅信息。 此视图存储在分发数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|订阅项目的唯一 ID。|  
|**srvid**|**smallint**|订阅服务器的服务器 ID。|  
|**dest_db**|**sysname**|订阅数据库的名称。|  
|**status**|**tinyint**|订阅的状态：<br /><br /> **0** = 非活动。<br /><br /> **1** = 已订阅。<br /><br /> **2** = 活动。|  
|**sync_type**|**tinyint**|初始同步的类型：<br /><br /> **1** = 自动。<br /><br /> **2** = 无。|  
|login_name|**sysname**|连接到发布服务器以添加订阅时使用的登录名。|  
|**subscription_type**|**int**|订阅的类型：<br /><br /> **0** = 推送-分发代理在分发服务器上运行。<br /><br /> **1** = 请求-分发代理在订阅服务器上运行。|  
|**distribution_jobid**|**binary(16)**|标识用于同步订阅的分发代理作业。|  
|**timestmap**|**timestamp**|创建订阅的日期和时间。|  
|**update_mode**|**tinyint**|更新模式：<br /><br /> **0** = 只读。<br /><br /> **1** = 立即更新。|  
|**loopback_detection**|**bit**|适用于作为双向事务复制拓扑的一部分的订阅。 环回检测将确定分发代理是否将在订阅服务器上发起的事务发送回订阅服务器：<br /><br /> **0** = 发送回。<br /><br /> **1** = 不发送回。|  
|**queued_reinit**|**bit**|指定是否将项目标记为初始化或重新初始化。 如果值为 **1** ，则指定订阅的项目已标记为进行初始化或重新初始化。|  
|**nosync_type**|**tinyint**|订阅初始化的类型：<br /><br /> **0** = 自动 (快照) <br /><br /> **1** = 仅复制支持<br /><br /> **2** = 用备份初始化<br /><br /> **3** = 从日志序列号 (LSN) <br /><br /> 有关详细信息，请参阅 [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)的 **\@ sync_type** 参数。<br /><br /> **三维空间** = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**srvname**|**sysname**|订阅服务器的名称。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Transact-sql&#41;&#40;复制视图 ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [syssubscriptions &#40;Transact-SQL&#41;](../../relational-databases/system-tables/syssubscriptions-transact-sql.md)  
  
  

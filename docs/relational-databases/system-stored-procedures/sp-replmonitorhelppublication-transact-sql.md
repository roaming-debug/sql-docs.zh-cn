---
description: sp_replmonitorhelppublication (Transact-SQL)
title: sp_replmonitorhelppublication (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorhelppublication_TSQL
- sp_replmonitorhelppublication
helpviewer_keywords:
- sp_replmonitorhelppublication
ms.assetid: 7928c50c-617f-41c5-9e0f-4e42e8be55dc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 70f401f52926fc389232b82167a94f9a4ded1e0e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193750"
---
# <a name="sp_replmonitorhelppublication-transact-sql"></a>sp_replmonitorhelppublication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  返回发布服务器上一个或多个发布的当前状态信息。 在分发服务器的分发数据库上执行此存储过程，用于监视复制。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_replmonitorhelppublication [ @publisher = ] 'publisher'  
    [ , [ @publisher_db = ] 'publisher_db'   
    [ , [ @publication = ] 'publication'   
    [ , [ @publication_type = ] publication_type ]   
    [ , [ @refreshpolicy = ] refreshpolicy ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 是要监视其状态的发布服务器的名称。 *发布服务器* 的 **sysname**，默认值为 NULL。 如果 **为 null**，则将返回使用分发服务器的所有发布服务器的信息。  
  
`[ @publisher_db = ] 'publisher_db'` 已发布数据库的名称。 *publisher_db* 的值为 **sysname**，默认值为 NULL。 如果为 NULL，则返回发布服务器上所有已发布数据库的信息。  
  
`[ @publication = ] 'publication'` 正在监视的发布的名称。 *发布* 为 **sysname**，默认值为 NULL。  
  
`[ @publication_type = ] publication_type` 如果发布的类型为。 *publication_type* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**0**|事务发布。|  
|**1**|快照发布。|  
|**2**|合并发布。|  
|NULL（默认值）|复制尝试确定发布类型。|  
  
`[ @refreshpolicy = ] refreshpolicy` 仅限内部使用。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**publisher_db**|**sysname**|发布服务器的名称。|  
|**发布**|**sysname**|发布的名称。|  
|**publication_type**|**int**|发布的类型，可以是以下值之一。<br /><br /> **0** = 事务发布<br /><br /> **1** = 快照发布<br /><br /> **2** = 合并发布|  
|**status**|**int**|与发布关联的所有复制代理的最大值求值状态，可以是下列值之一。<br /><br /> **1** = 已启动<br /><br /> **2** = 成功<br /><br /> **3** = 正在进行<br /><br /> **4** = 空闲<br /><br /> **5** = 正在重试<br /><br /> **6** = 失败|  
|**warning**|**int**|由属于该发布的订阅所生成的最大阈值警告，可以是下列一个或多个值进行逻辑或运算的结果。<br /><br /> **1** = 过期-对事务发布的订阅尚未在保持期阈值内同步。<br /><br /> **2** = 滞后时间-将数据从事务发布服务器复制到订阅服务器所用的时间超过了阈值（以秒为单位）。<br /><br /> **4** = mergeexpiration-合并发布的订阅尚未在保持期阈值内同步。<br /><br /> **8** = mergefastrunduration-通过快速网络连接完成合并订阅同步所用的时间超过了阈值（以秒为单位）。<br /><br /> **16** = mergeslowrunduration-完成合并订阅同步所用的时间超过了慢速或拨号网络连接的阈值（以秒为单位）。<br /><br /> **32** = mergefastrunspeed-合并订阅的同步过程中的行传递速率未能维持快速网络连接上的阈值速率（以每秒行数为单位）。<br /><br /> **64** = mergeslowrunspeed-合并订阅的同步过程中的行传递速率未能维持慢速或拨号网络连接的阈值速率（以每秒行数为单位）。|  
|**worst_latency**|**int**|在事务发布中，由日志读取器代理或分发代理传播的数据更改的最长滞后时间（以秒为单位）。|  
|**best_latency**|**int**|在事务发布中，由日志读取器代理或分发代理传播的数据更改的最短滞后时间（以秒为单位）。|  
|**average_latency**|**int**|在事务发布中，由日志读取器代理或分发代理传播的数据更改的平均滞后时间（以秒为单位）。|  
|**last_distsync**|**datetime**|上一次分发代理运行的日期时间。|  
|**保留**|**int**|发布的保持期。|  
|**latencythreshold**|**int**|为事务发布设置的滞后时间阈值。|  
|**expirationthreshold**|**int**|为合并发布设置的过期阈值。|  
|**agentnotrunningthreshold**|**int**|为代理设置的无需运行的最长时间阈值。|  
|**subscriptioncount**|**int**|对发布的订阅数。|  
|**runningdistagentcount**|**int**|为发布运行的分发代理数|  
|**snapshot_agentname**|**sysname**|发布的快照代理作业的名称。|  
|**logreader_agentname**|**sysname**|事务发布的日志读取器代理作业的名称。|  
|**qreader_agentname**|**sysname**|支持排队更新的事务发布的队列读取器代理作业的名称。|  
|**worst_runspeedPerf**|**int**|合并发布的最长同步时间。|  
|**best_runspeedPerf**|**int**|合并发布的最短同步时间。|  
|**average_runspeedPerf**|**int**|合并发布的平均同步时间。|  
|**retention_period_unit**|**int**|用于表示 *保持期* 的单位。|  
|**publisher**|**sysname**|发布内容的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_replmonitorhelppublication** 用于所有类型的复制。  
  
## <a name="permissions"></a>权限  
 只有分发数据库上的 **db_owner** 或 **replmonitor** 固定数据库角色的成员才能执行 **sp_replmonitorhelppublication**。  
  
## <a name="see-also"></a>另请参阅  
 [以编程方式监视复制](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  

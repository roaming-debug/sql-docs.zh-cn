---
description: MSdistribution_agents (Transact-SQL)
title: MSdistribution_agents (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdistribution_agents_TSQL
- MSdistribution_agents
dev_langs:
- TSQL
helpviewer_keywords:
- MSdistribution_agents system table
ms.assetid: 0e8f0653-1351-41d1-95d2-40f6d5a050ca
author: cawrites
ms.author: chadam
ms.openlocfilehash: 20b2d5f012c33ea12d4b60ed3a8d44ee475f3bf2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091464"
---
# <a name="msdistribution_agents-transact-sql"></a>MSdistribution_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在本地分发服务器上运行的每个分发代理在 **MSdistribution_agents** 表中各占一行。 此表存储在分发数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**id**|**int**|分发代理的 ID。|  
|name|**nvarchar (100)**|分发代理的名称。|  
|**publisher_database_id**|**int**|发布服务器数据库的 ID。|  
|**publisher_id**|**smallint**|发布服务器的 ID。|  
|**publisher_db**|**sysname**|发布服务器数据库的名称。|  
|**发布**|**sysname**|发布的名称。|  
|**subscriber_id**|**smallint**|订阅服务器的 ID，仅限已知代理使用。 对于匿名代理程序，此列是保留的。|  
|**subscriber_db**|**sysname**|订阅数据库的名称。|  
|**subscription_type**|**int**|订阅的类型：<br /><br /> **0** = 推送。<br /><br /> **1** = 请求。<br /><br /> **2** = 匿名。|  
|**local_job**|**bit**|指示在本地分发服务器上是否有 SQL Server 代理作业。|  
|**job_id**|**binary(16)**|作业标识号。|  
|**subscription_guid**|**binary(16)**|此代理的订阅 ID。|  
|**profile_id**|**int**|来自 [MSagent_profiles &#40;transact-sql&#41;](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) 表的配置 ID。|  
|**anonymous_subid**|**uniqueidentifier**|匿名代理的 ID。|  
|**subscriber_name**|**sysname**|订阅服务器名称，仅供匿名代理使用。|  
|**virtual_agent_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**anonymous_agent_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**creation_date**|**datetime**|创建分发或合并代理时的日期时间。|  
|**queue_id**|**sysname**|用于查找已排队的更新订阅操作所在队列的标识符。 对非排队订阅，该值为 NULL。 对于基于 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 消息队列的发布，该值为 GUID，它唯一标识用于订阅的队列。 对于基于 SQL Server 的队列发布，此列包含值 **SQL**。<br /><br /> 注意：使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 消息队列已弃用，不再受支持。|  
|**queue_status**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**offload_enabled**|**bit**|指示是否可以远程激活代理。<br /><br /> **0** 指定不能远程激活代理。<br /><br /> **1** 指定将远程激活代理，并指定在 *offload_server* 属性中指定的远程计算机上。|  
|**offload_server**|**sysname**|用于远程代理激活的服务器网络名称。|  
|dts_package_name|**sysname**|DTS 包的名称。 例如，对于名为 **DTSPub_Package** 的包，请指定 `@dts_package_name = N'DTSPub_Package'` 。|  
|dts_package_password|**nvarchar (524)**|包上的密码。|  
|dts_package_location|**int**|包位置。 包的位置可以是 **分发服务器** 或 **订阅服务器**。|  
|**sid**|**varbinary(85)**|分发代理或合并代理第一次执行时的安全标识号 (SID)。|  
|**queue_server**|**sysname**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**subscriber_security_mode**|**smallint**|代理在连接订阅服务器时所使用的安全模式，可以是下列模式之一：<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server 身份验证<br /><br /> **1** 个  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 身份验证。|  
|**subscriber_login**|**sysname**|连接订阅服务器时所使用的登录名。|  
|**subscriber_password**|**nvarchar (524)**|当连接到订阅服务器时使用的密码的加密值。|  
|**reset_partial_snapshot_progress**|**bit**|表示是否将放弃部分已下载的快照，以便可以再次启动整个快照进程。|  
|**job_step_uid**|**uniqueidentifier**|启动代理的 SQL Server 代理作业步骤的唯一 ID。|  
|**subscriptionstreams**|**tinyint**|设置每个分发代理允许的连接数，以将更改批并行应用于订阅服务器。 支持使用 1 到 64 之间的值。|  
|**memory_optimized**|**bit**|1指示订阅服务器可用于内存优化表。|  
|**job_login**|**sysname**||  
|**job_password**|**nvarchar (524)**||  
  
## <a name="see-also"></a>另请参阅  
 [复制表 (Transact-SQL)](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  

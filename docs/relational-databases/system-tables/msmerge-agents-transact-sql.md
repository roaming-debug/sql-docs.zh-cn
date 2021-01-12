---
description: MSmerge_agents (Transact-SQL)
title: MSmerge_agents (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_agents
- MSmerge_agents_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_agents system table
ms.assetid: 639d2ebb-2c37-4fe0-b14b-1637bc5fc221
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9380cd5c5e04293e0296c098ee7455eb803e3972
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096203"
---
# <a name="msmerge_agents-transact-sql"></a>MSmerge_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在订阅服务器上运行的每个合并代理在 **MSmerge_agents** 表中各占一行。 此表存储在分发数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**id**|**int**|合并代理的 ID。|  
|name|**nvarchar (100)**|合并代理的名称。|  
|**publisher_id**|**smallint**|发布服务器的 ID。|  
|**publisher_db**|**sysname**|发布服务器数据库的名称。|  
|**发布**|**sysname**|发布的名称。|  
|**subscriber_id**|**smallint**|订阅服务器 ID。|  
|**subscriber_db**|**sysname**|订阅数据库的名称。|  
|**local_job**|**bit**|指示在本地分发服务器上是否有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业。|  
|**job_id**|**binary(16)**|作业标识号。|  
|**profile_id**|**int**|**MSagent_profiles** 表中的配置 ID。|  
|**anonymous_subid**|**uniqueidentifier**|匿名代理的 ID。|  
|**subscriber_name**|**sysname**|订阅服务器的名称。|  
|**creation_date**|**datetime**|创建分发或合并代理的日期和时间。|  
|**offload_enabled**|**bit**|指定可远程激活代理。<br /><br /> **0** 指定不能远程激活代理。<br /><br /> **1** 指定将远程激活代理，并指定在 offload_server 属性中指定的远程计算机上。|  
|**offload_server**|**sysname**|指定用于远程代理激活的服务器网络名称。|  
|**sid**|**varbinary(85)**|分发代理或合并代理第一次执行时的安全标识号 (SID)。|  
|**subscriber_security_mode**|**smallint**|代理在连接订阅服务器时所使用的安全模式，可以是下列模式之一：<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。<br /><br /> **1** 个  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 身份验证。|  
|**subscriber_login**|**sysname**|连接订阅服务器时所使用的登录名。|  
|**subscriber_password**|**nvarchar (524)**|在连接到订阅服务器时所使用的密码加密值。|  
|**publisher_security_mode**|**smallint**|代理在连接到发布服务器时使用的安全模式，可以是以下项之一：<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。<br /><br /> **1** 个  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 身份验证。|  
|**publisher_login**|**sysname**|连接发布服务器时所使用的登录名。|  
|**publisher_password**|**nvarchar (524)**|连接发布服务器时所使用的密码的加密值。|  
|**job_step_uid**|**uniqueidentifier**|启动代理的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业步骤的唯一 ID。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

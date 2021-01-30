---
description: sp_update_agent_profile (Transact-SQL)
title: sp_update_agent_profile (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_update_agent_profile_TSQL
- sp_update_agent_profile
helpviewer_keywords:
- sp_update_agent_profile
ms.assetid: cc81f227-0df3-4151-bb4d-4f45ea997b71
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d165fa80f51d1d5b018bd67eda3c4fb06eb5bc52
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209647"
---
# <a name="sp_update_agent_profile-transact-sql"></a>sp_update_agent_profile (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更新复制代理所用的配置文件。 此存储过程在分发服务器上对分发数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_update_agent_profile [@agent_type=] agent_type, [ @agent_id= ] agent_id, [ @profile_id= ] profile_id  
```  
  
## <a name="arguments"></a>参数  
`[ @agent_type = ] 'agent_type'` 代理的类型。 *agent_type* 是 **int**，没有默认值，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|快照代理。|  
|**2**|日志读取器代理。|  
|**3**|分发代理。|  
|**4**|合并代理。|  
|**9**|队列读取器代理。|  
  
`[ @agent_id = ] 'agent_id'` 代理的 ID。 *agent_id* 为 **int**，没有默认值。  
  
`[ @profile_id = ] 'profile_id'` 代理应使用的配置文件的 ID。 *profile_id* 为 **int**，没有默认值。 若要查看为每个代理定义的配置文件的列表，请使用 [&#40;transact-sql&#41;sp_help_agent_profile ](../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md)。 有关系统配置文件的详细信息，请参阅 [复制代理配置文件](../../relational-databases/replication/agents/replication-agent-profiles.md)。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_update_agent_profile** 在所有类型的复制中使用。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_update_agent_profile** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [复制代理配置文件](../../relational-databases/replication/agents/replication-agent-profiles.md)   
 [sp_add_agent_profile &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-agent-profile-transact-sql.md)   
 [sp_change_agent_profile &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-change-agent-profile-transact-sql.md)   
 [sp_drop_agent_profile &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-drop-agent-profile-transact-sql.md)   
 [sp_help_agent_profile &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

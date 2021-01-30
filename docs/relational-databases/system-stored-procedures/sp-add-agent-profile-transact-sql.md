---
description: sp_add_agent_profile (Transact-SQL)
title: sp_add_agent_profile (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_add_agent_profile
- sp_add_agent_profile_TSQL
helpviewer_keywords:
- sp_add_agent_profile
ms.assetid: 5c246a33-2c21-4a77-9c2a-a2c9f0c5dda1
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: bd89f620f2640c9e88f03ef44909c844187d8c8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186084"
---
# <a name="sp_add_agent_profile-transact-sql"></a>sp_add_agent_profile (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  为复制代理创建新的配置文件。 此存储过程在分发服务器上的任何数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_add_agent_profile [ [ @profile_id= ] profile_id OUTPUT ]  
        , [ @profile_name= ] 'profile_name'   
        , [ @agent_type= ] 'agent_type' ]   
    [ , [ @profile_type= ] profile_type ]  
    [ , [ @description= ] 'description' ]  
    [ , [ @default= ] default ]  
```  
  
## <a name="arguments"></a>参数  
`[ @profile_id = ] profile_id` 与新插入的配置文件关联的 ID。 *profile_id* 为 **int** ，并且是一个可选的 OUTPUT 参数。 如果指定该 ID，则值将设置为新的配置文件 ID。  
  
`[ @profile_name = ] 'profile_name'` 配置文件的名称。 *profile_name* **sysname**，无默认值。  
  
`[ @agent_type = ] 'agent_type'` 复制代理的类型。 *agent_type* 是 **int**，没有默认值，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|快照代理|  
|**2**|日志读取器代理|  
|**3**|分发代理|  
|**4**|合并代理|  
|**9**|队列读取器代理|  
  
`[ @profile_type = ] profile_type` 配置文件的类型。*profile_type* 的值为 **int**，默认值为 **1**。  
  
 **0** 指示系统配置文件。 **1** 表示自定义配置文件。 只能使用此存储过程创建自定义配置文件;因此，唯一有效的值是 **1**。 仅 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 创建系统配置文件。  
  
`[ @description = ] 'description'` 配置文件的说明。 *描述* 为 **nvarchar (3000)**，无默认值。  
  
`[ @default = ] default`指示配置文件是否为 *agent_type * ** 的默认值。 *默认值* 为 **bit**，默认值为 **0**。 **1** 指示要添加的配置文件将成为 *agent_type* 指定的代理的新默认配置文件。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_add_agent_profile** 用于快照复制、事务复制和合并复制。  
  
 自定义代理配置文件与默认代理参数值一起添加。 使用 [sp_change_agent_parameter &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-change-agent-parameter-transact-sql.md) 更改这些默认值或 [sp_add_agent_parameter &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-agent-parameter-transact-sql.md) 添加其他参数。  
  
 执行 **sp_add_agent_profile** 时，将为新的自定义配置文件在 [MSagent_profiles &#40;transact-sql&#41;](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) 表中添加一行，并将此配置文件的关联默认参数添加到 [MSagent_parameters &#40;transact-sql&#41;](../../relational-databases/system-tables/msagent-parameters-transact-sql.md) 表中。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_add_agent_profile** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [使用复制代理配置文件](../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)   
 [复制代理配置文件](../../relational-databases/replication/agents/replication-agent-profiles.md)   
 [sp_add_agent_parameter &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-agent-parameter-transact-sql.md)   
 [sp_change_agent_parameter &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-change-agent-parameter-transact-sql.md)   
 [sp_change_agent_profile &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-change-agent-profile-transact-sql.md)   
 [sp_drop_agent_parameter &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-drop-agent-parameter-transact-sql.md)   
 [sp_drop_agent_profile &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-drop-agent-profile-transact-sql.md)   
 [sp_help_agent_parameter &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-agent-parameter-transact-sql.md)   
 [sp_help_agent_profile &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md)  
  
  

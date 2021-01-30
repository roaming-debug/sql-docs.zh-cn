---
description: syspolicy_system_health_state (Transact-SQL)
title: syspolicy_system_health_state (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syspolicy_system_health_state_TSQL
- syspolicy_system_health_state
dev_langs:
- TSQL
helpviewer_keywords:
- syspolicy_system_health_state view
ms.assetid: 00815106-9fe4-481d-a9e1-a256101887f4
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 245ce67d7ee94eca468c9cda775cff15f0b520a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180753"
---
# <a name="syspolicy_system_health_state-transact-sql"></a>syspolicy_system_health_state (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  每个基于策略的管理策略和目标查询表达式组合在表中各占一行。 使用 syspolicy_system_health_state 视图可通过编程方式检查服务器的策略运行状态。 下表介绍了 syspolicy_system_health_state 视图中的列。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|health_state_id|**bigint**|策略运行状态记录的标识符。|  
|policy_id|**int**|策略标识符。|  
|last_run_date|**datetime**|上次运行策略的日期和时间。|  
|target_query_expression_with_id|**nvarchar(400)**|定义评估策略所依据的目标的目标表达式，带有为标识变量所赋的值。|  
|target_query_expression|**nvarchar(max)**|定义评估策略所依据的目标的表达式。|  
|result|**bit**|此目标与策略有关的运行状态：<br /><br /> 0 = 失败<br /><br /> 1 = 成功|  
  
## <a name="remarks"></a>备注  
 syspolicy_system_health_state 视图显示每个活动（已启用）策略的目标查询表达式的最近运行状态。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 对象资源管理器和“对象资源管理器详细信息”页聚合此视图中的策略运行状态以显示关键运行状态。  
  
## <a name="permissions"></a>权限  
 要求具有 msdb 数据库中 PolicyAdministratorRole 角色的成员身份。  
  
## <a name="see-also"></a>另请参阅  
 [使用基于策略的管理来管理服务器](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)   
 [基于策略的管理视图 (Transact-SQL)](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)  
  
  

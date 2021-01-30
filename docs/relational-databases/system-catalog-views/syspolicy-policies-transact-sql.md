---
description: syspolicy_policies (Transact-SQL)
title: syspolicy_policies (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syspolicy_policies_TSQL
- syspolicy_policies
dev_langs:
- TSQL
helpviewer_keywords:
- syspolicy_policies view
ms.assetid: aecf35bb-187e-4f80-870f-48081b88974e
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8547fa15c966e6835c0b100e628217bc0281991e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192984"
---
# <a name="syspolicy_policies-transact-sql"></a>syspolicy_policies (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中每个基于策略的管理策略在表中各占一行。 syspolicy_policies 属于 msdb 数据库中的 dbo 架构。 下表介绍了 syspolicy_policies 视图中的列。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|policy_id|**int**|策略标识符。|  
|name|**sysname**|策略的名称。|  
|condition_id|**int**|此策略强制设定或测试的条件的 ID。|  
|root_condition_id|**int**|仅限内部使用。|  
|date_created|**datetime**|策略的创建日期和时间。|  
|execution_mode|**int**|策略的评估模式。 可能的值如下：<br /><br /> 0 = 按需<br /><br /> 当此模式由用户直接指定时，将对策略进行评估。<br /><br /> 1 = 更改时: 禁止<br /><br /> 这种自动模式使用 DDL 触发器来防止违反策略。<br /><br /> 2 = 更改时: 仅记录<br /><br /> 当发生相关更改时，这种自动模式使用事件通知对策略进行评估，并记录违反策略的情况。<br /><br /> 4 = 按计划<br /><br /> 这种自动模式使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业定期对策略进行评估。 此模式记录违反策略的情况。<br /><br /> 注意：值3不是可能的值。|  
|policy_category|**int**|此策略所属的基于策略的管理策略类别的 ID。 如果是默认策略组，则为 NULL。|  
|schedule_uid|**uniqueidentifier**|如果 execution_mode 为“按计划”，则包含计划的 ID，否则为 NULL。|  
|description|**nvarchar(max)**|策略说明。 说明列是可选的，可以为 NULL。|  
|help_text|**nvarchar(4000)**|属于 help_link 的超链接文本。|  
|help_link|**nvarchar (2083)**|策略创建者分配给该策略的其他帮助超链接。|  
|object_set_id|**int**|该策略评估的对象集的 ID。|  
|is_enabled|**bit**|指示当前是启用 (1) 还是禁用 (0) 了策略。|  
|job_id|**uniqueidentifier**|如果 execution_mode 为“按计划”，则包含运行策略的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业的 ID。|  
|created_by|**sysname**|创建策略的登录名。|  
|modified_by|**sysname**|最近修改策略的登录名。 如果从未进行修改，则为 NULL。|  
|date_modified|**datetime**|策略的创建日期和时间。 如果从未进行修改，则为 NULL。|  
  
## <a name="remarks"></a>备注  
 在排除基于策略的管理的故障时，请查询 [syspolicy_conditions](../../relational-databases/system-catalog-views/syspolicy-conditions-transact-sql.md) 视图以确定是否启用了策略。 此视图还会显示创建或上次更改策略的用户。  
  
## <a name="permissions"></a>权限  
 要求具有 msdb 数据库中 PolicyAdministratorRole 角色的成员身份。  
  
## <a name="see-also"></a>另请参阅  
 [使用基于策略的管理来管理服务器](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)   
 [基于策略的管理视图 (Transact-SQL)](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)  
  
  

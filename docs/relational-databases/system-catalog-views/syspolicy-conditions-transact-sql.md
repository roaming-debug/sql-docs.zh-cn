---
description: syspolicy_conditions (Transact-SQL)
title: syspolicy_conditions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syspolicy_conditions
- syspolicy_conditions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syspolicy_conditions view
ms.assetid: af97d26c-4bd5-4b08-be51-8419e3b2832c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e51c0d002d697898f4ac5d47c32cfded8235db35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199682"
---
# <a name="syspolicy_conditions-transact-sql"></a>syspolicy_conditions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中每个基于策略的管理条件在表中各占一行。 syspolicy_conditions 属于 msdb 数据库中的 dbo 架构。 下表介绍了 syspolicy_conditions 视图中的列。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|condition_id|**int**|此条件的标识符。 每个条件表示由一个或多个条件表达式组成的集合。|  
|name|**sysname**|条件名称。|  
|date_created|**datetime**|条件的创建日期和时间。|  
|description|**nvarchar(max)**|条件说明。 说明列是可选的，可以为 NULL。|  
|created_by|**sysname**|创建条件的登录名。|  
|modified_by|**sysname**|最近修改条件的登录名。 如果从未进行修改，则为 NULL。|  
|date_modified|**datetime**|条件的创建日期和时间。 如果从未进行修改，则为 NULL。|  
|is_name_condition|**smallint**|指定该条件是否为命名条件。<br /><br /> 0 = 条件表达式不包含 @Name 变量。<br /><br /> 1 = 条件表达式包含 @Name 变量。|  
|facet|**nvarchar(max)**|条件所基于的方面的名称。|  
|Expression|**nvarchar(max)**|方面状态的表达式。|  
|obj_name|**sysname**|如果条件表达式包含 @Name，则为赋给此变量的对象名。|  
  
## <a name="remarks"></a>备注  
 在排除基于策略的管理的故障时，请查询 syspolicy_conditions 视图，以确定创建条件或上次更改条件的用户。  
  
## <a name="permissions"></a>权限  
 要求具有 msdb 数据库中 PolicyAdministratorRole 角色的成员身份。  
  
## <a name="see-also"></a>另请参阅  
 [使用基于策略的管理来管理服务器](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)   
 [基于策略的管理视图 (Transact-SQL)](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)  
  
  

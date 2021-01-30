---
description: sp_syspolicy_add_policy_category_subscription (Transact-SQL)
title: sp_syspolicy_add_policy_category_subscription (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_add_policy_category_subscription
- sp_syspolicy_add_policy_category_subscription_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_add_policy_category_subscription
ms.assetid: 4284f550-9a3f-4726-8181-15e407fbf08f
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 279cec943ce0e93556442c3ec29c2139764e078c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195964"
---
# <a name="sp_syspolicy_add_policy_category_subscription-transact-sql"></a>sp_syspolicy_add_policy_category_subscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将策略类别订阅添加到指定的数据库。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_syspolicy_add_policy_category_subscription [ @target_type = ] 'target_type'  
    , [ @target_object = ] 'target_object'  
    , [ @policy_category = ] 'policy_category'  
    [ , [ @policy_category_subscription_id = ] policy_category_subscription_id OUTPUT ]  
```  
  
## <a name="arguments"></a>参数  
`[ @target_type = ] 'target_type'` 类别订阅的目标类型。 *target_type* 为 **sysname**，是必需的，并且必须设置为 "DATABASE"。  
  
`[ @target_object = ] 'target_object'` 将订阅该类别的数据库的名称。 *target_object* **sysname**，并且是必需的。  
  
`[ @policy_category = ] 'policy_category'` 要订阅的策略类别的名称。 *policy_category* **sysname**，并且是必需的。  
  
 若要获取 *policy_category* 的值，请在 "系统" 视图 policy_policy_categories 查询 msdb.dbo.sys。  
  
`[ @policy_category_subscription_id = ] policy_category_subscription_id` 类别订阅的标识符。 *policy_category_subscription_id* 是 **int**，并作为 OUTPUT 返回。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 您必须在 msdb 系统数据库的上下文中运行 sp_syspolicy_add_policy_category_subscription。  
  
 如果您指定的策略类别不存在，将创建新的策略类别，并且在您执行存储过程时对于所有数据库都将托管订阅。 如果你为新的类别清除托管的订阅，则该订阅将只适用于你指定为 *target_object* 的数据库。 有关如何更改托管的订阅设置的详细信息，请参阅 [sp_syspolicy_update_policy_category (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-syspolicy-update-policy-category-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 此存储过程在其当前所有者的上下文中运行。  
  
## <a name="examples"></a>示例  
 下面的示例配置指定的数据库以便订阅名为“Table Naming Policies”的策略类别。  
  
```  
EXEC msdb.dbo.sp_syspolicy_add_policy_category_subscription @target_type = N'DATABASE'  
, @target_object = N'AdventureWorks2012'  
, @policy_category = N'Table Naming Policies';  
  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [基于策略的管理存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_update_policy_category_subscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-update-policy-category-subscription-transact-sql.md)   
 [sp_syspolicy_unsubscribe_from_policy_category &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-unsubscribe-from-policy-category-transact-sql.md)  
  
  

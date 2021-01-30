---
description: sp_syspolicy_update_policy_category_subscription (Transact-SQL)
title: sp_syspolicy_update_policy_category_subscription (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_update_policy_category_subscription_TSQL
- sp_syspolicy_update_policy_category_subscription
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_update_policy_category_subscription
ms.assetid: d0769566-8f5c-4c8a-84d3-ee17ea6e0cb4
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 493da853bc06eb1c56986a99a0b2a2aac9c2e5c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174401"
---
# <a name="sp_syspolicy_update_policy_category_subscription-transact-sql"></a>sp_syspolicy_update_policy_category_subscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更新指定数据库的策略类别订阅。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_syspolicy_update_policy_category_subscription [ @policy_category_subscription_id = ] policy_category_subscription_id  
    [ , [ @target_type = ] 'target_type' ]  
    [ , [ @target_object = ] 'target_object' ]  
    , [ @policy_category = ] 'policy_category'  
```  
  
## <a name="arguments"></a>参数  
`[ @policy_category_subscription_id = ] policy_category_subscription_id` 要更新的策略类别订阅的标识符。 *policy_category_subscription_id* 为 **int**，并且是必需的。  
  
`[ @target_type = ] 'target_type'` 类别订阅的目标类型。 *target_type* 的默认值为 **sysname**，默认值为 NULL。  
  
 如果指定 *target_type*，则该值必须设置为 "DATABASE"。  
  
`[ @target_object = ] 'target_object'` 将订阅策略类别的数据库的名称。 *target_object* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @policy_category = ] 'policy_category'` 您希望数据库订阅的策略类别的名称。 *policy_category* 的默认值为 **sysname**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 您必须在 msdb 系统数据库的上下文中运行 sp_syspolicy_update_policy_category_subscription。  
  
 若要获取 *policy_category_subscription_id* 和 *policy_category* 的值，可以使用以下查询：  
  
```  
SELECT a.policy_category_subscription_id, a.target_type, a.target_object  
    , b.name AS policy_category  
FROM msdb.dbo.syspolicy_policy_category_subscriptions AS a  
INNER JOIN msdb.dbo.syspolicy_policy_categories AS b  
ON a.policy_category_id = b.policy_category_id;  
```  
  
## <a name="permissions"></a>权限  
 要求具有 PolicyAdministratorRole 固定数据库角色的成员身份。  
  
> [!IMPORTANT]  
>  可能的凭据提升：具有 PolicyAdministratorRole 角色的用户可以创建服务器触发器并计划策略执行，这可能会影响[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例的正常运行。 例如，PolicyAdministratorRole 角色中的用户可以创建一个策略，它可能会禁止在[!INCLUDE[ssDE](../../includes/ssde-md.md)]中创建大多数对象。 由于这种可能的凭据提升，只应将 PolicyAdministratorRole 角色授予受信任的用户，以控制的配置 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 。  
  
## <a name="examples"></a>示例  
 下面的示例更新现有策略类别订阅，以便 AdventureWorks2012 数据库订阅“Finance”策略类别。  
  
```  
EXEC msdb.dbo.sp_syspolicy_update_policy_category_subscription @policy_category_subscription_id = 1  
, @target_object = 'AdventureWorks2012'  
, @policy_category = 'Finance';  
  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [基于策略的管理存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_add_policy_category_subscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-add-policy-category-subscription-transact-sql.md)   
 [sp_syspolicy_delete_policy_category_subscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-delete-policy-category-subscription-transact-sql.md)  
  
  

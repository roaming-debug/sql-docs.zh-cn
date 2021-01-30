---
description: sp_syspolicy_rename_policy_category (Transact-SQL)
title: sp_syspolicy_rename_policy_category (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_rename_policy_category_TSQL
- sp_syspolicy_rename_policy_category
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_rename_policy_category
ms.assetid: 8a9c4a3a-91e8-435e-b721-e0293c92be3e
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 41bacd4093ebaf3337d311d0a3e2b4b20d246f90
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203126"
---
# <a name="sp_syspolicy_rename_policy_category-transact-sql"></a>sp_syspolicy_rename_policy_category (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在基于策略的管理中重命名现有策略类别。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_syspolicy_rename_policy_category { [ @name = ] 'name' | [ @policy_category_id = ] policy_category_id }  
    , [ @new_name = ] 'new_name'  
```  
  
## <a name="arguments"></a>参数  
`[ @name = ] 'name'` 要重命名的策略类别的名称。 *name* 为 **sysname**，如果 *policy_category_id* 为 NULL，则必须指定。  
  
`[ @policy_category_id = ] policy_category_id` 要重命名的策略类别的标识符。 *policy_category_id* 为 **int**，并且如果 *name* 为 NULL，则必须指定。  
  
`[ @new_name = ] 'new_name'` 策略类别的新名称。 *new_name* **sysname**，并且是必需的。 不能为 NULL 或空字符串。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 您必须在 msdb 系统数据库的上下文中运行 sp_syspolicy_rename_policy_category。  
  
 必须为 " *名称* " 或 " *policy_category_id*" 指定值。 两者不能均为 NULL。 若要获取这些值，请查询 msdb.dbo.syspolicy_policy_categories 系统视图。  
  
## <a name="permissions"></a>权限  
 要求具有 PolicyAdministratorRole 固定数据库角色的成员身份。  
  
> [!IMPORTANT]  
>  可能的凭据提升：具有 PolicyAdministratorRole 角色的用户可以创建服务器触发器并计划策略执行，这可能会影响[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例的正常运行。 例如，PolicyAdministratorRole 角色中的用户可以创建一个策略，它可能会禁止在[!INCLUDE[ssDE](../../includes/ssde-md.md)]中创建大多数对象。 由于这种可能的凭据提升，只应将 PolicyAdministratorRole 角色授予受信任的用户，以控制的配置 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 。  
  
## <a name="examples"></a>示例  
 下面的示例将名为“Test Category 1”的策略类别重命名为“Test Category 2”。  
  
```  
EXEC msdb.dbo.sp_syspolicy_rename_policy_category @name = N'Test Category 1'  
, @new_name = N'Test Category 2';  
  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [基于策略的管理存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_add_policy_category &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-add-policy-category-transact-sql.md)   
 [sp_syspolicy_delete_policy_category &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-delete-policy-category-transact-sql.md)   
 [sp_syspolicy_update_policy_category &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-update-policy-category-transact-sql.md)  
  
  

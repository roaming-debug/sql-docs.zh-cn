---
description: sp_helprolemember (Transact-SQL)
title: sp_helprolemember (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helprolemember_TSQL
- sp_helprolemember
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helprolemember
ms.assetid: 42797510-aa5d-4564-85ac-27418419af9c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 223d0fe0a72ed72f312e1aba5402ee9c14e9db63
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195086"
---
# <a name="sp_helprolemember-transact-sql"></a>sp_helprolemember (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关当前数据库中某个角色的直接成员的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helprolemember [ [ @rolename = ] 'role' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @rolename = ] ' role '` 当前数据库中的角色的名称。 *role* 的值为 **sysname**，默认值为 NULL。 *角色* 必须存在于当前数据库中。 如果未指定 *role* ，则返回所有包含当前数据库中的至少一个成员的角色。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**DbRole**|**sysname**|当前数据库中角色的名称。|  
|**名称**|**sysname**|数据库角色成员的名称 **。**|  
|**MemberSID**|**varbinary(85)**|**成员名称** 的安全标识符。|  
  
## <a name="remarks"></a>备注  
 如果数据库包含嵌套角色，则 **成员** 名称可能是角色的名称。 **sp_helprolemember** 不显示通过嵌套角色获取的成员身份。 例如，如果 User1 是 Role1 的成员，而 Role1 是 Role2 的成员，则 `EXEC sp_helprolemember 'Role2'` 将返回 Role1，而不是 Role1 的成员（在这个示例中为 User1）。 若要返回嵌套成员身份，必须对每个嵌套角色重复执行 **sp_helprolemember** 。  
  
 使用 **sp_helpsrvrolemember** 显示固定服务器角色的成员。  
  
 使用 [IS_ROLEMEMBER &#40;transact-sql&#41;](../../t-sql/functions/is-rolemember-transact-sql.md) 检查指定用户的角色成员身份。  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="examples"></a>示例  
 以下示例显示 `Sales` 角色的成员。  
  
```  
EXEC sp_helprolemember 'Sales';  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_droprolemember &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_helprole &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_helpsrvrolemember &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpsrvrolemember-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

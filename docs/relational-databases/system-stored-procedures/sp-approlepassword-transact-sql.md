---
description: sp_approlepassword (Transact-SQL)
title: sp_approlepassword (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_approlepassword
- sp_approlepassword_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_approlepassword
ms.assetid: 7967dc0b-bee2-4c63-b8e9-1c3ce2f5db2a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9e2b2f109ca1e5abe94db0d4aa491c16b95cc713
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203215"
---
# <a name="sp_approlepassword-transact-sql"></a>sp_approlepassword (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改当前数据库中应用程序角色的密码。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [ALTER APPLICATION ROLE](../../t-sql/statements/alter-application-role-transact-sql.md) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_approlepassword [ @rolename= ] 'role' , [ @newpwd = ] 'password'   
```  
  
## <a name="arguments"></a>参数  
`[ @rolename = ] 'role'` 应用程序角色的名称。 *Role* 的值为 **sysname**，无默认值。 *角色* 必须存在于当前数据库中。  
  
`[ @newpwd = ] 'password'` 是应用程序角色的新密码。 *password* 的值为 **sysname**，无默认值。 *密码* 不能为 NULL。  
  
> [!IMPORTANT]  
>  不要使用空密码。 请使用强密码。 有关详细信息，请参阅 [Strong Passwords](../../relational-databases/security/strong-passwords.md)。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 不能在用户定义的事务中执行 **sp_approlepassword** 。  
  
## <a name="permissions"></a>权限  
 需要对数据库具有 ALTER ANY APPLICATION ROLE 权限。  
  
## <a name="examples"></a>示例  
 以下示例将 `PayrollAppRole` 应用程序角色的密码设置为 `B3r12-36`。  
  
```  
EXEC sp_approlepassword 'PayrollAppRole', '''B3r12-36';  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [应用程序角色](../../relational-databases/security/authentication-access/application-roles.md)   
 [sp_addapprole &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addapprole-transact-sql.md)   
 [sp_setapprole (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

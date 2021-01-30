---
description: sp_password (Transact-SQL)
title: sp_password (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_password
- sp_password_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_password
ms.assetid: 0ecbec81-e637-44a9-a61e-11bf060ef084
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f154661d7cd03edfde83dd7774685e73e79aeb69
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195431"
---
# <a name="sp_password-transact-sql"></a>sp_password (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  添加或更改登录名的密码 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_password [ [ @old = ] 'old_password' , ]  
     { [ @new =] 'new_password' }  
     [ , [ @loginame = ] 'login' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @old = ] 'old_password'` 旧密码。 *old_password* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @new = ] 'new_password'` 新密码。 *new_password* **sysname**，无默认值。 如果未使用命名参数，则必须指定 *old_password* 。  
  
> [!IMPORTANT]  
>  不要使用空密码。 请使用强密码。 有关详细信息，请参阅 [Strong Passwords](../../relational-databases/security/strong-passwords.md)。  
  
`[ @loginame = ] 'login'` 受密码更改影响的登录名。 login 的数据类型为 sysname，默认值为 NULL。 *登录名* 必须已经存在，并且只能由 **sysadmin** 或 **securityadmin** 固定服务器角色的成员指定。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 **sp_password** 调用 ALTER LOGIN。 此语句支持附加选项。 有关更改密码的信息，请参阅 [ALTER LOGIN &#40;transact-sql&#41;](../../t-sql/statements/alter-login-transact-sql.md)。  
  
 不能在用户定义的事务中执行 **sp_password** 。  
  
## <a name="permissions"></a>权限  
 需要 ALTER ANY LOGIN 权限。 还需要 CONTROL SERVER 权限才能重置密码而无需提供旧密码，或者所更改的登录名具有 CONTROL SERVER 权限。  
  
 主体可更改其自己的密码。  
  
## <a name="examples"></a>示例  
  
### <a name="a-changing-the-password-of-a-login-without-knowing-the-old-password"></a>A. 在旧密码未知时更改登录名的密码  
 以下示例显示如何使用 `ALTER LOGIN` 将登录名 `Victoria` 的密码更改为 `B3r1000d#2-36`。 这是首选方法。 执行此命令的用户必须具有 CONTROL SERVER 权限。  
  
```  
ALTER LOGIN Victoria WITH PASSWORD = 'B3r1000d#2-36';  
GO  
```  
  
### <a name="b-changing-a-password"></a>B. 更改密码  
 以下示例显示如何使用 `ALTER LOGIN` 将登录名 `Victoria` 的密码由 `B3r1000d#2-36` 更改为 `V1cteAmanti55imE`。 这是首选方法。 用户 `Victoria` 无需其他权限即可执行此命令。 其他用户则需要 ALTER ANY LOGIN 权限。  
  
```  
ALTER LOGIN Victoria WITH   
     PASSWORD = 'V1cteAmanti55imE'   
     OLD_PASSWORD = 'B3r1000d#2-36';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_addlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_adduser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [sp_grantlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_revokelogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

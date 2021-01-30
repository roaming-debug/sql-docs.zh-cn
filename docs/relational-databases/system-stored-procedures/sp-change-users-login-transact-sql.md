---
description: sp_change_users_login (Transact-SQL)
title: sp_change_users_login (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 12/13/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_change_users_login
- sp_change_users_login_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_users_login
ms.assetid: 1554b39f-274b-4ef8-898e-9e246b474333
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 109b2a6aa015f8f66a327714264aa986060f9a0c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203720"
---
# <a name="sp_change_users_login-transact-sql"></a>sp_change_users_login (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将现有数据库用户映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 
  
 > [!IMPORTANT]
 > [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [ALTER USER](../../t-sql/statements/alter-user-transact-sql.md) 。  
  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_change_users_login [ @Action = ] 'action'   
    [ , [ @UserNamePattern = ] 'user' ]   
    [ , [ @LoginName = ] 'login' ]   
    [ , [ @Password = ] 'password' ]  
[;]  
```  
  
## <a name="arguments"></a>参数  
 [ @Action =] "*action*"  
 描述过程要执行的操作。 *操作* 是 **varchar (10)**。 *操作* 可以具有以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**Auto_Fix**|将当前数据库的 sys.database_principals 系统目录视图中的用户项链接到同名的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 如果不存在同名的登录名，将会创建一个。 检查 **Auto_Fix** 语句的结果，以确认确实进行了正确的链接。 避免在安全敏感情况下使用 **Auto_Fix** 。<br /><br /> 如果使用 **Auto_Fix**，则必须指定 " *用户* " 和 " *密码* " （如果登录名尚不存在）; 否则，必须指定 " *用户* "，但将忽略 " *密码* "。 *登录名* 必须为 NULL。 *用户* 必须是当前数据库中的有效用户。 不能将另一个用户映射到该登录名。|  
|**Report**|列出当前数据库中未链接到任何登录名的用户以及相应的安全标识符 (SID)。 *用户*、 *登录名* 和 *密码* 必须为 NULL 或未指定。<br /><br /> 若要使用系统表将报表选项替换为查询，请将 **sys.server_prinicpals** 中的条目与 **sys.database_principals** 中的条目进行比较。|  
|**Update_One**|将当前数据库中的指定 *用户* 链接到现有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *登录名*。 必须指定 *用户* 和 *登录名*。 *密码* 必须为 NULL 或未指定。|  
  
 [ @UserNamePattern =] "*user*"  
 当前数据库中的用户名。 *user* 的值为 **sysname**，默认值为 NULL。  
  
 [ @LoginName =] "*login*"  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]登录名。 login 的数据类型为 sysname，默认值为 NULL。  
  
 [ @Password =] '*password*'  
 分配给 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过指定 **Auto_Fix** 创建的新登录名的密码。 如果已存在匹配的登录名，则会映射用户和登录名，并忽略 *密码* 。 如果不存在匹配的登录名，sp_change_users_login 将创建新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名，并为新的登录名分配 *密码* 作为密码。 *密码* 为 **sysname**，且不能为 NULL。  
  
> **重要说明!!** 始终使用 [强密码！](../../relational-databases/security/strong-passwords.md)
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|UserName|**sysname**|数据库用户名。|  
|UserSID|**varbinary(85)**|用户的安全标识符。|  
  
## <a name="remarks"></a>备注  
 使用 sp_change_users_login 将当前数据库中的数据库用户链接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 如果用户登录名已更改，则使用 sp_change_users_login 将用户链接到新的登录，而不会丢失用户的权限。 新的 *登录名* 不能是 sa， *用户* 不能为 dbo、guest 或 INFORMATION_SCHEMA 用户。  
  
 sp_change_users_login 不能用于将数据库用户映射到 Windows 级主体、证书或非对称密钥。  
  
 sp_change_users_login 不能与通过 Windows 主体创建的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名一起使用，也不能与使用 CREATE USER WITHOUT LOGIN 创建的用户一起使用。  
  
 不能在用户定义的事务中执行 sp_change_users_login。  
  
## <a name="permissions"></a>权限  
 要求具有 db_owner 固定数据库角色中的成员资格。 只有 sysadmin 固定服务器角色的成员才能指定 **Auto_Fix** 选项。  
  
## <a name="examples"></a>示例  
  
### <a name="a-showing-a-report-of-the-current-user-to-login-mappings"></a>A. 显示当前用户与登录名的映射的报告  
 下例生成当前数据库中的用户及其安全标识符 (SID) 的报告。  
  
```  
EXEC sp_change_users_login 'Report';  
```  
  
### <a name="b-mapping-a-database-user-to-a-new-sql-server-login"></a>B. 将数据库用户映射到新的 SQL Server 登录名  
 在以下示例中，数据库用户与新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名关联。 数据库用户 `MB-Sales` 首先映射到另一个登录名，然后重新映射到登录名 `MaryB`。  
  
```  
--Create the new login.  
CREATE LOGIN MaryB WITH PASSWORD = '982734snfdHHkjj3';  
GO  
--Map database user MB-Sales to login MaryB.  
USE AdventureWorks2012;  
GO  
EXEC sp_change_users_login 'Update_One', 'MB-Sales', 'MaryB';  
GO  
```  
  
### <a name="c-automatically-mapping-a-user-to-a-login-creating-a-new-login-if-it-is-required"></a>C. 自动将用户映射到登录名（必要时新建一个登录名）  
 以下示例显示如何使用 `Auto_Fix` 将现有用户映射到同名的登录名，以及如何在不存在登录名 `Mary` 的情况下，创建密码为 `B3r12-3x$098f6` 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `Mary`。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_change_users_login 'Auto_Fix', 'Mary', NULL, 'B3r12-3x$098f6';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_adduser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [sp_helplogins &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helplogins-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
  

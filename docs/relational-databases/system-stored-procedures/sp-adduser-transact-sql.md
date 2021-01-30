---
description: sp_adduser (Transact-SQL)
title: sp_adduser (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_adduser
- sp_adduser_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_adduser
ms.assetid: 61a40eb4-573f-460c-9164-bd1bbfaf8b25
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c250837ad65e0b14fc5612eae69be6f93d8f23e6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198366"
---
# <a name="sp_adduser-transact-sql"></a>sp_adduser (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  向当前数据库中添加新的用户。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [CREATE USER](../../t-sql/statements/create-user-transact-sql.md) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_adduser [ @loginame = ] 'login'   
    [ , [ @name_in_db = ] 'user' ]   
    [ , [ @grpname = ] 'role' ]   
```  
  
## <a name="arguments"></a>参数  
`[ @loginame = ] 'login'`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]登录名或 Windows 登录名。 *login* 是 **sysname**，无默认值。 *登录名* 必须是现有的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名或 Windows 登录名。  
  
`[ @name_in_db = ] 'user'` 新数据库用户的名称。 *用户* 为 **sysname**，默认值为 NULL。 如果未指定 *user* ，则新数据库用户的名称默认为 *登录* 名。 指定 *user* 后，新用户在数据库中的名称将不同于服务器级别的登录名。  
  
`[ @grpname = ] 'role'` 新用户成为其成员的数据库角色。 *role* 的值为 **sysname**，默认值为 NULL。 *role* 必须是当前数据库中的有效数据库角色。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 **sp_adduser** 还将创建一个具有该用户名称的架构。  
  
 在添加完用户之后，可以使用 GRANT、DENY 和 REVOKE 等语句来定义控制用户所执行的活动的权限。  
  
 使用 **sys.server_principals** 显示有效登录名的列表。  
  
 使用 **sp_helprole** 显示有效角色名称的列表。 当指定一个角色时，用户会自动地获得那些为该角色定义的权限。 如果未指定角色，则用户将获得授予默认 **公共** 角色的权限。 若要将用户添加到角色，必须提供 *用户名* 的值。  (*用户名* 可以与 *login_id* 相同。 )   
  
 每个数据库中已存在用户 **来宾** 。 如果先前禁用了此用户，则添加用户 **guest** 会启用该用户。 默认情况下，在新数据库中禁用用户 **来宾** 。  
  
 不能在用户定义的事务内执行 **sp_adduser** 。  
  
 你无法添加 **来宾** 用户，因为在每个数据库中已经存在 **来宾** 用户。 若要启用 **来宾** 用户，请授予 **guest** CONNECT 权限，如下所示：  
  
```  
GRANT CONNECT TO guest;  
GO  
```  
  
## <a name="permissions"></a>权限  
 要求具有数据库的所有权。  
  
## <a name="examples"></a>示例  
  
### <a name="a-adding-a-database-user"></a>A. 添加数据库用户  
 以下示例使用现有的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `Vidur`，将数据库用户 `Recruiting` 添加到当前数据库中的现有 `Vidur` 角色。  
  
```  
EXEC sp_adduser 'Vidur', 'Vidur', 'Recruiting';  
```  
  
### <a name="b-adding-a-database-user-with-the-same-login-id"></a>B. 添加数据库用户（使用相同的登录 ID）  
 以下示例将用户 `Arvind` 添加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `Arvind` 的当前数据库。 此用户属于默认 **公共** 角色。  
  
```  
EXEC sp_adduser 'Arvind';  
```  
  
### <a name="c-adding-a-database-user-with-a-different-name-than-its-server-level-login"></a>C. 添加数据库用户（使用不同于其服务器级别登录名的名称）  
 以下示例将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `BjornR` 添加到具有用户名 `Bjorn` 的当前数据库，并将数据库用户 `Bjorn` 添加到 `Production` 数据库角色。  
  
```  
EXEC sp_adduser 'BjornR', 'Bjorn', 'Production';  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sys.server_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sp_addrole (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrole-transact-sql.md)   
 [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)   
 [sp_dropuser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_grantdbaccess (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [sp_grantlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

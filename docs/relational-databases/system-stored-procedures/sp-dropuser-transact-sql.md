---
description: sp_dropuser (Transact-SQL)
title: sp_dropuser (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropuser
- sp_dropuser_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropuser
ms.assetid: e28f18f9-7ecf-4568-89f4-fe5c520df386
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ba5b9eb814f48003d93bf6d7b05c2b1c37c1ea76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99157143"
---
# <a name="sp_dropuser-transact-sql"></a>sp_dropuser (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从当前数据库中删除数据库用户。 **sp_dropuser** 提供与早期版本的兼容性 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 改用 [DROP USER](../../t-sql/statements/drop-user-transact-sql.md) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_dropuser [ @name_in_db = ] 'user'  
```  
  
## <a name="arguments"></a>参数  
`[ @name_in_db = ] 'user'` 要删除的用户的名称。 *用户* 是 **sysname**，无默认值。 *用户* 必须存在于当前数据库中。 指定 Windows 登录时，请使用数据库用于标识该登录的名称。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 **sp_dropuser** 执行 **sp_revokedbaccess** 以从当前数据库中删除用户。  
  
 使用 **sp_helpuser** 显示可从当前数据库中删除的用户名列表。  
  
 删除数据库用户时，将同时删除该用户的所有别名。 如果此用户拥有一个与用户同名的空架构，则此架构也将被删除。 如果用户在数据库中拥有其他任何安全对象，则不会删除该用户。 必须首先将对象的所有权转让给其他主体。 有关详细信息，请参阅 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)。 删除数据库用户时，将自动删除与该用户相关联的权限，并且将该用户从其所属的所有数据库角色中删除。  
  
 **sp_dropuser** 不能用于从 **master** 或 **tempdb** 数据库中删除数据库所有者 (**dbo**) **INFORMATION_SCHEMA** 用户或 **来宾** 用户。 在非系统数据库中， `EXEC sp_dropuser 'guest'` 将撤消用户 **来宾** 的 CONNECT 权限。 但不会删除用户本身。  
  
 不能在用户定义的事务中执行 **sp_dropuser** 。  
  
## <a name="permissions"></a>权限  
 需要对数据库具有 ALTER ANY USER 权限。  
  
## <a name="examples"></a>示例  
 以下示例从当前数据库中删除用户 `Albert`。  
  
```  
EXEC sp_dropuser 'Albert';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_grantdbaccess (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [DROP USER (Transact-SQL)](../../t-sql/statements/drop-user-transact-sql.md)   
 [sp_revokedbaccess (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-revokedbaccess-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

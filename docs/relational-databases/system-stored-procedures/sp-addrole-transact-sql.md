---
description: sp_addrole (Transact-SQL)
title: sp_addrole (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addrole
- sp_addrole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addrole
ms.assetid: e8a21642-8440-419a-8585-93d3d9d44f00
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c6217f78d47e193c8583caca0925f2d4b587c439
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200170"
---
# <a name="sp_addrole-transact-sql"></a>sp_addrole (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在当前数据库中创建新的数据库角色。  
  
> [!IMPORTANT]
>  包含 **sp_addrole** 是为了与的早期版本兼容 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，在未来版本中可能不支持。 请改用 [CREATE ROLE](../../t-sql/statements/create-role-transact-sql.md) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addrole [ @rolename = ] 'role' [ , [ @ownername = ] 'owner' ]   
```  
  
## <a name="arguments"></a>参数  
`[ @rolename = ] 'role'` 新数据库角色的名称。 *role* 是 **sysname**，无默认值。 *role* 必须是 (ID) 的有效标识符，并且不能已存在于当前数据库中。  
  
`[ @ownername = ] 'owner'` 新数据库角色的所有者。 *所有者* 为 **sysname**，默认值为当前正在执行的用户。 *所有者* 必须是当前数据库中的数据库用户或数据库角色。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库角色的名称可以包含 1 到 128 个字符，包括字母、符号和数字。 数据库角色的名称不能：包含反斜杠字符 (\\) ，为 NULL，或是 (**""**) 的空字符串。  
  
 添加数据库角色后，请使用 [sp_addrolemember &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md) 将主体添加到角色。 当使用 GRANT、DENY 或 REVOKE 语句将权限应用于数据库角色时，数据库角色的成员将继承这些权限，就好像将权限直接应用于其帐户一样。  
  
> [!NOTE]  
>  不能创建新服务器角色。 只能在数据库级别上创建角色。  
  
 不能在用户定义的事务内使用 **sp_addrole** 。  
  
## <a name="permissions"></a>权限  
 需要对数据库具有 CREATE ROLE 权限。 如果创建架构，则需要对数据库的 CREATE SCHEMA 权限。 如果将 *owner* 指定为用户或组，则需要模拟该用户或组。 如果将 *owner* 指定为角色，则需要对该角色或该角色的成员具有 ALTER 权限。 如果将所有者指定为应用程序角色，则需要对此应用程序角色的 ALTER 权限。  
  
## <a name="examples"></a>示例  
 以下示例向当前数据库中添加名为 `Managers` 的新角色。  
  
```  
EXEC sp_addrole 'Managers';  
```  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE ROLE (Transact-SQL)](../../t-sql/statements/create-role-transact-sql.md)  
  
  

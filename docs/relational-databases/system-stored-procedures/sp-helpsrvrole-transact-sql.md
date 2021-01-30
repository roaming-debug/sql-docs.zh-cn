---
description: sp_helpsrvrole (Transact-SQL)
title: sp_helpsrvrole (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpsrvrole_TSQL
- sp_helpsrvrole
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpsrvrole
ms.assetid: 5c7f39f3-c261-4f70-8beb-08242d4ac242
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1f3c2c937e03edaaff888b90199ffeea806829bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209296"
---
# <a name="sp_helpsrvrole-transact-sql"></a>sp_helpsrvrole (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 固定服务器角色的列表。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpsrvrole [ [ @srvrolename = ] 'role' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @srvrolename = ] 'role'` 固定服务器角色的名称。 *role* 的值为 **sysname**，默认值为 NULL。 *role* 可以为下列值之一。  
  
|固定服务器角色|说明|  
|-----------------------|-----------------|  
|sysadmin|系统管理员|  
|securityadmin|安全管理员|  
|serveradmin|服务器管理员|  
|setupadmin|安装程序管理员|  
|processadmin|进程管理员|  
|diskadmin|磁盘管理员|  
|dbcreator|数据库创建者|  
|bulkadmin|可执行 BULK INSERT 语句|  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|ServerRole|**sysname**|服务器角色的名称|  
|说明|**sysname**|ServerRole 的说明|  
  
## <a name="remarks"></a>备注  
 固定服务器角色在服务器级上定义，这些角色具有执行特定服务器级管理活动的权限。 不能添加、删除或更改固定服务器角色。  
  
 若要在服务器角色中添加或删除成员，请参阅 [ALTER SERVER ROLE &#40;transact-sql&#41;](../../t-sql/statements/alter-server-role-transact-sql.md)。  
  
 所有登录名都是 public 的成员。 sp_helpsrvrole 无法识别公共角色，因为在内部，不 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将公共作为角色实现。  
  
 sp_helpsrvrole 不会将用户定义的服务器角色当作参数使用。 若要列出用户定义的服务器角色，请参阅 [ALTER SERVER ROLE &#40;transact-sql&#41;](../../t-sql/statements/alter-server-role-transact-sql.md)中的示例。  
  
## <a name="permissions"></a>权限  
 要求具有 public 角色的成员身份。  
  
## <a name="examples"></a>示例  
  
### <a name="a-listing-the-fixed-server-roles"></a>A. 列出固定服务器角色  
 以下查询返回固定服务器角色的列表。  
  
```  
EXEC sp_helpsrvrole ;  
```  
  
### <a name="b-listing-fixed-and-user-defined-server-roles"></a>B. 列出固定和用户定义的服务器角色  
 以下查询返回固定和用户定义服务器角色的列表。  
  
```  
SELECT * FROM sys.server_principals WHERE type = 'R' ;  
```  
  
### <a name="c-returning-a-description-of-a-fixed-server-role"></a>C. 返回固定服务器角色的说明  
 以下查询返回 `diskadmin` 固定服务器角色的名称和说明。  
  
```  
sp_helpsrvrole 'diskadmin' ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [服务器级别角色](../../relational-databases/security/authentication-access/server-level-roles.md)   
 [sp_addsrvrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md)   
 [sp_dropsrvrolemember &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropsrvrolemember-transact-sql.md)   
 [sp_helpsrvrolemember &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpsrvrolemember-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

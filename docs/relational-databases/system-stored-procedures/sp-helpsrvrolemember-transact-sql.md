---
description: sp_helpsrvrolemember (Transact-SQL)
title: sp_helpsrvrolemember (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpsrvrolemember
- sp_helpsrvrolemember_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpsrvrolemember
ms.assetid: d0714913-8d6b-4de3-b042-3ae9934f839d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f17a4be7d5e9b1511a840857c933ed240d0e3d31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198697"
---
# <a name="sp_helpsrvrolemember-transact-sql"></a>sp_helpsrvrolemember (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 固定服务器角色成员的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpsrvrolemember [ [ @srvrolename = ] 'role' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @srvrolename = ] 'role'` 固定服务器角色的名称。 *role* 的值为 **sysname**，默认值为 NULL。 如果未指定 *role*，则结果集将包括所有固定服务器角色的相关信息。  
  
 *role* 可以是以下任一值。  
  
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
|MemberName|**sysname**|ServerRole 成员的名称|  
|MemberSID|**varbinary(85)**|MemberName 的安全标识符|  
  
## <a name="remarks"></a>备注  
 可以使用 sp_helprolemember 显示数据库角色的成员。  
  
 所有登录名都是 public 的成员。 sp_helpsrvrolemember 无法识别公共角色，因为在内部，不 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将公共作为角色实现。  
  
 若要在服务器角色中添加或删除成员，请参阅 [ALTER SERVER ROLE &#40;transact-sql&#41;](../../t-sql/statements/alter-server-role-transact-sql.md)。  
  
 sp_helpsrvrolemember 不会将用户定义的服务器角色当作参数使用。 若要确定用户定义的服务器角色的成员，请参阅 [ALTER SERVER role &#40;transact-sql&#41;](../../t-sql/statements/alter-server-role-transact-sql.md)中的示例。  
  
## <a name="permissions"></a>权限  
 要求具有 public 角色的成员身份。  
  
## <a name="examples"></a>示例  
 以下示例将列出 `sysadmin` 固定服务器角色的成员。  
  
```  
EXEC sp_helpsrvrolemember 'sysadmin';  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_helprole &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_helprolemember &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helprolemember-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [安全函数 (Transact-SQL)](../../t-sql/functions/security-functions-transact-sql.md)  
  
  

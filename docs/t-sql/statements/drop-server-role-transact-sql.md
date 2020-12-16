---
description: DROP SERVER ROLE (Transact-SQL)
title: DROP SERVER ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP SERVER ROLE
- DROP_SERVER_ROLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER ROLE, DROP
- DROP SERVER ROLE statement
ms.assetid: a2a1e6e6-e40c-4d6a-81be-d197b80bf226
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0e6074c99c03ae52b70fdff0ce4f6ff3c317890b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481768"
---
# <a name="drop-server-role-transact-sql"></a>DROP SERVER ROLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md)]

  删除用户定义的服务器角色。  
  
 用户定义的服务器角色是 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 中的新增角色。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
DROP SERVER ROLE role_name  
```  
  
## <a name="arguments"></a>参数  
 role_name  
 指定要从服务器中删除的用户定义的服务器角色。  
  
## <a name="remarks"></a>注解  
 无法从服务器中删除拥有安全对象的用户定义服务器角色。 若要删除拥有安全对象的用户定义服务器角色，必须先转移这些安全对象的所有权或删除这些安全对象。  
  
 无法删除拥有成员的用户定义服务器角色。 若要删除拥有成员的用户定义服务器角色，必须先使用 [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md) 删除该角色的成员。  
  
 无法删除固定服务器角色。  
  
 通过查询 [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md) 目录视图可查看有关角色成员身份的信息。  
  
## <a name="permissions"></a>权限  
 要求具有服务器角色的 CONTROL 权限或 ALTER ANY SERVER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-to-drop-a-server-role"></a>A. 删除服务器角色  
 以下示例删除服务器角色 `purchasing`。  
  
```sql  
DROP SERVER ROLE purchasing;  
GO  
```  
  
### <a name="b-to-view-role-membership"></a>B. 查看角色成员身份  
 若要查看角色成员身份，请使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的“服务器角色(成员)”页或执行以下查询：  
  
```sql  
SELECT SRM.role_principal_id, SP.name AS Role_Name,   
SRM.member_principal_id, SP2.name  AS Member_Name  
FROM sys.server_role_members AS SRM  
JOIN sys.server_principals AS SP  
    ON SRM.Role_principal_id = SP.principal_id  
JOIN sys.server_principals AS SP2   
    ON SRM.member_principal_id = SP2.principal_id  
ORDER BY  SP.name,  SP2.name  
```  
  
### <a name="c-to-view-role-membership"></a>C. 查看角色成员身份  
 若要确定服务器角色是否拥有其他服务器角色，请执行以下查询：  
  
```sql  
SELECT SP1.name AS RoleOwner, SP2.name AS Server_Role  
FROM sys.server_principals AS SP1  
JOIN sys.server_principals AS SP2  
    ON SP1.principal_id = SP2.owning_principal_id   
ORDER BY SP1.name ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER ROLE (Transact-SQL)](../../t-sql/statements/alter-role-transact-sql.md)   
 [CREATE ROLE (Transact-SQL)](../../t-sql/statements/create-role-transact-sql.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [DROP ROLE (Transact-SQL)](../../t-sql/statements/drop-role-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sys.database_role_members (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
  

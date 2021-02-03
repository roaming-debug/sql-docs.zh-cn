---
description: GRANT 系统对象权限 (Transact-SQL)
title: GRANT 系统对象权限 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- encryption [SQL Server], system objects
- system objects [SQL Server]
- GRANT statement, system objects
ms.assetid: 9d4e89f4-478f-419a-8b50-b096771e3880
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8f0cf133d3e8697b186852424853ce754b5a0290
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209526"
---
# <a name="grant-system-object-permissions-transact-sql"></a>GRANT 系统对象权限 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  授予对系统对象（例如，系统存储过程、扩展存储过程、函数以及视图）的权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
GRANT { SELECT | EXECUTE } ON [ sys.]system_object TO principal   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 [ sys.] .  
 只有在引用目录视图和动态管理视图时才需要 sys 限定符。  
  
 system_object  
 指定要对其授予权限的对象。  
  
 principal  
 指定要向其授予权限的主体。  
  
## <a name="remarks"></a>注解  
 可使用该语句授予对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装的特定存储过程、扩展存储过程、表值函数、标量函数、视图、目录视图、兼容性视图、INFORMATION_SCHEMA 视图、动态管理视图以及系统表的权限。 上述每个系统对象都作为服务器的资源数据库 (mssqlsystemresource) 中的唯一记录而存在。 该资源数据库为只读。 指向对象的链接作为各数据库的 sys 架构中的一条记录显示。 可以授予、拒绝和撤消执行或选择系统对象的权限。  
  
 授予执行或选择对象的权限不一定会提供使用该对象所需的所有权限。 多数对象执行的操作都需要其他权限。 例如，被授予对 sp_addlinkedserver 的 EXECUTE 权限的用户无法创建链接服务器，除非该用户也是 sysadmin 固定服务器角色的成员。  
  
 默认名称解析将解析资源数据库的非限定过程名称。 因此，仅当指定目录视图和动态管理视图时，才需要 sys 限定符。  
  
 不支持授予对触发器以及对系统对象列的权限。  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 升级期间，对系统对象的权限将予以保留。  
  
 在 [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md) 目录视图中可以查看系统对象。 在 [master](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md) 数据库中的 sys.database_permissions 目录视图中可以查看对系统对象的权限。  
  
 下面的查询将返回有关系统对象的权限的信息：  
  
```sql  
SELECT * FROM master.sys.database_permissions AS dp   
    JOIN sys.system_objects AS so  
    ON dp.major_id = so.object_id  
    WHERE dp.class = 1 AND so.parent_object_id = 0 ;  
GO  
```  
  
## <a name="permissions"></a>权限  
 需要 CONTROL SERVER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-granting-select-permission-on-a-view"></a>A. 授予对视图的 SELECT 权限  
 以下示例授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `Sylvester1` 权限，以选择列出 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的视图。 然后，本例授予查看不属于该用户的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的元数据所需的其他权限。  
  
```sql  
USE AdventureWorks2012;  
GRANT SELECT ON sys.sql_logins TO Sylvester1;  
GRANT VIEW SERVER STATE to Sylvester1;  
GO  
```  
  
### <a name="b-granting-execute-permission-on-an-extended-stored-procedure"></a>B. 授予对扩展存储过程的 EXECUTE 权限  
 以下示例向 `EXECUTE` 授予了对 `xp_readmail` 的 `Sylvester1` 权限。  
  
```sql  
GRANT EXECUTE ON xp_readmail TO Sylvester1;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.system_objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md)   
 [sys.database_permissions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)   
 [REVOKE 系统对象权限 (Transact-SQL)](../../t-sql/statements/revoke-system-object-permissions-transact-sql.md)   
 [DENY 系统对象权限 (Transact-SQL)](../../t-sql/statements/deny-system-object-permissions-transact-sql.md)  
  
  

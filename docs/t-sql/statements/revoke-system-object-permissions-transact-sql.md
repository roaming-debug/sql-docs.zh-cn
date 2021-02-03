---
description: REVOKE 系统对象权限 (Transact-SQL)
title: REVOKE 系统对象权限 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- REVOKE statement, system objects
- permissions [SQL Server], system objects
ms.assetid: 84983238-dd7d-45bd-99bb-52c9d8e96a87
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c3c0df1a646caa6562c640b119abe3817d15c094
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185922"
---
# <a name="revoke-system-object-permissions-transact-sql"></a>REVOKE 系统对象权限 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从主体中撤消对系统对象（例如，存储过程、扩展存储过程、函数以及视图）的权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
REVOKE { SELECT | EXECUTE } ON [sys.]system_object FROM principal   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 [sys.] .  
 只有在引用目录视图和动态管理视图时才需要 sys 限定符。  
  
 system_object  
 指定要对其撤消权限的对象。  
  
 principal  
 指定要从中撤消权限的主体。  
  
## <a name="remarks"></a>备注  
 可使用该语句撤消对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装的特定存储过程、扩展存储过程、表值函数、标量函数、视图、目录视图、兼容性视图、INFORMATION_SCHEMA 视图、动态管理视图以及系统表的权限。 每个系统对象都作为唯一的一条记录存在于资源数据库 (mssqlsystemresource) 中。 该资源数据库为只读。 指向对象的链接作为各数据库的 sys 架构中的一条记录显示。  
  
 默认名称解析将解析资源数据库的非限定过程名称。 因此， 只有当指定目录视图和动态管理视图时，才需要 sys. 限定符。  
  
> [!CAUTION]  
>  撤消对系统对象的权限会导致依赖这些权限的应用程序失败。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 使用目录视图，并且在更改了对目录视图的默认权限之后可能无法发挥预期的作用。  
  
 不支持撤消对触发器以及对系统对象列的权限。  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 升级期间，对系统对象的权限将予以保留。  
  
 在 [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md) 目录视图中可以查看系统对象。  
  
## <a name="permissions"></a>权限  
 需要 CONTROL SERVER 权限。  
  
## <a name="examples"></a>示例  
 以下示例从 `EXECUTE` 中撤消对 `sp_addlinkedserver` 的 `public` 权限。  
  
```sql  
REVOKE EXECUTE ON sys.sp_addlinkedserver FROM public;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.system_objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md)   
 [sys.database_permissions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)   
 [GRANT 系统对象权限 (Transact-SQL)](../../t-sql/statements/grant-system-object-permissions-transact-sql.md)   
 [DENY 系统对象权限 (Transact-SQL)](../../t-sql/statements/deny-system-object-permissions-transact-sql.md)  
  
  

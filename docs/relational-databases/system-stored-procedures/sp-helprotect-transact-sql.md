---
description: sp_helprotect (Transact-SQL)
title: sp_helprotect (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helprotect
- sp_helprotect_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helprotect
ms.assetid: faaa3e40-1c95-43c2-9fdc-c61a1d3cc0c3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f64ca2d34d11811bec87c1e4e378b424065299c4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211927"
---
# <a name="sp_helprotect-transact-sql"></a>sp_helprotect (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回一个报表，报表中包含当前数据库中某对象的用户权限或语句权限的信息。  
  
> [!IMPORTANT]  
>  **sp_helprotect** 不返回有关中引入的安全对象的信息 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。 改用 [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md) 和 [fn_builtin_permissions](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md) 。  
  
 不列出始终分配给固定服务器角色或固定数据库角色的权限。 不包括基于其在角色中的成员身份接收权限的登录名或用户。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helprotect [ [ @name = ] 'object_statement' ]   
     [ , [ @username = ] 'security_account' ]   
     [ , [ @grantorname = ] 'grantor' ]   
     [ , [ @permissionarea = ] 'type' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @name = ] 'object_statement'` 当前数据库或语句中具有报告权限的对象的名称。 *object_statement* 为 **nvarchar (776)**，默认值为 NULL，表示将返回所有的对象权限和语句权限。 如果值为一个对象（表、视图、存储过程或扩展存储过程），则该对象必须是当前数据库中的有效对象。 对象名称可以 _包含所有者限定符_**。**_对象_。  
  
 如果 *object_statement* 是语句，则它可以是 CREATE 语句。  
  
`[ @username = ] 'security_account'` 为其返回权限的主体的名称。 *security_account* 的数据值为 **sysname**，默认值为 NULL，表示将返回当前数据库中的所有主体。 当前数据库中必须存在 *security_account* 。  
  
`[ @grantorname = ] 'grantor'` 被授予权限的主体的名称。 *授权* 者为 **sysname**，默认值为 NULL，它返回数据库中任何主体授予的权限的所有信息。  
  
`[ @permissionarea = ] 'type'` 一个字符串，该字符串指示是显示 (字符串 **o**) 的对象权限、语句权限 (字符串 **s**) ，还是同时显示 (**os**) 。 *类型* 为 **varchar (10)**，默认值为 **os**。 *类型* 可以是 **o** 和 **s** 的任意组合，无论在 **o** 和 **s** 之间有或不包含逗号或空格。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**所有者**|**sysname**|对象所有者的名称。|  
|**对象**|**sysname**|对象的名称。|  
|**被授权者**|**sysname**|被授予权限的主体的名称。|  
|**授权者**|**sysname**|向指定的被授权者进行授权的主体的名称。|  
|**ProtectType**|**nvarchar (10)**|保护类型的名称：<br /><br /> GRANT REVOKE|  
|**Action**|**nvarchar(60)**|权限的名称。 依赖于对象类型的有效的权限语句。|  
|**列**|**sysname**|权限的类型：<br /><br /> All = 权限适用于对象所有的当前列。<br /><br /> New = 权限适用于任何以后可以在对象上进行更改（使用 ALTER 语句）的新列。<br /><br /> All+New = All 和 New 的组合。<br /><br /> 如果权限类型不适用于列，则返回一个期间。|  
  
## <a name="remarks"></a>备注  
 以下过程中的所有参数都是可选的。 如果不使用参数执行 `sp_helprotect`，则显示当前数据库中所有已经授予或拒绝的权限。  
  
 如果指定了一部分参数而不是指定全部参数，则使用命名参数来标识特定的参数，或者使用 `NULL` 作为占位符。 例如，若要报告授权者数据库所有者 (`dbo`) 的所有权限，请执行：  
  
```  
EXEC sp_helprotect NULL, NULL, dbo;  
```  
  
 或  
  
```  
EXEC sp_helprotect @grantorname = 'dbo';  
```  
  
 输出报表按权限类别、所有者、对象、被授权者、授权者、保护类型类别、保护类型、操作以及列连续 ID 进行排序。  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
 返回的信息取决于对元数据的访问权限的限制。 主体对其不具有权限的实体将不会显示。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-listing-the-permissions-for-a-table"></a>A. 列出某个表的权限  
 以下示例列出 `titles` 表的权限。  
  
```  
EXEC sp_helprotect 'titles';  
```  
  
### <a name="b-listing-the-permissions-for-a-user"></a>B. 列出某个用户的权限  
 以下示例列出当前数据库中用户 `Judy` 所拥有的所有权限。  
  
```  
EXEC sp_helprotect NULL, 'Judy';  
```  
  
### <a name="c-listing-the-permissions-granted-by-a-specific-user"></a>C. 列出由某个特定用户授予的权限  
 以下示例列出当前数据库中由用户 `Judy` 授予的所有权限，并使用 `NULL` 作为缺少的参数的占位符。  
  
```  
EXEC sp_helprotect NULL, NULL, 'Judy';  
```  
  
### <a name="d-listing-the-statement-permissions-only"></a>D. 仅列出语句权限  
 以下示例列出当前数据库中的所有语句权限，并使用 `NULL` 作为缺少的参数的占位符。  
  
```  
EXEC sp_helprotect NULL, NULL, NULL, 's';   
```  
  
### <a name="e-listing-the-permissions-for-a-create-statement"></a>e. 列出 CREATE 语句的权限  
 下面的示例列出授予了 CREATE TABLE 权限的所有用户。  
  
```  
EXEC sp_helprotect @name = 'CREATE TABLE';  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE (Transact-SQL)](../../t-sql/statements/revoke-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

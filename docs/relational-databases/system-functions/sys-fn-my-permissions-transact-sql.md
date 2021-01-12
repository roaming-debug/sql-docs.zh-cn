---
description: sys.fn_my_permissions (Transact-SQL)
title: sys.fn_my_permissions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.fn_my_permissions_TSQL
- fn_my_permissions_TSQL
- sys.fn_my_permissions
- fn_my_permissions
dev_langs:
- TSQL
helpviewer_keywords:
- fn_my_permissions function
- sys.fn_my_permissions function
ms.assetid: 30f97f00-03d8-443a-9de9-9ec420b7699b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5e67f2be709f91ee3214f490cfbcc2b932ad7dc5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101315"
---
# <a name="sysfn_my_permissions-transact-sql"></a>sys.fn_my_permissions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有效授予主体对安全对象的权限的列表。 [HAS_PERMS_BY_NAME](../../t-sql/functions/has-perms-by-name-transact-sql.md)相关的函数。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
fn_my_permissions ( securable , 'securable_class' )  
```  
  
## <a name="arguments"></a>参数  
 securable  
 安全对象的名称。 如果安全对象为服务器或数据库，则该值应设置为 NULL。 securable 是 sysname 类型的标量表达式。 *安全* 对象的名称可以是多部分名称。  
  
 "*securable_class*"  
 为其列出权限的安全对象的类的名称。 *securable_class* 是 **sysname**。 *securable_class* 必须是下列其中一项：应用程序角色、程序集、非对称密钥、证书、协定、数据库、终结点、全文目录、登录名、消息类型、对象、远程服务绑定、角色、路由、架构、服务器、服务、对称密钥、类型、用户和 XML 架构集合。  
  
## <a name="columns-returned"></a>返回的列  
 下表列出了 **fn_my_permissions** 返回的列。 返回的每一行说明了当前安全上下文拥有的对安全对象的一种权限。 如果查询失败，则返回 NULL。  
  
|列名称|类型|描述|  
|-----------------|----------|-----------------|  
|entity_name|**sysname**|对其有效授予所列权限的安全对象的名称。|  
|subentity_name|**sysname**|如果安全对象具有列，则为列名；否则为 NULL。|  
|permission_name|**nvarchar**|权限的名称。|  
  
## <a name="remarks"></a>备注  
 该表值函数返回调用主体持有的对指定安全对象的有效权限的列表。 有效权限包括下列任何一种权限：  
  
-   直接授予主体并且不被拒绝的权限。  
  
-   由主体持有的更高级权限暗含的、且不被拒绝的权限。  
  
-   授予主体所属的角色或组的、且不被拒绝的权限。  
  
-   由主体所属的角色或组持有的、且不被拒绝的权限。  
  
 权限评估始终在调用方的安全上下文中执行。 若要确定其他某个主体是否具有有效的权限，调用方必须对该主体具有 IMPERSONATE 权限。  
  
 对于架构级实体，可接受由一部分、两部分或三部分组成的非空名称。 对于数据库级实体，将接受由一个部分构成的名称，其值为 "*当前数据库*"。 对于服务器本身，则需要一个 Null（表示“当前服务器”）。 **fn_my_permissions** 无法检查对链接服务器的权限。  
  
 以下查询将返回内置安全对象类的列表：  
  
```  
SELECT DISTINCT class_desc FROM fn_builtin_permissions(default)  
    ORDER BY class_desc;  
GO  
```  
  
 如果默认值是作为 *安全* 对象或 *securable_class* 的值提供的，则该值将被解释为 NULL。  
  
## <a name="examples"></a>示例  
  
### <a name="a-listing-effective-permissions-on-the-server"></a>A. 列出对服务器的有效权限  
 以下示例返回调用方对服务器的有效权限的列表。  
  
```  
SELECT * FROM fn_my_permissions(NULL, 'SERVER');  
GO  
```  
  
### <a name="b-listing-effective-permissions-on-the-database"></a>B. 列出对数据库的有效权限  
 以下示例返回调用方对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的有效权限的列表。  
  
```  
USE AdventureWorks2012;  
SELECT * FROM fn_my_permissions (NULL, 'DATABASE');  
GO  
```  
  
### <a name="c-listing-effective-permissions-on-a-view"></a>C. 列出对视图的有效权限  
 以下示例返回调用方对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库内 `vIndividualCustomer` 架构中 `Sales` 视图的有效权限的列表。  
  
```  
USE AdventureWorks2012;  
SELECT * FROM fn_my_permissions('Sales.vIndividualCustomer', 'OBJECT')   
    ORDER BY subentity_name, permission_name ;   
GO   
```  
  
### <a name="d-listing-effective-permissions-of-another-user"></a>D. 列出另一个用户的有效权限  
 以下示例返回数据库用户 `Wanida` 对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库内 `Employee` 架构中 `HumanResources` 表的有效权限的列表。 调用方需要对用户 `Wanida` 具有 IMPERSONATE 权限。  
  
```  
EXECUTE AS USER = 'Wanida';  
SELECT * FROM fn_my_permissions('HumanResources.Employee', 'OBJECT')   
    ORDER BY subentity_name, permission_name ;    
REVERT;  
GO  
```  
  
### <a name="e-listing-effective-permissions-on-a-certificate"></a>E. 列出对证书的有效权限  
 以下示例返回调用方对当前数据库中证书 `Shipping47` 的有效权限的列表。  
  
```  
SELECT * FROM fn_my_permissions('Shipping47', 'CERTIFICATE');  
GO  
```  
  
### <a name="f-listing-effective-permissions-on-an-xml-schema-collection"></a>F. 列出对 XML 架构集合的有效权限  
 下面的示例返回调用方对数据库中名为的 XML 架构集合的有效权限的列表 `ProductDescriptionSchemaCollection` [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 。  
  
```  
USE AdventureWorks2012;  
SELECT * FROM fn_my_permissions('ProductDescriptionSchemaCollection',  
    'XML SCHEMA COLLECTION');  
GO  
```  
  
### <a name="g-listing-effective-permissions-on-a-database-user"></a>G. 列出对数据库用户的有效权限  
 以下示例返回调用方对当前数据库中用户 `MalikAr` 的有效权限的列表。  
  
```  
SELECT * FROM fn_my_permissions('MalikAr', 'USER');  
GO  
```  
  
### <a name="h-listing-effective-permissions-of-another-login"></a>H. 列出另一个登录名的有效权限  
 以下示例返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `WanidaBenshoof` 对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库内 `Employee` 架构中 `HumanResources` 表的有效权限的列表。 调用方需要对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `WanidaBenshoof` 具有 IMPERSONATE 权限。  
  
```  
EXECUTE AS LOGIN = 'WanidaBenshoof';  
SELECT * FROM fn_my_permissions('AdventureWorks2012.HumanResources.Employee', 'OBJECT')   
    ORDER BY subentity_name, permission_name ;    
REVERT;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全函数 (Transact-SQL)](../../t-sql/functions/security-functions-transact-sql.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [安全对象](../../relational-databases/security/securables.md)   
 [权限层次结构（数据库引擎）](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.fn_builtin_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [EXECUTE AS (Transact-SQL)](../../t-sql/statements/execute-as-transact-sql.md)  
  
  

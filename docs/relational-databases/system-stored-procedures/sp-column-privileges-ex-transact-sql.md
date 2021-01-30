---
description: sp_column_privileges_ex (Transact-SQL)
title: sp_column_privileges_ex (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_column_privileges_ex
- sp_column_privileges_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_column_privileges_ex
ms.assetid: 98cb6e58-4007-40fc-b048-449fb2e7e6be
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c4ce393362851e17471829272544df86840153fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174508"
---
# <a name="sp_column_privileges_ex-transact-sql"></a>sp_column_privileges_ex (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回指定链接服务器上的指定表的列特权。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_column_privileges_ex [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]   
     [ , [ @table_catalog = ] 'table_catalog' ]   
     [ , [ @column_name = ] 'column_name' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @table_server = ] 'table_server'` 要返回其信息的链接服务器的名称。 *table_server* **sysname**，无默认值。  
  
`[ @table_name = ] 'table_name'` 包含指定列的表的名称。 *table_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @table_schema = ] 'table_schema'` 表架构。 *table_schema* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @table_catalog = ] 'table_catalog'` 指定 *table_name* 所在的数据库的名称。 *table_catalog* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @column_name = ] 'column_name'` 为其提供特权信息的列的名称。 *column_name* 的值为 **sysname**，默认值为 NULL (所有公用) 。  
  
## <a name="result-sets"></a>结果集  
 下表显示结果集列。 返回的结果按 **TABLE_QUALIFIER**、 **TABLE_OWNER**、 **TABLE_NAME**、 **COLUMN_NAME** 和 **特权** 进行排序。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**TABLE_CAT**|**sysname**|表限定符名称。 各种 DBMS 产品支持表的三部分命名 (_限定符_**。**_所有者_**。**_名称_) 。 在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，此列表示数据库名称。 在某些产品中，该列表示表所在的数据库环境的服务器名。 此字段可以为 NULL。|  
|**TABLE_SCHEM**|**sysname**|表所有者名称。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，此列表示创建该表的数据库用户的名称。 此字段始终返回值。|  
|**TABLE_NAME**|**sysname**|表名。 此字段始终返回值。|  
|**COLUMN_NAME**|**sysname**|列名称，为返回的每个列的 **TABLE_NAME** 。 此字段始终返回值。|  
|**GRANTOR**|**sysname**|已 **向列出的** 被授权者授予对此 **COLUMN_NAME** 的权限的数据库用户名。 在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，此列始终与 **TABLE_OWNER** 相同。 此字段始终返回值。<br /><br /> **授权** 者列可以是数据库所有者 (**TABLE_OWNER**) 或数据库所有者通过 GRANT 语句中的 WITH GRANT OPTION 子句授予权限的人。|  
|**GRANTEE**|**sysname**|已被列出的 **授权** 者授予此 **COLUMN_NAME** 的权限的数据库用户名。 此字段始终返回值。|  
|**PRIVILEGE**|**varchar (** 32 **)**|可用列权限中的一个。 列权限可以是下列值中的一个（或定义实现时数据源支持的其他值）：<br /><br /> SELECT = **被** 授权者可以检索列的数据。<br /><br /> INSERT = **当** 被授权者)  (插入新行 **时，被** 授权者可以为此列提供数据。<br /><br /> UPDATE = **被** 授权者可以修改列中的现有数据。<br /><br /> REFERENCE = 被授权者 **可以通过主键** /外键关系引用外表中的列。 主键/外键关系是使用表约束定义的。|  
|**IS_GRANTABLE**|**varchar (** 3 **)**|指示是否允许 **被授权** 者向其他用户授予权限， (通常称为 "授予授权" 权限) 。 可以是 YES、NO 或 NULL。 未知值或 NULL 值表示不能使用“授予再授予”(grant with grant) 的数据源。|  
  
## <a name="permissions"></a>权限  
 需要对架构的 SELECT 权限。  
  
## <a name="examples"></a>示例  
 以下示例返回 `HumanResources.Department` 链接服务器上 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中 `Seattle1` 表的列特权信息。  
  
```  
EXEC sp_column_privileges_ex @table_server = 'Seattle1',   
   @table_name = 'Department',   
   @table_schema = 'HumanResources',  
   @table_catalog ='AdventureWorks2012';  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_table_privileges_ex &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-table-privileges-ex-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

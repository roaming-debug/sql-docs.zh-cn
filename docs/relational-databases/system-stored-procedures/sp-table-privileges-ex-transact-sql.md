---
description: sp_table_privileges_ex (Transact-SQL)
title: sp_table_privileges_ex (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_table_privileges_ex
- sp_table_privileges_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_table_privileges_ex
ms.assetid: b58d4a07-5c40-4f17-b66e-6d6b17188dda
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 41c9548f0c4841b6a9c1845a22c7883cd42ad88b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203673"
---
# <a name="sp_table_privileges_ex-transact-sql"></a>sp_table_privileges_ex (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关指定链接服务器中的指定表的特权信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_table_privileges_ex [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]   
     [ , [ @table_catalog = ] 'table_catalog' ]  
     [ , [@fUsePattern =] 'fUsePattern']  
```  
  
## <a name="arguments"></a>参数  
`[ @table_server = ] 'table_server'` 要返回其信息的链接服务器的名称。 *table_server* **sysname**，无默认值。  
  
`[ @table_name = ] 'table_name']` 要为其提供表特权信息的表的名称。 *table_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @table_schema = ] 'table_schema'` 表架构。 在某些 DBMS 环境中是表所有者。 *table_schema* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @table_catalog = ] 'table_catalog'` 指定 *table_name* 所在的数据库的名称。 *table_catalog* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @fUsePattern = ] 'fUsePattern'` 确定字符 "_"、"%"、"[" 和 "]" 是否解释为通配符。 有效值为 0（模式匹配为关闭状态）和 1（模式匹配为打开状态）。 *fUsePattern* 的值为 **bit**，默认值为1。  
  
## <a name="return-code-values"></a>返回代码值  
 无  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**TABLE_CAT**|**sysname**|表限定符名称。 各种 DBMS 产品支持表的三部分命名 (_限定符_**。**_所有者_**。**_名称_) 。 在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，此列表示数据库名称。 在某些产品中，该列表示表所在的数据库环境的服务器名。 此字段可以为 NULL。|  
|**TABLE_SCHEM**|**sysname**|表所有者名称。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，此列表示创建该表的数据库用户的名称。 此字段始终返回值。|  
|**TABLE_NAME**|**sysname**|表名。 此字段始终返回值。|  
|**GRANTOR**|**sysname**|已 **向列出的** 被授权者授予对此 **TABLE_NAME** 的权限的数据库用户名。 在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，此列始终与 **TABLE_OWNER** 相同。 此字段始终返回值。 此外，授权者列可以是数据库所有者 (**TABLE_OWNER**) 或数据库所有者通过 grant 语句中的 WITH GRANT OPTION 子句授予权限的用户。|  
|**GRANTEE**|**sysname**|已被列出的 **授权** 者授予此 **TABLE_NAME** 的权限的数据库用户名。 此字段始终返回值。|  
|**PRIVILEGE**|**varchar (** 32 **)**|可用的表权限之一。 表权限可以是以下值之一或定义实现时数据源支持的其他值：<br /><br /> SELECT = **被** 授权者可以检索一列或多列的数据。<br /><br /> INSERT = **被** 授权者可以为一个或多个列的新行提供数据。<br /><br /> UPDATE = **被** 授权者可以修改一个或多个列的现有数据。<br /><br /> DELETE = **被** 授权者可以从表中删除行。<br /><br /> REFERENCE = 被授权者 **可以通过主键** /外键关系引用外表中的列。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，主键/外键关系是使用表约束定义的。<br /><br /> 指定给被特定表权限的 **被授权者的** 操作的作用域是依赖于数据源的。 例如，"更新" 权限可以使被 **授权** 者可以更新一个数据源的表中的所有列，并且只 **更新对其他** 数据源具有 "更新" 权限的那些列。|  
|**IS_GRANTABLE**|**varchar (** 3 **)**|指示 **是否允许被授权** 者向其他用户授予权限。 这常常称为“授予再授予”权限。 可以是 YES、NO 或 NULL。 未知的（或 NULL）值指不适用“授予再授予”的数据源。|  
  
## <a name="remarks"></a>备注  
 返回的结果按 **TABLE_QUALIFIER**、 **TABLE_OWNER**、 **TABLE_NAME** 和 **特权** 进行排序。  
  
## <a name="permissions"></a>权限  
 需要对架构的 SELECT 权限。  
  
## <a name="examples"></a>示例  
 以下示例从指定的链接数据库 `Product` 返回有关 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中名称以 `Seattle1` 开头的表的特权信息。  ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 假设为链接服务器) 。  
  
```  
EXEC sp_table_privileges_ex @table_server = 'Seattle1',   
   @table_name = 'Product%',   
   @table_schema = 'Production',  
   @table_catalog ='AdventureWorks2012';  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_column_privileges_ex &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-column-privileges-ex-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [分布式查询存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)  
  
  

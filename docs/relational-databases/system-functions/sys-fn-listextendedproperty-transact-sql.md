---
description: sys.fn_listextendedproperty (Transact-SQL)
title: sys.fn_listextendedproperty (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_listextendedproperty
- fn_listextendedproperty_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_listextendedproperty function
- displaying extended properties
- database extended properties [SQL Server]
- viewing extended properties
- column extended properties [SQL Server]
- sys.fn_listextendedproperties function
- database objects [SQL Server], extended properties
- extended properties [SQL Server], columns
- table extended properties [SQL Server]
ms.assetid: 59bbb91f-a277-4a35-803e-dcb91e847a49
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e8b60e2fabcccd2040c3dceb1307b7f7e7907bda
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201940"
---
# <a name="sysfn_listextendedproperty-transact-sql"></a>sys.fn_listextendedproperty (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回数据库对象的扩展属性值。  
 
 
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
fn_listextendedproperty (   
    { default | 'property_name' | NULL }   
  , { default | 'level0_object_type' | NULL }   
  , { default | 'level0_object_name' | NULL }   
  , { default | 'level1_object_type' | NULL }   
  , { default | 'level1_object_name' | NULL }   
  , { default | 'level2_object_type' | NULL }   
  , { default | 'level2_object_name' | NULL }   
  )   
```  
  
## <a name="arguments"></a>参数  
 {默认值 |"*property_name*" |无效  
 属性的名称。 *property_name* **sysname**。 有效输入包括默认值 NULL 或属性名。  
  
 {默认值 |"*level0_object_type*" |无效  
 用户或用户定义类型。 *level0_object_type* 是 **varchar (128)**，默认值为 NULL。 有效输入包括 ASSEMBLY、CONTRACT、EVENT NOTIFICATION、FILEGROUP、MESSAGE TYPE、PARTITION FUNCTION、PARTITION SCHEME、REMOTE SERVICE BINDING、ROUTE、SCHEMA、SERVICE、TRIGGER、TYPE、USER 和 NULL。  
  
> [!IMPORTANT]  
>  作为级别 0 类型的 USER 和 TYPE 将在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中删除。 请避免在新的开发工作中使用这些功能，并考虑修改当前使用这些功能的应用程序。 改用 SCHEMA 替代 USER 作为级别 0 类型。 对于 TYPE，请使用 SCHEMA 作为级别 0 类型，使用 TYPE 作为级别 1 类型。  
  
 {默认值 |"*level0_object_name*" |无效  
 所指定的级别 0 对象类型的名称。 *level0_object_name* 的值为 **sysname** ，默认值为 NULL。 有效输入包括默认值 NULL 或对象名称。  
  
 {默认值 |"*level1_object_type*" |无效  
 级别 1 对象的类型。 *level1_object_type* 是 **varchar (128)** ，默认值为 NULL。 有效的输入包括：AGGREGATE、DEFAULT、FUNCTION、LOGICAL FILE NAME、PROCEDURE、QUEUE、RULE、SYNONYM、TABLE、TYPE、VIEW、XML SCHEMA COLLECTION 和 NULL。  
  
> [!NOTE]  
>  默认值映射到 NULL，而“default”映射到对象类型 DEFAULT。  
  
 {默认值 |"*level1_object_name*" |无效  
 所指定的级别 1 对象类型的名称。 *level1_object_name* 的值为 **sysname** ，默认值为 NULL。 有效输入包括默认值 NULL 或对象名称。  
  
 {默认值 |"*level2_object_type*" |无效  
 级别 2 对象的类型。 *level2_object_type* 是 **varchar (128)** ，默认值为 NULL。 有效输入包括 DEFAULT、默认值（映射到 NULL）和 NULL。 *Level2_object_type* 的有效输入为 COLUMN、CONSTRAINT、EVENT NOTIFICATION、INDEX、PARAMETER、TRIGGER 和 NULL。  
  
 {默认值 |"*level2_object_name*" |无效  
 所指定的级别 2 对象类型的名称。 *level2_object_name* 的值为 **sysname** ，默认值为 NULL。 有效输入包括默认值 NULL 或对象名称。  
  
## <a name="tables-returned"></a>返回的表  
 下面是 fn_listextendedproperty 返回的表的格式。  
  
|列名称|数据类型|  
|-----------------|---------------|  
|objtype|**sysname**|  
|objname|**sysname**|  
|name|**sysname**|  
|value|**sql_variant**|  
  
 如果返回的表为空，可能对象没有扩展属性或用户不具有列出对象扩展属性的权限。 当返回数据库本身的扩展属性时，objtype 和 objname 列将为 NULL。  
  
## <a name="remarks"></a>备注  
 如果 *property_name* 的值为 NULL 或默认值，则 fn_listextendedproperty 返回指定对象的所有属性。  
  
 如果指定了对象类型，并且对应的对象名的值为 NULL 或默认值，则 fn_listextendedproperty 将返回指定类型的所有对象的所有扩展属性。  
  
 对象是按级别区分的，级别 0 为最高，级别 2 为最低。 如果指定了较低级别的对象（级别 1 或级别 2）的类型和名称，则父对象类型和名称应当为 NULL 或默认值以外的给定值。 否则，此函数返回空结果集。  
  
 **objname** 已固定为 Latin1_General_CI_AI。 不过，可以通过在比较中覆盖排序规则来解决此问题。  
  
```  
SELECT o.[object_id] AS 'table_id', o.[name] 'table_name',  
0 AS 'column_order', NULL AS 'column_name', NULL AS 'column_datatype',  
NULL AS 'column_length', Cast(e.value AS varchar(500)) AS 'column_description'  
FROM AdventureWorks.sys.objects AS o  
LEFT JOIN sys.fn_listextendedproperty(N'MS_Description', N'user',N'HumanResources',N'table', N'Employee', null, default) AS e  
    ON o.name = e.objname COLLATE SQL_Latin1_General_CP1_CI_AS  
WHERE o.name = 'Employee';  
```  
  
## <a name="permissions"></a>权限  
 列出对象的扩展属性的权限随对象类型的不同而有所不同。  
  
## <a name="examples"></a>示例  
  
### <a name="a-displaying-extended-properties-on-a-database"></a>A. 显示数据库的扩展属性  
 以下示例显示为数据库对象本身设置的所有扩展属性。  
  
```  
USE AdventureWorks2012;  
GO  
SELECT objtype, objname, name, value  
FROM fn_listextendedproperty(default, default, default, default, default, default, default);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `objtype    objname     name            value`  
  
 `---------  ---------   -----------     ----------------------------`  
  
 `NULL       NULL        MS_Description  AdventureWorks2008 Sample OLTP Database`  
  
 `(1 row(s) affected)`  
  
### <a name="b-displaying-extended-properties-on-all-columns-in-a-table"></a>B. 显示表中所有列的扩展属性  
 下面的示例列出了表中的列的扩展属性 `ScrapReason` 。 这包含在架构 `Production` 中。  
  
```  
USE AdventureWorks2012;  
GO  
SELECT objtype, objname, name, value  
FROM fn_listextendedproperty (NULL, 'schema', 'Production', 'table', 'ScrapReason', 'column', default);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `objtype objname      name            value`  
  
 `------- -----------  -------------   ------------------------`  
  
 `COLUMN ScrapReasonID MS_Description  Primary key for ScrapReason records.`  
  
 `COLUMN Name          MS_Description  Failure description.`  
  
 `COLUMN ModifiedDate  MS_Description  Date the record was last updated.`  
  
 `(3 row(s) affected)`  
  
### <a name="c-displaying-extended-properties-on-all-tables-in-a-schema"></a>C. 显示架构中所有表的扩展属性  
 下面的示例列出了包含在架构中的所有表的扩展属性 `Sales` 。  
  
```  
USE AdventureWorks2012;  
GO  
SELECT objtype, objname, name, value  
FROM fn_listextendedproperty (NULL, 'schema', 'Sales', 'table', default, NULL, NULL);  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_addextendedproperty &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md)   
 [sp_dropextendedproperty &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropextendedproperty-transact-sql.md)   
 [sp_updateextendedproperty &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-updateextendedproperty-transact-sql.md)   
 [sys.extended_properties &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md)  
  
  

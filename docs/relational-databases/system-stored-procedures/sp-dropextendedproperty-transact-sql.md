---
description: sp_dropextendedproperty (Transact-SQL)
title: sp_dropextendedproperty (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropextendedproperty_TSQL
- sp_dropextendedproperty
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropextendedproperty
ms.assetid: 4851865a-86ca-4823-991a-182dd1934075
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c42236cc1f0157d774ea38d356ba8f122407c2c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205136"
---
# <a name="sp_dropextendedproperty-transact-sql"></a>sp_dropextendedproperty (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  删除现有的扩展属性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_dropextendedproperty   
    [ @name = ] { 'property_name' }  
      [ , [ @level0type = ] { 'level0_object_type' }   
        , [ @level0name = ] { 'level0_object_name' }   
            [ , [ @level1type = ] { 'level1_object_type' }   
              , [ @level1name = ] { 'level1_object_name' }   
                [ , [ @level2type = ] { 'level2_object_type' }   
                  , [ @level2name = ] { 'level2_object_name' }   
                ]   
            ]   
        ]   
    ]   
```  
  
## <a name="arguments"></a>参数  
 [ @name =] {'*property_name*'}  
 要删除的属性的名称。 *property_name* 为 **sysname** ，且不能为 NULL。  
  
 [ @level0type =] {'*level0_object_type*'}  
 所指定的级别 0 对象类型的名称。 *level0_object_type* 是 **varchar (128)**，默认值为 NULL。  
  
 有效输入包括：ASSEMBLY、CONTRACT、EVENT NOTIFICATION、FILEGROUP、MESSAGE TYPE、PARTITION FUNCTION、PARTITION SCHEME、REMOTE SERVICE BINDING、ROUTE、SCHEMA、SERVICE、USER、TRIGGER、TYPE 和 NULL。  
  
> [!IMPORTANT]  
>  作为级别 0 类型的 USER 和 TYPE 将在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中删除。 请避免在新的开发工作中使用这些功能，并考虑修改当前使用这些功能的应用程序。 改用 SCHEMA 替代 USER 作为级别 0 类型。 对于 TYPE，请使用 SCHEMA 作为级别 0 类型，使用 TYPE 作为级别 1 类型。  
  
 [ @level0name =] {'*level0_object_name*'}  
 所指定的级别 0 对象类型的名称。 *level0_object_name* 的值为 **sysname** ，默认值为 NULL。  
  
 [ @level1type =] {'*level1_object_type*'}  
 级别 1 对象的类型。 *level1_object_type* 是 **varchar (128)** ，默认值为 NULL。 有效的输入包括：AGGREGATE、DEFAULT、FUNCTION、LOGICAL FILE NAME、PROCEDURE、QUEUE、RULE、SYNONYM、TABLE、TABLE_TYPE、TYPE、VIEW、XML SCHEMA COLLECTION 和 NULL。  
  
 [ @level1name =] {'*level1_object_name*'}  
 所指定的级别 1 对象类型的名称。 *level1_object_name* 的值为 **sysname** ，默认值为 NULL。  
  
 [ @level2type =] {'*level2_object_type*'}  
 级别 2 对象的类型。 *level2_object_type* 是 **varchar (128)** ，默认值为 NULL。 有效的输入包括：COLUMN、CONSTRAINT、EVENT NOTIFICATION、INDEX、PARAMETER、TRIGGER 和 NULL。  
  
 [ @level2name =] {'*level2_object_name*'}  
 所指定的级别 2 对象类型的名称。 *level2_object_name* 的值为 **sysname** ，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 为了指定扩展属性，将数据库中的对象分为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 三个级别：0、1和2。 级别0是最高级别，定义为数据库作用域内包含的对象。 级别 1 的对象包含在架构作用域或用户作用域中，而级别 2 的对象包含在级别 1 对象中。 可以为这些级别中任一级别的对象定义扩展属性。 引用某个级别中的对象必须用所有更高级别对象的类型和名称进行限制。  
  
 给定有效的 *property_name*，如果所有对象类型和名称都为空，并且当前数据库中存在属性，则删除该属性。 请参阅本主题后面部分中的示例 B。  
  
## <a name="permissions"></a>权限  
 db_owner 和 db_ddladmin 固定数据库角色的成员可以删除任意对象的扩展属性，但以下情况例外：db_ddladmin 不能向数据库本身添加属性，也不能向用户或角色中添加属性。  
  
 用户可以删除他们所拥有的对象的扩展属性，或者删除他们对其具有 ALTER 或 CONTROL 权限的对象的扩展属性。  
  
## <a name="examples"></a>示例  
  
### <a name="a-dropping-an-extended-property-on-a-column"></a>A. 删除列上的扩展属性  
 以下示例从架构 `caption` 内包含的表 `id` 中的列 `T1` 上删除属性 `dbo`。  
  
```  
CREATE TABLE T1 (id int , name char (20));  
GO  
EXEC sp_addextendedproperty   
     @name = 'caption'   
    ,@value = 'Employee ID'   
    ,@level0type = 'schema'   
    ,@level0name = dbo  
    ,@level1type = 'table'  
    ,@level1name = 'T1'  
    ,@level2type = 'column'  
    ,@level2name = id;  
GO  
EXEC sp_dropextendedproperty   
     @name = 'caption'   
    ,@level0type = 'schema'   
    ,@level0name = dbo  
    ,@level1type = 'table'  
    ,@level1name = 'T1'  
    ,@level2type = 'column'  
    ,@level2name = id;  
GO  
DROP TABLE T1;  
GO  
```  
  
### <a name="b-dropping-an-extended-property-on-a-database"></a>B. 删除数据库中的扩展属性  
 下面的示例 `MS_Description` 从示例数据库中删除名为的属性 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 。 由于属性位于数据库本身中，因此不指定对象类型和名称。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_dropextendedproperty   
@name = N'MS_Description';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sys.fn_listextendedproperty &#40;Transact-sql&#41;](../../relational-databases/system-functions/sys-fn-listextendedproperty-transact-sql.md)   
 [sp_addextendedproperty &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md)   
 [sp_updateextendedproperty &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-updateextendedproperty-transact-sql.md)   
 [sys.extended_properties &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md)  
  
  

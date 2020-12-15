---
description: sp_addextendedproperty (Transact-SQL)
title: sp_addextendedproperty (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 01/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_addextendedproperty
- sp_addextendedproperty_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addextendedproperty
ms.assetid: 565483ea-875b-4133-b327-d0006d2d7b4c
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 184328e9b6d5c197b06f89f151942535a90f7f91
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474648"
---
# <a name="sp_addextendedproperty-transact-sql"></a>sp_addextendedproperty (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  将新扩展属性添加到数据库对象中。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addextendedproperty  
    [ @name = ] { 'property_name' }  
    [ , [ @value = ] { 'value' }   
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
[;]  
```  
  
## <a name="arguments"></a>自变量  
 [ @name ] = {'*property_name*'}  
 要添加的属性的名称。 *property_name* 为 **sysname** ，且不能为 NULL。 名称还可以包括空格或非字母数字字符串以及二进制值。  
  
 [ @value =] {"*value*"}  
 要与属性关联的值。 *值* **sql_variant**，默认值为 NULL。 *value* 的大小不能超过 7,500 个字节。  
  
 [ @level0type =] {'*level0_object_type*'}  
 级别 0 对象的类型。 *level0_object_type* 是 **varchar (128)**，默认值为 NULL。  
  
 有效输入包括：ASSEMBLY、CONTRACT、EVENT NOTIFICATION、FILEGROUP、MESSAGE TYPE、PARTITION FUNCTION、PARTITION SCHEME、REMOTE SERVICE BINDING、ROUTE、SCHEMA、SERVICE、USER、TRIGGER、TYPE、PLAN GUIDE 和 NULL。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]将来的  版本中将删除在级别 1 类型对象的扩展属性中指定 USER 作为级别 0 类型的功能。 改用 SCHEMA 作为级别 0 类型。 例如，在定义表的扩展属性时，指定表的架构而不是用户名。 将来的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本中将删除指定 TYPE 作为级别 0 类型的功能。 对于 TYPE，请使用 SCHEMA 作为级别 0 类型，使用 TYPE 作为级别 1 类型。  
  
 [ @level0name =] {'*level0_object_name*'}  
 所指定的级别 0 对象类型的名称。 *level0_object_name* 的值为 **sysname** ，默认值为 NULL。  
  
 [ @level1type =] {'*level1_object_type*'}  
 级别 1 对象的类型。 *level1_object_type* 是 **varchar (128)**，默认值为 NULL。 有效的输入包括 AGGREGATE、DEFAULT、FUNCTION、LOGICAL FILE NAME、PROCEDURE、QUEUE、RULE、SEQUENCE、同义词、TABLE、TABLE_TYPE、TYPE、VIEW、XML SCHEMA COLLECTION 和 NULL。    
 [ @level1name =] {'*level1_object_name*'}  
 所指定的级别 1 对象类型的名称。 *level1_object_name* 的默认值为 **sysname**，默认值为 NULL。  
  
 [ @level2type =] {'*level2_object_type*'}  
 级别 2 对象的类型。 *level2_object_type* 是 **varchar (128)**，默认值为 NULL。 有效的输入包括：COLUMN、CONSTRAINT、EVENT NOTIFICATION、INDEX、PARAMETER、TRIGGER 和 NULL。  
  
 [ @level2name =] {'*level2_object_name*'}  
 所指定的级别 2 对象类型的名称。 *level2_object_name* 的默认值为 **sysname**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 为了指定扩展属性， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中的对象分为三个级别：0、1 和 2。 级别 0 是最高级别，定义为在数据库作用域内包含的对象。 级别 1 的对象包含在架构作用域或用户作用域中，而级别 2 的对象包含在级别 1 对象中。 可以为这些级别中任一级别的对象定义扩展属性。  
  
 引用某个级别中的对象必须用拥有或包含它们的更高级别对象的名称进行限制。 例如，当将扩展属性添加到表列（级别 2）时，还必须指定包含该列的表名（级别 1）以及包含该表的架构（级别 0）。  
  
 如果所有对象类型和名称都为空，则属性属于当前数据库本身。  
  
 对于系统对象、用户定义数据库的作用域以外的对象或者未在参数中作为有效输入列出的对象，不允许使用扩展属性。  
  
 内存优化表中不允许使用扩展属性。  
  
## <a name="replicating-extended-properties"></a>复制扩展属性  
 仅在发布服务器和订阅服务器之间的初始同步中复制扩展属性。 如果在初始同步之后添加或修改扩展属性，则不会复制该更改。 有关如何复制数据库对象的详细信息，请参阅 [发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)。  
  
## <a name="schema-vs-user"></a>架构与User  
 建议不要在将扩展属性应用于数据库对象时指定 USER 作为级别 0 类型，因为这会导致名称解析不明确。 例如，假定用户 Mary 拥有两个架构（Mary 和 MySchema），并且这两个架构都包含名为 MyTable 的表。 如果 Mary 将扩展属性添加到表 MyTable 并指定 **@level0type = N'USER '**， **@level0name = Mary**，则扩展属性应用于哪个表并不明确。 为了保持向后兼容，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将属性应用于名为 Mary 的架构中包含的表。  
  
## <a name="permissions"></a>权限  
 db_owner 和 db_ddladmin 固定数据库角色的成员可以向任何对象添加扩展属性，但以下情况例外：db_ddladmin 不能向数据库本身添加属性，也不能向用户或角色中添加属性。  
  
 用户可以将扩展属性添加到他们所拥有的对象中，或者添加到他们对其具有 ALTER 或 CONTROL 权限的对象中。  
  
## <a name="examples"></a>示例  
  
### <a name="a-adding-an-extended-property-to-a-database"></a>A. 将扩展属性添加到数据库中  
 `'Caption'` 以下示例将值为 `'AdventureWorks2012 Sample OLTP Database'` 的属性名称 `AdventureWorks2012` 添加到  示例数据库中。  
  
```  
USE AdventureWorks2012;  
GO  
--Add a caption to the AdventureWorks2012 Database object itself.  
EXEC sp_addextendedproperty   
@name = N'Caption',   
@value = 'AdventureWorks2012 Sample OLTP Database';  
```  
  
### <a name="b-adding-an-extended-property-to-a-column-in-a-table"></a>B. 将扩展属性添加到表中的列  
 `PostalCode` 以下示例将 caption 属性添加到 `Address`表中的  列。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_addextendedproperty   
@name = N'Caption',   
@value = 'Postal code is a required column.',  
@level0type = N'Schema', @level0name = 'Person',  
@level1type = N'Table',  @level1name = 'Address',  
@level2type = N'Column', @level2name = 'PostalCode';  
GO  
```  
  
### <a name="c-adding-an-input-mask-property-to-a-column"></a>C. 将输入 input mask 添加到列中  
 `99999 or 99999-9999 or #### ###`以下示例将输入 input mask 属性“ `PostalCode` ”添加到 `Address`表中的  列。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_addextendedproperty   
@name = N'Input Mask ', @value = '99999 or 99999-9999 or #### ###',  
@level0type = N'Schema', @level0name = 'Person',  
@level1type = N'Table', @level1name = 'Address',   
@level2type = N'Column',@level2name = 'PostalCode';  
GO  
```  
  
### <a name="d-adding-an-extended-property-to-a-filegroup"></a>D. 将扩展属性添加到文件组中  
 `PRIMARY` 下面的示例向  文件组添加了一个扩展属性。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_DescriptionExample',   
@value = N'Primary filegroup for the AdventureWorks2012 sample database.',   
@level0type = N'FILEGROUP', @level0name = 'PRIMARY';  
GO  
```  
  
### <a name="e-adding-an-extended-property-to-a-schema"></a>E. 将扩展属性添加到架构中  
 下面的示例向  架构添加了一个扩展属性。 `HumanResources`  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_addextendedproperty   
@name = N'MS_DescriptionExample',  
@value = N'Contains objects related to employees and departments.',  
@level0type = N'SCHEMA',   
@level0name = 'HumanResources';  
```  
  
### <a name="f-adding-an-extended-property-to-a-table"></a>F. 将扩展属性添加到表中  
 `Address` 下面的示例将扩展属性添加到 `Person` 架构中的  表。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_DescriptionExample',   
@value = N'Street address information for customers, employees, and vendors.',   
@level0type = N'SCHEMA', @level0name = 'Person',  
@level1type = N'TABLE',  @level1name = 'Address';  
GO  
```  
  
### <a name="g-adding-an-extended-property-to-a-role"></a>G. 将扩展属性添加到角色中  
 下面的示例创建了一个应用程序角色并向该角色添加了一个扩展属性。  
  
```  
USE AdventureWorks2012;   
GO  
CREATE APPLICATION ROLE Buyers  
WITH Password = '987G^bv876sPY)Y5m23';   
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_Description',   
@value = N'Application Role for the Purchasing Department.',  
@level0type = N'USER',  
@level0name = 'Buyers';  
```  
  
### <a name="h-adding-an-extended-property-to-a-type"></a>H. 将扩展属性添加到类型中  
 下面的示例向类型添加了一个扩展属性。  
  
```  
USE AdventureWorks2012;   
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_Description',   
@value = N'Data type (alias) to use for any column that represents an order number. For example a sales order number or purchase order number.',   
@level0type = N'SCHEMA',   
@level0name = N'dbo',   
@level1type = N'TYPE',   
@level1name = N'OrderNumber';  
```  
  
### <a name="i-adding-an-extended-property-to-a-user"></a>I. 向用户添加扩展属性  
 下面的示例创建了一个用户并向该用户添加了一个扩展属性。  
  
```  
USE AdventureWorks2012;   
GO  
CREATE USER CustomApp WITHOUT LOGIN ;   
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_Description',   
@value = N'User for an application.',   
@level0type = N'USER',   
@level0name = N'CustomApp';  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sys.fn_listextendedproperty &#40;Transact-sql&#41;](../../relational-databases/system-functions/sys-fn-listextendedproperty-transact-sql.md)   
 [sp_dropextendedproperty &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropextendedproperty-transact-sql.md)   
 [sp_updateextendedproperty &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-updateextendedproperty-transact-sql.md)  
  
  

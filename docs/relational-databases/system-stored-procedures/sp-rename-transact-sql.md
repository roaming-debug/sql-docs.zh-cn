---
description: sp_rename (Transact-SQL)
title: sp_rename (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 10/14/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_rename_TSQL
- sp_rename
dev_langs:
- TSQL
helpviewer_keywords:
- renaming indexes
- renaming columns
- sp_rename
- renaming tables
ms.assetid: bc3548f0-143f-404e-a2e9-0a15960fc8ed
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: 2e24d74b8c2d2d3e661ab5169ae800e99af42f68
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104610789"
---
# <a name="sp_rename-transact-sql"></a>sp_rename (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  在当前数据库中更改用户创建对象的名称。 此对象可以是表、索引、列、别名数据类型或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 公共语言运行时 (CLR) 用户定义类型。  
  
> [!NOTE]
> 在中 [!INCLUDE[ssazuresynapse](../../includes/ssazuresynapse_md.md)] ，sp_rename 处于 **预览阶段** ，只能用于重命名 **dbo** 架构中用户对象的列。

> [!CAUTION]  
>  更改对象名的任一部分都可能破坏脚本和存储过程。 我们建议您不要使用此语句来重命名存储过程、触发器、用户定义函数或视图；而是删除该对象，然后使用新名称重新创建该对象。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```sql  
-- Transact-SQL Syntax for sp_rename in SQL Server and Azure SQL Database
sp_rename [ @objname = ] 'object_name' , [ @newname = ] 'new_name'   
    [ , [ @objtype = ] 'object_type' ]   
```  

```sql  
-- Transact-SQL Syntax for sp_rename (preview) in Azure Synapse Analytics
sp_rename [ @objname = ] 'object_name' , [ @newname = ] 'new_name'   
    , [ @objtype = ] 'COLUMN'   
``` 
  
## <a name="arguments"></a>参数  
 [ @objname =] "*object_name*"  
 用户对象或数据类型的当前限定或非限定名称。 如果要重命名的对象是表中的列，则 *object_name* 必须是表的形式。*列* 或 *架构。* 如果要重命名的对象是一个索引，则 *object_name* 必须是表的形式。*索引* 或 *架构。* 如果要重命名的对象是一个约束，则 *object_name* 的格式必须为 *schema。*  
  
 只有在指定了限定对象时才必须使用引号。 如果提供的是完全限定名称（包括数据库名称），则数据库名称必须是当前数据库的名称。 *object_name* 为 **nvarchar (776)**，无默认值。  
  
 [ @newname =] "*new_name*"  
 指定对象的新名称。 *new_name* 必须是由一个部分构成的名称，并且必须遵循标识符的规则。 *newname* 为 **sysname**，无默认值。  
  
> [!NOTE]  
>  触发器名称不能以 # 或 ## 开头。  
  
 [ @objtype =] "*object_type*"  
 要重命名的对象的类型。 *object_type* 是 **varchar (13)**，默认值为 NULL，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|COLUMN|要重命名的列。|  
|DATABASE|用户定义数据库。 重命名数据库时需要此对象类型。|  
|INDEX|用户定义索引。 重命名带统计信息的索引时，也会自动重命名统计信息。|  
|OBJECT|在 [sys.databases](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)中跟踪的类型的项。 例如，OBJECT 可用于重命名包含约束（CHECK、FOREIGN KEY、PRIMARY/UNIQUE KEY）、用户表和规则的对象。|  
|STATISTICS|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。<br /><br /> 由用户显式创建的统计信息或使用索引隐式创建的统计信息。 重命名索引的统计信息时，也会自动重命名索引。|  
|USERDATATYPE|通过执行[CREATE TYPE](../../t-sql/statements/create-type-transact-sql.md)或[Sp_addtype](../../relational-databases/system-stored-procedures/sp-addtype-transact-sql.md)添加的[CLR 用户定义类型](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)。|  
  
[ @objtype =] "*COLUMN*" **适用于**： Azure Synapse Analytics  
在 sp_rename 中 (预览) [!INCLUDE[ssazuresynapse](../../includes/ssazuresynapse_md.md)] ， *列* 是一个必需参数，它指定要重命名的对象类型是一列。 它是一个 **varchar (13)** ，无默认值，必须始终包含在 sp_rename (preview) 语句中。 仅当列是非分布列时，才能对其进行重命名。

## <a name="return-code-values"></a>返回代码值  
 0（成功）或非零数字（失败）  
  
## <a name="remarks"></a>备注  
**适用** 于SQL Server (支持的所有版本) 和 Azure SQL 数据库  
 每当重命名 PRIMARY KEY 或 UNIQUE 约束时，sp_rename 都会自动重命名关联的索引。 如果重命名的索引与 PRIMARY KEY 约束关联，则 sp_rename 也会自动重命名该 PRIMARY KEY 约束。  

**适用** 于SQL Server (支持的所有版本) 和 Azure SQL 数据库  
 sp_rename 可用于重命名主要和次要的 XML 索引。  
  
**适用** 于SQL Server (支持的所有版本) 和 Azure SQL 数据库  
 重命名存储过程、函数、视图或触发器时，不会在 [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) 目录视图的 "定义" 列中更改相应对象的名称，也不会使用 [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) 内置函数获取该对象的名称。 因此，我们建议不要使用 sp_rename 重命名这些对象类型。 而是删除对象，然后使用新名称重新创建该对象。  

**适用** 于SQL Server (支持的所有版本) 、Azure SQL Database 和 Azure Synapse Analytics  
 重命名表或列等对象将不会自动重命名对该对象的引用。 您必须手动修改引用已重命名对象的任何对象。 例如，如果您重命名表列，并且触发器中引用了该列，则必须修改触发器以反映新的列名。 请使用 [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) 在重命名对象之前列出对象的依赖关系。  

**适用** 于SQL Server (支持的所有版本) 、Azure SQL Database 和 Azure Synapse Analytics  
 只能更改当前数据库中的对象名称或数据类型名称。 大多数系统数据类型和系统对象的名称都不能更改。  
  
## <a name="permissions"></a>权限  
 若要重命名对象、列和索引，则需要对该对象具有 ALTER 权限。 若要重命名用户类型，则需要对该类型具有 CONTROL 权限。 若要重命名数据库，则需要具备 sysadmin 或 dbcreator 固定服务器角色的成员身份  
  
## <a name="examples"></a>示例  
  
### <a name="a-renaming-a-table"></a>A. 重命名表  
 下面的示例将 `SalesTerritory` 架构中的 `SalesTerr` 表重命名为 `Sales` 。  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_rename 'Sales.SalesTerritory', 'SalesTerr';  
GO  
```  
  
### <a name="b-renaming-a-column"></a>B. 重命名列  
 下面的示例将 `TerritoryID` 表中的列重命名 `SalesTerritory` 为 `TerrID` 。  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_rename 'Sales.SalesTerritory.TerritoryID', 'TerrID', 'COLUMN';  
GO  
```  
  
### <a name="c-renaming-an-index"></a>C. 重命名索引  
 下面的示例将 `IX_ProductVendor_VendorID` 索引重命名为 `IX_VendorID`。  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_rename N'Purchasing.ProductVendor.IX_ProductVendor_VendorID', N'IX_VendorID', N'INDEX';  
GO  
```  
  
### <a name="d-renaming-an-alias-data-type"></a>D. 重命名别名数据类型  
 下面的示例将 `Phone` 别名数据类型重命名为 `Telephone`。  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_rename N'Phone', N'Telephone', N'USERDATATYPE';  
GO  
```  
  
### <a name="e-renaming-constraints"></a>E. 重命名约束  
 下面的示例重命名 PRIMARY KEY 约束、CHECK 约束和 FOREIGN KEY 约束。 重命名约束时，必须指定约束所属的架构。  
  
```sql  
USE AdventureWorks2012;   
GO  
-- Return the current Primary Key, Foreign Key and Check constraints for the Employee table.  
SELECT name, SCHEMA_NAME(schema_id) AS schema_name, type_desc  
FROM sys.objects  
WHERE parent_object_id = (OBJECT_ID('HumanResources.Employee'))   
AND type IN ('C','F', 'PK');   
GO  
  
-- Rename the primary key constraint.  
sp_rename 'HumanResources.PK_Employee_BusinessEntityID', 'PK_EmployeeID';  
GO  
  
-- Rename a check constraint.  
sp_rename 'HumanResources.CK_Employee_BirthDate', 'CK_BirthDate';  
GO  
  
-- Rename a foreign key constraint.  
sp_rename 'HumanResources.FK_Employee_Person_BusinessEntityID', 'FK_EmployeeID';  
  
-- Return the current Primary Key, Foreign Key and Check constraints for the Employee table.  
SELECT name, SCHEMA_NAME(schema_id) AS schema_name, type_desc  
FROM sys.objects  
WHERE parent_object_id = (OBJECT_ID('HumanResources.Employee'))   
AND type IN ('C','F', 'PK');  
  
```  
  
```  
  
name                                  schema_name        type_desc  
------------------------------------- ------------------ ----------------------  
FK_Employee_Person_BusinessEntityID   HumanResources     FOREIGN_KEY_CONSTRAINT  
PK_Employee_BusinessEntityID          HumanResources     PRIMARY_KEY_CONSTRAINT  
CK_Employee_BirthDate                 HumanResources     CHECK_CONSTRAINT  
CK_Employee_MaritalStatus             HumanResources     CHECK_CONSTRAINT  
CK_Employee_HireDate                  HumanResources     CHECK_CONSTRAINT  
CK_Employee_Gender                    HumanResources     CHECK_CONSTRAINT  
CK_Employee_VacationHours             HumanResources     CHECK_CONSTRAINT  
CK_Employee_SickLeaveHours            HumanResources     CHECK_CONSTRAINT  
  
(7 row(s) affected)  
  
name                                  schema_name        type_desc  
------------------------------------- ------------------ ----------------------  
FK_Employee_ID                        HumanResources     FOREIGN_KEY_CONSTRAINT  
PK_Employee_ID                        HumanResources     PRIMARY_KEY_CONSTRAINT  
CK_BirthDate                          HumanResources     CHECK_CONSTRAINT  
CK_Employee_MaritalStatus             HumanResources     CHECK_CONSTRAINT  
CK_Employee_HireDate                  HumanResources     CHECK_CONSTRAINT  
CK_Employee_Gender                    HumanResources     CHECK_CONSTRAINT  
CK_Employee_VacationHours             HumanResources     CHECK_CONSTRAINT  
CK_Employee_SickLeaveHours            HumanResources     CHECK_CONSTRAINT  
  
(7 row(s) affected)  
  
```  
  
### <a name="f-renaming-statistics"></a>F. 重命名统计信息  
 下面的示例创建一个名为 contactMail1 的统计信息对象，然后使用 sp_rename 将该统计信息重命名为 NewContact。 重命名统计信息时，必须采用格式 schema.table.statistics_name 指定该对象。  
  
```sql  
CREATE STATISTICS ContactMail1  
    ON Person.Person (BusinessEntityID, EmailPromotion)  
    WITH SAMPLE 5 PERCENT;  
  
sp_rename 'Person.Person.ContactMail1', 'NewContact','Statistics';  
  
```  

## <a name="examples-sssdwfull"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]
### <a name="g-renaming-a-column"></a>G. 重命名列  
 下面的示例将 `c1` 表中的列重命名 `table1` 为 `col1` 。 

> [!NOTE]
> 此 [!INCLUDE[ssazuresynapse](../../includes/ssazuresynapse_md.md)] 功能仍处于预览阶段，当前仅可用于 **dbo** 架构中的对象。 
  
```sql  
CREATE TABLE table1 (c1 INT, c2 INT);
EXEC sp_rename 'table1.c1', 'col1', 'COLUMN';
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.sql_expression_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)   
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  

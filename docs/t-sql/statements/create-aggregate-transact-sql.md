---
description: CREATE AGGREGATE (Transact-SQL)
title: CREATE AGGREGATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_AGGREGATE_TSQL
- CREATE AGGREGATE
- AGGREGATE_TSQL
- AGGREGATE
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE AGGREGATE statement
- aggregate functions [SQL Server], user-defined
- user-defined functions [CLR integration]
ms.assetid: 62eebc19-9f15-4245-94fa-b3fcd64a9d42
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 773c2010cdefa2455e0800ed0370cbc118c2dab9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098427"
---
# <a name="create-aggregate-transact-sql"></a>CREATE AGGREGATE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  创建用户定义聚合函数，其实现是在 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 的程序集类中定义的。 对于要将聚合函数绑定到其实现的[!INCLUDE[ssDE](../../includes/ssde-md.md)]，必须首先使用 CREATE ASSEMBLY 语句将包含该实现的 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 程序集上载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例中。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
CREATE AGGREGATE [ schema_name . ] aggregate_name  
        (@param_name <input_sqltype>   
        [ ,...n ] )  
RETURNS <return_sqltype>  
EXTERNAL NAME assembly_name [ .class_name ]  
  
<input_sqltype> ::=  
        system_scalar_type | { [ udt_schema_name. ] udt_type_name }  
  
<return_sqltype> ::=  
        system_scalar_type | { [ udt_schema_name. ] udt_type_name }  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *schema_name*  
 用户定义聚合函数所属的架构的名称。  
  
 aggregate_name  
 要创建的聚合函数的名称。  
  
 **@** _param_name_  
 用户定义聚合函数中的一个或多个参数。 在执行聚合函数时，用户必须提供参数的值。 通过将 at 符号 (@) 用作第一个字符来指定参数名称。 参数名称必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。 该函数的参数是局部参数。  
  
 system_scalar_type  
 要存放输入参数值或返回值的任一 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统标量数据类型。 除 text、ntext 和 image 之外的所有标量数据类型都可用作用户定义聚合函数的参数。 不能指定非标量类型（如 cursor 和 table）。  
  
 udt_schema_name  
 CLR 用户定义类型所属的架构的名称。 如果未指定，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将按以下顺序引用 udt_type_name：  
  
-   本机 SQL 类型命名空间。  
  
-   当前数据库中当前用户的默认架构。  
  
-   当前数据库中的 **dbo** 架构。  
  
 udt_type_name  
 当前数据库中已创建的 CLR 用户定义类型的名称。 如果未指定 udt_schema_name，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 假定该类型属于当前用户的架构。  
  
 assembly_name [ .class_name ]  
 指定与用户定义的聚合函数绑定在一起的程序集以及（可选）该程序集所属的架构名称和该程序集中实现该用户定义聚合函数的类名称。 必须先使用 CREATE ASSEMBLY 语句在数据库中创建了该程序集。 class_name 必须是有效的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识符并与该程序集中现有的类名称相匹配。 如果编写类所用的编程语言使用了命名空间（如 C#），则 class_name 可以是命名空间限定名称。 如果未指定 class_name，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 假定该名称与 aggregate_name 相同。  
  
## <a name="remarks"></a>注解  
 默认情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法运行 CLR 代码。 您可以创建、修改和删除引用托管代码模块的数据库对象，但是，除非通过使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 启用了 [clr enabled option](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md)，否则这些模块中的代码将不在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中运行。  
  
 在 assembly_name 及其方法中引用的程序集的类应满足在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中实现用户定义聚合函数的所有要求。 有关详细信息，请参阅 [CLR 用户定义聚合](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-aggregates.md)。  
  
## <a name="permissions"></a>权限  
 需要 CREATE AGGREGATE 权限以及对 EXTERNAL NAME 子句中指定的程序集的 REFERENCES 权限。  
  
## <a name="examples"></a>示例  
 下面的示例假定编译了 StringUtilities.csproj 示例应用程序。 有关详细信息，请参阅[字符串实用工具函数示例](/previous-versions/sql/sql-server-2016/ff878119(v=sql.130))。  
  
 该示例创建聚合函数 `Concatenate`。 在创建该聚合函数之前，在本地数据库中注册了程序集 `StringUtilities.dll`。  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SamplesPath nvarchar(1024)  
-- You may have to modify the value of the this variable if you have  
--installed the sample some location other than the default location.  
  
SELECT @SamplesPath = REPLACE(physical_name, 'Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\master.mdf', 'Microsoft SQL Server\130\Samples\Engine\Programmability\CLR\')   
     FROM master.sys.database_files   
     WHERE name = 'master';  
  
CREATE ASSEMBLY StringUtilities FROM @SamplesPath + 'StringUtilities\CS\StringUtilities\bin\debug\StringUtilities.dll'  
WITH PERMISSION_SET=SAFE;  
GO  
  
CREATE AGGREGATE Concatenate(@input nvarchar(4000))  
RETURNS nvarchar(4000)  
EXTERNAL NAME [StringUtilities].[Microsoft.Samples.SqlServer.Concatenate];  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [DROP AGGREGATE (Transact-SQL)](../../t-sql/statements/drop-aggregate-transact-sql.md)  
  

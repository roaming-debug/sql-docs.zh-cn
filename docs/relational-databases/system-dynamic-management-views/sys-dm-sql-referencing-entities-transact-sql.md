---
description: sys.dm_sql_referencing_entities (Transact-SQL)
title: sys.dm_sql_referencing_entities (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_sql_referencing_entities
- dm_sql_referencing_entities_TSQL
- sys.dm_sql_referencing_entities_TSQL
- dm_sql_referencing_entities
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_sql_referencing_entities dynamic management function
ms.assetid: c16f8f0a-483f-4feb-842e-da90426045ae
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 109791409e050201f64a7f7f9b207ccac58372b9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093975"
---
# <a name="sysdm_sql_referencing_entities-transact-sql"></a>sys.dm_sql_referencing_entities (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  针对当前数据库中按名称引用另一个用户定义实体的每个实体均返回一行。 两个实体之间的依赖关系是在另一个实体（称为 "*引用实体* *"）的* 持久化 SQL 表达式中按名称显示的。 例如，如果将某个用户定义类型 (UDT) 指定为被引用的实体，则此函数将返回在其定义中按名称引用该类型的每个用户定义实体。 该函数不会返回其他数据库中可能引用该指定实体的实体。 必须在 master 数据库的上下文中执行该函数，以便将服务器级 DDL 触发器作为引用实体返回。  
  
 可以使用此动态管理函数来报告当前数据库中引用指定实体的以下类型实体：  
  
-   绑定到架构或非绑定到架构的实体  
  
-   数据库级 DDL 触发器  
  
-   服务器级 DDL 触发器  
  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sys.dm_sql_referencing_entities (  
    ' schema_name.referenced_entity_name ' , ' <referenced_class> ' )  
  
<referenced_class> ::=  
{  
    OBJECT  
  | TYPE  
  | XML_SCHEMA_COLLECTION  
  | PARTITION_FUNCTION  
}  
```  
  
## <a name="arguments"></a>参数  
 `schema_name.referenced_entity_name` 被引用实体的名称。  
  
 `schema_name` 是必需的，但是当被引用的类是 PARTITION_FUNCTION 时除外。  
  
 `schema_name.referenced_entity_name` 为 **nvarchar (517)**。  
  
 `<referenced_class> ::= { OBJECT  | TYPE | XML_SCHEMA_COLLECTION | PARTITION_FUNCTION }` 被引用实体的类。 每个语句只能指定一个类。  
  
 `<referenced_class>` 为 **nvarchar** (60) 。  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|referencing_schema_name|**sysname**|引用实体所属的架构。 可以为 Null。<br /><br /> 对于数据库级和服务器级 DDL 触发器，为 NULL。|  
|referencing_entity_name|**sysname**|引用实体的名称。 不可为 null。|  
|referencing_id|**int**|引用实体的 ID。 不可为 null。|  
|referencing_class|**tinyint**|引用实体的类。 不可为 null。<br /><br /> 1 = 对象<br /><br /> 12 = 数据库级 DDL 触发器<br /><br /> 13 = 服务器级 DDL 触发器|  
|referencing_class_desc|**nvarchar(60)**|引用实体的类的说明。<br /><br /> OBJECT<br /><br /> DATABASE_DDL_TRIGGER<br /><br /> SERVER_DDL_TRIGGER|  
|is_caller_dependent|**bit**|指示被引用的实体的 ID 解析发生在运行时，因为它依赖于调用方的架构。<br /><br /> 1 = 引用实体可能会引用该实体，但是，被引用的实体的 ID 解析依赖于调用方，因此不能确定此解析。 仅对于非绑定到架构的引用且被引用对象为存储过程、扩展存储过程或在 EXECUTE 语句中调用的用户定义函数时，才会出现这种情况。<br /><br /> 0 = 被引用的实体不依赖于调用方。|  
  
## <a name="exceptions"></a>异常  
 在满足以下任一条件时将返回空的结果集：  
  
-   指定了系统对象。  
  
-   当前数据库中不存在指定的实体。  
  
-   指定的实体不引用任何实体。  
  
-   传递了无效参数。  
  
 当指定的被引用的实体是带编号的存储过程时，将返回错误。  
  
## <a name="remarks"></a>备注  
 下表列出了为其创建和维护依赖关系信息的实体类型。 不为规则、默认值、临时表、临时存储过程或系统对象创建或维护依赖关系信息。  
  
|实体类型|引用实体|被引用的实体|  
|-----------------|------------------------|-----------------------|  
|表|是*|是|  
|视图|是|是|  
|[!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程**|是|是|  
|CLR 存储过程|否|是|  
|[!INCLUDE[tsql](../../includes/tsql-md.md)] 用户定义函数|是|是|  
|CLR 用户定义函数|否|是|  
|CLR 触发器（DML 和 DDL）|否|否|  
|[!INCLUDE[tsql](../../includes/tsql-md.md)] DML 触发器|是|否|  
|[!INCLUDE[tsql](../../includes/tsql-md.md)] 数据库级 DDL 触发器|是|否|  
|[!INCLUDE[tsql](../../includes/tsql-md.md)] 服务器级 DDL 触发器|是|否|  
|扩展的存储过程|否|是|  
|队列|否|是|  
|同义词|否|是|  
|类型（别名和 CLR 用户定义类型）|否|是|  
|XML 架构集合|否|是|  
|分区函数|否|是|  
  
 \* 仅当表引用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 计算列、CHECK 约束或 DEFAULT 约束的定义中的模块、用户定义类型或 XML 架构集合时，才会将该表作为引用实体进行跟踪。  
  
 ** 整数值大于 1 的带编号的存储过程将不会作为引用实体或被引用的实体进行跟踪。  
  
## <a name="permissions"></a>权限  
  
### <a name="sskatmai---sssql11"></a>[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] - [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
-   需要对引用对象拥有 CONTROL 权限。 当被引用的实体是分区函数时，要求对数据库拥有 CONTROL 权限。  
  
-   需要 sys.dm_sql_referencing_entities 上的 SELECT 权限。 默认情况下，SELECT 权限授予 public。  
  
### <a name="sssql14---sscurrent"></a>[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] - [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]  
  
-   不需要对引用对象拥有任何权限。 如果用户只对某些引用实体拥有 VIEW DEFINITION 权限，可能返回部分结果。  
  
-   当引用实体为对象时，要求对对象拥有 VIEW DEFINITION 权限。  
  
-   当引用实体为数据库级 DDL 触发器时，要求对数据库拥有 VIEW DEFINITION 权限。  
  
-   当引用实体为服务器级 DDL 触发器时，要求对服务器拥有 VIEW ANY DEFINITION 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-the-entities-that-refer-to-a-given-entity"></a>A. 返回引用给定实体的实体  
 下面的示例返回当前数据库中引用指定表的实体。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT referencing_schema_name, referencing_entity_name, referencing_id, referencing_class_desc, is_caller_dependent  
FROM sys.dm_sql_referencing_entities ('Production.Product', 'OBJECT');  
GO  
```  
  
### <a name="b-returning-the-entities-that-refer-to-a-given-type"></a>B. 返回引用给定类型的实体  
 下面的示例返回引用别名类型 `dbo.Flag` 的实体。 结果集中显示了使用此类型的两个存储过程。 此 `dbo.Flag` 类型还用于表中的多个列的定义中 `HumanResources.Employee` ; 但是，由于该类型不在表中的计算列、CHECK 约束或 DEFAULT 约束的定义中，因此不会为该表返回任何行 `HumanResources.Employee` 。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT referencing_schema_name, referencing_entity_name, referencing_id, referencing_class_desc, is_caller_dependent  
FROM sys.dm_sql_referencing_entities ('dbo.Flag', 'TYPE');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 referencing_schema_name referencing_entity_name   referencing_id referencing_class_desc is_caller_dependent  
 ----------------------- -------------------------  ------------- ---------------------- -------------------  
 HumanResources          uspUpdateEmployeeHireInfo  1803153469    OBJECT_OR_COLUMN       0  
 HumanResources          uspUpdateEmployeeLogin     1819153526    OBJECT_OR_COLUMN       0  
 (2 row(s) affected)`  
 ``` 
 
## <a name="see-also"></a>另请参阅  
 [sys.dm_sql_referenced_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.sql_expression_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
  

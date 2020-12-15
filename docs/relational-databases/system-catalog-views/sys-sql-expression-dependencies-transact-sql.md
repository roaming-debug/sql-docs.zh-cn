---
description: sys.sql_expression_dependencies (Transact-SQL)
title: sys.sql_expression_dependencies (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sql_expression_dependencies
- sql_expression_dependencies_TSQL
- sql_expression_dependencies
- sys.sql_expression_dependencies_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_expression_dependencies catalog view
ms.assetid: 78a218e4-bf99-4a6a-acbf-ff82425a5946
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2a9810d1c1fbda616b6dec588375529f4cbbb15c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477408"
---
# <a name="syssql_expression_dependencies-transact-sql"></a>sys.sql_expression_dependencies (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  当前数据库中用户定义实体的每个按名称依赖项在此表中均占一行。 这包括本机编译标量用户定义函数和其他模块之间的依赖项 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 两个实体之间的依赖关系是在另一个实体（称为 "*引用实体* *"）的* 持久化 SQL 表达式中按名称显示的。 例如，在视图定义中引用表时，作为引用实体的视图将依赖于表这个被引用的实体。 如果删除该表，则该视图不可用。  
  
 有关详细信息，请参阅[内存中 OLTP 的标量用户定义函数](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md)。  
  
 您可以使用此目录视图来报告以下实体的依赖关系信息：  
  
-   绑定到架构的实体。  
  
-   非绑定到架构的实体。  
  
-   跨数据库和跨服务器的实体。 报告了实体名称；但实体 ID 尚未解析。  
  
-   绑定到架构的实体的列级依赖关系。 使用 [sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)可以返回非绑定到架构的对象的列级依赖关系。  
  
-   服务器级别的 DDL 触发器（在 master 数据库的上下文中时）。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|referencing_id|**int**|引用实体的 ID。 不可为 null。|  
|referencing_minor_id|**int**|引用实体为列时的列 ID；否则为 0。 不可为 null。|  
|referencing_class|**tinyint**|引用实体的类。<br /><br /> 1 = 对象或列<br /><br /> 12 = 数据库 DDL 触发器<br /><br /> 13 = 服务器 DDL 触发器<br /><br /> 不可为 null。|  
|referencing_class_desc|**nvarchar(60)**|对引用实体的类的说明。<br /><br /> OBJECT_OR_COLUMN<br /><br /> DATABASE_DDL_TRIGGER<br /><br /> SERVER_DDL_TRIGGER<br /><br /> 不可为 null。|  
|is_schema_bound_reference|**bit**|1 = 被引用的实体绑定到架构。<br /><br /> 0 = 被引用的实体未绑定到架构。<br /><br /> 不可为 null。|  
|referenced_class|**tinyint**|被引用的实体的类。<br /><br /> 1 = 对象或列<br /><br /> 6 = 类型<br /><br /> 10 = XML 架构集合<br /><br /> 21 = 分区函数<br /><br /> 不可为 null。|  
|referenced_class_desc|**nvarchar(60)**|对被引用的实体的类的说明。<br /><br /> OBJECT_OR_COLUMN<br /><br /> TYPE<br /><br /> XML_SCHEMA_COLLECTION<br /><br /> PARTITION_FUNCTION<br /><br /> 不可为 null。|  
|referenced_server_name|**sysname**|被引用的实体的服务器的名称。<br /><br /> 此列是为通过指定由四个部分组成的有效名称所生成的跨服务器依赖关系填充的。 有关多部分名称的信息，请参阅 [Transact-sql 语法约定 &#40;transact-sql&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。<br /><br /> 对于非绑定到架构的实体，如果实体被引用时没有指定由四个部分组成的名称，此列为 NULL。<br /><br /> 对于绑定到架构的实体，此值为 NULL，因为它们必须在同一数据库中，因此只能使用由两部分组成 (*schema*) 名称进行定义。|  
|referenced_database_name|**sysname**|被引用的实体的数据库的名称。<br /><br /> 此列是为通过指定由三个部分或四个部分组成的有效名称生成的跨数据库或跨服务器引用填充的。<br /><br /> 对于非绑定到架构的引用，当使用由一个部分或两个部分组成的名称指定时，此列为 NULL。<br /><br /> 对于绑定到架构的实体，此值为 NULL，因为它们必须在同一数据库中，因此只能使用由两部分组成 (*schema*) 名称进行定义。|  
|referenced_schema_name|**sysname**|被引用的实体所属的架构。<br /><br /> 对于非绑定到架构的引用，如果实体被引用时没有指定架构名称，此列为 NULL。<br /><br /> 对于绑定到架构的引用，此列永不为 NULL，原因在于必须使用由两部分组成的名称来定义和引用绑定到架构的实体。|  
|referenced_entity_name|**sysname**|被引用的实体的名称。 不可为 null。|  
|referenced_id|**int**|被引用的实体的 ID。 对于绑定到架构的引用，此列的值始终为 NULL。 对于跨服务器和跨数据库引用，此列的值始终为 NULL。<br /><br /> 对于数据库内的引用，如果无法确定 ID，则为 NULL。 对于非绑定到架构的引用，在以下情况下将无法解析 ID：<br /><br /> 被引用实体不存在于数据库中。<br /><br /> 被引用的实体的架构依赖于调用方的架构，并在运行时解析。 在这种情况下，is_caller_dependent 设置为 1。|  
|referenced_minor_id|**int**|引用实体为列时被引用的列的 ID；否则为 0。 不可为 null。<br /><br /> 当列在引用实体中按名称标识时，或者当 SELECT * 语句中使用了父实体时，被引用的实体为列。|  
|is_caller_dependent|**bit**|指示被引用的实体的架构绑定在运行时发生，因此，实体 ID 的解析依赖于调用方的架构。 当被引用的实体为存储过程、扩展存储过程或在 EXECUTE 语句中调用的非绑定到架构的用户定义函数时，将会出现这种情况。<br /><br /> 1 = 被引用的实体依赖于调用方并在运行时解析。 在这种情况下，referenced_id 为 NULL。<br /><br /> 0 = 被引用的实体 ID 不依赖调用方。<br /><br /> 对于绑定到架构的引用、显式指定架构名称的跨数据库和跨服务器的引用，始终为 0。 例如，对格式为 `EXEC MyDatabase.MySchema.MyProc` 的实体的引用不依赖于调用方。 但是，格式为 `EXEC MyDatabase..MyProc` 的引用依赖调用方。|  
|is_ambiguous|**bit**|指示引用不明确，可在运行时解析为用户定义函数、用户定义类型 (UDT) 或对 **xml** 类型的列的 xquery 引用。<br /><br /> 例如，假定语句 `SELECT Sales.GetOrder() FROM Sales.MySales` 是在存储过程中定义的。 在执行存储过程之前，并不知道 `Sales.GetOrder()` 是 `Sales` 架构中的用户定义函数还是带有名为 `Sales` 的方法、类型为 UDT 且名为 `GetOrder()` 的列。<br /><br /> 1 = 引用不明确。<br /><br /> 0 = 引用是明确的，或者在调用视图时可以成功绑定实体。<br /><br /> 对于架构绑定引用，始终为0。|  
  
## <a name="remarks"></a>备注  
 下表列出了为其创建和维护依赖关系信息的实体类型。 不为规则、默认值、临时表、临时存储过程或系统对象创建或维护依赖关系信息。  

> [!NOTE]
> Azure Synapse Analytics 和并行数据仓库支持表、视图、筛选统计信息和 Transact-sql 存储过程的实体类型。  只为表、视图和筛选统计信息创建并维护依赖关系信息。  
  
|实体类型|引用实体|被引用的实体|  
|-----------------|------------------------|-----------------------|  
|表|是*|是|  
|查看|是|是|  
|筛选索引|是**|否|  
|筛选统计信息|是**|否|  
|[!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程 * * _|是|是|  
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
  
 \_ 仅当表引用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 计算列、CHECK 约束或 DEFAULT 约束的定义中的模块、用户定义类型或 XML 架构集合时，才会将该表作为引用实体进行跟踪。  
  
 ** 筛选谓词中使用的每列都作为引用实体进行跟踪。  
  
 * * _ 整数值大于1的带编号的存储过程不会作为引用或被引用的实体进行跟踪。  
  
## <a name="permissions"></a>权限  
 要求对数据库具有 VIEW DEFINITION 权限，并对数据库的 sys.sql_expression_dependencies 具有 SELECT 权限。 默认情况下，SELECT 权限仅授予 db_owner 固定数据库角色的成员。 将 SELECT 和 VIEW DEFINITION 权限授予其他用户时，被授权者可以查看数据库中的所有依赖关系。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-entities-that-are-referenced-by-another-entity"></a>A. 返回另一实体引用的实体  
 下例返回在 `Production.vProductAndDescription` 视图中引用的表和列。 该视图依赖于 `referenced_entity_name` 和 `referenced_column_name` 列中返回的实体（表和列）。  
  
```  
USE AdventureWorks2012;  
GO  
SELECT OBJECT_NAME(referencing_id) AS referencing_entity_name,   
    o.type_desc AS referencing_desciption,   
    COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
    referencing_class_desc,  
    referenced_server_name, referenced_database_name, referenced_schema_name,  
    referenced_entity_name,   
    COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,  
    is_caller_dependent, is_ambiguous  
FROM sys.sql_expression_dependencies AS sed  
INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
WHERE referencing_id = OBJECT_ID(N'Production.vProductAndDescription');  
GO  
  
```  
  
### <a name="b-returning-entities-that-reference-another-entity"></a>B. 返回引用其他实体的实体  
 下面的示例返回引用表 `Production.Product` 的实体。 `referencing_entity_name` 列中返回的实体依赖于 `Product` 表。  
  
```  
USE AdventureWorks2012;  
GO  
SELECT OBJECT_SCHEMA_NAME ( referencing_id ) AS referencing_schema_name,  
    OBJECT_NAME(referencing_id) AS referencing_entity_name,   
    o.type_desc AS referencing_desciption,   
    COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
    referencing_class_desc, referenced_class_desc,  
    referenced_server_name, referenced_database_name, referenced_schema_name,  
    referenced_entity_name,   
    COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,  
    is_caller_dependent, is_ambiguous  
FROM sys.sql_expression_dependencies AS sed  
INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
WHERE referenced_id = OBJECT_ID(N'Production.Product');  
GO  
  
```  
  
### <a name="c-returning-cross-database-dependencies"></a>C. 返回跨数据库依赖关系  
 下面的示例返回所有跨数据库的依赖关系。 此示例首先创建数据库 `db1` 以及两个引用数据库 `db2` 和 `db3` 中的表的存储过程。 然后，对 `sys.sql_expression_dependencies` 表进行查询，以报告这两个过程和表之间的跨数据库依赖关系。 请注意，在被引用实体 `referenced_schema_name` 的 `t3` 列中返回 NULL，原因是过程定义中没有为该实体指定架构名称。  
  
```  
CREATE DATABASE db1;  
GO  
USE db1;  
GO  
CREATE PROCEDURE p1 AS SELECT _ FROM db2.s1.t1;  
GO  
CREATE PROCEDURE p2 AS  
    UPDATE db3..t3  
    SET c1 = c1 + 1;  
GO  
SELECT OBJECT_NAME (referencing_id),referenced_database_name,   
    referenced_schema_name, referenced_entity_name  
FROM sys.sql_expression_dependencies  
WHERE referenced_database_name IS NOT NULL;  
GO  
USE master;  
GO  
DROP DATABASE db1;  
GO  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_sql_referenced_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.dm_sql_referencing_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)  
  
  

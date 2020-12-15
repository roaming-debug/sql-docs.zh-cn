---
description: sys.dm_sql_referenced_entities (Transact-SQL)
title: sys.dm_sql_referenced_entities (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 05/01/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_sql_referenced_entities_TSQL
- dm_sql_referenced_entities
- sys.dm_sql_referenced_entities
- sys.dm_sql_referenced_entities_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_sql_referenced_entities dynamic management function
ms.assetid: 077111cb-b860-4d61-916f-bac5d532912f
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: cd6c12416a4e1626b7439ace6921b5d1981f0051
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484599"
---
# <a name="sysdm_sql_referenced_entities-transact-sql"></a>sys.dm_sql_referenced_entities (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

为中指定的引用实体的定义中的名称引用的每个用户定义实体返回一行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 当一个用户定义的实体（称为 " *被引用的实体*"）出现在另一个用户定义实体（称为 " *引用实体*"）的持久化 SQL 表达式中时，将创建两个实体之间的依赖关系。 例如，如果存储过程是指定的引用实体，则此函数会返回该存储过程中引用的所有用户定义实体，例如表、视图、用户定义类型 (UDT) 或其他存储过程。  
  
 可以使用此动态管理函数报告指定的引用实体引用的以下类型的实体：  
  
-   绑定到架构的实体  
  
-   非绑定到架构的实体  
  
-   跨数据库和跨服务器的实体  
  
-   绑定到架构的实体和非绑定到架构的实体的列级依赖关系  
  
-   用户定义类型（别名和 CLR UDT）  
  
-   XML 架构集合  
  
-   分区函数  

## <a name="syntax"></a>语法  
  
```  
sys.dm_sql_referenced_entities (  
    ' [ schema_name. ] referencing_entity_name ' ,
    ' <referencing_class> ' )  
  
<referencing_class> ::=  
{  
    OBJECT  
  | DATABASE_DDL_TRIGGER  
  | SERVER_DDL_TRIGGER  
}  
```  
  
## <a name="arguments"></a>自变量  
 [ *schema_name*。 ] *referencing_entity_name*  
 是引用实体的名称。 当引用类为 OBJECT 时，需要 *schema_name* 。  
  
 *schema_name. referencing_entity_name* **(517) 为 nvarchar**。  
  
 *<referencing_class>* ：： = {OBJECT |DATABASE_DDL_TRIGGER |SERVER_DDL_TRIGGER}  
 指定的引用实体的类。 每个语句只能指定一个类。  
  
 *<referencing_class>* 为 **nvarchar (60)**。  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|referencing_minor_id|**int**|引用实体为列时的列 ID；否则为 0。 不可为 null。|  
|referenced_server_name|**sysname**|被引用的实体的服务器的名称。<br /><br /> 此列是为通过指定由四个部分组成的有效名称所生成的跨服务器依赖关系填充的。 有关多部分名称的信息，请参阅 [Transact-sql 语法约定 &#40;transact-sql&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。<br /><br /> 对于非绑定到架构的依赖关系，如果引用实体时没有指定由四个部分组成的名称，此列为 NULL。<br /><br /> 对于绑定到架构的实体，此值为 NULL，因为它们必须在同一数据库中，因此只能使用由两部分组成 (*schema*) 名称进行定义。|  
|referenced_database_name|**sysname**|被引用的实体的数据库的名称。<br /><br /> 此列是为通过指定由三个部分或四个部分组成的有效名称生成的跨数据库或跨服务器引用填充的。<br /><br /> 对于非绑定到架构的引用，当使用由一个部分或两个部分组成的名称指定时，此列为 NULL。<br /><br /> 对于绑定到架构的实体，此值为 NULL，因为它们必须在同一数据库中，因此只能使用由两部分组成 (*schema*) 名称进行定义。|  
|referenced_schema_name|**sysname**|被引用的实体所属的架构。<br /><br /> 对于非绑定到架构的引用，如果实体被引用时没有指定架构名称，此列为 NULL。<br /><br /> 对于绑定到架构的引用，此列永远不会为 NULL。|  
|referenced_entity_name|**sysname**|被引用的实体的名称。 不可为 null。|  
|referenced_minor_name|**sysname**|引用的实体是列时是列名；否则是 NULL。 例如，referenced_minor_name 在列出所引用的实体自身的行中是 NULL。<br /><br /> 当列在引用实体中按名称标识时，或者当 SELECT * 语句中使用了父实体时，被引用的实体为列。|  
|referenced_id|**int**|被引用的实体的 ID。 当 referenced_minor_id 不为 0 时，referenced_id 是该列被定义时所在的实体。<br /><br /> 对于跨服务器的引用，此列始终为 NULL。<br /><br /> 对于跨数据库的引用，当由于数据库脱机或实体无法绑定而无法确定 ID 时，此列为 NULL。<br /><br /> 对于数据库内的引用，如果无法确定 ID，则为 NULL。 对于非绑定到架构的引用，如果数据库中不存在被引用的实体或名称解析是依赖于调用方的，则不能解析该 ID。  在后一种情况下，is_caller_dependent 设置为1。<br /><br /> 对于绑定到架构的引用，此列永远不会为 NULL。|  
|referenced_minor_id|**int**|引用的实体是列时是列 ID；否则是 0。 例如，referenced_minor_is 在列出所引用的实体本身的行中是 0。<br /><br /> 对于非绑定到架构的引用，只有当可以绑定所有被引用的实体时才报告列依赖关系。 如果无法绑定任何被引用的实体，则不报告列级依赖关系，并且 referenced_minor_id 是 0。 请参见示例 D。|  
|referenced_class|**tinyint**|被引用的实体的类。<br /><br /> 1 = 对象或列<br /><br /> 6 = 类型<br /><br /> 10 = XML 架构集合<br /><br /> 21 = 分区函数|  
|referenced_class_desc|**nvarchar(60)**|对被引用的实体的类的说明。<br /><br /> OBJECT_OR_COLUMN<br /><br /> TYPE<br /><br /> XML_SCHEMA_COLLECTION<br /><br /> PARTITION_FUNCTION|  
|is_caller_dependent|**bit**|指示被引用的实体的架构绑定发生于运行时，因此实体 ID 的解析依赖于调用方的架构。 当被引用的实体为存储过程、扩展存储过程或在 EXECUTE 语句中调用的用户定义函数时，将会出现这种情况。<br /><br /> 1 = 被引用的实体依赖调用方并在运行时解析。 在这种情况下，referenced_id 为 NULL。<br /><br /> 0 = 被引用的实体 ID 不依赖调用方。 对于绑定到架构的引用、显式指定架构名称的跨数据库和跨服务器的引用，始终为 0。 例如，对格式为 `EXEC MyDatabase.MySchema.MyProc` 的实体的引用不依赖于调用方。 但是，格式为 `EXEC MyDatabase..MyProc` 的引用依赖调用方。|  
|is_ambiguous|**bit**|指示引用不明确，可在运行时解析为用户定义函数、用户定义类型 (UDT) 或对 **xml** 类型的列的 xquery 引用。 例如，假定语句 `SELECT Sales.GetOrder() FROM Sales.MySales` 是在存储过程中定义的。 在执行存储过程之前，并不知道 `Sales.GetOrder()` 是 `Sales` 架构中的用户定义函数还是带有名为 `Sales` 的方法、类型为 UDT 且名为 `GetOrder()` 的列。<br /><br /> 1 = 引用的是用户定义函数还是使用用户定义类型 (UDT) 方法的列，这一点是不明确的。<br /><br /> 0 = 引用是明确的，或者在调用函数时可以成功绑定实体。<br /><br /> 对于绑定到架构的引用始终为 0。|  
|is_selected|**bit**|1 = 选中了对象或列。|  
|is_updated|**bit**|1 = 修改了对象或列。|  
|is_select_all|**bit**|1 = 对象用于 SELECT * 子句中（仅限对象级）。|  
|is_all_columns_found|**bit**|1 = 可以找到对象的所有列依赖关系。<br /><br /> 0 = 找不到对象的列依赖关系。|
|is_insert_all|**bit**|1 = 对象用于不包含列列表的 INSERT 语句 (仅限对象级) 。<br /><br />此列是在 SQL Server 2016 中添加的。|  
|is_incomplete|**bit**|1 = 对象或列有绑定错误且不完整。<br /><br />此列已添加到 SQL Server 2016 SP2 中。|
| &nbsp; | &nbsp; | &nbsp; |

## <a name="exceptions"></a>异常  
 在满足以下任一条件时将返回空的结果集：  
  
-   指定了系统对象。  
  
-   当前数据库中不存在指定的实体。  
  
-   指定的实体不引用任何实体。  
  
-   传递了无效参数。  
  
 当指定的引用实体是带编号的存储过程时，返回错误。  
  
 无法解析列依赖关系时，返回错误 2020。 此错误不会阻止查询返回对象级依赖关系。  
  
## <a name="remarks"></a>备注  
 可以在任何数据库的上下文中执行此函数以返回引用服务器级 DDL 触发器的实体。  
  
 下表列出了为其创建和维护依赖关系信息的实体类型。 不为规则、默认值、临时表、临时存储过程或系统对象创建或维护依赖关系信息。  
  
|实体类型|引用实体|被引用的实体|  
|-----------------|------------------------|-----------------------|  
|表|是*|是|  
|查看|是|是|  
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
| &nbsp; | &nbsp; | &nbsp; |

 \* 仅当表引用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 计算列、CHECK 约束或 DEFAULT 约束的定义中的模块、用户定义类型或 XML 架构集合时，才会将该表作为引用实体进行跟踪。  
  
 ** 整数值大于 1 的带编号的存储过程将不会作为引用实体或被引用的实体进行跟踪。  
  
## <a name="permissions"></a>权限  
 要求对 sys.dm_sql_referenced_entities 拥有 SELECT 权限并对引用实体拥有 VIEW DEFINITION 权限。 默认情况下，SELECT 权限授予 public。 要求对数据库拥有 VIEW DEFINITION 权限或 ALTER DATABASE DDL TRIGGER 权限（当引用实体为数据库级 DDL 触发器时）。 当引用实体为服务器级 DDL 触发器时，要求对服务器拥有 VIEW ANY DEFINITION 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-return-entities-that-are-referenced-by-a-database-level-ddl-trigger"></a>A. 返回数据库级 DDL 触发器引用的实体  
 下面的示例返回数据库级 DDL 触发器 `ddlDatabaseTriggerLog` 引用的实体（表和列）。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT
        referenced_schema_name,
        referenced_entity_name,
        referenced_minor_name,
        referenced_minor_id,
        referenced_class_desc
    FROM
        sys.dm_sql_referenced_entities (
            'ddlDatabaseTriggerLog',
            'DATABASE_DDL_TRIGGER')
;
GO  
```  
  
### <a name="b-return-entities-that-are-referenced-by-an-object"></a>B. 返回对象引用的实体  
 下面的示例返回用户定义函数 `dbo.ufnGetContactInformation` 引用的实体。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT
        referenced_schema_name,
        referenced_entity_name,
        referenced_minor_name,
        referenced_minor_id,
        referenced_class_desc,
        is_caller_dependent,
        is_ambiguous
    FROM
        sys.dm_sql_referenced_entities (
            'dbo.ufnGetContactInformation',
            'OBJECT')
;
GO  
```  
  
### <a name="c-return-column-dependencies"></a>C. 返回列依赖关系  
 下面的示例创建具有定义为列 `Table1` 与 `c` 之和的计算列 `a` 的表 `b`。 然后 `sys.dm_sql_referenced_entities` 视图被调用。 该视图返回两行，每行针对计算列中定义的一列。  
  
```sql  
CREATE TABLE dbo.Table1 (a int, b int, c AS a + b);  
GO  
SELECT
        referenced_schema_name AS schema_name,  
        referenced_entity_name AS table_name,  
        referenced_minor_name  AS referenced_column,  
        COALESCE(
            COL_NAME(OBJECT_ID(N'dbo.Table1'),
            referencing_minor_id),
            'N/A') AS referencing_column_name  
    FROM
        sys.dm_sql_referenced_entities ('dbo.Table1', 'OBJECT')
;
GO

-- Remove the table.  
DROP TABLE dbo.Table1;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```console
 schema_name table_name referenced_column referencing_column  
 ----------- ---------- ----------------- ------------------  
 dbo         Table1     a                 c  
 dbo         Table1     b                 c  
```

### <a name="d-returning-non-schema-bound-column-dependencies"></a>D. 返回非绑定到架构的列依赖关系  
 下面的示例删除 `Table1` 并创建 `Table2` 和存储过程 `Proc1`。 该过程引用 `Table2` 以及不存在的表 `Table1`。 视图 `sys.dm_sql_referenced_entities` 和指定为引用实体的存储过程一起运行。 结果集对于 `Table1` 显示一行，对于 `Table2` 显示 3 行。 因为 `Table1` 不存在，所以列依赖关系无法解析，并返回错误 2020。 `is_all_columns_found` 列为 `Table1` 返回 0，指示存在无法发现的列。  
  
```sql  
DROP TABLE IF EXISTS dbo.Table1;
GO  
CREATE TABLE dbo.Table2 (c1 int, c2 int);  
GO  
CREATE PROCEDURE dbo.Proc1 AS  
    SELECT a, b, c FROM Table1;  
    SELECT c1, c2 FROM Table2;  
GO  
SELECT
        referenced_id,
        referenced_entity_name AS table_name,
        referenced_minor_name  AS referenced_column_name,
        is_all_columns_found
    FROM
        sys.dm_sql_referenced_entities ('dbo.Proc1', 'OBJECT');
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```console
 referenced_id table_name   referenced_column_name  is_all_columns_found  
 ------------- ------------ ----------------------- --------------------  
 935674381     Table2       NULL                    1  
 935674381     Table2       C1                      1  
 935674381     Table2       C2                      1  
 NULL          Table1       NULL                    0  

Msg 2020, Level 16, State 1, Line 1
The dependencies reported for entity "dbo.Proc1" might not include
  references to all columns. This is either because the entity
  references an object that does not exist or because of an error
  in one or more statements in the entity.  Before rerunning the
  query, ensure that there are no errors in the entity and that
  all objects referenced by the entity exist.
 ```
  
### <a name="e-demonstrating-dynamic-dependency-maintenance"></a>E. 演示动态依赖关系维护  

本例 E 假设已运行示例 D。 示例 E 显示了动态维护依赖关系。 该示例执行以下操作：

1. 重新创建 `Table1` ，它在示例 D 中被删除。
2. 运行 `sys.dm_sql_referenced_entities` 时，将用指定为引用实体的存储过程再次运行。

结果集显示这两个表及其各自的列都是在存储过程中定义的。 此外，`is_all_columns_found` 列为所有对象和列返回 1。

```sql  
CREATE TABLE Table1 (a int, b int, c AS a + b);  
GO   
SELECT
        referenced_id,
        referenced_entity_name AS table_name,
        referenced_minor_name  AS column_name,
        is_all_columns_found
    FROM
        sys.dm_sql_referenced_entities ('dbo.Proc1', 'OBJECT');
GO  
DROP TABLE Table1, Table2;  
DROP PROC Proc1;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```console
 referenced_id table_name   referenced_column_name  is_all_columns_found  
 ------------- ------------ ----------------------- --------------------  
 935674381     Table2       NULL                    1 
 935674381     Table2       c1                      1 
 935674381     Table2       c2                      1 
 967674495     Table1       NULL                    1 
 967674495     Table1       a                       1  
 967674495     Table1       b                       1  
 967674495     Table1       c                       1  
 ```
 
### <a name="f-returning-object-or-column-usage"></a>F. 返回对象或列的用法  
 下面的示例返回存储过程 `HumanResources.uspUpdateEmployeePersonalInfo` 的对象和列依赖关系。 此过程根据指定的 `NationalIDNumber` 值更新表的列、 `BirthDate,``MaritalStatus` 和 `Gender` `Employee` `BusinessEntityID` 。 另一个存储过程 `upsLogError` 是在 TRY .。。捕获执行错误的 CATCH 块。 `is_selected`、`is_updated` 和 `is_select_all` 列返回与如何在引用对象中使用这些对象和列有关的信息。 进行了修改的表和列由 is_updated 列中的 1 指示。 仅选择 `BusinessEntityID` 列，既不选择、也不修改存储过程 `uspLogError`。  

```sql  
USE AdventureWorks2012;
GO
SELECT
        referenced_entity_name AS table_name,
        referenced_minor_name  AS column_name,
        is_selected,  is_updated,  is_select_all
    FROM
        sys.dm_sql_referenced_entities(
            'HumanResources.uspUpdateEmployeePersonalInfo',
            'OBJECT')
;
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```console
 table_name    column_name         is_selected is_updated is_select_all  
 ------------- ------------------- ----------- ---------- -------------  
 uspLogError   NULL                0           0          0  
 Employee      NULL                0           1          0  
 Employee      BusinessEntityID    1           0          0  
 Employee      NationalIDNumber    0           1          0  
 Employee      BirthDate           0           1          0  
 Employee      MaritalStatus       0           1          0  
 Employee      Gender              0           1          0
 ```
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_sql_referencing_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)   
 [sys.sql_expression_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
  

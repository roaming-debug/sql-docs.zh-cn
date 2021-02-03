---
description: CREATE TYPE (Transact-SQL)
title: CREATE TYPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/05/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- sql13.swb.sysdatatype.properties.f1
- CREATE TYPE
- TYPE_TSQL
- TYPE
- CREATE_TYPE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- UDTs [SQL Server], creating
- CLR user-defined types [SQL Server]
- user-defined table types [SQL Server]
- user-defined types [SQL Server], creating
- CREATE TYPE statement
- alias data types [SQL Server], creating
- data types [SQL Server], creating
ms.assetid: 2202236b-e09f-40a1-bbc7-b8cff7488905
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2063543c497070bd8366cae9a8691b3701fc4330
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177906"
---
# <a name="create-type-transact-sql"></a>CREATE TYPE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中的当前数据库中创建别名数据类型或用户定义类型。 别名数据类型的实现基于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本机系统类型。 用户定义类型通过 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 公共语言运行时 (CLR) 中的程序集的类来实现。 若要将用户定义类型绑定到其实现，必须首先使用 [CREATE ASSEMBLY](../../t-sql/statements/create-assembly-transact-sql.md) 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中注册包含该类型实现的 CLR 程序集。  
  
 默认情况下，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中运行 CLR 代码的功能处于关闭状态。 可以创建、修改和删除引用托管代码模块的数据库对象，但除非使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 启用 [clr enabled Option](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md)，否则不会在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中执行这些引用。  
 
> [!NOTE]  
>  本主题讨论 .NET Framework CLR 与 SQL Server 的集成。 CLR 集成不适用于 Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)]。
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- User-defined Data Type Syntax    
CREATE TYPE [ schema_name. ] type_name  
{   
    [
      FROM base_type   
      [ ( precision [ , scale ] ) ]  
      [ NULL | NOT NULL ]
    ]
    | EXTERNAL NAME assembly_name [ .class_name ]   
    | AS TABLE ( { <column_definition> | <computed_column_definition> [ ,... n ] }
      [ <table_constraint> ] [ ,... n ]    
      [ <table_index> ] [ ,... n ] } )
 
} [ ; ]  
  
<column_definition> ::=  
column_name <data_type>  
    [ COLLATE collation_name ]   
    [ NULL | NOT NULL ]  
    [   
        DEFAULT constant_expression ]   
      | [ IDENTITY [ ( seed ,increment ) ]   
    ]  
    [ ROWGUIDCOL ] [ <column_constraint> [ ...n ] ]   
  
<data type> ::=   
[ type_schema_name . ] type_name   
    [ ( precision [ , scale ] | max |   
                [ { CONTENT | DOCUMENT } ] xml_schema_collection ) ]   
  
<column_constraint> ::=   
{     { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
        [   
            WITH ( <index_option> [ ,...n ] )   
        ]  
  | CHECK ( logical_expression )   
}   
  
<computed_column_definition> ::=  
  
column_name AS computed_column_expression   
[ PERSISTED [ NOT NULL ] ]  
[   
    { PRIMARY KEY | UNIQUE }  
        [ CLUSTERED | NONCLUSTERED ]  
        [   
            WITH ( <index_option> [ ,...n ] )  
        ]  
    | CHECK ( logical_expression )   
]   
  
<table_constraint> ::=  
{   
    { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
    ( column [ ASC | DESC ] [ ,...n ] )   
        [   
    WITH ( <index_option> [ ,...n ] )   
        ]  
    | CHECK ( logical_expression )   
}   
  
<index_option> ::=  
{  
    IGNORE_DUP_KEY = { ON | OFF }  
}  

< table_index > ::=  
  INDEX constraint_name  
     [ CLUSTERED | NONCLUSTERED ]   (column [ ASC | DESC ] [ ,... n ] )} }  
```  
  
```syntaxsql
-- User-defined Memory Optimized Table Types Syntax  
CREATE TYPE [schema_name. ] type_name  
AS TABLE ( { <column_definition> [ ,... n ] }  
    | [ <table_constraint> ] [ ,... n ]    
    | [ <table_index> ] [ ,... n ] } )
    [ WITH ( <table_option> [ ,... n ] ) ]  
 [ ; ]  
  
<column_definition> ::=  
column_name <data_type>  
    [ COLLATE collation_name ]   [ NULL | NOT NULL ]    [  
      [ IDENTITY [ (1 , 1) ]  
    ]  
    [ <column_constraint> [ ... n ] ]    [ <column_index> ]  
  
<data type> ::=  
 [type_schema_name . ] type_name [ (precision [ , scale ]) ]  
  
<column_constraint> ::=  
{ PRIMARY KEY {   NONCLUSTERED HASH WITH (BUCKET_COUNT = bucket_count) 
                | NONCLUSTERED } }  
  
< table_constraint > ::=  
{ PRIMARY KEY { NONCLUSTERED HASH (column [ ,... n ] ) 
                   WITH (BUCKET_COUNT = bucket_count) 
               | NONCLUSTERED  (column [ ASC | DESC ] [ ,... n ] )  } }  
  
<column_index> ::=  
  INDEX index_name  
{ { [ NONCLUSTERED ] HASH WITH (BUCKET_COUNT = bucket_count) 
     | NONCLUSTERED } }  
  
< table_index > ::=  
  INDEX constraint_name  
{ { [ NONCLUSTERED ] HASH (column [ ,... n ] ) WITH (BUCKET_COUNT = bucket_count) 
 |  [NONCLUSTERED]  (column [ ASC | DESC ] [ ,... n ] )} }  
  
<table_option> ::=  
{  
    [MEMORY_OPTIMIZED = {ON | OFF}]  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *schema_name*  
 别名数据类型或用户定义类型所属架构的名称。  
  
 type_name  
 别名数据类型或用户定义类型的名称。 类型名称必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。  
  
 base_type  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供的数据类型，别名数据类型以此类型为基础。 base_type 为 sysname，无默认值，并且可以是下列值之一：  
  
|||||  
|-|-|-|-|  
|**bigint**|**binary(** *n* **)**|**bit**|**char(** *n* **)**|  
|**date**|**datetime**|**datetime2**|**datetimeoffset**|  
|**decimal**|**float**|**图像**|**int**|  
|**money**|**nchar(** *n* **)**|**ntext**|**numeric**|  
|**nvarchar(** *n* &#124; **max)**|**real**|**smalldatetime**|**smallint**|  
|**smallmoney**|**sql_variant**|**text**|**time**|  
|**tinyint**|**uniqueidentifier**|**varbinary(** *n* &#124; **max)**|**varchar(** *n* &#124; **max)**|  
  
 base_type 还可以是映射到这些系统数据类型之一的任何数据类型同义词。  
  
 *精度*  
 对于 decimal 或 numeric，其值为非负整数，用于指示可保留的十进制数字位数的最大值，包括小数点左边和右边的数字 。 有关详细信息，请参阅 [decimal 和 numeric (Transact-SQL)](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)。  
  
 *scale*  
 对于 decimal 或 numeric，其值为非负整数，用于指示十进制数字的小数点右边最多可保留多少位，它必须小于或等于精度值 。 有关详细信息，请参阅 [decimal 和 numeric (Transact-SQL)](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)。  
  
 NULL | NOT NULL  
 指定此类型是否可容纳空值。 如果未指定，则默认值为 NULL。  
  
 assembly_name  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指定可在公共语言运行时中引用用户定义类型的实现的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 程序集。 assembly_name 应与当前数据库的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的现有程序集匹配。  
  
> [!NOTE]  
>  EXTERNAL_NAME 在包含数据库中不可用。  
  
 **[.** *class_name*  **]**  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指定实现用户定义类型的程序集内的类。 class_name 必须是有效的标识符，并且它必须作为类存在于可见程序集中。 class_name 区分大小写，不考虑数据库的排序规则，且必须与对应的程序集中的类名完全匹配。 如果用于编写类的编程语言使用命名空间概念（例如 C#），则类名可以是用方括号 ([ ]) 括起来的限定命名空间的名称。 如果未指定 class_name，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 假定该名称与 type_name 相同 。  
  
 \<column_definition>  
 定义用户定义表类型的列。  
  
 \<data type>  
 定义用户定义表类型的列中的数据类型。 有关数据类型的详细信息，请参阅[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)。 有关表的详细信息，请参阅 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)。  
  
 \<column_constraint>  
 定义用户定义表类型的列约束。 支持的约束包括 PRIMARY KEY、UNIQUE 和 CHECK。 有关表的详细信息，请参阅 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)。  
  
 \<computed_column_definition>  
 将计算列表达式定义为用户定义表类型中的列。 有关表的详细信息，请参阅 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)。  
  
 \<table_constraint>  
 定义用户定义表类型的表约束。 支持的约束包括 PRIMARY KEY、UNIQUE 和 CHECK。  
  
 \<index_option>  
 指定对唯一聚集索引或唯一非聚集索引执行多行插入操作时出现重复键值的错误响应。 有关索引选项的详细信息，请参阅 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)。  
 
  `INDEX *index_name* [ CLUSTERED | NONCLUSTERED ] (*column_name* [ ASC | DESC ] [ ,... *n* ] )`  
     
**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。

指定在表上创建索引。 这可以是聚集索引，也可以是非聚集索引。 该索引包含列出的列，并按照升序或降序对数据进行排序。
  
 INDEX  
 必须在 CREATE TABLE 语句中指定列索引和表索引。 内存优化表不支持 CREATE INDEX 和 DROP INDEX。  
  
 MEMORY_OPTIMIZED  
 **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。  
  
 指示表类型是否为内存优化表。 默认情况下，此选项处于关闭状态；表（类型）不是内存优化表（类型）。 内存优化表类型是内存优化用户表，它保留在磁盘上的架构与其他用户表类似。  
  
 BUCKET_COUNT  
 **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。  
  
 指示应在哈希索引中创建的存储桶数。 哈希索引中 BUCKET_COUNT 的最大值为 1,073,741,824。 有关桶计数的详细信息，请参阅[内存优化表索引](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md)。 bucket_count 是必需的参数。  
  
 HASH  
 **适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。  
  
 指示创建哈希索引。 仅在内存优化表中支持哈希索引。  
  
## <a name="remarks"></a>备注  
 在 assembly_name 中引用的程序集的类及其方法应满足在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中实现用户定义类型的所有要求。 有关这些要求的详细信息，请参阅 [CLR 用户定义类型](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)。  
  
 其他注意事项包括以下几点：  
  
-   类可以具有重载方法，但只能从托管代码内而不能从 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调用这些方法。  
  
-   如果 assembly_name 是 SAFE 或 EXTERNAL_ACCESS，则必须将所有静态成员声明为 const 或 readonly 。  
  
 在数据库内，任何从 CLR 上载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的指定类型都只能注册一个用户定义类型。 如果数据库中已存在 CLR 类型的用户定义类型，则在对 CLR 类型创建用户定义类型时，CREATE TYPE 会因错误而失败。 如果一个 CLR 类型可被映射到多个用户定义类型，则要求使用此限制来避免 SQL 类型解析过程中的混乱情况。  
  
 如果类型中的任何赋值函数方法未返回 void，则 CREATE TYPE 语句将不会执行。  
  
 若要修改用户定义类型，必须使用 DROP TYPE 语句除去该类型，然后重新创建它。  
  
 与使用 sp_addtype 创建的用户定义类型不同，对于使用 CREATE TYPE 创建的类型，不会向 public 数据库角色自动授予该类型的 REFERENCES 权限 。 此权限必须单独授予。  
  
 在用户定义表类型中，column_name \<data type> 中使用的结构化用户定义类型是定义表类型的数据库架构作用域的一部分。 若要访问数据库不同作用域中的结构化用户定义类型，请使用由两部分组成的名称。  
  
 在用户定义表类型中，计算列的主键必须是 PERSISTED 和 NOT NULL。  
  
## <a name="memory-optimized-table-types"></a>内存优化表类型  
 从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始，可在主内存中而不是磁盘上执行表类型中的数据的处理。 有关详细信息，请参阅[内存中 OLTP&#40;内存中优化&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)。 对于演示如何创建内存优化表类型的代码示例，请参阅[创建内存优化表和本机编译的存储过程](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md)。  
  
## <a name="permissions"></a>权限  
 要求在当前数据库中具有 CREATE TYPE 权限，以及具有对 *schema_name* 的 ALTER 权限。 如果未指定 *schema_name* ，则将应用用于确定当前用户的架构的默认名称解析规则。 如果指定了 assembly_name，则用户必须拥有该程序集或对其具有 REFERENCES 权限。  

 如果 CREATE TABLE 语句中的任何列被定义为用户定义类型，则需要具有对此用户定义类型的 REFERENCES 权限。
 
   >[!NOTE]
  > 对于创建表（包含使用用户定义类型的列）的用户，需要用户定义类型的 REFERENCES 权限。
  > 如果必须在 TempDB 中创建此表，那么在创建表之前每次都需要显式授予 REFERENCES 权限，或需要将此数据类型和 REFERENCES 权限添加到 Model 数据库中。 如果这样做，该数据类型和权限将永久在 TempDB 中可用。 否则，当 SQL Server 重新启动时，用户定义的数据类型和权限将消失。 有关详细信息，请参阅 [CREATE TABLE](./create-table-transact-sql.md#permissions-1)
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-an-alias-type-based-on-the-varchar-data-type"></a>A. 基于 varchar 数据类型创建别名类型  
 以下示例基于系统提供的 `varchar` 数据类型创建别名类型。  
  
```sql  
CREATE TYPE SSN  
FROM varchar(11) NOT NULL ;  
```  
  
### <a name="b-creating-a-user-defined-type"></a>B. 创建用户定义类型  
 以下示例创建类型 `Utf8String`，该类型在程序集 `utf8string` 中引用类 `utf8string`。 创建类型之前，应在本地数据库中注册程序集 `utf8string`。 将 CREATE ASSEMBLY 语句的二进制部分替换为有效描述。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
```sql  
CREATE ASSEMBLY utf8string  
AUTHORIZATION [dbi]   
FROM 0x4D... ;  
GO  
CREATE TYPE Utf8String   
EXTERNAL NAME utf8string.[Microsoft.Samples.SqlServer.utf8string] ;  
GO  
```  
  
### <a name="c-creating-a-user-defined-table-type"></a>C. 创建用户定义表类型  
 下面的示例创建一个具有两列的用户定义表类型。 有关如何创建和使用表值参数的详细信息，请参阅[使用表值参数（数据库引擎）](../../relational-databases/tables/use-table-valued-parameters-database-engine.md)。  
  
```sql  
CREATE TYPE LocationTableType AS TABLE   
    ( LocationName VARCHAR(50)  
    , CostRate INT );  
GO  
```  

### <a name="d-creating-a-user-defined-table-type-with-primary-key-and-index"></a>D. 创建包含主键和索引的用户定义表类型
下面的示例创建包含三列的用户定义表类型，其中一列 (`Name`) 包含主键，另一列 (`Price`) 包含非聚集索引。  有关如何创建和使用表值参数的详细信息，请参阅[使用表值参数（数据库引擎）](../../relational-databases/tables/use-table-valued-parameters-database-engine.md)。

```sql
CREATE TYPE InventoryItem AS TABLE
(
    [Name] NVARCHAR(50) NOT NULL,
    SupplierId BIGINT NOT NULL,
    Price DECIMAL (18, 4) NULL,
    PRIMARY KEY (
        Name
    ),
    INDEX IX_InventoryItem_Price (
        Price
    )
)
GO
```
  
## <a name="see-also"></a>另请参阅  
 [CREATE ASSEMBLY (Transact-SQL)](../../t-sql/statements/create-assembly-transact-sql.md)   
 [DROP TYPE (Transact-SQL)](../../t-sql/statements/drop-type-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)    
 [CLR 用户定义类型](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)     
 [使用 SQL Server 中的用户定义类型](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-in-sql-server.md)     

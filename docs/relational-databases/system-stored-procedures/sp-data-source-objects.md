---
description: 'sp_data_source_objects (Transact-sql) '
title: sp_data_source_objects |Microsoft Docs
ms.custom: ''
ms.date: 11/14/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sp_data_source_objects
helpviewer_keywords:
- PolyBase
ms.assetid: 48066431-fed2-4a8a-85af-ac704689e183
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7f3aa43750cebfbbac77e3807845f1ed154af1ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067212"
---
# <a name="sp_data_source_objects-transact-sql"></a>sp_data_source_objects (Transact-sql) 

[!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

返回可虚拟化的表对象的列表。

> [!NOTE]
> 此过程在 [SQL 2019 CU5](../../big-data-cluster/release-notes-big-data-cluster.md#cu5)中引入。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>语法  

```syntaxsql
sp_data_source_objects  
         [ @data_source = ] 'data_source'
     [ , [ @object_root_name = ] 'object_root_name' ]
     [ , [ @max_search_depth = ] max_search_depth ]
     [ , [ @search_options = ] 'search_options' ]
[ ; ]
```  
  
## <a name="arguments"></a>参数  

`[ @data_source = ] 'data_source'`   
要从中获取元数据的外部数据源的名称。 `@data_source` 上声明的默认值为 `sysname`。  

`[ @object_root_name = ] 'object_root_name'`   
此参数是要搜索)  (的对象的名称的根。 `@object_root_name` 的 `nvarchar(max)` 值为，默认值为 `NULL` 。

此调用仅返回以为设置的值开头的外部对象 `@object_root_name` 。

如果 ODBC 数据源连接到关系数据库管理系统 (RDBMS) 使用由三部分组成的名称，则 `@object_root_name` 不能包含部分数据库名称。 在这些情况下，参数 `@object_root_name` 应包含所有三个部分，第三部分是要搜索的对象名称。
> [!CAUTION]
> 由于外部数据平台之间的差异，如果提供了的默认值，一些平台将不返回任何结果 `NULL` 。 某些视为 `NULL` 缺乏筛选器。 例如，如果为提供，Oracle RDMBS 将不返回结果 `NULL` `@object_root_name` 。

`[ @max_search_depth = ] max_search_depth`   
此值指定在) 超过要搜索的部分中 (的最大深度 `@object_root_name` 。 `@max_search_depth` 的 `int` 值为，默认值为1。

例如，为 `@max_search_depth` 1， `@object_root_name` 表示 SQL Server 数据库的名称，则返回数据库中包含的架构。

`@max_search_depth`如果为 `NULL` ，则将返回有关 `@object_root_name` 在目录或架构中是否存在且为非空的信息。

`[ @search_options = ] 'search_options'`   
`search_options`参数为 nvarchar (max) ，默认值为 `NULL` 。

此参数未使用，但可能会在将来实现。

## <a name="result-sets"></a>结果集

| 列名 | 数据类型 | 说明 |
|--|--|--|
| Object_Type | nvarchar(200) | 对象的类型 (例如：表或数据库) 。 |
| OBJECT_NAME | nvarchar(max) | 对象的完全限定名称。 使用后端特定引号字符进行转义。 |
| OBJECT_LEAF_NAME | nvarchar(max) | 非限定对象名称。 |
| TABLE_LOCATION | nvarchar(max) | 可用于 CREATE EXTERNAL TABLE 语句的有效表位置字符串。 `NULL`如果不适用，则为。 |
  
## <a name="permissions"></a>权限

需要 ALTER ANY EXTERNAL DATA SOURCE 权限。  

## <a name="remarks"></a>备注  

SQL Server 实例必须安装  [PolyBase](../../relational-databases/polybase/polybase-guide.md) 功能。

此存储过程支持的连接器：

- SQL Server
- Oracle
- Teradata
- MongoDB
- CosmosDB

该存储过程不支持通用 ODBC dta 源连接器。

空与非空的概念与 ODBC 驱动程序和[ `SQLTables` 函数](../native-client-odbc-api/sqltables.md)的行为相关。 非空表示对象包含表而非行。 例如，空架构在 SQL Server 中不包含任何表。 空数据库在 Teradata 中不包含任何表。

对象类型由外部数据源的 ODBC 驱动程序确定。 每个外部数据源决定作为表的内容。 这可以包括数据库对象，如 TeraData 中的函数或 Oracle 中的同义词。 PolyBase 无法连接到某些 ODBC 对象作为外部表，因此不会在 TABLE_LOCATION 列中包含值。 尽管如此，其中一个对象的存在可能会使数据库或架构为非空。

使用 `sp_data_source_objects` 和 [`sp_data_source_table_columns`](sp-data-source-table-columns.md) 来发现外部对象。 这些系统存储过程返回可虚拟化的表的架构。 Azure Data Studio 使用这两个存储过程来支持 [数据虚拟化](../../azure-data-studio/extensions/data-virtualization-extension.md)。 使用 [sp_data_source_table_columns](sp-data-source-table-columns.md) 发现 SQL Server 数据类型中表示的外部表架构。

### <a name="data-source-type-specific-remarks"></a>数据源类型特定备注

* Teradata

   Teradata 系统视图不 (RLS) 使用行级别安全性，因此用户可以看到它们不能查询的表是否存在。

* MongoDB

   某些早期版本的 MongoDB 限制了将所有数据库列出到类似管理员的用户的能力。 如果用户没有此权限，则在使用 null 执行此过程时，可能会出现身份验证错误 `@object_root_name` 。

## <a name="examples"></a>示例  

### <a name="sql-server"></a>SQL Server

下面的示例返回所有数据库、架构和表/视图

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 3;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | "database" | database | Null |
| SCHEMA | "数据库"。供 | dbo | Null |
| TABLE | "数据库"。dbo "."面向 | 客户 | [数据库]。[dbo]。面向 |
| TABLE | "数据库"。dbo "."内容 | item | [数据库]。[dbo]。内容 |
| TABLE | "数据库"。dbo "."民族 | 民族 | [数据库]。[dbo]。民族 |

下面的示例返回所有数据库

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | "UserDatabase" | UserDatabase | Null |
| DATABASE | “master” | 主 | Null |
| DATABASE | msdb | msdb | Null |
| DATABASE | tempdb | tempdb | Null |
| DATABASE | "database" | database | Null |

下面的示例返回数据库中的所有架构

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[database]'; 
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| SCHEMA | "数据库"。供 | dbo | Null |
| SCHEMA | "数据库"。INFORMATION_SCHEMA " | INFORMATION_SCHEMA | Null |
| SCHEMA | "数据库"。系统 | sys | Null |

下面的示例返回架构中的所有表 

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[database].[dbo]'; 
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| TABLE | "数据库"。dbo "."面向 | 客户 | [数据库]。[dbo]。面向 |
| TABLE | "数据库"。dbo "."内容 | item | [数据库]。[dbo]。内容 |
| TABLE | "数据库"。dbo "."民族 | 民族 | [数据库]。[dbo]。民族 |
| TABLE | "数据库"。dbo "."订购 | 订单 | [数据库]。[dbo]。订购 |
| TABLE | "数据库"。dbo "."包含 | 部件 | [数据库]。[dbo]。包含 |

### <a name="oracle"></a>Oracle

下面的示例返回完整的架构和表、函数、视图等。

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[OracleObjectRoot]'; 
DECLARE @max_search_depth INT = 2; 
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| VIEW | "SYS"。ALL_SQLSET_STATEMENTS " | ALL_SQLSET_STATEMENTS | [ORACLEOBJECTROOT].[SYS]。[ALL_SQLSET_STATEMENTS] |
| SYSTEM TABLE | "SYS"。启动 $ " | 启动 $ | [ORACLEOBJECTROOT].[SYS]。[启动 $] |
| SYNONYM | "公共"。ALL_ALL_TABLES " | ALL_ALL_TABLES | Null |
| SCHEMA | "database" | database | Null |
| TABLE | "数据库"。面向 | 客户 | [ORACLEOBJECTROOT].[数据库]。面向 |

### <a name="teradata"></a>Teradata

下面的示例返回所有数据库和表、函数、视图等。

```SQL
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 2;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |  
|--|--|--|--|
| FUNCTION | "SYSLIB"."ExtractRoles" | ExtractRoles | Null |  
| SYSTEM TABLE | "DBC"。UDTCast" | UDTCast | [DBC]。[UDTCast] |  
| TYPE | "SYSUDTLIB"."XML | XML | Null |  
| DATABASE | "database" | database | Null |
| TABLE | "数据库"。面向 | 客户 | [数据库]。面向 |  

### <a name="mongo-db"></a>Mongo DB

下面的示例返回所有数据库和表。

```SQL
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 2;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | "database" | database | Null |
| TABLE | "数据库"。面向 | 客户 | [数据库]。面向 |
| TABLE | "数据库"。内容 | item | [数据库]。内容 |
| TABLE | "数据库"。民族 | 民族 | [数据库]。民族 |
| TABLE | "数据库"。订购 | 订单 | [数据库]。订购 |

## <a name="see-also"></a>另请参阅

- [sp_data_source_columns](./sp-data-source-table-columns.md)   
- [CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)
- [Azure Data Studio 的数据虚拟化扩展](../../azure-data-studio/extensions/data-virtualization-extension.md)   
- [PolyBase 入门](../polybase/polybase-guide.md)   
- [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
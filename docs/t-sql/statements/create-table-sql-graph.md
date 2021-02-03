---
description: CREATE TABLE（SQL 图形）
title: CREATE TABLE（SQL 图形）| Microsoft Docs
ms.custom: ''
ms.date: 09/09/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SQL_GRAPH_TSQL
- TABLE
- CREATE_TABLE_TSQL
- NODE
- NODE_TSQL
- AS_NODE
- AS_NODE_TSQL
- EDGE
- EDGE_TSQL
- AS_EDGE
- AS_EDGE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- graph
- SQL graph
- CREATE TABLE SQL graph
- NODE
- EDGE
- SQL graph, CREATE TABLE statement
ms.assetid: ''
author: shkale-msft
ms.author: shkale
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7d946a2c834fa910a0a7b76b321a55bfbdd91198
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177893"
---
# <a name="create-table-sql-graph"></a>CREATE TABLE（SQL 图形）
[!INCLUDE[SQL Server 2017](../../includes/applies-to-version/sqlserver2017.md)]

以 `NODE` 或 `EDGE` 表的形式创建新 SQL 图形表。 
  
> [!NOTE]   
>  有关标准 Transact-SQL 语句，请参阅 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)。
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE TABLE   
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( { <column_definition> } 
       | <computed_column_definition>
       | <column_set_definition>
       | [ <table_constraint> ] [ ,... n ]
       | [ <table_index> ] }
          [ ,...n ]
    )   
    AS [ NODE | EDGE ]
    [ ON { partition_scheme_name ( partition_column_name )
           | filegroup
           | "default" } ]
[ ; ] 

< table_constraint > ::=
[ CONSTRAINT constraint_name ]
{
    { PRIMARY KEY | UNIQUE }
        [ CLUSTERED | NONCLUSTERED ]
        (column [ ASC | DESC ] [ ,...n ] )
        [
            WITH FILLFACTOR = fillfactor
           |WITH ( <index_option> [ , ...n ] )
        ]
        [ ON { partition_scheme_name (partition_column_name)
            | filegroup | "default" } ]
    | FOREIGN KEY
        ( column [ ,...n ] )
        REFERENCES referenced_table_name [ ( ref_column [ ,...n ] ) ]
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ NOT FOR REPLICATION ]
    | CONNECTION
        ( { node_table TO node_table } 
          [ , {node_table TO node_table }]
          [ , ...n ]
        )
        [ ON DELETE { NO ACTION | CASCADE } ]
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )
```  
  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
本文档仅列出与 SQL 图形相关的参数。 有关受支持参数的完整列表和说明，请参阅 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)

 database_name    
 要在其中创建表的数据库的名称。 database_name 须指定现有数据库的名称。 如果未指定，则 database_name 默认为当前数据库。 当前连接的登录名必须与 database_name 所指定数据库中的一个现有用户 ID 关联，并且该用户 ID 必须具有 CREATE TABLE 权限。  
  
 schema_name    
 新表所属架构的名称。  
  
 *table_name*      
 是节点或边界表的名称。 表名必须遵循有关[标识符](../../relational-databases/databases/database-identifiers.md)的规则。 除了本地临时表名（以单个数字符号 (#) 为前缀的名称）不能超过 116 个字符外，table_name 最多可包含 128 个字符。  
  
 NODE   
 创建节点表。

 EDGE  
 创建边界表。  
 
 *table_constraint*   
 指定添加到表中的 PRIMARY KEY、UNIQUE、FOREIGN KEY、CONNECTION constraint、CHECK 约束或 DEFAULT 定义的属性。
 
 > [!NOTE]   
 > CONNECTION 约束仅适用于边缘表类型。
 
 ON { partition_scheme | filegroup | "default" }    
 指定存储表的分区架构或文件组。 如果指定了 partition_scheme，则该表将成为已分区表，其分区存储在 partition_scheme 所指定的一个或多个文件组的集合中。 如果指定了 filegroup，则该表将存储在已命名文件组中。 数据库中必须存在该文件组。 如果指定了 default，或者根本未指定 ON，则该表将存储在默认文件组中。 CREATE TABLE 中指定的表的存储机制以后不能进行更改。

 ON {partition_scheme | filegroup | "default"}    
 也可在 PRIMARY KEY 约束或 UNIQUE 约束中指定。 这些约束会创建索引。 如果 filegroup 未指定，则索引会存储在已命名文件组中。 如果指定了 default，或者根本未指定 ON，则索引将与表存储在同一文件组中。 如果 PRIMARY KEY 约束或 UNIQUE 约束创建聚集索引，则表的数据页将与索引存储在同一文件组中。 如果指定了 CLUSTERED 或约束另外创建了聚集索引，并且指定的 partition_scheme 不同于表定义的 partition_scheme 或 filegroup，或反之，则只接受约束定义，而忽略其他定义。
  
## <a name="remarks"></a>备注

不支持以节点或边界表的形式创建临时表。  

不支持以临时表的形式创建节点或边界表。

节点或边界表不支持延伸数据库。

节点或边界表不能是外部表（PolyBase 不支持图形表）。 

未分区的图节点/边界表不能更改为分区图节点/边界表。 
  
 
## <a name="examples"></a>示例  
  
### <a name="a-create-a-node-table"></a>A. 创建 `NODE` 表
 下面的示例演示如何创建 `NODE` 表

```sql
 CREATE TABLE Person (
        ID INTEGER PRIMARY KEY, 
        name VARCHAR(100), 
        email VARCHAR(100)
 ) AS NODE;
```

### <a name="b-create-an-edge-table"></a>B. 创建 `EDGE` 表
下面的示例演示如何创建 `EDGE` 表

```sql
 CREATE TABLE friends (
    id INTEGER PRIMARY KEY,
    start_date DATe
 ) AS EDGE;
```

```sql
 -- Create a likes edge table, this table does not have any user defined attributes   
 CREATE TABLE likes AS EDGE;
```

下一个示例建模了一条规则，即只有用户可以和其他用户进行联结，这意味着此边界不允许引用除人员之外的任何节点。

```
/* Create friend edge table with CONSTRAINT, restricts for nodes and it direction */
CREATE TABLE dbo.FriendOf(
  CONSTRAINT cnt_Person_FriendOf_Person
    CONNECTION (dbo.Person TO dbo.Person) 
)AS EDGE;
```


## <a name="see-also"></a>另请参阅 
 [ALTER TABLE table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)   
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [INSERT（SQL 图形）](../../t-sql/statements/insert-sql-graph.md)]  
 [使用 SQL Server 2017 进行图形处理](../../relational-databases/graphs/sql-graph-overview.md)


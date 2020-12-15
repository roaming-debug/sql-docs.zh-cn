---
description: SQL Graph 体系结构
title: SQL Graph 体系结构 |Microsoft Docs
ms.custom: ''
ms.date: 09/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: language-reference
helpviewer_keywords:
- SQL graph
- SQL graph, architecture
ms.assetid: ''
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f167741a2064020cfbc7fdc43e881a74609e4ac6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480158"
---
# <a name="sql-graph-architecture"></a>SQL Graph 体系结构  
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

了解如何构建 SQL Graph。 了解基础知识会使其他 SQL Graph 文章更易于理解。
 
## <a name="sql-graph-database"></a>SQL Graph 数据库
用户可以为每个数据库创建一个关系图。 关系图是节点和边缘表的集合。 节点或边界表可以在数据库中的任何架构下创建，但它们都属于一个逻辑关系图。 节点表是类似节点类型的集合。 例如，Person 节点表包含属于关系图的所有人员节点。 同样，边缘表也是类似类型的边缘的集合。 例如，朋友边缘表包含将某人员连接到其他人的所有边缘。 由于节点和边缘存储在表中，因此在节点或边界表上支持在常规表上支持的大多数操作。 
 
 
![显示 SQL Graph 数据库体系结构的关系图。](../../relational-databases/graphs/media/sql-graph-architecture.png "Sql graph 数据库体系结构")   

图1： SQL Graph 数据库体系结构
 
## <a name="node-table"></a>节点表
节点表表示图形架构中的实体。 每次创建节点表时，还会创建一个隐式 `$node_id` 列，该列用于唯一标识数据库中的给定节点。 中的值 `$node_id` 是自动生成的，是 `object_id` 该节点表和内部生成的 bigint 值的组合。 但是，在 `$node_id` 选择该列时，将显示 JSON 字符串形式的计算值。 此外， `$node_id` 是一个伪列，它映射到其中包含十六进制字符串的内部名称。 `$node_id`从表中选择时，列名称将显示为 `$node_id_<hex_string>` 。 使用查询中的伪列名时，建议的方法是查询内部 `$node_id` 列，并使用包含十六进制字符串的内部名称。

建议用户在创建节点表时在列上创建唯一约束或索引 `$node_id` ，但如果没有创建，则会自动创建默认的唯一非聚集索引。 不过，关系图伪列上的任何索引都是在基础内部列上创建的。 也就是说，对列创建的索引 `$node_id` 将显示在内部 `graph_id_<hex_string>` 列上。   


## <a name="edge-table"></a>边缘表
边缘表表示关系图中的关系。 边缘始终定向并连接两个节点。 使用边缘表，用户可以为图形中的多对多关系建模。 边缘表中不能有任何用户定义的属性。 每次创建边缘表时，还会在边缘表中创建三个隐式列：

|列名称    |说明  |
|---   |---  |
|`$edge_id`   |唯一标识数据库中的给定边缘。 它是生成的列，并且值是边缘表 object_id 和内部生成的 bigint 值的组合。 但是，在 `$edge_id` 选择该列时，将显示 JSON 字符串形式的计算值。 `$edge_id` 是一个伪列，它映射到其中包含十六进制字符串的内部名称。 `$edge_id`从表中选择时，列名称将显示为 `$edge_id_\<hex_string>` 。 使用查询中的伪列名时，建议的方法是查询内部 `$edge_id` 列，并使用包含十六进制字符串的内部名称。 |
|`$from_id`   |存储 `$node_id` 从中产生边缘的节点的。  |
|`$to_id`   |存储 `$node_id` 节点的，其中的边缘终止。 |

给定边缘可以连接的节点由和列中插入的数据控制 `$from_id` `$to_id` 。 在第一个版本中，不能对边缘表定义约束，以限制其连接任意两种类型的节点。 也就是说，边缘可以连接图形中的任意两个节点，而不考虑它们的类型。

与列类似 `$node_id` ，建议用户在创建边缘表时对列创建唯一索引或约束 `$edge_id` ，但如果没有创建，则会在此列上自动创建默认的唯一非聚集索引。 不过，关系图伪列上的任何索引都是在基础内部列上创建的。 也就是说，对列创建的索引 `$edge_id` 将显示在内部 `graph_id_<hex_string>` 列上。 此外，对于 OLTP 方案，还建议用户在 (上创建索引 `$from_id` ， `$to_id`) 列，以便在边缘的方向上更快地查找。  

图2显示了如何将节点和边缘表存储在数据库中。 

![显示节点和边缘表表示形式的关系图。](../../relational-databases/graphs/media/person-friends-tables.png "人员节点和好友边缘表")   

图2：节点和边缘表表示形式



## <a name="metadata"></a>元数据
使用这些元数据视图可以查看节点或边缘表的属性。
 
### <a name="systables"></a>sys.tables
下面的新的位类型将添加到 SYS。张. 如果 `is_node` 设置为1，则表示该表是节点表，如果 `is_edge` 设置为1，则表示该表是一个边缘表。
 
|列名 |数据类型 |说明 |
|--- |---|--- |
|is_node |bit |1 = 这是一个节点表 |
|is_edge |bit |1 = 这是一个边缘表 |
 
### <a name="syscolumns"></a>sys.columns
`sys.columns`视图包含其他列 `graph_type` 和 `graph_type_desc` ，用于指示 "节点" 和 "边缘" 表中的列的类型。
 
|列名 |数据类型 |说明 |
|--- |---|--- |
|graph_type |int |带有一组值的内部列。 对于 graph 列，值介于1-8 和其他值之间。  |
|graph_type_desc |nvarchar(60)  |带有一组值的内部列 |
 
下表列出了列的有效值 `graph_type`

|列值  |说明  |
|---   |---   |
|1  |GRAPH_ID  |
|2  |GRAPH_ID_COMPUTED  |
|3  |GRAPH_FROM_ID  |
|4  |GRAPH_FROM_OBJ_ID  |
|5  |GRAPH_FROM_ID_COMPUTED  |
|6  |GRAPH_TO_ID  |
|7  |GRAPH_TO_OBJ_ID  |
|8  |GRAPH_TO_ID_COMPUTED  |


`sys.columns` 还存储有关在节点或边缘表中创建的隐式列的信息。 可以从 sys.databases 检索以下信息，但是，用户无法从节点或边缘表中选择这些列。 

节点表中的隐式列

|列名    |数据类型  |is_hidden  |评论  |
|---  |---|---|---  |
|graph_id_\<hex_string> |BIGINT |1  |内部 `graph_id` 列  |
|$node _id_\<hex_string> |NVARCHAR   |0  |外部节点 `node_id` 列  |

边缘表中的隐式列

|列名    |数据类型  |is_hidden  |评论  |
|---  |---|---|---  |
|graph_id_\<hex_string> |BIGINT |1  |内部 `graph_id` 列  |
|$edge _id_\<hex_string> |NVARCHAR   |0  |外部 `edge_id` 列  |
|from_obj_id_\<hex_string>  |INT    |1  |节点内部 `object_id`  |
|from_id_\<hex_string>  |BIGINT |1  |节点内部 `graph_id`  |
|$from _id_\<hex_string> |NVARCHAR   |0  |节点外部 `node_id`  |
|to_obj_id_\<hex_string>    |INT    |1  |节点内部 `object_id`  |
|to_id_\<hex_string>    |BIGINT |1  |节点内部 `graph_id`  |
|$to _id_\<hex_string>   |NVARCHAR   |0  |节点外部 `node_id`  |
 
### <a name="system-functions"></a>系统函数
添加了下列内置函数。 这将帮助用户从生成的列中提取信息。 请注意，这些方法不会验证用户的输入。 如果用户指定了无效 `sys.node_id` 的方法，将提取相应的部分并将其返回。 例如，OBJECT_ID_FROM_NODE_ID 将采用 `$node_id` 作为输入，并将返回表的 object_id，此节点属于。 
 
|内置   |说明  |
|---  |---  |
|OBJECT_ID_FROM_NODE_ID |从中提取 object_id `node_id`  |
|GRAPH_ID_FROM_NODE_ID  |从中提取 graph_id `node_id`  |
|NODE_ID_FROM_PARTS |构造和中的 node_id `object_id``graph_id`  |
|OBJECT_ID_FROM_EDGE_ID |提取 `object_id` 自 `edge_id`  |
|GRAPH_ID_FROM_EDGE_ID  |从中提取标识 `edge_id`  |
|EDGE_ID_FROM_PARTS |构造 `edge_id` `object_id` 和标识  |



## <a name="transact-sql-reference"></a>Transact-SQL 参考 
了解 [!INCLUDE[tsql-md](../../includes/tsql-md.md)] SQL Server 和 AZURE SQL 数据库中引入的扩展，这些扩展支持创建和查询图形对象。 查询语言扩展可帮助查询并使用 ASCII 艺术语法遍历关系图。
 
### <a name="data-definition-language-ddl-statements"></a>数据定义语言 (DDL) 语句

|任务   |相关文章  |说明
|---  |---  |---  |
|CREATE TABLE |[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-sql-graph.md)|`CREATE TABLE` 现已扩展为支持将表创建为节点或边缘。 请注意，边缘表可以是也可以没有任何用户定义的属性。  |
|ALTER TABLE    |[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)|节点和边缘表可以通过与关系表相同的方式进行更改，使用 `ALTER TABLE` 。 用户可以添加或修改用户定义的列、索引或约束。 但是，更改内部图形列（如 `$node_id` 或 `$edge_id` ）将导致错误。  |
|CREATE INDEX   |[CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)  |用户可以对节点和边缘表中的伪列和用户定义列创建索引。 支持所有索引类型，包括聚集列存储索引和非聚集列存储索引。  |
|创建边缘约束    |[Transact-sql&#41;的边缘约束 &#40;](../../relational-databases/tables/graph-edge-constraints.md)  |用户现在可以对边缘表创建边缘约束，以强制实施特定的语义，还可以保持数据完整性  |
|DROP TABLE |[DROP TABLE (Transact-SQL)](../../t-sql/statements/drop-table-transact-sql.md)  |可以使用与关系表相同的方式删除节点和边缘表 `DROP TABLE` 。 但在此版本中，不支持在删除节点或节点表时，不会有任何边界指向已删除的节点和边缘的级联删除。 建议在删除节点表后，用户会手动删除连接到该节点表中节点的任何边缘以维护关系图的完整性。  |


### <a name="data-manipulation-language-dml-statements"></a>数据操作语言 (DML) 语句

|任务   |相关文章  |说明
|---  |---  |---  |
|INSERT |[INSERT (Transact-SQL)](../../t-sql/statements/insert-sql-graph.md)|插入到节点表与在关系表中插入没有什么不同。 列的值 `$node_id` 将自动生成。 尝试在或列中插入 `$node_id` 值 `$edge_id` 将导致错误。 `$from_id` `$to_id` 当插入到边缘表中时，用户必须为和列提供值。 `$from_id` 和 `$to_id` 是 `$node_id` 给定边缘连接到的节点的值。  |
|DELETE | [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)|从节点或边缘表中删除数据时，可以通过与从关系表中删除数据相同的方式删除数据。 但是，在此版本中，没有任何约束来确保在删除节点时不支持边缘指向已删除的节点和边缘的级联删除。 建议每次删除节点时，也会删除该节点的所有连接边缘，以维持图形的完整性。  |
|UPDATE |[UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)  |用户定义列中的值可以使用 UPDATE 语句进行更新。 不允许更新内部图形列 `$node_id` 、 `$edge_id` 、 `$from_id` 和 `$to_id` 。  |
|MERGE |[MERGE (Transact-SQL)](../../t-sql/statements/merge-transact-sql.md)  |`MERGE` 语句在节点或边界表上受支持。  |


### <a name="query-statements"></a>查询语句

|任务   |相关文章  |说明
|---  |---  |---  |
|SELECT |[SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)|节点和边缘以表的形式存储在一起，因此在 SQL Server 或 Azure SQL 数据库中的表上支持的大多数操作都在节点和边缘表上受支持  |
|MATCH  | [MATCH &#40;Transact-sql&#41;](../../t-sql/queries/match-sql-graph.md)|引入了 MATCH，以支持模式匹配和遍历关系图。  |



## <a name="limitations-and-known-issues"></a>限制和已知问题  
对于此版本中的节点和边缘表，存在某些限制：
* 本地或全局临时表不能是节点或边界表。
* 表类型和表变量不能声明为节点或边界表。 
* 不能将节点和边缘表创建为经系统版本控制的临时表。   
* 节点和边界表不能是内存优化表。  
* 用户无法 `$from_id` `$to_id` 使用 update 语句更新边缘的和列。 若要更新边缘所连接的节点，用户必须插入指向新节点的新边缘，并删除上一个节点。
* 不支持对 graph 对象的跨数据库查询。 


## <a name="next-steps"></a>后续步骤
若要开始学习新语法，请参阅 [SQL Graph 数据库-示例](./sql-graph-sample.md)
 


---
description: 'SHORTEST_PATH (Transact-sql) '
title: SQL Graph) 的最短路径 (|Microsoft Docs
ms.custom: ''
ms.date: 07/01/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: language-reference
f1_keywords:
- SHORTEST PATH
- SHORTEST_PATH
dev_langs:
- TSQL
helpviewer_keywords:
- MATCH statement [SQL Server], SQL graph
- SQL graph, MATCH statement
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-ver15||=azuresqldb-mi-current
ms.openlocfilehash: 9e5a6f6506e4ce40b3b89f32dd58329eb4667760
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100351398"
---
# <a name="shortest_path-transact-sql"></a>SHORTEST_PATH (Transact-sql) 

[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi.md)]

  指定关系图的搜索条件，该搜索条件是以递归方式或重复方式搜索的。 可以在 SELECT 语句中与 graph 节点和边缘表匹配中使用 SHORTEST_PATH。 
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="shortest-path"></a>最短路径
SHORTEST_PATH 函数允许你查找：    
* 两个给定节点/实体之间的最短路径
* )  (单个源最短路径。
* 从多个源节点到多个目标节点的最短路径。

它采用任意长度的模式作为输入，并返回两个节点之间存在的最短路径。 此函数只能在 MATCH 内使用。 函数在任意两个给定节点之间仅返回一个最短路径。 如果存在两个或两个以上的源节点和目标节点对之间的两个或两个以上的最短路径 () ，则该函数只返回遍历期间首先找到的一个路径。 请注意，只能在 SHORTEST_PATH 函数内指定任意长度的模式。 

有关语法，请参阅 [SQL Graph)  (匹配 ](../../t-sql/queries/match-sql-graph.md) 。 

## <a name="for-path"></a>对于路径
对于，路径必须与 FROM 子句中的任何节点或边界表名称一起使用，这将参与任意长度的模式。 对于 "路径"，指示引擎节点或边缘表将返回一个已排序的集合，该集合表示沿遍历的路径找到的节点或边缘的列表。 不能直接在 SELECT 子句中投影这些表中的属性。 若要投影这些表中的属性，必须使用图形路径聚合函数。  

## <a name="arbitrary-length-pattern"></a>任意长度模式
此模式包括节点和边缘，必须反复遍历这些节点和边缘，直到达到所需的节点或达到模式中指定的最大迭代数。 每次执行查询时，执行此模式的结果将是从开始节点到结束节点的路径遍历的节点和边缘的有序集合。 这是正则表达式样式语法模式，支持以下两种模式限定符：

* **"+"**：重复模式1次或多次。 找到最短路径后立即终止。
* **{1,n}** ：重复模式 1到“n”次。 找到最短的时立即终止。

## <a name="last_node"></a>LAST_NODE
LAST_NODE () 函数允许链接两个任意长度的遍历模式。 它可用于以下情况：    
* 一个查询中使用了多个最短路径模式，一个模式从上一模式的最后一个节点开始。
* 两个最短路径模式合并到同一个 LAST_NODE () 。

## <a name="graph-path-order"></a>图形路径顺序
Graph 路径顺序指的是输出路径中数据的顺序。 输出路径顺序始终从模式的非递归部分开始，后面是在递归部分中显示的节点/边缘。 在查询优化/执行期间遍历关系图的顺序与输出中打印的顺序无关。 同样，递归模式中箭头的方向也不会影响关系图路径顺序。 

## <a name="graph-path-aggregate-functions"></a>Graph 路径聚合函数
由于任意长度模式中涉及的节点和边缘返回 (的节点 () s 的集合，) 在该路径) 遍历的 (，因此用户无法使用常规 attributename 语法直接投影属性。 对于需要从中间节点或边缘表投影属性值的查询，在遍历的路径中，使用以下图形路径聚合函数： STRING_AGG、LAST_VALUE、SUM、AVG、MIN、MAX 和 COUNT。 在 SELECT 子句中使用这些聚合函数的常规语法为：

```syntaxsql
<GRAPH_PATH_AGGREGATE_FUNCTION>(<expression> , <separator>)  <order_clause>

    <order_clause> ::=
        { WITHIN GROUP (GRAPH PATH) }

    <GRAPH_PATH_AGGREGATE_FUNCTION> ::=
          STRING_AGG
        | LAST_VALUE
        | SUM
        | COUNT
        | AVG
        | MIN
        | MAX

```

### <a name="string_agg"></a>STRING_AGG
STRING_AGG 函数采用表达式和分隔符作为输入并返回一个字符串。 用户可以在 SELECT 子句中使用此函数来投影所遍历路径中的中间节点或边缘中的属性。 

### <a name="last_value"></a>LAST_VALUE
若要从遍历的路径的最后一个节点投影属性，可以使用 LAST_VALUE 聚合函数。 将边缘表别名作为此函数的输入提供是错误的，只能使用节点表名称或别名。

**最后一个节点**：最后一个节点引用在遍历的路径中最后显示的节点，而不考虑匹配谓词中的箭头方向。 例如：`MATCH(SHORTEST_PATH(n(-(e)->p)+) )`。 此处，路径中的最后一个节点将是最后访问的 P 节点。 

相反，最后一个节点是此模式的输出关系图路径中的最后一个第 n 个节点： `MATCH(SHORTEST_PATH((n<-(e)-)+p))`    

### <a name="sum"></a>SUM
此函数返回在遍历路径中显示的已提供节点/边缘属性值或表达式的总和。

### <a name="count"></a>COUNT
此函数返回路径中所需节点/边缘属性的非 null 值的数量。 COUNT 函数支持 \* 带有节点或边界表别名的 "" 运算符。 如果没有节点或边缘表别名，则的使用 \* 不明确，将导致错误。

```syntaxsql
{  COUNT( <expression> | <node_or_edge_alias>.* )  <order_clause>  }
```

### <a name="avg"></a>AVG
返回在所遍历的路径中出现的所提供的节点/边缘属性值或表达式的平均值。

### <a name="min"></a>MIN
返回所提供的节点/边缘属性值或所遍历路径中出现的表达式的最小值。

### <a name="max"></a>MAX
返回所提供的节点/边缘属性值或所遍历路径中出现的表达式的最大值。

## <a name="remarks"></a>备注  
shortest_path 函数只能在 MATCH 内使用。     
仅 shortest_path 支持 LAST_NODE。     
如果查找加权最短路径，则不支持所有路径或所有最短路径。         
在某些情况下，可能会为具有更多跃点数的查询生成错误的计划，从而提高查询执行时间。 使用哈希联接提示可能会有所帮助。    


## <a name="examples"></a>示例 
对于此处所示的示例查询，我们将使用在[SQL Graph](./sql-graph-sample.md)中创建的节点和边缘表示例

### <a name="a--find-shortest-path-between-2-people"></a>A.  查找两名人员之间的最短路径
 在下面的示例中，我们找到 Jacob 与 Alice 之间的最短路径。 我们需要从 graph 示例脚本创建的人员节点和 FriendOf 边缘。 

```sql
SELECT PersonName, Friends
FROM (  
    SELECT
        Person1.name AS PersonName, 
        STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends,
        LAST_VALUE(Person2.name) WITHIN GROUP (GRAPH PATH) AS LastNode
    FROM
        Person AS Person1,
        friendOf FOR PATH AS fo,
        Person FOR PATH  AS Person2
    WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2)+))
    AND Person1.name = 'Jacob'
) AS Q
WHERE Q.LastNode = 'Alice'
```

 ### <a name="b--find-shortest-path-from-a-given-node-to-all-other-nodes-in-the-graph"></a>B.  查找关系图中从给定节点到所有其他节点的最短路径。 
 下面的示例查找 Jacob 连接到的关系图中的所有人员，以及从 Jacob 开始到所有这些人员的最短路径。 

```sql
SELECT
    Person1.name AS PersonName, 
    STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends
FROM
    Person AS Person1,
    friendOf FOR PATH AS fo,
    Person FOR PATH  AS Person2
WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2)+))
AND Person1.name = 'Jacob'
```

### <a name="c--count-the-number-of-hopslevels-traversed-to-go-from-one-person-to-another-in-the-graph"></a>C.  计算在图形中遍历以从一个人到另一个人的跃点/级别数。
 下面的示例查找 Jacob 与 Alice 之间的最短路径，并打印从 Jacob 到 Alice 需要的跃点数。 

```sql
 SELECT PersonName, Friends, levels
FROM (  
    SELECT
        Person1.name AS PersonName, 
        STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends,
        LAST_VALUE(Person2.name) WITHIN GROUP (GRAPH PATH) AS LastNode,
        COUNT(Person2.name) WITHIN GROUP (GRAPH PATH) AS levels
    FROM
        Person AS Person1,
        friendOf FOR PATH AS fo,
        Person FOR PATH  AS Person2
    WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2)+))
    AND Person1.name = 'Jacob'
) AS Q
WHERE Q.LastNode = 'Alice'
```

### <a name="d-find-people-1-3-hops-away-from-a-given-person"></a>D. 查找离开给定人员的人员1-3 跃点
下面的示例查找 Jacob 与他连接到的关系图1-3 跃点的所有用户之间的最短路径。 

```sql
SELECT
    Person1.name AS PersonName, 
    STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends
FROM
    Person AS Person1,
    friendOf FOR PATH AS fo,
    Person FOR PATH  AS Person2
WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2){1,3}))
AND Person1.name = 'Jacob'
```

### <a name="e-find-people-exactly-2-hops-away-from-a-given-person"></a>E. 查找来自给定人员的完全2个跃点
下面的示例查找 Jacob 与图形中正好有2个跃点的用户之间的最短路径。 

```sql
SELECT PersonName, Friends
FROM (
    SELECT
        Person1.name AS PersonName, 
        STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends,
        COUNT(Person2.name) WITHIN GROUP (GRAPH PATH) AS levels
    FROM
        Person AS Person1,
        friendOf FOR PATH AS fo,
        Person FOR PATH  AS Person2
    WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2){1,3}))
    AND Person1.name = 'Jacob'
) Q
WHERE Q.levels = 2
```

## <a name="see-also"></a>另请参阅  
 [匹配 (SQL Graph) ](../../t-sql/queries/match-sql-graph.md)    
 [CREATE TABLE &#40;SQL Graph&#41;](../../t-sql/statements/create-table-sql-graph.md)   
 [INSERT（SQL 图形）](../../t-sql/statements/insert-sql-graph.md)]  
 [使用 SQL Server 2017 进行图形处理](../../relational-databases/graphs/sql-graph-overview.md)     
 

---
description: MATCH (Transact-SQL)
title: MATCH (SQL Graph) | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- MATCH
- MATCH_TSQL
- SHORTEST_PATH
dev_langs:
- TSQL
helpviewer_keywords:
- MATCH statement [SQL Server], SQL graph
- SQL graph, MATCH statement
- Shortest Path, shortest_path
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0ebd5ea961301fb330106430af939664b6bce3e9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439062"
---
# <a name="match-transact-sql"></a>MATCH (Transact-SQL)
[!INCLUDE[SQL Server 2017](../../includes/applies-to-version/sqlserver2017.md)]

  指定图形的搜索条件。 MATCH 只能在 SELECT 语句中作为 WHERE 子句的一部分，与图形节点和边缘表一起使用。 
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
MATCH (<graph_search_pattern>)

<graph_search_pattern>::=
  {  
      <simple_match_pattern> 
    | <arbitrary_length_match_pattern>  
    | <arbitrary_length_match_last_node_predicate> 
  }

<simple_match_pattern>::=
  {
      LAST_NODE(<node_alias>) | <node_alias>   { 
          { <-( <edge_alias> )- } 
        | { -( <edge_alias> )-> }
        <node_alias> | LAST_NODE(<node_alias>)
        } 
  }
  [ { AND } { ( <simple_match_pattern> ) } ]
  [ ,...n ]

<node_alias> ::=
  node_table_name | node_table_alias 

<edge_alias> ::=
  edge_table_name | edge_table_alias


<arbitrary_length_match_pattern>  ::=
  { 
    SHORTEST_PATH( 
      <arbitrary_length_pattern> 
      [ { AND } { <arbitrary_length_pattern> } ] 
      [ ,…n] 
    )
  } 

<arbitrary_length_match_last_node_predicate> ::=
  {  LAST_NODE( <node_alias> ) = LAST_NODE( <node_alias> ) }


<arbitrary_length_pattern> ::=
    {  LAST_NODE( <node_alias> )   | <node_alias>
     ( <edge_first_al_pattern> [<edge_first_al_pattern>…,n] )
     <al_pattern_quantifier> 
  }
    |  ( {<node_first_al_pattern> [<node_first_al_pattern> …,n] )
        <al_pattern_quantifier> 
        LAST_NODE( <node_alias> ) | <node_alias> 
 }
    
<edge_first_al_pattern> ::=
  { (  
        { -( <edge_alias> )->   } 
      | { <-( <edge_alias> )- } 
      <node_alias>
      ) 
  } 

<node_first_al_pattern> ::=
  { ( 
      <node_alias> 
        { <-( <edge_alias> )- } 
      | { -( <edge_alias> )-> }
      ) 
  } 


<al_pattern_quantifier> ::=
  {
        +
      | { 1 , n }
  }

n -  positive integer only.
 
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
graph_search_pattern    
指定图形中的搜索模式或遍历路径。 此模式使用 ASCII 图表语法来遍历图形中的路径。 模式将按照所提供的箭头方向通过边缘从一个节点转到另一个节点。 边缘名称或别名是在括号内提供。 节点名称或别名显示在箭头两端。 模式中，箭头可以指向两个方向中的任意一个方向。

node_alias  
FROM 子句中提供的节点表的名称或别名。

edge_alias  
FROM 子句中提供的边缘表的名称或别名。

SHORTEST_PATH   
最短路径函数用于查找图形中两个给定节点之间的最短路径，或图形中给定节点与其他所有节点之间的最短路径。 它需要使用在图形中重复搜索的任意长度模式作为输入。 

arbitrary_length_match_pattern  
指定在到达相应节点前，或在达到模式中指定的最高迭代次数前，必须重复遍历的节点和边缘。 

al_pattern_quantifier   
任意长度模式需要使用正则表达式样式模式数量值，以指定给定搜索模式的重复次数。 支持的搜索模式数量值为：   
* **+** ：重复模式 1 次或多次。 找到最短路径后立即终止。    
* **{1,n}** ：重复模式 1到“n”次。 找到最短路径后立即终止。     

## <a name="remarks"></a>备注  
MATCH 中的节点名称可以重复。  换而言之，在同一个查询中可以按任意次数遍历节点。  
MATCH 中的边缘名不能重复。  
边缘可以指向两个方向中的任意一个方向，但它必须具有显式方向。  
MATCH 模式中不支持 OR 和 NOT 运算符。 在 WHERE 子句中使用 AND 可以将 MATCH 与其他表达式结合使用。 但是不支持使用 OR 或 NOT 将其与其他表达式结合使用。 

## <a name="examples"></a>示例  
### <a name="a--find-a-friend"></a>A.  查找好友 
 以下示例创建了一个 Person 节点表和好友边缘表，在其中插入一些数据，然后使用 MATCH 查找图中人物 Alice 的好友。

 ```sql
 -- Create person node table
 CREATE TABLE dbo.Person (ID INTEGER PRIMARY KEY, name VARCHAR(50)) AS NODE;
 CREATE TABLE dbo.friend (start_date DATE) AS EDGE;

 -- Insert into node table
 INSERT INTO dbo.Person VALUES (1, 'Alice');
 INSERT INTO dbo.Person VALUES (2,'John');
 INSERT INTO dbo.Person VALUES (3, 'Jacob');

-- Insert into edge table
INSERT INTO dbo.friend VALUES ((SELECT $node_id FROM dbo.Person WHERE name = 'Alice'),
        (SELECT $node_id FROM dbo.Person WHERE name = 'John'), '9/15/2011');

INSERT INTO dbo.friend VALUES ((SELECT $node_id FROM dbo.Person WHERE name = 'Alice'),
        (SELECT $node_id FROM dbo.Person WHERE name = 'Jacob'), '10/15/2011');

INSERT INTO dbo.friend VALUES ((SELECT $node_id FROM dbo.Person WHERE name = 'John'),
        (SELECT $node_id FROM dbo.Person WHERE name = 'Jacob'), '10/15/2012');

-- use MATCH in SELECT to find friends of Alice
SELECT Person2.name AS FriendName
FROM Person Person1, friend, Person Person2
WHERE MATCH(Person1-(friend)->Person2)
AND Person1.name = 'Alice';
 ```

 ### <a name="b--find-friend-of-a-friend"></a>B.  查找好友的好友
 以下示例尝试查找 Alice 好友的好友。 

 ```sql
SELECT Person3.name AS FriendName 
FROM Person Person1, friend, Person Person2, friend friend2, Person Person3
WHERE MATCH(Person1-(friend)->Person2-(friend2)->Person3)
AND Person1.name = 'Alice';
 ```

### <a name="c--more-match-patterns"></a>C.  更多 `MATCH` 模式
 以下提供了更多在 MATCH 中指定模式的方法。

 ```sql
 -- Find a friend
    SELECT Person2.name AS FriendName
    FROM Person Person1, friend, Person Person2
    WHERE MATCH(Person1-(friend)->Person2);
    
-- The pattern can also be expressed as below

    SELECT Person2.name AS FriendName
    FROM Person Person1, friend, Person Person2 
    WHERE MATCH(Person2<-(friend)-Person1);


-- Find 2 people who are both friends with same person
    SELECT Person1.name AS Friend1, Person2.name AS Friend2
    FROM Person Person1, friend friend1, Person Person2, 
         friend friend2, Person Person0
    WHERE MATCH(Person1-(friend1)->Person0<-(friend2)-Person2);
    
-- this pattern can also be expressed as below

    SELECT Person1.name AS Friend1, Person2.name AS Friend2
    FROM Person Person1, friend friend1, Person Person2, 
         friend friend2, Person Person0
    WHERE MATCH(Person1-(friend1)->Person0 AND Person2-(friend2)->Person0);
 ```
 

## <a name="see-also"></a>另请参阅  
 [CREATE TABLE &#40;SQL Graph&#41;](../../t-sql/statements/create-table-sql-graph.md)   
 [INSERT（SQL 图形）](../../t-sql/statements/insert-sql-graph.md)]  
 [使用 SQL Server 2017 进行图形处理](../../relational-databases/graphs/sql-graph-overview.md)  

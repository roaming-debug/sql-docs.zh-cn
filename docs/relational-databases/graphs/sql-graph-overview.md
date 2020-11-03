---
description: SQL Server 和 Azure SQL 数据库中的图形处理
title: 图形处理
titleSuffix: SQL Server and Azure SQL Database
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: language-reference
helpviewer_keywords:
- SQL graph
- SQL graph, overview
ms.assetid: ''
author: shkale-msft
ms.author: shkale
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2017||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 79ffa64e3a567a219e192c9108b04e2c96efaf5c
ms.sourcegitcommit: 442fbe1655d629ecef273b02fae1beb2455a762e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93235622"
---
# <a name="graph-processing-with-sql-server-and-azure-sql-database"></a>SQL Server 和 Azure SQL 数据库中的图形处理
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供图形数据库功能，以便为多对多关系建模。 Graph 关系集成到中 [!INCLUDE[tsql-md](../../includes/tsql-md.md)] ，并获得了使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 作为基础数据库管理系统的好处。


## <a name="what-is-a-graph-database"></a>什么是图形数据库？  
图数据库是节点（或顶点）和边缘（或关系）的集合。 节点表示实体（例如，某个人或组织），边缘表示该实体连接的两个节点之间的关系（例如，爱好或朋友）。 节点和边缘都可能具有与其相关联的属性。 下面是使图形数据库独一无二的某些功能：  
-    边缘或关系是图形数据库中的第一类实体，可以带有关联的特性或属性。 
-    单个边缘可以灵活连接图形数据库中的多个节点。
-    可以轻松表达模式匹配和多跃点导航查询。
-    可以轻松表达传递闭包和多态查询。

## <a name="when-to-use-a-graph-database"></a>何时使用图形数据库

关系数据库可以实现图形数据库的任何内容。 但是，图形数据库可以简化特定类型的查询。 此外，通过特定的优化，某些查询的性能可能更好。 选择关系数据库还是图形数据库取决于以下因素：  
-    您的应用程序具有分层数据。 HierarchyID 数据类型可用于实现层次结构，但有一些限制。 例如，它不允许存储节点的多个父项。
-    您的应用程序具有复杂的多对多关系;随着应用程序的发展，会添加新的关系。
-    需要分析互联的数据和关系。

## <a name="graph-features-introduced-in-sssqlv14"></a>图形功能在中引入 [!INCLUDE[sssqlv14](../../includes/sssqlv14-md.md)] 
我们开始将图形扩展添加到 SQL Server，以便更轻松地存储和查询图形数据。 第一版中引入了以下功能。 


### <a name="create-graph-objects"></a>创建图形对象
[!INCLUDE[tsql-md](../../includes/tsql-md.md)] 扩展将允许用户创建节点或边界表。 节点和边缘都可以有与之关联的属性。 由于，节点和边缘以表的形式存储，因此在节点或边界表上支持对关系表支持的所有操作。 以下是示例：  

```   
CREATE TABLE Person (ID INTEGER PRIMARY KEY, Name VARCHAR(100), Age INT) AS NODE;
CREATE TABLE friends (StartDate date) AS EDGE;
```   

![显示节点和边缘的关系图以表的形式存储。](../../relational-databases/graphs/media/person-friends-tables.png "人员节点和好友边缘表")  
节点和边缘作为表存储  

### <a name="query-language-extensions"></a>查询语言扩展  
`MATCH`引入 New 子句是为了支持模式匹配和通过图形的多跳点导航。 `MATCH`函数使用 ASCII 艺术样式语法进行模式匹配。 例如：  

```   
-- Find friends of John
SELECT Person2.Name 
FROM Person Person1, Friends, Person Person2
WHERE MATCH(Person1-(Friends)->Person2)
AND Person1.Name = 'John';
```   
 
### <a name="fully-integrated-in-ssnoversion-engine"></a>完全集成在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 引擎中 
图扩展已完全集成到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 引擎中。 使用相同的存储引擎、元数据、查询处理器等来存储和查询图形数据。 在单个查询中跨关系图和关系数据进行查询。 结合了图形功能和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 列存储、HA、R services 等其他技术。SQL graph 数据库还支持中提供的所有安全性和符合性功能 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。
 
### <a name="tooling-and-ecosystem"></a>工具和生态系统

受益于现有的工具和提供的生态系统 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 诸如备份和还原、导入和导出等工具，BCP 只需使用即可。 其他工具或服务（如 SSIS、SSRS 或 Power BI）将使用图形表，就像它们处理关系表一样。

## <a name="edge-constraints"></a>边缘约束
边缘约束在图形边缘表中定义，是一对节点表 (s，) 给定的边缘类型可以连接。 这样，用户便可以更好地控制其图形架构。 使用边缘约束的帮助，用户可以限制允许给定边缘连接的节点类型。 

若要了解有关如何创建和使用边缘约束的详细信息，请参阅 [边缘约束](../../relational-databases/tables/graph-edge-constraints.md)

## <a name="merge-dml"></a>合并 DML 
[MERGE](../../t-sql/statements/merge-transact-sql.md)语句基于与源表的联接结果对目标表执行插入、更新或删除操作。 例如，可以根据目标表与源表之间的差异，在目标表中插入、更新或删除行，从而同步两个表。 在 MERGE 语句中使用 MATCH 谓词现在在 Azure SQL 数据库和 SQL Server vNext 上受支持。 也就是说，现在可以通过使用 MATCH 谓词在单个语句中指定关系图关系（而不是单独的 INSERT/UPDATE/DELETE 语句），将当前图形数据 (节点或边缘表) 与新数据合并。

若要了解有关如何在合并 DML 中使用 match 的详细信息，请参阅 [Merge 语句](../../t-sql/statements/merge-transact-sql.md)

## <a name="shortest-path"></a>最短路径
[SHORTEST_PATH](./sql-graph-shortest-path.md)函数查找关系图中任意2个节点之间的最短路径，或从给定节点开始到关系图中的所有其他节点。 最短路径还可用于在图形中查找传递的闭包或任意长度的遍历。 

 ## <a name="next-steps"></a>后续步骤  
阅读 [SQL Graph 数据库体系结构](./sql-graph-architecture.md)
   


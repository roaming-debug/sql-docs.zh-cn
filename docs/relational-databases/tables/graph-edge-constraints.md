---
description: 图形边缘约束
title: 图形边缘约束 | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- edge constraints [SQL Server]
- CONNECTION constraints
- edge constraints [Azure SQL Database]
- graph edge constraints
- SQL Graph
author: shkale-msft
ms.author: shkale
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: c74b5d0f91e1a6bdcf54df1bf00e60d4ff7a72cd
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97490097"
---
# <a name="edge-constraints"></a>边缘约束

[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]

边缘约束可用于对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 图形数据库中的边缘表强制执行数据完整性和特定语义。

## <a name="edge-constraints"></a><a name="Connection"></a> 边缘约束

在图形功能的第一个版本中，边缘表没有对边缘的终结点强制执行任何操作。 也就是说，无论何种类型，图形数据库中的边缘都可以将任何节点连接到任何其他节点。

此版本引入了边缘约束，使用户能够向其边缘表添加约束，从而强制执行特定语义，同时保持数据完整性。 将新的边缘添加到具有边缘约束的边缘表时，[!INCLUDE[ssDE](../../includes/ssde-md.md)] 强制使边缘尝试连接的节点存在于正确的节点表中。 如果节点仍被边缘引用，则还要确保不会删除该节点。

### <a name="edge-constraint-clauses"></a>边缘约束子句

每个边缘约束都由一个或多个边缘约束子句组成。 边缘约束子句是给定边缘可以连接的 FROM 和 TO 节点对。

假设图形中有 `Product` 和 `Customer` 节点，并使用 `Bought` 边缘连接这些节点。 边缘约束子句指定 FROM 和 TO 节点对以及边缘的方向。 在这种情况下，边缘约束子句将为 `Customer` TO `Product`。 也就是说，将允许插入范围从 `Customer` 到 `Product` 的 `Bought`。 尝试插入范围从 `Product` 到 `Customer` 的边缘失败。

- 边缘约束子句包含对其强制执行边缘约束的 FROM 和 TO 节点对表。
- 用户可以为每个边缘约束指定多个边缘约束子句，这些子句将应用为析取。
- 如果在单个边缘表上创建多个边缘约束，则边缘必须满足允许的所有约束。

### <a name="indexes-on-edge-constraints"></a>边缘约束的索引

创建边缘约束不会自动在边缘表中的 `$from_id` 和 `$to_id` 列上创建相应的索引。 如果拥有点查找查询或 OLTP 工作负载，建议在“`$from_id`，`$to_id`对上手动创建索引。

### <a name="on-delete-referential-actions-on-edge-constraints"></a>ON 删除边缘约束上的引用操作
使用边缘约束上的级联操作，用户可以定义当用户删除给定边缘连接到的节点时，数据库引擎所采取的操作。 可以定义以下引用操作：  
不执行任何操作     
尝试删除具有连接边缘的节点时，数据库引擎将引发错误。  

级联     
从数据库中删除某个节点时，会删除连接边缘。  

## <a name="working-with-edge-constraints"></a>使用边缘约束

可以使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 定义 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的边缘约束。 边缘约束只能在图形边缘表上进行定义。 若要创建、删除或修改边缘约束，必须对表拥有 ALTER  权限。

### <a name="create-edge-constraints"></a>创建边缘约束

下面几个示例展示了如何对新表或现有表创建边缘约束

#### <a name="to-create-an-edge-constraint-on-a-new-edge-table"></a>在新边缘表上创建边缘约束

下面的示例对“bought”  边缘表创建边缘约束。

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      ,CustomerName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      ,ProductName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
         ,CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product) ON DELETE NO ACTION
   )
   AS EDGE;
   ```

#### <a name="defining-referential-actions-on-a-new-edge-table"></a>定义对新边缘表的引用操作 

下面的示例对“bought”边缘表创建边缘约束并定义删除级联引用操作  。 

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      ,CustomerName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      ,ProductName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
         ,CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product) ON DELETE CASCADE
   )
   AS EDGE;
   ```

#### <a name="to-add-edge-constraint-to-an-existing-edge-table"></a>将边缘约束添加到现有边缘表的具体步骤

下面的示例使用 ALTER TABLE 将边缘约束添加到“bought”  边缘表。

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY,
      , CustomerName VARCHAR(100)
   )
   AS NODE;
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
   )
   AS EDGE;
GO
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT1 CONNECTION (Customer TO Product);
```  

#### <a name="create-a-new-edge-constraint-on-existing-edge-table-with-additional-edge-constraint-clauses"></a>对现有边缘表新建边缘约束（其中包含附加边缘约束子句）

下面的示例使用 `ALTER TABLE` 命令，将包含附加边缘约束子句的新边缘约束添加到“bought”  边缘表。
  
```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Supplier
   (
      ID INTEGER PRIMARY KEY
      , SupplierName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
      , CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product)
   )
   AS EDGE;
-- Drop the existing edge constraint first and then create a new one.
ALTER TABLE bought DROP CONSTRAINT EC_BOUGHT;
GO
-- User ALTER TABLE to create a new edge constraint.
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT1 CONNECTION (Customer TO Product, Supplier TO Product);
```  

在上面的示例中，EC_BOUGHT1  约束中有两个边缘约束子句，一个用于将“Customer”  连接到“Product”  ，另一个用于将“Supplier”  连接到“Product”  。 这两个子句都可应用于析取。 即，给定的边缘必须满足这两个子句之一，才能在边缘表中使用。

#### <a name="creating-a-new-edge-constraint-on-existing-edge-table-with-new-edge-constraint-clause"></a>对现有边缘表新建边缘约束（其中包含新边缘约束子句）

下面的示例使用 `ALTER TABLE` 命令，将包含新边缘约束子句的新边缘约束添加到“bought”  边缘表。
  
 ```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Supplier
   (
      ID INTEGER PRIMARY KEY
      , SupplierName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT,
         CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product)
   )
   AS EDGE;
GO
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT1 CONNECTION (Supplier TO Product);
 ```  

上面的示例对“bought”  边缘表单独创建了两个边缘约束：EC_BOUGHT  和 EC_BOUGHT1  。 这两个边缘约束都具有不同的边缘约束子句。 如果一个边缘表在其上具有多个边缘约束，则给定的边缘表必须满足所有  边缘约束，才能在边缘表中使用它。 因为此处没有能够同时满足 EC_BOUGHT  和 EC_BOUGHT1  的任何边缘，所以购买  边缘表必须保持为空。

为了在此边缘表中成功实现插入操作，必须删除其中的一个边缘约束，或必须将这两个边缘约束全部删除，且应创建一个新的边缘约束，该约束中包含这两个边缘约束子句。

```sql
-- Drop the existing edge constraint first and then create a new one.
ALTER TABLE bought DROP CONSTRAINT EC_BOUGHT;
GO
ALTER TABLE bought DROP CONSTRAINT EC_BOUGHT1;
GO
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT_NEW CONNECTION (Customer TO Product, Supplier TO Product);
GO
```  

为了能够存在于“bought”  边缘中，给定边缘必须满足 EC_BOUGHT_NEW  约束中的两个边缘约束子句之一。 因此，将允许使用尝试将有效的客户  连接到产品  或将供应商  连接到产品  节点的任何边缘。

### <a name="delete-edge-constraints"></a>删除边缘约束

下面的示例先标识边缘约束名称，再删除边缘约束。  
  
```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   ) AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
      , CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product)
    )
    AS EDGE;
GO
-- Return the name of edge constraint.
SELECT name  
   FROM sys.edge_constraints  
   WHERE type = 'EC' AND parent_object_id = OBJECT_ID('bought');  
GO  
-- Delete the primary key constraint.  
ALTER TABLE bought
DROP CONSTRAINT EC_BOUGHT;
```  

### <a name="modify-edge-constraints"></a>修改边缘约束

若要使用 Transact-SQL 修改边缘约束，必须首先删除现有的边缘约束，然后用新定义重新创建。


### <a name="view-edge-constraints"></a>查看边缘约束

[!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。

此实例返回 tempdb 数据库中的边缘表 `bought` 的所有边缘约束及其属性。  

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Supplier
   (
      ID INTEGER PRIMARY KEY
      , SupplierName VARCHAR(100)
   )
   AS NODE;
   GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;

-- CREATE edge table with edge constraints.
CREATE TABLE bought
   (
      PurchaseCount INT
      , CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product, Supplier TO Product)
   )
   AS EDGE;

-- Query sys.edge_constraints and sys.edge_constraint_clauses to view
-- edge constraint properties.
SELECT
   EC.name AS edge_constraint_name
   , OBJECT_NAME(EC.parent_object_id) AS edge_table_name
   , OBJECT_NAME(ECC.from_object_id) AS from_node_table_name
   , OBJECT_NAME(ECC.to_object_id) AS to_node_table_name
   , is_disabled
   , is_not_trusted
FROM sys.edge_constraints EC
   INNER JOIN sys.edge_constraint_clauses ECC
   ON EC.object_id = ECC.object_id
WHERE EC.parent_object_id = object_id('bought');
```  

## <a name="related-tasks"></a>相关任务

[CREATE TABLE（SQL 图形）](../../t-sql/statements/create-table-sql-graph.md)  
[ALTER TABLE table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)  

若要了解 SQL Server 中的图形技术，请参阅[使用 SQL Server 和 Azure SQL 数据库进行图形处理](../graphs/sql-graph-overview.md)。

---
description: 唯一约束和 CHECK 约束
title: 唯一约束和 CHECK 约束 | Microsoft Docs
ms.custom: ''
ms.date: 06/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- constraints [SQL Server], Visual Database Tools
- Visual Database Tools [SQL Server], constraints
ms.assetid: 637098af-2567-48f8-90f4-b41df059833e
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: efd7fb070a81715b88d6949fac47c972bdd0643d
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88418961"
---
# <a name="unique-constraints-and-check-constraints"></a>唯一约束和 CHECK 约束
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  UNIQUE 约束和 CHECK 约束是可用于在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中强制数据完整性的两种类型的约束。 这些是重要的数据库对象。  
  
 本主题包含以下各节：  
  
 [UNIQUE 约束](#Unique)  
  
 [CHECK 约束](#Check)  
  
 [相关任务](#Tasks)  
  
##  <a name="unique-constraints"></a><a name="Unique"></a> UNIQUE 约束  
 约束是 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 为您强制执行的规则。 例如，您可以使用 UNIQUE 约束确保在非主键列中不输入重复的值。 尽管 UNIQUE 约束和 PRIMARY KEY 约束都强制唯一性，但想要强制一列或多列组合（不是主键）的唯一性时应使用 UNIQUE 约束而不是 PRIMARY KEY 约束。  
  
 UNIQUE 约束允许 NULL 值，这一点与 PRIMARY KEY 约束不同。 不过，当与参与 UNIQUE 约束的任何值一起使用时，每列只允许一个空值。 FOREIGN KEY 约束可以引用 UNIQUE 约束。  
  
 默认情况下，向表中的现有列添加 UNIQUE 约束后， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将检查列中的现有数据，以确保所有值都是唯一的。 如果向含有重复值的列添加 UNIQUE 约束， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将返回错误消息，并且不添加约束。  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将自动创建 UNIQUE 索引来强制执行 UNIQUE 约束的唯一性要求。 因此，如果试图插入重复行， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将返回错误消息，说明该操作违反了 UNIQUE 约束，不能将该行添加到表中。 除非显式指定了聚集索引，否则，默认情况下将创建唯一的非聚集索引以强制执行 UNIQUE 约束。  
  
##  <a name="check-constraints"></a><a name="Check"></a> CHECK 约束  
 通过限制一个或多个列可接受的值，CHECK 约束可以强制域完整性。 可以通过任何基于逻辑运算符返回 TRUE 或 FALSE 的逻辑（布尔）表达式创建 CHECK 约束。 例如，可以通过创建 CHECK 约束将 **salary** 列中值的范围限制为从 $15,000 到 $100,000 之间的数据。 这可防止薪金超出常规薪金范围之外。 逻辑表达式将如下： `salary >= 15000 AND salary <= 100000`。  
  
 可以将多个 CHECK 约束应用于单个列。 还可以通过在表级创建 CHECK 约束，将一个 CHECK 约束应用于多个列。 例如，多列 CHECK 约束可用于确认 **country_region** 列值为 **USA** 的任意行是否在 **state** 列中还有一个由两个字符构成的值。 这使得在一个位置可以同时检查多个条件。  
  
 CHECK 约束类似于 FOREIGN KEY 约束，因为可以控制放入列中的值。 但是，它们在确定有效值的方式上有所不同：FOREIGN KEY 约束从其他表获得有效值列表，而 CHECK 约束通过逻辑表达式确定有效值。  
  
> [!CAUTION]  
>  包括隐式或显式数据类型转换的约束可能会导致某些操作失败。 例如，为表定义的作为分区切换的源的此类约束可能会导致 ALTER TABLE...SWITCH 操作失败。 在约束定义中避免数据类型转换。  
  
### <a name="limitations-of-check-constraints"></a>CHECK 约束的限制  
 CHECK 约束不接受计算结果为 FALSE 的值。 因为空值的计算结果为 UNKNOWN，所以表达式中存在这些值可能会覆盖约束。 例如，假设对指定 **MyColumn** 只能包含值 10（即 **MyColumn=10** ）的 **int** 列 **MyColumn** 应用一个约束。 如果将值 NULL 插入到 **MyColumn**， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将插入 NULL 且不返回错误。  
  
 如果 CHECK 约束检查的条件对于表中的任何行都不是 FALSE，它将返回 TRUE。 CHECK 约束在行级执行。 如果刚创建的表没有任何行，则此表的任何 CHECK 约束都视为有效。 这种情况可能会产生意外结果，如下面的示例所示。  
  
```  
CREATE TABLE CheckTbl (col1 int, col2 int);  
GO  
CREATE FUNCTION CheckFnctn()  
RETURNS int  
AS   
BEGIN  
   DECLARE @retval int  
   SELECT @retval = COUNT(*) FROM CheckTbl  
   RETURN @retval  
END;  
GO  
ALTER TABLE CheckTbl  
ADD CONSTRAINT chkRowCount CHECK (dbo.CheckFnctn() >= 1 );  
GO  
```  
  
 添加的 `CHECK` 约束指定表 `CheckTbl`必须至少包含一行。 但是，因为表中不包含任何可供检查此约束的条件的行，所以 ALTER TABLE 语句将成功。  
  
 执行 DELETE 语句时不验证 CHECK 约束。 因此，使用特定类型的 CHECK 约束对表执行 DELETE 语句时可能会产生意外结果。 例如，假设对表 `CheckTbl`执行下列语句。  
  
```  
INSERT INTO CheckTbl VALUES (10, 10);  
GO  
DELETE CheckTbl WHERE col1 = 10;  
```  
  
 即使 `DELETE` 约束指定表 `CHECK` 必须至少包含 `CheckTbl` 行， `1` 语句也会成功。  
  
##  <a name="related-tasks"></a><a name="Tasks"></a> 相关任务  
  
> [!NOTE]  
>  如果表是为复制发布的，则必须使用 Transact-SQL 语句 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) 或 SQL Server 管理对象 (SMO) 对架构进行更改。 使用表设计器或数据库关系图设计器更改架构后，会尝试删除并重新创建表。 由于您不能删除已发布的对象，因此架构更改将失败。  
  
|任务|主题|  
|----------|-----------|  
|介绍如何创建唯一约束。|[创建唯一约束](../../relational-databases/tables/create-unique-constraints.md)|  
|介绍如何修改唯一约束。|[修改唯一约束](../../relational-databases/tables/modify-unique-constraints.md)|  
|介绍如何删除唯一约束。|[删除唯一约束](../../relational-databases/tables/delete-unique-constraints.md)|  
|介绍当复制代理在表中插入或更新数据时如何禁用 CHECK 约束。|[对复制禁用 CHECK 约束](../../relational-databases/tables/disable-check-constraints-for-replication.md)|  
|介绍在表中添加、更新或删除数据时如何禁用 CHECK 约束。|[对 INSERT 和 UPDATE 语句禁用 CHECK 约束](../../relational-databases/tables/disable-check-constraints-with-insert-and-update-statements.md)|  
|介绍如何更改约束表达式或更改对特定条件启用或禁用约束的选项。|[修改 CHECK 约束](../../relational-databases/tables/modify-check-constraints.md)|  
|介绍如何删除 CHECK 约束。|[删除 CHECK 约束](../../relational-databases/tables/delete-check-constraints.md)|  
|介绍如何查看 CHECK 约束的属性。|[唯一约束和 CHECK 约束](../../relational-databases/tables/unique-constraints-and-check-constraints.md)|  
  
  

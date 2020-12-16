---
description: 使用插入的和删除的表
title: 使用插入的和删除的表 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- inserted tables
- UPDATE statement [SQL Server], DML triggers
- DELETE statement [SQL Server], DML triggers
- INSTEAD OF triggers
- deleted tables
- INSERT statement [SQL Server], DML triggers
- DML triggers, deleted or inserted tables
ms.assetid: ed84567f-7b91-4b44-b5b2-c400bda4590d
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bd89afe79f1e6cf7a5745b04378c1b403142c56e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97426398"
---
# <a name="use-the-inserted-and-deleted-tables"></a>使用插入的和删除的表
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  DML 触发器语句使用两种特殊的表：删除的表和插入的表。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会自动创建和管理这两种表。 您可以使用这两种驻留内存的临时表来测试特定数据修改的影响以及设置 DML 触发器操作条件。 但不能直接修改表中的数据或对表执行数据定义语言 (DDL) 操作，例如 CREATE INDEX。  
  
 在 DML 触发器中，inserted 和 deleted 表主要用于执行以下操作：  
  
-   扩展表之间的引用完整性。  
  
-   在以视图为基础的基表中插入或更新数据。  
  
-   检查错误并采取相应的措施。  
  
-   找出数据修改前后表的状态差异并基于该差异采取相应的措施。  
  
 删除的表用于存储 DELETE 和 UPDATE 语句所影响的行的副本。 在执行 DELETE 或 UPDATE 语句的过程中，行从触发器表中删除，并传输到删除的表中。 删除的表和触发器表通常没有相同的行。  
  
 插入的表用于存储 INSERT 和 UPDATE 语句所影响的行的副本。 在执行插入或更新事务过程中，新行会同时添加到 inserted 表和触发器表中。 插入的表中的行是触发器表中的新行的副本。  
  
 更新事务类似于在删除操作之后执行插入操作；首先，旧行被复制到删除的表中，然后，新行被复制到触发器表和插入的表中。  
  
 在设置触发器条件时，应使用激发触发器的操作相应的插入的和删除的表。 尽管在测试 INSERT 时引用删除的表或在测试 DELETE 时引用插入的表不会导致任何错误，但在这些情况下，这些触发器测试表将不包含任何行。  
  
> [!NOTE]  
>  如果触发器操作取决于数据修改所影响的行数，则应对多行数据修改（基于 SELECT 语句的 INSERT、DELETE 或 UPDATE）使用测试（例如检查 @@ROWCOUNT），然后采取相应的措施。 有关详细信息，请参阅 [创建 DML 触发器以处理多行数据](../../relational-databases/triggers/create-dml-triggers-to-handle-multiple-rows-of-data.md)。
  
 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 不允许在 AFTER 触发器的插入和删除的表中引用 **text**、 **ntext** 或 **image** 列。 但会包括这些数据类型，这只是为了向后兼容。 大量数据的首选存储是使用 **varchar(max)** 、 **nvarchar(max)** 和 **varbinary(max)** 数据类型。 AFTER 和 INSTEAD OF 触发器均支持插入和删除的表中的 **varchar(max)** 、 **nvarchar(max)** 和 **varbinary(max)** 数据。 有关详细信息，请参阅 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)。  
  
 **在触发器中使用插入的表以强制实施业务规则的示例**  
  
 由于 CHECK 约束只能引用定义了列级或表级约束的列，表间的任何约束（在本例中是业务规则）都必须定义为触发器。  
  
 以下示例将创建一个 DML 触发器。 如果有人试图将一个新采购订单插入到 `PurchaseOrderHeader` 表中，此触发器将进行检查以确保供应商具有良好的信用等级。 若要获取与刚插入的采购订单对应的供应商信用等级，必须引用 `Vendor` 表并将其与插入的表联接。 如果信用等级太低，则显示信息，并且不执行该插入操作。
  
 [!code-sql[TriggerDDL#CreateTrigger3](../../relational-databases/triggers/codesnippet/tsql/use-the-inserted-and-del_1.sql)]  
  
## <a name="using-the-inserted-and-deleted-tables-in-instead-of-triggers"></a>在 INSTEAD OF 触发器中使用插入的和删除的表  
 传递给为表定义的 INSTEAD OF 触发器的插入的和删除的表与传递给 AFTER 触发器的插入的和删除的表遵守相同的规则。 插入的和删除的表的格式与在其上定义 INSTEAD OF 触发器的表的格式相同。 插入的和删除的表中的每一列都直接映射到基表中的列。  
  
 以下是关于引用带 INSTEAD OF 触发器的表的 INSERT 或 UPDATE 语句何时必须提供列值的规则，当引用的表不带 INSTEAD OF 触发器时也一样：  
  
-   不能为计算列或具有 **timestamp** 数据类型的列指定值。  
  
-   不能为具有 IDENTITY 属性的列指定值，除非该表的 IDENTITY_INSERT 为 ON。 当 IDENTITY_INSERT 为 ON 时，INSERT 语句必须提供一个值。  
  
-   INSERT 语句必须为所有无 DEFAULT 约束的 NOT NULL 列提供值。  
  
-   对于除计算列、标识列或 **timestamp** 列以外的任何列，任何允许空值的列或具有 DEFAULT 定义的 NOT NULL 列的值都是可选的。  
  
 当 INSERT、UPDATE 或 DELETE 语句引用具有 INSTEAD OF 触发器的视图时， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将调用该触发器，而不是对任何表采取任何直接操作。 即使插入的和删除的表中为该视图生成的信息格式不同于基表中的数据格式，触发器也必须使用插入的和删除的表中的信息来生成实现基表中请求的操作所需的任何语句。  
  
 传递给为视图定义的 INSTEAD OF 触发器的插入的和删除的表的格式与为该视图定义的 SELECT 语句的选择列表的格式一致。 例如：  
  
```  
USE AdventureWorks2012;  
GO  
CREATE VIEW dbo.EmployeeNames (BusinessEntityID, LName, FName)  
AS  
SELECT e.BusinessEntityID, p.LastName, p.FirstName  
FROM HumanResources.Employee AS e   
JOIN Person.Person AS p  
ON e.BusinessEntityID = p.BusinessEntityID;  
```  
  
 此视图的结果集有三列：一个 **int** 列和两个 **nvarchar** 列。 传递给视图上定义的 INSTEAD OF 触发器的插入和删除的表也有一个名为 **的** int `BusinessEntityID`列、一个名为 **的** nvarchar `LName`列和一个名为 **的** nvarchar `FName`列。  
  
 视图的选择列表还可以包含不直接映射到单个基表列的表达式。 一些视图表达式（例如常量调用或函数调用）可能不引用任何列，并且这些表达式会被忽略。 复杂的表达式会引用多个列，但在插入的和删除的表中，每个插入的行仅有一个相应的值。 如果视图中的简单表达式引用包含复杂表达式的计算列，则这些简单表达式也有同样的问题。 视图上的 INSTEAD OF 触发器必须处理这些类型的表达式。  
  
  

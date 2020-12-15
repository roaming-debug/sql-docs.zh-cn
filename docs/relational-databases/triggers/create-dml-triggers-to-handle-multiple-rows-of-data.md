---
description: 创建 DML 触发器以处理多行数据
title: 创建 DML 触发器以处理多行数据
ms.custom: seo-dt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- multiple row DML triggers
- UPDATE statement [SQL Server], DML triggers
- DELETE statement [SQL Server], DML triggers
- multirow DML triggers [SQL Server]
- INSERT statement [SQL Server], DML triggers
- DML triggers, multirow
ms.assetid: d476c124-596b-4b27-a883-812b6b50a735
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5738b2194142b83130a22c851c23e53495306257
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97403232"
---
# <a name="create-dml-triggers-to-handle-multiple-rows-of-data"></a>创建 DML 触发器以处理多行数据
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  为 DML 触发器编写代码时，请考虑导致触发器激发的语句可能是影响多行数据（而不是单行）的单个语句。 这对于 UPDATE 和 DELETE 触发器很常见，因为这些语句经常影响多行。 而这对于 INSERT 触发器比较少见，因为基本 INSERT 语句仅添加单行。 但是，由于 INSERT 触发器可以通过 INSERT INTO (*table_name*) SELECT 语句触发，因此插入多行可能导致调用单个触发器。  
  
 在下列情况下关于多行的注意事项尤为重要：DML 触发器的功能自动重新计算一个表中的汇总值，并将结果存储在另一个表中以继续进行计数。  
  
> [!NOTE]  
>  我们建议不要在触发器中使用游标，因为它们可能会降低性能。 若要设计一个影响多行的触发器，请使用基于行集的逻辑，而不要使用游标。  
  
## <a name="examples"></a>示例  
 下列示例中的 DML 触发器用于在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库的另一个表中存储某列的运行总计。  
  
### <a name="a-storing-a-running-total-for-a-single-row-insert"></a>A. 存储单行插入的运行总计  
 第一种 DML 触发器在一行数据加载到 `PurchaseOrderDetail` 表中时适合于单行插入。 INSERT 语句激发 DML 触发器，新行在触发器执行期间加载到 **插入的** 表中。 `UPDATE` 语句读取该行的 `LineTotal` 列值，并将该值与 `SubTotal` 表的 `PurchaseOrderHeader` 列中的现有值相加。 `WHERE` 子句确保 `PurchaseOrderDetail` 表中的更新行与 `PurchaseOrderID` 插入的 **表中** 行相匹配。  
  
```  
-- Trigger is valid for single-row inserts.  
USE AdventureWorks2012;  
GO  
CREATE TRIGGER NewPODetail  
ON Purchasing.PurchaseOrderDetail  
AFTER INSERT AS  
   UPDATE PurchaseOrderHeader  
   SET SubTotal = SubTotal + LineTotal  
   FROM inserted  
   WHERE PurchaseOrderHeader.PurchaseOrderID = inserted.PurchaseOrderID ;  
```  
  
### <a name="b-storing-a-running-total-for-a-multirow-or-single-row-insert"></a>B. 存储多行或单行插入的运行总计  
 对于多行插入，示例 A 中的 DML 触发器可能不会正确运行；位于 UPDATE 语句 (`SubTotal` + `LineTotal`) 中赋值表达式右侧的表达式只能是单个值，而非值列表。 因此，该触发器的作用是检索 **插入的** 表中任意一行的值，并将该值与 `SubTotal` 表中的现有 `PurchaseOrderHeader` 值相加，以获得特定 `PurchaseOrderID` 值。 如果某个 `PurchaseOrderID` 值在 **插入的** 表中出现多次，则此操作可能无法达到预期效果。  
  
 若要正确更新 `PurchaseOrderHeader` 表，必须允许对 **插入的** 表中的多行使用触发器。 可以通过使用 `SUM` 函数达到此目的，该函数计算每个 `LineTotal` 的 **插入的** 表中许多行的总 `PurchaseOrderID`。 `SUM` 函数包含在相关子查询（括号中的 `SELECT` 语句）中。 此子查询将为 `PurchaseOrderID` 插入的 **表中的每个** 返回一个值，该值与 `PurchaseOrderID` 表中的 `PurchaseOrderHeader` 匹配或相关。  
  
```  
-- Trigger is valid for multirow and single-row inserts.  
USE AdventureWorks2012;  
GO  
CREATE TRIGGER NewPODetail2  
ON Purchasing.PurchaseOrderDetail  
AFTER INSERT AS  
   UPDATE Purchasing.PurchaseOrderHeader  
   SET SubTotal = SubTotal +   
      (SELECT SUM(LineTotal)  
      FROM inserted  
      WHERE PurchaseOrderHeader.PurchaseOrderID  
       = inserted.PurchaseOrderID)  
   WHERE PurchaseOrderHeader.PurchaseOrderID IN  
      (SELECT PurchaseOrderID FROM inserted);  
```  
  
 此触发器还适合于单行插入； `LineTotal` 值列的和为单行的和。 但是，对于此触发器，相关子查询和 `IN` 子句中使用的 `WHERE` 运算符需要从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中进行其他处理。 这对于单行插入来说，是不必要的。  
  
### <a name="c-storing-a-running-total-based-on-the-type-of-insert"></a>C. 基于插入类型存储运行总计  
 可以更改触发器以针对不同行数使用最优方法。 例如，可以在触发器逻辑中使用 `@@ROWCOUNT` 函数来区分单行插入和多行插入。  
  
```  
-- Trigger valid for multirow and single row inserts  
-- and optimal for single row inserts.  
USE AdventureWorks2012;  
GO  
CREATE TRIGGER NewPODetail3  
ON Purchasing.PurchaseOrderDetail  
FOR INSERT AS  
IF @@ROWCOUNT = 1  
BEGIN  
   UPDATE Purchasing.PurchaseOrderHeader  
   SET SubTotal = SubTotal + LineTotal  
   FROM inserted  
   WHERE PurchaseOrderHeader.PurchaseOrderID = inserted.PurchaseOrderID  
END  
ELSE  
BEGIN  
      UPDATE Purchasing.PurchaseOrderHeader  
   SET SubTotal = SubTotal +   
      (SELECT SUM(LineTotal)  
      FROM inserted  
      WHERE PurchaseOrderHeader.PurchaseOrderID  
       = inserted.PurchaseOrderID)  
   WHERE PurchaseOrderHeader.PurchaseOrderID IN  
      (SELECT PurchaseOrderID FROM inserted)  
END;  
```  
  
## <a name="see-also"></a>另请参阅  
 [DML 触发器](../../relational-databases/triggers/dml-triggers.md)  
  
  

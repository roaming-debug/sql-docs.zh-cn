---
description: MSSQLSERVER_4186
title: MSSQLSERVER_4186 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 4186 (Database Engine error)
ms.assetid: 1ae88554-f291-45bc-a186-6f41d9cd0fca
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 32df0694f8398bc39cb9e306b3e3da7905b1e212
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185606"
---
# <a name="mssqlserver_4186"></a>MSSQLSERVER_4186
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|4186|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称||  
|消息正文|无法在 OUTPUT 子句中引用列 '%ls.%.*ls'，因为该列的定义中包含一个子查询或者引用一个执行用户或系统数据访问的函数。 默认情况下，如果函数未绑定到架构，则会认为该函数执行数据访问。 请考虑从列定义中删除子查询或函数，或者从 OUTPUT 子句中删除该列。|  
  
## <a name="explanation"></a>说明  
为了防止出现不确定的行为，当某个列是通过下列方法之一定义时，OUTPUT 子句不能通过视图或内联表值函数引用该列：  
  
-   子查询。  
  
-   执行用户数据访问或系统数据访问或者被认为执行此种访问的用户定义函数。  
  
-   定义中包含执行用户数据访问或系统数据访问的用户定义函数的计算列。  
  
### <a name="examples"></a>示例  
**子查询定义的视图列**  
  
以下示例创建使用选择列表中的子查询定义 `State` 列的视图。 然后，UPDATE 语句在 OUTPUT 子句中引用 `State` 列，并且因为选择列表中的子查询而失败。  
  
```  
USE AdventureWorks2012;  
GO  
CREATE VIEW dbo.V1  
AS  
    SELECT City,  
-- subquery to return the State name  
           (SELECT Name FROM Person.StateProvince AS sp   
            WHERE sp.StateProvinceID = a.StateProvinceID) AS State  
    FROM Person.Address AS a;  
GO  
--Reference the State column in the OUTPUT clause of an UPDATE statement  
UPDATE dbo.V1   
SET City = City + 'Test'   
OUTPUT deleted.City, deleted.State, inserted.City, inserted.State  
WHERE State = 'Texas';  
GO  
```  
  
**函数定义的视图列**  
  
以下示例创建使用选择列表中的 `dbo.ufnGetStock` 数据访问标量函数定义 `CurrentInventory` 列的视图。 然后，UPDATE 语句在 OUTPUT 子句中引用此 `CurrentInventory` 列。  
  
```  
USE AdventureWorks2012;  
GO  
CREATE VIEW Production.ReorderLevels  
AS  
    SELECT ProductID, ProductModelID, ReorderPoint,  
           dbo.ufnGetStock(ProductID) AS CurrentInventory  
    FROM Production.Product;  
GO  
  
UPDATE Production.ReorderLevels  
SET ReorderPoint += CurrentInventory  
OUTPUT deleted.ReorderPoint, deleted.CurrentInventory,  
       inserted.ReorderPoint, inserted.CurrentInventory  
WHERE ProductModelID BETWEEN 75 and 80;  
```  
  
## <a name="user-action"></a>用户操作  
若要更正 4186 错误，可以使用下列方法之一：  
  
-   使用联接（而不是子查询）定义视图或函数中的列。 例如，您可以重写 `dbo.V1` 视图，如下所示。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    CREATE VIEW dbo.V1  
    AS  
        SELECT City, sp.Name AS State  
        FROM Person.Address AS a   
        JOIN Person.StateProvince AS sp   
        ON sp.StateProvinceID = a.StateProvinceID;  
    ```  
  
-   检查用户定义函数的定义。 如果函数未执行用户数据访问或系统数据访问，请更改此函数，使其包含 WITH SCHEMABINDING 子句。  
  
-   从 OUTPUT 子句中删除列。  
  
## <a name="see-also"></a>另请参阅  
[OUTPUT 子句 (Transact-SQL)](~/t-sql/queries/output-clause-transact-sql.md)  
  

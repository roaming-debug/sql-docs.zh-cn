---
description: 在 AUTO 模式下自动格式化 JSON 输出 (SQL Server)
title: 在 AUTO 模式下自动格式化 JSON 输出
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR JSON AUTO
ms.assetid: 178a2a4e-e0f6-49b9-9895-396956d3c7d9
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7161ce97faa4d1baab514df45429592629e2518b
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96125143"
---
# <a name="format-json-output-automatically-with-auto-mode-sql-server"></a>在 AUTO 模式下自动格式化 JSON 输出 (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

若要根据 **SELECT** 语句的结构自动格式化 **FOR JSON** 子句的输出，请指定 **AUTO** 选项。  
  
指定 AUTO 选项时，将根据 SELECT 列表及其源表中的列顺序自动确定 JSON 输出的格式。 无法更改此格式。
 
替代方法是使用 PATH 选项保持对输出的控制。
-   有关 PATH 选项的详细信息，请参阅[在 PATH 模式下格式化嵌套的 JSON 输出](../../relational-databases/json/format-nested-json-output-with-path-mode-sql-server.md)。
-   有关这两个选项的概述，请参阅[使用 FOR JSON 将查询结果格式化为 JSON](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md)。

使用 **FOR JSON AUTO** 选项的查询必须具有 **FROM** 子句。  
  
下面的一些示例展示了如何使用 **AUTO** 选项指定 **FOR JSON** 子句。 建议使用 [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) 作为 JSON 查询的查询编辑器，因为它会自动格式化 JSON 结果（如本文所示），而不是显示平面字符串。
  
## <a name="examples"></a>示例

### <a name="example-1"></a>示例 1
 **查询**  
  
如果查询仅引用一个表，则 FOR JSON AUTO 子句的结果类似于 FOR JSON PATH 的结果。 在这种情况下，FOR JSON AUTO 不会创建嵌套对象。 唯一的区别是 FOR JSON AUTO 会将以点分隔的别名（例如以下示例中的 `Info.MiddleName`）输出为带有点的键，而不是输出为嵌套对象。  
  
```sql  
SELECT TOP 5   
       BusinessEntityID As Id,  
       FirstName, LastName,  
       Title As 'Info.Title',  
       MiddleName As 'Info.MiddleName'  
   FROM Person.Person  
   FOR JSON AUTO  
```  
  
 **结果**  
  
```json  
[{
    "Id": 1,
    "FirstName": "Ken",
    "LastName": "Sánchez",
    "Info.MiddleName": "J"
}, {
    "Id": 2,
    "FirstName": "Terri",
    "LastName": "Duffy",
    "Info.MiddleName": "Lee"
}, {
    "Id": 3,
    "FirstName": "Roberto",
    "LastName": "Tamburello"
}, {
    "Id": 4,
    "FirstName": "Rob",
    "LastName": "Walters"
}, {
    "Id": 5,
    "FirstName": "Gail",
    "LastName": "Erickson",
    "Info.Title": "Ms.",
    "Info.MiddleName": "A"
}]
```  

### <a name="example-2"></a>示例 2

**查询**  
  
联接表时，第一个表中的列会作为根对象的属性生成。 第二个表中的列会作为嵌套对象的属性生成。 第二个表的表名或别名（例如以下示例中的 `D`）用作嵌套数组的名称。  
  
```sql  
SELECT TOP 2 SalesOrderNumber,  
        OrderDate,  
        UnitPrice,  
        OrderQty  
FROM Sales.SalesOrderHeader H  
   INNER JOIN Sales.SalesOrderDetail D  
     ON H.SalesOrderID = D.SalesOrderID  
FOR JSON AUTO   
```  
  
**结果**  
  
```json  
[{
    "SalesOrderNumber": "SO43659",
    "OrderDate": "2011-05-31T00:00:00",
    "D": [{
        "UnitPrice": 24.99,
        "OrderQty": 1
    }]
}, {
    "SalesOrderNumber": "SO43659",
    "D": [{
        "UnitPrice": 34.40
    }, {
        "UnitPrice": 134.24,
        "OrderQty": 5
    }]
}]
```  

### <a name="example-3"></a>示例 3
 
**查询**  
你可以在 SELECT 语句中嵌套 FOR JSON PATH 子查询，而不使用 FOR JSON AUTO，如以下示例中所示。 此示例输出与上一示例相同的结果。  
  
```sql  
SELECT TOP 2  
    SalesOrderNumber,  
    OrderDate,  
    (SELECT UnitPrice, OrderQty  
      FROM Sales.SalesOrderDetail AS D  
      WHERE H.SalesOrderID = D.SalesOrderID  
     FOR JSON PATH) AS D  
FROM Sales.SalesOrderHeader AS H  
FOR JSON PATH  
```  
  
**结果**  
  
```json  
[{
    "SalesOrderNumber": "SO43659",
    "OrderDate": "2011-05-31T00:00:00",
    "D": [{
        "UnitPrice": 24.99,
        "OrderQty": 1
    }]
}, {
    "SalesOrderNumber": "SO4390",
    "D": [{
        "UnitPrice": 24.99
    }]
}]
```  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>详细了解 SQL Server 和 Azure SQL 数据库中的 JSON  
  
### <a name="microsoft-videos"></a>Microsoft 视频

有关 SQL Server 和 Azure SQL 数据库中内置 JSON 支持的视频介绍，请观看以下视频：

-   [SQL Server 2016 和 JSON 支持](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [在 SQL Server 2016 和 Azure SQL 数据库中使用 JSON](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON 充当 NoSQL 和关系环境之间的桥梁](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)

## <a name="see-also"></a>另请参阅  
 [FOR 子句 (Transact-SQL)](../../t-sql/queries/select-for-clause-transact-sql.md)  

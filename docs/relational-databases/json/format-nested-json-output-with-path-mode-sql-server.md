---
description: 在 PATH 模式下格式化嵌套 JSON 输出 (SQL Server)
title: 在 PATH 模式下格式化嵌套 JSON 输出
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
ms.assetid: 032761b0-6358-42e4-b05c-dbfd663ac881
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 719bde6741232fe06d7d7f461cb6bbf2436ed821
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478108"
---
# <a name="format-nested-json-output-with-path-mode-sql-server"></a>在 PATH 模式下格式化嵌套 JSON 输出 (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

若要保持对 FOR JSON 子句输出的完全控制，请指定 PATH 选项。  
  
借助 **PATH** 模式，你可以创建包装器对象，并嵌套复杂属性。 结果会格式化为 JSON 对象数组。  
  
替代方法是使用 AUTO 选项根据 SELECT 语句的结构自动格式化输出。
 -   有关 AUTO 选项的详细信息，请参阅[在 AUTO 模式下自动格式化 JSON 输出](../../relational-databases/json/format-json-output-automatically-with-auto-mode-sql-server.md)。
 -   有关这两个选项的概述，请参阅[使用 FOR JSON 将查询结果格式化为 JSON](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md)。
 
下面的一些示例展示了如何使用 **PATH** 选项指定 **FOR JSON** 子句。 使用以点分隔的列名称或使用嵌套查询来格式化嵌套结果，如下面的示例所示。 默认情况下，FOR JSON 输出中不包括 null 值。  建议使用 [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) 作为 JSON 查询的查询编辑器，因为它会自动格式化 JSON 结果（如本文所示），而不是显示平面字符串。

## <a name="example---dot-separated-column-names"></a>示例 - 以点分隔的列名称  
以下查询将 AdventureWorks `Person` 表的前五行格式化为 JSON。  

FOR JSON PATH 子句使用列别名或列名来确定 JSON 输出中的键名称。 如果别名中包含点，则 PATH 选项将创建嵌套对象。  

 **查询**  
  
```sql  
SELECT TOP 5   
       BusinessEntityID As Id,  
       FirstName, LastName,  
       Title As 'Info.Title',  
       MiddleName As 'Info.MiddleName'  
   FROM Person.Person  
   FOR JSON PATH   
```  
  
 **结果**  
  
```json  
[{
    "Id": 1,
    "FirstName": "Ken",
    "LastName": "Sanchez",
    "Info": {
        "MiddleName": "J"
    }
}, {
    "Id": 2,
    "FirstName": "Terri",
    "LastName": "Duffy",
    "Info": {
        "MiddleName": "Lee"
    }
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
    "Info": {
        "Title": "Ms.",
        "MiddleName": "A"
    }
}]
```  
   
## <a name="example---multiple-tables"></a>示例 - 多个表  
如果查询中引用了多个表，FOR JSON PATH 将使用列别名嵌套每个列。 以下查询将为查询中联接的每个（OrderHeader，OrderDetails）对创建一个 JSON 对象。 
  
 **查询**  
  
```sql  
SELECT TOP 2 H.SalesOrderNumber AS 'Order.Number',  
        H.OrderDate AS 'Order.Date',  
        D.UnitPrice AS 'Product.Price',  
        D.OrderQty AS 'Product.Quantity'  
FROM Sales.SalesOrderHeader H  
   INNER JOIN Sales.SalesOrderDetail D  
     ON H.SalesOrderID = D.SalesOrderID  
FOR JSON PATH   
```  
  
 **结果**  
  
```json  
[{
    "Order": {
        "Number": "SO43659",
        "Date": "2011-05-31T00:00:00"
    },
    "Product": {
        "Price": 2024.9940,
        "Quantity": 1
    }
}, {
    "Order": {
        "Number": "SO43659"
    },
    "Product": {
        "Price": 2024.9940
    }
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

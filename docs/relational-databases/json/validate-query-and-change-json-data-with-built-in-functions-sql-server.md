---
description: 使用内置函数 (SQL Server) 验证、查询和更改 JSON 数据
title: 使用内置函数验证、查询和更改 JSON 数据
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- JSON, built-in functions
- functions (JSON)
ms.assetid: 6b6c7673-d818-4fa9-8708-b4ed79cb1b41
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: cab935cd68d3d44279e23a1f542cb37b8ec61dbf
ms.sourcegitcommit: 28fecbf61ae7b53405ca378e2f5f90badb1a296a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2020
ms.locfileid: "96595148"
---
# <a name="validate-query-and-change-json-data-with-built-in-functions-sql-server"></a>使用内置函数 (SQL Server) 验证、查询和更改 JSON 数据
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

JSON 的内置支持包括本主题简要介绍的下列内置函数。  
  
-   [ISJSON](#ISJSON) 测试字符串是否包含有效 JSON。  
  
-   [JSON_VALUE](#VALUE) 从 JSON 字符串中提取标量值。  
  
-   [JSON_QUERY](#QUERY) 从 JSON 字符串中提取对象或数组。  
  
-   [JSON_MODIFY](#MODIFY) 更新 JSON 字符串中属性的值，并返回已更新的 JSON 字符串。  
 
## <a name="json-text-for-the-examples-on-this-page"></a>此页上的示例 JSON 文本

此页上的示例使用与以下示例中所示内容类似的 JSON 文本：

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller",
         "givenName": "Lisa",
         "gender": "female",
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

此 JSON 文档包含嵌套的复杂元素，存储在下面的示例表中：

```sql
CREATE TABLE Families (
   id int identity constraint PK_JSON_ID primary key,
   doc nvarchar(max)
)
``` 

##  <a name="validate-json-text-by-using-the-isjson-function"></a><a name="ISJSON"></a> 使用 ISJSON 函数验证 JSON 文本  
 **ISJSON** 函数测试字符串是否包含有效 JSON。  
  
下面的示例将返回 JSON 列包含有效 JSON 文本的行。 请注意，如果没有显式 JSON 约束，则可在 NVARCHAR 列中输入任意文本：  
  
```sql  
SELECT *
FROM Families
WHERE ISJSON(doc) > 0 
```  

有关详细信息，请参阅 [ISJSON (Transact-SQL)](../../t-sql/functions/isjson-transact-sql.md)。  
  
##  <a name="extract-a-value-from-json-text-by-using-the-json_value-function"></a><a name="VALUE"></a> 使用 JSON_VALUE 函数从 JSON 文本中提取值  
**JSON_VALUE** 函数从 JSON 字符串中提取标量值。 下面的查询将返回其中 `id` JSON 字段与值 `AndersenFamily` 一致的文档，按 `city` 和 `state` JSON 字段排序：

```sql  
SELECT JSON_VALUE(f.doc, '$.id')  AS Name, 
       JSON_VALUE(f.doc, '$.address.city') AS City,
       JSON_VALUE(f.doc, '$.address.county') AS County
FROM Families f 
WHERE JSON_VALUE(f.doc, '$.id') = N'AndersenFamily'
ORDER BY JSON_VALUE(f.doc, '$.address.city') DESC, JSON_VALUE(f.doc, '$.address.state') ASC
```  

此查询结果显示在下表中：

| 名称 | 城市 | 县 |
| --- | --- | --- |
| AndersenFamily | NY | 曼哈顿 |

有关详细信息，请参阅 [JSON_VALUE (Transact-SQL)](../../t-sql/functions/json-value-transact-sql.md)。  
  
##  <a name="extract-an-object-or-an-array-from-json-text-by-using-the-json_query-function"></a><a name="QUERY"></a> 使用 JSON_QUERY 函数从 JSON 文本中提取对象或数组  

**JSON_QUERY** 函数从 JSON 字符串中提取对象或数组。 下面的示例演示了如何在查询结果中返回 JSON 片段。  
  
```sql
SELECT JSON_QUERY(f.doc, '$.address') AS Address,
       JSON_QUERY(f.doc, '$.parents') AS Parents,
       JSON_QUERY(f.doc, '$.parents[0]') AS Parent0
FROM Families f 
WHERE JSON_VALUE(f.doc, '$.id') = N'AndersenFamily'
```  
此查询结果显示在下表中：

| 地址 | 父项 | Parent0 |
| --- | --- | --- |
| { "state":"NY", "county":"Manhattan", "city":"NY" } | [{ "familyName":"Wakefield", "givenName":"Robin" }, {"familyName":"Miller", "givenName":"Ben" } ]| { "familyName":"Wakefield", "givenName":"Robin" } |

有关详细信息，请参阅 [JSON_QUERY (Transact-SQL)](../../t-sql/functions/json-query-transact-sql.md)。  

## <a name="parse-nested-json-collections"></a>分析嵌套式 JSON 集合

通过 `OPENJSON` 函数，可将 JSON 子数组转换为行集，然后将其与父元素联接在一起。 例如，可返回所有家庭文档，并将其与存储为内部 JSON 数组的 `children` 对象“联接”起来：

```sql
SELECT JSON_VALUE(f.doc, '$.id')  AS Name, 
       JSON_VALUE(f.doc, '$.address.city') AS City,
       c.givenName, c.grade
FROM Families f
        CROSS APPLY OPENJSON(f.doc, '$.children')
            WITH(grade int, givenName nvarchar(100))  c
```

此查询结果显示在下表中：

| 名称 | 城市 | givenName | 年级 |
| --- | --- | --- | --- |
| AndersenFamily | NY | Jesse | 1 |
| AndersenFamily | NY | Lisa | 8 |

最终我们得到两个行，因为一个父行与通过分析子级子数组的两个元素产生的两个子行联接在一起。 `OPENJSON` 函数分析 `doc` 列中的 `children` 片段，并返回每个元素中的 `grade` 和 `givenName` 作为一个行集。 此行集可以与父文档进行联接。
 
## <a name="query-nested-hierarchical-json-sub-arrays"></a>查询嵌套式分层 JSON 子数组

可应用多个 `CROSS APPLY OPENJSON` 调用以查询嵌套式 JSON 结构。 本示例中使用的 JSON 文档具有名为 `children` 的嵌套数组，其中每个子级都有 `pets` 的嵌套数组。 下面的查询将分析每个文档中的子级，将每个数组对象作为行返回，然后分析 `pets` 数组：

```sql
SELECT  familyName,
    c.givenName AS childGivenName,
    c.firstName AS childFirstName,
    p.givenName AS petName 
FROM Families f 
    CROSS APPLY OPENJSON(f.doc) 
        WITH (familyName nvarchar(100), children nvarchar(max) AS JSON)
        CROSS APPLY OPENJSON(children) 
        WITH (givenName nvarchar(100), firstName nvarchar(100), pets nvarchar(max) AS JSON) as c
            OUTER APPLY OPENJSON (pets)
            WITH (givenName nvarchar(100))  as p
```

首次进行 `OPENJSON` 调用将使用 AS JSON 子句返回 `children` 数组的片段。 此数组片段将提供给第二个 `OPENJSON` 函数，该函数将返回每个小孩的 `firstName` 和 `givenName` 以及`pets` 数组。 `pets` 的数组将提供给第三个 `OPENJSON` 函数，该函数将返回宠物的 `givenName`。
此查询结果显示在下表中：

| familyName | childGivenName | childFirstName | petName |
| --- | --- | --- | --- |
| AndersenFamily | Jesse | Merriam | Goofy |
| AndersenFamily | Jesse | Merriam | Shadow |
| AndersenFamily | Lisa | Miller| `NULL` |

根文档与两个 `children` 行进行了联接，这两个行由生成两个行（或元组）的首次 `OPENJSON(children)` 调用返回。 然后，使用 `OUTER APPLY` 运算符将每行与由 `OPENJSON(pets)` 生成的新行进行联接。 Jesse 有两只宠物，因此 `(AndersenFamily, Jesse, Merriam)` 与为 Goofy 和 Shadow 生成的两行进行了联接。 Lisa 没有宠物，因此 `OPENJSON(pets)` 没有为该元组返回的任何行。 但是，由于我们使用的是 `OUTER APPLY`，因此这列的结果是 `NULL`。 如果我们使用 `CROSS APPLY` 而不是 `OUTER APPLY`，则不会在结果中返回 Lisa，因为没有可以与该元组联接的任何宠物行。

##  <a name="compare-json_value-and-json_query"></a><a name="JSONCompare"></a> 对比 JSON_VALUE 与 JSON_QUERY  
**JSON_VALUE** 和 **JSON_QUERY** 之间的主要区别在于 **JSON_VALUE** 返回标量值，而 **JSON_QUERY** 返回数组或对象。  
  
请参考以下示例 JSON 文本。  
  
```json  
{
    "a": "[1,2]",
    "b": [1, 2],
    "c": "hi"
}  
```  
  
在此示例 JSON 文本中，数据成员“a”和“c”是字符串值，而数据成员“b”是数组。 **JSON_VALUE** 和 **JSON_QUERY** 返回以下结果：  
  
|路径|**JSON_VALUE** 返回|**JSON_QUERY** 返回|  
|-----------|-----------------------------|-----------------------------|  
|**$**|NULL 或错误|`{ "a": "[1,2]", "b": [1,2], "c":"hi"}`|  
|**$.a**|[1,2]|NULL 或错误|  
|**$.b**|NULL 或错误|[1,2]|  
|**$.b[0]**|1|NULL 或错误|  
|**$.c**|hi|NULL 或错误|  
  
## <a name="test-json_value-and-json_query-with-the-adventureworks-sample-database"></a>使用 AdventureWorks 示例数据库测试 JSON_VALUE 和 JSON_QUERY  
通过使用 AdventureWorks 示例数据库运行以下示例，对本主题中所述的内置函数进行测试。 有关何处获取 AdventureWorks 的信息，以及如何通过运行脚本添加用于测试的 JSON 数据，请参阅[测试驱动的内置 JSON 支持](json-data-sql-server.md#test-drive-built-in-json-support-with-the-adventureworks-sample-database)。
  
在下面的示例中，`SalesOrder_json` 表中的 `Info` 列包含了 JSON 文本。  
  
### <a name="example-1---return-both-standard-columns-and-json-data"></a>示例 1 - 返回标准列和 JSON 数据  
下面的查询返回将返回标准关系列以及 JSON 列的值。  
  
```sql  
SELECT SalesOrderNumber, OrderDate, Status, ShipDate, Status, AccountNumber, TotalDue,
 JSON_QUERY(Info,'$.ShippingInfo') ShippingInfo,
 JSON_QUERY(Info,'$.BillingInfo') BillingInfo,
 JSON_VALUE(Info,'$.SalesPerson.Name') SalesPerson,
 JSON_VALUE(Info,'$.ShippingInfo.City') City,
 JSON_VALUE(Info,'$.Customer.Name') Customer,
 JSON_QUERY(OrderItems,'$') OrderItems
FROM Sales.SalesOrder_json
WHERE ISJSON(Info) > 0
```  
  
### <a name="example-2--aggregate-and-filter-json-values"></a>示例 2 - 聚合和筛选 JSON 值  
下面的查询将按客户名称（存储在 JSON 中）和状态（存储在普通列中）来汇总小计。 然后将按市/县（存储在 JSON 中）和 OrderDate（存储在普通列中）来筛选结果。  
  
```sql  
DECLARE @territoryid INT;
DECLARE @city NVARCHAR(32);

SET @territoryid=3;

SET @city=N'Seattle';

SELECT JSON_VALUE(Info, '$.Customer.Name') AS Customer, Status, SUM(SubTotal) AS Total
FROM Sales.SalesOrder_json
WHERE TerritoryID=@territoryid
 AND JSON_VALUE(Info, '$.ShippingInfo.City') = @city
 AND OrderDate > '1/1/2015'
GROUP BY JSON_VALUE(Info, '$.Customer.Name'), Status
HAVING SUM(SubTotal)>1000
```  
  
##  <a name="update-property-values-in-json-text-by-using-the-json_modify-function"></a><a name="MODIFY"></a> 使用 JSON_MODIFY 函数更新 JSON 文本中的属性值  
JSON_MODIFY 函数更新 JSON 字符串中属性的值，并返回已更新的 JSON 字符串。  
  
以下示例将更新包含 JSON 的变量中的 JSON 属性的值。  
  
```sql  
SET @info = JSON_MODIFY(@jsonInfo, "$.info.address[0].town", 'London')    
```  
  
 有关详细信息，请参阅 [JSON_MODIFY &#40;Transact-SQL&#41;](../../t-sql/functions/json-modify-transact-sql.md)。  
  
## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>详细了解 SQL Server 和 Azure SQL 数据库中的 JSON  
  
### <a name="microsoft-videos"></a>Microsoft 视频

有关 SQL Server 和 Azure SQL 数据库中内置 JSON 支持的视频介绍，请观看以下视频：

-   [SQL Server 2016 和 JSON 支持](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [在 SQL Server 2016 和 Azure SQL 数据库中使用 JSON](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON 充当 NoSQL 和关系环境之间的桥梁](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
  
## <a name="see-also"></a>另请参阅  
 [ISJSON (Transact-SQL)](../../t-sql/functions/isjson-transact-sql.md)   
 [JSON_VALUE (Transact-SQL)](../../t-sql/functions/json-value-transact-sql.md)   
 [JSON_QUERY (Transact-SQL)](../../t-sql/functions/json-query-transact-sql.md)   
 [JSON_MODIFY (Transact-SQL)](../../t-sql/functions/json-modify-transact-sql.md)   
 [JSON 路径表达式 (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)  
  
  

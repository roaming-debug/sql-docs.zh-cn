---
description: OPENJSON (Transact-SQL)
title: OPENJSON (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPENJSON
- OPENJSON_TSQL
helpviewer_keywords:
- OPENJSON rowset function
- JSON, importing
- JSON, converting from
ms.assetid: 233d0877-046b-4dcc-b5da-adeb22f78531
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: b29315567520dff0d4700aeb476279bf6cfe24b7
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "100340645"
---
# <a name="openjson-transact-sql"></a>OPENJSON (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

**OPENJSON** 是一种表值函数，可分析 JSON 文本，并以行和列的形式从 JSON 输入返回对象和属性。 换句话说，**OPENJSON** 对 JSON 文档提供行集视图。 可以显式指定行集中的列以及用于填充列的 JSON 属性路径。 由于 **OPENJSON** 返回一组行，因此可以在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的 `FROM` 子句中使用 **OPENJSON**，就如同可以使用任何其他表、视图或表值函数一样。  
  
对于不能直接使用 JSON 的应用或服务，可以使用 **OPENJSON** 将 JSON 数据导入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，或者将 JSON 数据转换为关系格式。  
  
> [!NOTE]  
>  **OPENJSON** 函数仅在兼容级别 130 或更高级别下可用。 如果数据库兼容级别低于 130，SQL Server 将无法找到并运行 OPENJSON 函数。 其他 JSON 函数在所有兼容性级别均可用。
> 
> 可以在 `sys.databases` 视图或数据库属性中查看兼容级别。 可以使用以下命令更改数据库的兼容级别：  
> 
> `ALTER DATABASE DatabaseName SET COMPATIBILITY_LEVEL = 130`
>
> 兼容级别 120 可能是默认级别，即使在新的 Azure SQL 数据库中也是如此。  
  
 ![“主题链接”图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标")[Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
OPENJSON( jsonExpression [ , path ] )  [ <with_clause> ]

<with_clause> ::= WITH ( { colName type [ column_path ] [ AS JSON ] } [ ,...n ] )
```  

**OPENJSON** 表值函数会分析作为第一个参数提供的 jsonExpression，并返回包含来自表达式中 JSON 对象的数据的一行或多行。 jsonExpression 可以包含嵌套子对象。 如果要分析 jsonExpression 中的子对象，则可以为 JSON 子对象指定 path参数。

### <a name="openjson"></a>openjson

![OPENJSON TVF 的语法](../../relational-databases/json/media/openjson-syntax.png "OPENJSON 语法")  

默认情况下，**OPENJSON** 表值函数返回三列，这些列包含在 jsonExpression  中找到的每个 {键:值} 对的键名称、值和类型。 作为替代方法，可以通过提供 with_clause 来显式指定 **OPENJSON** 返回的结果集的架构。
  
### <a name="with_clause"></a>with_clause
  
![OPENJSON TVF 中的 WITH 子句的语法](../../relational-databases/json/media/openjson-shema-syntax.png "OPENJSON WITH 语法")

with_clause 包含 **OPENJSON** 要返回的列及其类型的列表。 默认情况下，**OPENJSON** 将 jsonExpression 中的键与 with_clause 中的列名进行匹配（在此情况下，匹配键意味着它区分大小写）。 如果列名与键名称不匹配，则可以提供可选的 column_path，它是在 jsonExpression 中引用键的 [JSON 路径表达式](../../relational-databases/json/json-path-expressions-sql-server.md) 。 

## <a name="arguments"></a>参数

### <a name="jsonexpression"></a>*jsonExpression*

是包含 JSON 文本的 Unicode 字符表达式。  
  
OPENJSON 循环访问 JSON 表达式中的数组的元素或对象的属性，并为每个元素或属性返回一行。 下面的示例返回作为 jsonExpression 提供的对象的每个属性：  

```sql
DECLARE @json NVARCHAR(2048) = N'{
   "String_value": "John",
   "DoublePrecisionFloatingPoint_value": 45,
   "DoublePrecisionFloatingPoint_value": 2.3456,
   "BooleanTrue_value": true,
   "BooleanFalse_value": false,
   "Null_value": null,
   "Array_value": ["a","r","r","a","y"],
   "Object_value": {"obj":"ect"}
}';

SELECT * FROM OpenJson(@json);
```

**结果：**

| key                                | 值                 | type |
| :--                                | :----                 | :--- |
| String_value                       | John                  | 1 |
| DoublePrecisionFloatingPoint_value | 45                    | 2 |
| DoublePrecisionFloatingPoint_value | 2.3456                | 2 |
| BooleanTrue_value                  | true                  | 3 |
| BooleanFalse_value                 | false                 | 3 |
| Null_value                         | Null                  | 0 |
| Array_value                        | ["a","r","r","a","y"] | 4 |
| Object_value                       | {"obj":"ect"}         | 5 |
| &nbsp; | &nbsp; | &nbsp; |

- DoublePrecisionFloatingPoint_value 遵循 IEEE-754。

### <a name="path"></a>*路径*

是在 jsonExpression 中引用对象或数组的可选 JSON 路径表达式。 **OPENJSON** 会定位到指定位置处的 JSON 文本，并且仅分析引用的片段。 有关详细信息，请参阅 [JSON 路径表达式 (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)。

在 [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] 和 [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)] 中，可提供变量作为 path 的值。
  
下面的示例通过指定 path 来返回嵌套对象：  

```sql  
DECLARE @json NVARCHAR(4000) = N'{  
      "path": {  
            "to":{  
                 "sub-object":["en-GB", "en-UK","de-AT","es-AR","sr-Cyrl"]  
                 }  
              }  
 }';

SELECT [key], value
FROM OPENJSON(@json,'$.path.to."sub-object"')
```  
  
 **结果**  
  
|密钥|值|  
|---------|-----------|  
|0|en-GB|  
|1|en-UK|  
|2|de-AT|  
|3|es-AR|  
|4|sr-Cyrl|  

当 **OPENJSON** 分析 JSON 数组时，该函数以键的形式返回 JSON 文本中元素的索引。

用于将路径各步与 JSON 表达式的属性进行匹配的比较不区分大小写且无法识别排序规则（即是 BIN2 比较）。 

### <a name="with_clause"></a>*with_clause*

显式定义 **OPENJSON** 函数要返回的输出架构。 可选 with_clause 可以包含以下元素：

colName 是输出列的名称。  
  
默认情况下，**OPENJSON** 使用列的名称与 JSON 文本中的属性进行匹配。 例如，如果在架构中指定列 name，则 OPENJSON 会尝试使用 JSON 文本中的属性“name”填充此列。 可以使用 column_path 参数替代此默认映射。  
  
type  
是输出列的数据类型。  

> [!NOTE]
> 如果还使用 **AS JSON** 选项，则列 type 必须是 `NVARCHAR(MAX)`。
  
column_path  
是指定要在指定列中返回的属性的 JSON 路径。 有关详细信息，请参阅本主题前面的 path 参数说明。  
  
使用 column_path 可在输出列的名称与属性的名称不匹配时替代重写默认映射规则。  
  
用于将路径各步与 JSON 表达式的属性进行匹配的比较不区分大小写且无法识别排序规则（即是 BIN2 比较）。  
  
有关路径的详细信息，请参阅 [JSON 路径表达式 (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)。  
  
*AS JSON*  
在列定义中使用 **AS JSON** 选项可指定引用的属性包含内部 JSON 对象或数组。 如果指定 **AS JSON** 选项，列的类型必须是 NVARCHAR(MAX)。

- 如果没有为列指定 **AS JSON**，则函数会从指定路径上的指定 JSON 属性返回标量值（例如 int、string、true、false）。 如果路径表示对象或数组，并且在指定路径上找不到属性，则函数在宽松模式下返回 NULL，或在严格模式下返回错误。 此行为与 **JSON_VALUE** 函数的行为类似。  
  
- 如果为列指定 **AS JSON**，则函数会从指定路径上的指定 JSON 属性返回 JSON 片段。 如果路径表示标量值，并且在指定路径上找不到属性，则函数在宽松模式下返回 NULL，或在严格模式下返回错误。 此行为与 **JSON_QUERY** 函数的行为类似。  
  
> [!NOTE]  
> 如果要从 JSON 属性返回嵌套 JSON 片段，则必须提供 **AS JSON** 标志。 未使用此选项时，如果找不到属性，则 OPENJSON 会返回 NULL 值而不是引用的 JSON 对象或数组，或在严格模式下返回运行时错误。  
  
例如，以下查询返回数组的元素并进行格式设置：  
  
```sql  
DECLARE @json NVARCHAR(MAX) = N'[  
  {  
    "Order": {  
      "Number":"SO43659",  
      "Date":"2011-05-31T00:00:00"  
    },  
    "AccountNumber":"AW29825",  
    "Item": {  
      "Price":2024.9940,  
      "Quantity":1  
    }  
  },  
  {  
    "Order": {  
      "Number":"SO43661",  
      "Date":"2011-06-01T00:00:00"  
    },  
    "AccountNumber":"AW73565",  
    "Item": {  
      "Price":2024.9940,  
      "Quantity":3  
    }  
  }
]'  
   
SELECT *
FROM OPENJSON ( @json )  
WITH (   
              Number   VARCHAR(200)   '$.Order.Number',  
              Date     DATETIME       '$.Order.Date',  
              Customer VARCHAR(200)   '$.AccountNumber',  
              Quantity INT            '$.Item.Quantity',  
              [Order]  NVARCHAR(MAX)  AS JSON  
 )
```  
  
**结果**
  
|Number|Date|客户|数量|订单|  
|------------|----------|--------------|--------------|-----------|  
|SO43659|2011-05-31T00:00:00|AW29825|1|{"Number":"SO43659","Date":"2011-05-31T00:00:00"}|  
|SO43661|2011-06-01T00:00:00|AW73565|3|{"Number":"SO43661","Date":"2011-06-01T00:00:00"}|  
  
## <a name="return-value"></a>返回值
OPENJSON 函数返回的列取决于 WITH 选项。  
  
1. 当调用具有默认架构的 OPENJSON 时（即当未在 WITH 子句中指定显式架构时），该函数返回具有以下各列的表：  
    1.  **Key**。 一个 nvarchar(4000) 值，包含指定属性的名称或指定数组中元素的索引。 键列具有 BIN2 排序规则。  
    2.  **值**。 一个 nvarchar(max) 值，包含属性的值。 值列从 jsonExpression 继承其排序规则。
    3.  **Type**。 一个 int 值，包含值的类型。 仅当使用具有默认架构的 OPENJSON 时，才返回 **Type** 列。 类型列具有以下值之一：  
  
        |类型列的值|JSON 数据类型|  
        |------------------------------|--------------------|  
        |0|Null|  
        |1|字符串|  
        |2|number|  
        |3|true/false|  
        |4|array|  
        |5|对象 (object)|  
  
     仅返回第一级属性。 如果 JSON 文本的格式不正确，则语句会失败。  

2. 当调用 OPENJSON 并且在 WITH 子句中指定显式架构时，该函数返回具有在 WITH 子句中定义的架构的表。

> [!NOTE]  
> 只有在你结合使用 OPENJSON 和默认架构时，“Key”、“Value”和“Type”列才会返回，它们不适用于显式架构。

## <a name="remarks"></a>备注  

在 OPENJSON 的第二个参数或 with_clause 中使用的 json_path可以以 lax 或 strict 关键字开头 。

- 在 **lax** 模式下，**OPENJSON** 在找不到指定路径上的对象或值时不会引发错误。 如果找不到路径，则 **OPENJSON** 返回空结果集或 NULL 值。
- 在 **strict** 模式下，**OPENJSON** 在找不到路径时返回错误。

此页面上的某些示例显式指定路径模式（宽松或严格）。 路径模式是可选项。 如果未显式指定路径模式，则宽松模式是默认值。 有关路径模式和路径表达式的详细信息，请参阅 [JSON 路径表达式 (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)。    

with_clause 中的列名会与 JSON 文本中的键进行匹配。 如果指定列名 `[Address.Country]`，则它会与键 `Address.Country` 进行匹配。 如果要在对象 `Address` 中引用嵌套键 `Country`，则必须在列路径中指定路径 `$.Address.Country`。

json_path 可以包含具有字母数字字符的键。 如果在键中包含特殊字符，则使用双引号在 json_path 中对键名称进行转义。 例如，在以下 JSON 文本中，`$."my key $1".regularKey."key with . dot"` 与值 1 进行匹配：

```json
{
  "my key $1": {
    "regularKey":{
      "key with . dot": 1
    }
  }
}
```  

## <a name="examples"></a>示例  
  
### <a name="example-1---convert-a-json-array-to-a-temporary-table"></a>示例 1 - 将 JSON 数组转换为临时表

下面的示例以 JSON 数字数组的形式提供标识符的列表。 查询将 JSON 数组转换为标识符表，并筛选有指定 ID 的所有产品。  
  
```sql  
DECLARE @pSearchOptions NVARCHAR(4000) = N'[1,2,3,4]'

SELECT *
FROM products
INNER JOIN OPENJSON(@pSearchOptions) AS productTypes
 ON product.productTypeID = productTypes.value
```  
  
此查询与下面的示例等效。 但是在下面的示例中，必须在查询中嵌入数字而不是将它们作为参数进行传递。  
  
```sql  
SELECT *
FROM products
WHERE product.productTypeID IN (1,2,3,4)
```  
  
### <a name="example-2---merge-properties-from-two-json-objects"></a>示例 2 - 合并来自两个 JSON 对象的属性

下面的示例选择两个 JSON 对象的所有属性的并集。 这两个对象具有重复的 name 属性。 该示例使用键值从结果中排除重复行。  
  
```sql  
DECLARE @json1 NVARCHAR(MAX),@json2 NVARCHAR(MAX)

SET @json1=N'{"name": "John", "surname":"Doe"}'

SET @json2=N'{"name": "John", "age":45}'

SELECT *
FROM OPENJSON(@json1)
UNION ALL
SELECT *
FROM OPENJSON(@json2)
WHERE [key] NOT IN (SELECT [key] FROM OPENJSON(@json1))
```  
  
### <a name="example-3---join-rows-with-json-data-stored-in-table-cells-using-cross-apply"></a>示例 3 - 使用 CROSS APPLY 联接包含存储在表单元格中的 JSON 数据的行

在下面的示例中，`SalesOrderHeader` 表具有一个 `SalesReason` 文本列，它包含采用 JSON 格式的 `SalesOrderReasons` 的数组。 `SalesOrderReasons` 对象包含属性，例如 Quality 和 Manufacturer。 该示例创建一个报表，它将每个销售订单行联接到相关销售原因。 OPENJSON 运算符会扩展销售原因的 JSON 数组，如同原因是存储在单独的子表中一样。 CROSS APPLY 运算符随后将每个销售订单行与 OPENJSON 表值函数返回的行联接。  
  
```sql  
SELECT SalesOrderID,OrderDate,value AS Reason
FROM Sales.SalesOrderHeader
CROSS APPLY OPENJSON(SalesReasons)
```  
  
> [!TIP] 
> 必须扩展存储在各个字段中的 JSON 数组，并将它们与其父行联接时，通常使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] CROSS APPLY 运算符。 有关 CROSS APPLY 的详细信息，请参阅 [FROM (Transact SQL)](../../t-sql/queries/from-transact-sql.md)。  
  
可以通过将 `OPENJSON` 与要返回的行的显式定义架构一起使用，来重写相同查询：  
  
```sql  
SELECT SalesOrderID, OrderDate, value AS Reason  
FROM Sales.SalesOrderHeader  
     CROSS APPLY OPENJSON (SalesReasons) WITH (value NVARCHAR(100) '$')
```  
  
在此示例中，`$` 路径引用数组中的每个元素。 如果要显式强制转换返回值，则可以使用此类型的查询。  
  
### <a name="example-4---combine-relational-rows-and-json-elements-with-cross-apply"></a>示例 4 - 使用 CROSS APPLY 合并关系行和 JSON 元素

以下查询将关系行和 JSON 元素合并到下表中显示的结果中。  
  
```sql  
SELECT store.title, location.street, location.lat, location.long  
FROM store  
CROSS APPLY OPENJSON(store.jsonCol, 'lax $.location')   
     WITH (street VARCHAR(500) ,  postcode VARCHAR(500) '$.postcode' ,  
     lon int '$.geo.longitude', lat int '$.geo.latitude')  
     AS location
```  
  
**结果**
  
|title|street|postcode|lon|lat|  
|-----------|------------|--------------|---------|---------|  
|Whole Food Markets|17991 Redmond Way|WA  98052|47.666124|-122.10155|  
|Sears|148th Ave NE|WA  98052|47.63024|-122.141246,17|  
  
### <a name="example-5---import-json-data-into-sql-server"></a>示例 5 - 将 JSON 数据导入 SQL Server 中

下面的示例展示了将整个 JSON 对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中。  
  
```sql  
DECLARE @json NVARCHAR(max)  = N'{  
  "id" : 2,  
  "firstName": "John",  
  "lastName": "Smith",  
  "isAlive": true,  
  "age": 25,  
  "dateOfBirth": "2015-03-25T12:00:00",  
  "spouse": null  
  }';  
   
  INSERT INTO Person  
  SELECT *   
  FROM OPENJSON(@json)  
  WITH (id INT,  
        firstName NVARCHAR(50), lastName NVARCHAR(50),   
        isAlive BIT, age INT,  
        dateOfBirth DATETIME, spouse NVARCHAR(50))
```  

### <a name="example-6---simple-example-with-json-content"></a>示例 6 - 使用 JSON 内容的简单示例

```sql
--simple cross apply example
DECLARE @JSON NVARCHAR(MAX) = N'[
{
"OrderNumber":"SO43659",
"OrderDate":"2011-05-31T00:00:00",
"AccountNumber":"AW29825",
"ItemPrice":2024.9940,
"ItemQuantity":1
},
{
"OrderNumber":"SO43661",
"OrderDate":"2011-06-01T00:00:00",
"AccountNumber":"AW73565",
"ItemPrice":2024.9940,
"ItemQuantity":3
}
]'

SELECT root.[key] AS [Order],TheValues.[key], TheValues.[value]
FROM OPENJSON ( @JSON ) AS root
CROSS APPLY OPENJSON ( root.value) AS TheValues
```

## <a name="see-also"></a>另请参阅

- [JSON 路径表达式 (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)   
- [用 OPENJSON (SQL Server) 将 JSON 数据转换为行和列](../../relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server.md)   
- [使用具有默认架构的 OPENJSON (SQL Server)](../../relational-databases/json/use-openjson-with-the-default-schema-sql-server.md)   
- [使用具有显式架构的 OPENJSON (SQL Server)](../../relational-databases/json/use-openjson-with-an-explicit-schema-sql-server.md)  
  

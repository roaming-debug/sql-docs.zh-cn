---
description: SQL Server 中的 JSON 数据
title: 使用 JSON 数据
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: quickstart
helpviewer_keywords:
- JSON
- JSON, built-in support
ms.assetid: c9a4e145-33c3-42b2-a510-79813e67806a
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||= azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 551a0ad762f6cc8fdc0a347c5896a3bbbea27b82
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075988"
---
# <a name="json-data-in-sql-server"></a>SQL Server 中的 JSON 数据

[!INCLUDE[appliesto-ss2016-asdb-asdw-xxx-md.md](../../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)]

JSON 是一种流行的数据格式，用于在现代 Web 和移动应用程序中交换数据。 JSON 还可用于在日志文件或 Microsoft Azure Cosmos DB 等 NoSQL 数据库中存储非结构化数据。 许多 REST Web 服务以 JSON 文本格式返回结果，或接受采用 JSON 格式的数据。 例如，大多数 Azure 服务（如 Azure 搜索、Azure 存储和 Azure Cosmos DB）都提供返回或使用 JSON 的 REST 终结点。 JSON 也是用于通过 AJAX 调用在网页与 Web 服务器之间交换数据的主要格式。 

SQL Server 中的 JSON 函数使用户能在同一数据库中将 NoSQL 和相关概念合并。 现在，可以将经典关系列与同一表中包含格式化为 JSON 文本的文档的列合并，在关系结构中分析和导入 JSON 文档，或者将关系数据格式化为 JSON 文本。 在以下视频中可看到 JSON 函数如何在 SQL Server 和 Azure SQL 数据库中连接关系概念和 NoSQL 概念：

*JSON 充当 NoSQL 和关系环境之间的桥梁*
> [!VIDEO https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds/player]

下面是 JSON 文本的示例：

```json
[
  {
    "name": "John",
    "skills": ["SQL", "C#", "Azure"]
  },
  {
    "name": "Jane",
    "surname": "Doe"
  }
]
```

通过使用 SQL Server 内置函数和运算符，你可以对 JSON 文本执行以下操作：

- 分析 JSON 文本和读取或修改值。  
- 将 JSON 对象数组转换为表格式。  
- 在转换后的 JSON 对象上运行任意 Transact-SQL 查询。  
- 将 Transact-SQL 查询的结果设置为 JSON 格式。  
  
![内置的 JSON 支持的概述](../../relational-databases/json/media/jsonslides1overview.png "内置的 JSON 支持的概述")  
  
## <a name="key-json-capabilities-of-sql-server-and-sql-database"></a>SQL Server 和 SQL 数据库的关键 JSON 功能

下一部分介绍 SQL Server 随其内置 JSON 支持一起提供的主要功能。 在以下视频中可看到如何使用 JSON 函数和运算符：

*SQL Server 2016 和 JSON 支持*
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support/player?WT.mc_id=dataexposed-c9-niner]

### <a name="extract-values-from-json-text-and-use-them-in-queries"></a>从 JSON 文本中提取值并在查询中使用这些值

如果使用存储在数据库表中的 JSON 文本，则可以使用以下内置函数来读取或修改 JSON 文本中的值：  

- [ISJSON (Transact-SQL)](../../t-sql/functions/isjson-transact-sql.md) 测试字符串是否包含有效 JSON。
- [JSON_VALUE (Transact-SQL)](../../t-sql/functions/json-value-transact-sql.md) 从 JSON 字符串中提取标量值。
- [JSON_QUERY (Transact-SQL)](../../t-sql/functions/json-query-transact-sql.md) 从 JSON 字符串中提取对象或数组。
- [JSON_MODIFY (Transact-SQL)](../../t-sql/functions/json-modify-transact-sql.md) 更改 JSON 字符串中的值。

**示例**

在以下示例中，查询同时使用表中的关系数据和 JSON 数据（存储在名为 `jsonCol` 的列中）：  
  
```sql  
SELECT Name, Surname,
  JSON_VALUE(jsonCol, '$.info.address.PostCode') AS PostCode,
  JSON_VALUE(jsonCol, '$.info.address."Address Line 1"') + ' '
  + JSON_VALUE(jsonCol, '$.info.address."Address Line 2"') AS Address,
  JSON_QUERY(jsonCol, '$.info.skills') AS Skills
FROM People
WHERE ISJSON(jsonCol) > 0
  AND JSON_VALUE(jsonCol, '$.info.address.Town') = 'Belgrade'
  AND Status = 'Active'
ORDER BY JSON_VALUE(jsonCol, '$.info.address.PostCode')
```  
  
对于应用程序和工具来说，从标量表列中提取的值与从 JSON 列中提取的值没有任何差异。 可以在 Transact-SQL 查询的任何组成部分（包括 WHERE、ORDER BY 或 GROUP BY 子句、窗口聚合，等等）中使用来自 JSON 文本的值。 JSON 函数使用类似于 JavaScript 的语法来引用 JSON 文本内的值。

有关详细信息，请参阅[使用内置函数验证、查询和更改 JSON 数据 (SQL Server)](../../relational-databases/json/validate-query-and-change-json-data-with-built-in-functions-sql-server.md)、[JSON_VALUE (Transact-SQL)](../../t-sql/functions/json-value-transact-sql.md) 和 [JSON_QUERY (Transact-SQL)](../../t-sql/functions/json-query-transact-sql.md)。  
  
### <a name="change-json-values"></a>更改 JSON 值

如果必须修改部分 JSON 文本，可以使用 [JSON_MODIFY (Transact-SQL)](../../t-sql/functions/json-modify-transact-sql.md) 函数更新 JSON 字符串中属性的值，并返回已更新的 JSON 字符串。 以下示例将更新包含 JSON 的变量中的属性的值：  
  
```sql
DECLARE @json NVARCHAR(MAX);
SET @json = '{"info": {"address": [{"town": "Belgrade"}, {"town": "Paris"}, {"town":"Madrid"}]}}';
SET @json = JSON_MODIFY(@json, '$.info.address[1].town', 'London');
SELECT modifiedJson = @json;
```

**结果**

|modifiedJson|  
|--------|  
|{"info":{"address":[{"town":"Belgrade"},{"town":"London"},{"town":"Madrid"}]}|  
  
### <a name="convert-json-collections-to-a-rowset"></a>将 JSON 集合转换为行集

在 SQL Server 中查询 JSON 不需要自定义查询语言。 可以使用标准的 T-SQL 查询 JSON 数据。 如果必须基于 JSON 数据创建查询或报表，可以通过调用 OPENJSON 行集函数，轻松地将 JSON 数据转换为行与列  。 有关详细信息，请参阅[用 OPENJSON 将 JSON 数据转换为行和列 (SQL Server)](../../relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server.md)。  
  
以下示例调用 OPENJSON，并且将 `@json` 变量中存储的对象数组转换为可使用标准 SQL SELECT 语句查询的行集   ：  
  
```sql  
DECLARE @json NVARCHAR(MAX);
SET @json = N'[
  {"id": 2, "info": {"name": "John", "surname": "Smith"}, "age": 25},
  {"id": 5, "info": {"name": "Jane", "surname": "Smith"}, "dob": "2005-11-04T12:00:00"}
]';

SELECT *
FROM OPENJSON(@json)
  WITH (
    id INT 'strict $.id',
    firstName NVARCHAR(50) '$.info.name',
    lastName NVARCHAR(50) '$.info.surname',
    age INT,
    dateOfBirth DATETIME2 '$.dob'
  );
```

**结果**

|ID|firstName|lastName|age|dateOfBirth|  
|--------|---------------|--------------|---------|-----------------|  
|2|John|Smith|25||  
|5|Jane|Smith||2005-11-04T12:00:00|  
  
**OPENJSON** 将 JSON 对象的数组转换为表，其中每个对象表示为一行，键/值对将作为单元返回。 输出遵循下列规则：

- OPENJSON 将 JSON 值转换为 WITH 子句中指定的类型   。
- **OPENJSON** 可以处理规则的键/值对以及分层组织的对象。
- 不需要返回 JSON 文本中包含的所有字段。
- 如果 JSON 值不存在，OPENJSON  返回 NULL 值。
- 可以选择在类型规范后指定一个路径，以引用嵌套属性或按不同的名称引用属性。
- 路径中可选的 **strict** 前缀指定 JSON 文本中必须存在指定属性的值。

有关详细信息，请参阅[用 OPENJSON 将 JSON 数据转换为行和列 (SQL Server)](../../relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server.md) 和 [OPENJSON (Transact-SQL)](../../t-sql/functions/openjson-transact-sql.md)。  

JSON 文档可能包含无法直接映射到标准关系列中的子元素和层次结构数据。 在这种情况下，可通过将父实体与子数组联接，平展 JSON 层次结构。

在下面的示例中，数组中的第二个对象包含表示人员技能的子数组。 每个子对象都可使用附加 `OPENJSON` 函数调用进行分析：

```sql  
DECLARE @json NVARCHAR(MAX);
SET @json = N'[  
  {"id": 2, "info": {"name": "John", "surname": "Smith"}, "age": 25},
  {"id": 5, "info": {"name": "Jane", "surname": "Smith", "skills": ["SQL", "C#", "Azure"]}, "dob": "2005-11-04T12:00:00"}  
]';

SELECT *  
FROM OPENJSON(@json)  
  WITH (
    id INT 'strict $.id',
    firstName NVARCHAR(50) '$.info.name',
    lastName NVARCHAR(50) '$.info.surname',  
    age INT,
    dateOfBirth DATETIME2 '$.dob',
    skills NVARCHAR(MAX) '$.info.skills' AS JSON
  )
OUTER APPLY OPENJSON(skills)
  WITH (skill NVARCHAR(8) '$');
```

首个 `OPENJSON` 中返回技能数组，作为原始 JSON 文本片段，并使用 `APPLY` 运算符传递给其他 `OPENJSON` 函数  。 第二个 `OPENJSON` 函数将分析 JSON 数组并将字符串值返回为单列行集，这一行集将与第一个 `OPENJSON` 的结果联接。
此查询的结果如下表所示：

**结果**

|ID|firstName|lastName|age|dateOfBirth|技能|  
|--------|---------------|--------------|---------|-----------------|----------|  
|2|John|Smith|25|||  
|5|Jane|Smith||2005-11-04T12:00:00|SQL|
|5|Jane|Smith||2005-11-04T12:00:00|C#|
|5|Jane|Smith||2005-11-04T12:00:00|Azure|

`OUTER APPLY OPENJSON` 将联接一级实体和子数组，并返回平展后的结果集。 由于 JOIN，将对每个技能重复第二行。

### <a name="convert-sql-server-data-to-json-or-export-json"></a>将 SQL Server 数据转换为 JSON 或导出 JSON

>[!NOTE]
>不支持将 Azure Synapse Analytics 数据转换为 JSON，也不支持导出 JSON。

通过将 **FOR JSON** 子句添加到 **SELECT** 语句中，可将 SQL Server 数据或 SQL 查询结果的格式设置为 JSON。 使用 FOR JSON 委托从客户端应用程序到 SQL Server 的 JSON 输出格式  。 有关详细信息，请参阅[借助 FOR JSON 将查询结果格式化为 JSON (SQL Server)](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md)。  
  
以下示例使用带有 FOR JSON 子句的 PATH 模式  ：  
  
```sql  
SELECT id, firstName AS "info.name", lastName AS "info.surname", age, dateOfBirth AS dob
FROM People
FOR JSON PATH;
```

“应用程序池:” **FOR JSON** 子句将 SQL 结果的格式设置为 JSON 文本，该格式可提供给识别 JSON 的任何应用。 PATH 选项在 SELECT 子句中使用以点分隔的别名，以嵌套查询结果中的对象。  
  
**结果**

```json  
[
  {
    "id": 2,
    "info": {
      "name": "John",
      "surname": "Smith"
    },
    "age": 25
  },
  {
    "id": 5,
    "info": {
      "name": "Jane",
      "surname": "Smith"
    },
    "dob": "2005-11-04T12:00:00"
  }
]
```  
  
有关详细信息，请参阅[借助 FOR JSON 将查询结果格式化为 JSON (SQL Server)](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md) 和 [FOR 子句 (Transact-SQL)](../../t-sql/queries/select-for-clause-transact-sql.md)。  

## <a name="use-cases-for-json-data-in-sql-server"></a>SQL Server 中 JSON 数据的用例

通过 SQL Server 和 Azure SQL 数据库中的 JSON 支持，可合并关系和 NoSQL 概念。 可轻松将关系数据转换为半结构化数据，反之亦然。 但是，JSON 不可替换现有关系模型。 下面是几种得益于 SQL Server 和 SQL 数据库中的 JSON 支持的特定用例。

### <a name="simplify-complex-data-models"></a>简化复杂的数据模型

请考虑使用 JSON 字段代替多个子表对数据模型进行反规范化。

### <a name="store-retail-and-e-commerce-data"></a>存储零售和电子商务数据

在非规范化模型中存储有关含大量变量属性的产品的信息，以实现灵活性。

### <a name="process-log-and-telemetry-data"></a>处理日志和遥测数据

使用 Transact-SQL 语言的所有功能加载、查询和分析存储为 JSON 文件的日志数据。

### <a name="store-semi-structured-iot-data"></a>存储半结构化的 IoT 数据

需要对 IoT 数据进行实时分析时，可直接将传入数据加载到数据库，而不是将其暂存在存储位置中。

### <a name="simplify-rest-api-development"></a>简化 REST API 开发

将数据库中的关系数据轻松转换为支持网站的 REST API 使用的 JSON 格式。

## <a name="combine-relational-and-json-data"></a>合并关系数据和 JSON 数据

SQL Server 提供混合模型，用于通过标准 Transact-SQL 语言存储和处理关系数据与 JSON 数据。 可以将 JSON 文档的集合组织到表中，在它们之间建立关系，将表中存储的强类型标量列与 JSON 列中存储的灵活键/值对合并，以及使用完整 Transact SQL 查询一个或多个表中的标量值和 JSON 值。

JSON 文本存储在 `VARCHAR` 或 `NVARCHAR` 列中，并编制了纯文本形式的索引。 任何支持文本的 SQL Server 功能或组件均支持 JSON，因此 JSON 和其他 SQL Server 功能之间的交互几乎没有任何约束。 可将 JSON 存储在内存中或临时表中，以及对 JSON 文本应用行级别安全性谓词等。

如果在单纯的 JSON 工作负载中，你想要使用专用于处理 JSON 文档的自定义查询语言，可以考虑 Microsoft Azure [Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)。  
  
以下用例说明如何在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中使用内置的 JSON 支持。  

## <a name="store-and-index-json-data-in-sql-server"></a>在 SQL Server 中存储 JSON 数据并编制索引

JSON 是一种文本格式，因此 JSON 文档可存储于 SQL 数据库中的 `NVARCHAR` 列。 由于所有 SQL Server 子系统中支持 `NVARCHAR` 类型，可将 JSON 文档放置于具有“聚集列存储”索引的表、内存优化表或者可使用 OPENROWSET 或 PolyBase 读取的外部文件   。

要了解有关在 SQL Server 中存储、索引和优化 JSON 数据的详细信息，请参阅以下文章：

- [在 SQL Server 或 SQL 数据库中存储 JSON 文档](store-json-documents-in-sql-tables.md)
- [对 JSON 数据编制索引](index-json-data.md)
- [使用内存中 OLTP 优化 JSON 处理](optimize-json-processing-with-in-memory-oltp.md)

### <a name="load-json-files-into-sql-server"></a>将 JSON 文件加载到 SQL Server  

可将文件中存储的信息格式化为标准 JSON 或行分隔的 JSON。 SQL Server 可以导入 JSON 文件的内容，使用 OPENJSON 或 JSON_VALUE 函数分析内容，并将其加载到表中   。  
  
-   如果 JSON 文档存储在可由 SQL Server 访问的本地文件、共享网络驱动器或 Azure 文件位置，则可以使用批量导入将 JSON 数据加载到 SQL Server。
  
-   如果行分隔的 JSON 文件存储在 Azure Blob 存储或 Hadoop 文件系统中，你可以用 PolyBase 来加载 JSON 文本，以 Transact-SQL 代码形式分析文本，然后将其载入表中。  

### <a name="import-json-data-into-sql-server-tables"></a>将 JSON 数据导入 SQL Server 表

如果必须将 JSON 数据从外部服务加载到 SQL Server，则可以使用 OPENJSON 将数据导入 SQL Server，而不是在应用程序层中分析数据  。  
  
```sql  
DECLARE @jsonVariable NVARCHAR(MAX);

SET @jsonVariable = N'[
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
]';

-- INSERT INTO <sampleTable>  
SELECT SalesOrderJsonData.*
FROM OPENJSON (@jsonVariable, N'$')
  WITH (
    Number VARCHAR(200) N'$.Order.Number',
    Date DATETIME N'$.Order.Date',
    Customer VARCHAR(200) N'$.AccountNumber',
    Quantity INT N'$.Item.Quantity'
  ) AS SalesOrderJsonData;
```

你可以通过外部 REST 服务提供 JSON 变量的内容，然后将这些内容从客户端 JavaScript 框架作为参数发送，或者从外部文件加载。 可以在 SQL Server 表中轻松插入、更新或合并来自 JSON 文本的结果。

## <a name="analyze-json-data-with-sql-queries"></a>使用 SQL 查询分析 JSON 数据

如果必须筛选或聚合 JSON 数据用于报表，可以使用 OPENJSON 将 JSON 转换为关系格式  。 然后，可使用标准 [!INCLUDE[tsql](../../includes/tsql-md.md)] 和内置函数来准备报表。  
  
```sql
SELECT Tab.Id, SalesOrderJsonData.Customer, SalesOrderJsonData.Date
FROM SalesOrderRecord AS Tab
CROSS APPLY OPENJSON (Tab.json, N'$.Orders.OrdersArray')
  WITH (
    Number VARCHAR(200) N'$.Order.Number',
    Date DATETIME N'$.Order.Date',
    Customer VARCHAR(200) N'$.AccountNumber',
    Quantity INT N'$.Item.Quantity'
  ) AS SalesOrderJsonData
WHERE JSON_VALUE(Tab.json, '$.Status') = N'Closed'
ORDER BY JSON_VALUE(Tab.json, '$.Group'), Tab.DateModified;
```  
  
你可以在同一个查询中使用 JSON 文本中的标准表列和值。 可以在 `JSON_VALUE(Tab.json, '$.Status')` 表达式上添加索引来提高查询的性能。 有关详细信息，请参阅[对 JSON 数据编制索引](../../relational-databases/json/index-json-data.md)。

## <a name="return-data-from-a-sql-server-table-formatted-as-json"></a>从格式化为 JSON 的 SQL Server 表返回数据

如果你的 Web 服务从数据库层提取数据并以 JSON 格式返回数据，或者你使用接受已格式化为 JSON 的数据的 JavaScript 框架或库，则可以直接在 SQL 查询中设置 JSON 输出的格式。 你可以使用 FOR JSON 将 JSON 格式设置委托给 SQL Server，而非编写代码或者包含一个库来转换表格查询结果并将对象序列化为 JSON 格式  。  
  
例如，你可能想要生成符合 OData 规范的 JSON 输出。 Web 服务需要采用以下格式的请求和响应： 
  
- 请求： `/Northwind/Northwind.svc/Products(1)?$select=ProductID,ProductName`  

- 响应： `{"@odata.context": "https://services.odata.org/V4/Northwind/Northwind.svc/$metadata#Products(ProductID,ProductName)/$entity", "ProductID": 1, "ProductName": "Chai"}`  
  
此 OData URL 代表针对 `ID` 为 1 的产品的 ProductID 和 ProductName 列的请求。 可以使用 **FOR JSON** 按 SQL Server 中所需的格式设置输出格式。  
  
```sql  
SELECT 'https://services.odata.org/V4/Northwind/Northwind.svc/$metadata#Products(ProductID,ProductName)/$entity' AS '@odata.context',
  ProductID,
  Name as ProductName
FROM Production.Product
WHERE ProductID = 1
FOR JSON AUTO;
```  
  
此查询的输出是完全符合 OData 规范的 JSON 文本。格式设置和转义由 SQL Server 处理。 SQL Server 还可将格式查询结果设置为任何格式，如 OData JSON 或 GeoJSON。  
  
## <a name="test-drive-built-in-json-support-with-the-adventureworks-sample-database"></a>使用 AdventureWorks 示例数据库测试驱动内置 JSON 支持

若要获取 AdventureWorks 示例数据库，请至少从 [GitHub](https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks) 下载数据库文件和示例以及脚本文件。

将示例数据库还原到 SQL Server 2016 实例后，请提取示例文件，然后从 JSON 文件夹中打开“JSON Sample Queries procedures views and indexes.sql”文件  。 运行此文件中的脚本，将某些现有数据的格式重新设置为 JSON 数据，对 JSON 数据测试示例查询和报告，为 JSON 数据编制索引，然后导入和导出 JSON。  
  
下面是你可以对该文件中包含的脚本执行的操作：  
  
- 使现有架构非规范化以创建 JSON 数据的列。

  - 将 SalesReasons、SalesOrderDetails、SalesPerson、Customer 和包含销售订单相关信息的表中的信息存储到 SalesOrder_json 表的 JSON 列中。  
  
  - 将 EmailAddresses/PersonPhone 表中的信息作为 JSON 对象的数组存储到 Person_json 表中。  
  
- 创建查询 JSON 数据的过程和视图。  
  
- 对 JSON 数据编制索引。 为 JSON 属性和全文索引创建索引。  
  
- 导入和导出 JSON。 创建并运行以 JSON 结果形式导出 Person 和 SalesOrder 表内容，并使用 JSON 输入导入和更新 Person 与 SalesOrder 表的过程。  
  
- 运行查询示例。 运行一些查询，用于调用步骤 2 和 4 中创建的存储过程和视图。  
  
- 清理脚本。 如果想要保留步骤 2 和 4 中创建的存储过程和视图，则不要运行此部分。  
  
## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>详细了解 SQL Server 和 Azure SQL 数据库中的 JSON  
  
### <a name="microsoft-videos"></a>Microsoft 视频

有关 SQL Server 和 Azure SQL 数据库中内置 JSON 支持的视频介绍，请观看以下视频：

*在 SQL Server 2016 和 Azure SQL 数据库中使用 JSON*
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database/player?WT.mc_id=dataexposed-c9-niner]

在 SQL Server 中使用 JSON 函数构建 REST API 
> [!VIDEO https://www.youtube.com/embed/0m6GXF3-5WI]

### <a name="reference-articles"></a>参考文章

- [FOR 子句 (Transact-SQL)](../../t-sql/queries/select-for-clause-transact-sql.md) (FOR JSON)  
- [OPENJSON (Transact-SQL)](../../t-sql/functions/openjson-transact-sql.md)  
- [JSON 函数 (Transact-SQL)](../../t-sql/functions/json-functions-transact-sql.md)  
  - [ISJSON (Transact-SQL)](../../t-sql/functions/isjson-transact-sql.md)  
  - [JSON_VALUE (Transact-SQL)](../../t-sql/functions/json-value-transact-sql.md)  
  - [JSON_QUERY (Transact-SQL)](../../t-sql/functions/json-query-transact-sql.md)  
  - [JSON_MODIFY (Transact-SQL)](../../t-sql/functions/json-modify-transact-sql.md)  

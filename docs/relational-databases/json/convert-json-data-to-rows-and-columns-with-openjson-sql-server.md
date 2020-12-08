---
description: 使用 OPENJSON 分析和转换 JSON 数据 (SQL Server)
title: 使用 OPENJSON 分析和转换 JSON 数据
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- OPENJSON
- JSON, importing
- importing JSON
ms.assetid: 0c139901-01e2-49ef-9d62-57e08e32c68e
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||= azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 519d66511b71cb0045623b239cc82d0e80791968
ms.sourcegitcommit: 28fecbf61ae7b53405ca378e2f5f90badb1a296a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2020
ms.locfileid: "96595069"
---
# <a name="parse-and-transform-json-data-with-openjson-sql-server"></a>使用 OPENJSON 分析和转换 JSON 数据 (SQL Server)
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

**OPENJSON** 行集函数可将 JSON 文本转换为一组行和列。 使用 OPENJSON 将 JSON 集合转换为行集后，可以在返回的数据上运行任意 SQL 查询或将其插入到 SQL Server 表中。 
  
**OPENJSON** 函数采用单个 JSON 对象或 JSON 对象的集合，并将其转换为一行或多行。 OPENJSON 函数默认返回以下数据：
-   从 JSON 对象中，该函数返回在第一个级别找到的所有“键/值”对。
-   从 JSON 数组中，该函数返回数组的所有元素及其索引。  

可以添加可选的 WITH 子句来提供显式定义输出结构的架构。  
  
## <a name="option-1---openjson-with-the-default-output"></a>选项 1 - 具有默认输出的 OPENJSON
在不提供结果的显式架构的情况下使用 OPENJSON 函数时（即，在 OPENJSON 之后不使用 WITH 子句），该函数将返回包含以下三列的表：
1.  输入对象中属性的名称（或输入数组中元素的索引）。
2.  属性或数组元素的值。
3.  类型（例如，字符串、数字、布尔值、数组或对象）。

OPENJSON 以单独的行返回 JSON 对象的每个属性或数组的每个元素。  

下面是使用具有默认架构（即不包含可选的 WITH 子句）的 OPENJSON 的快捷示例，该示例为 JSON 对象的每个属性返回一行。  

**示例**

```sql
DECLARE @json NVARCHAR(MAX)

SET @json='{"name":"John","surname":"Doe","age":45,"skills":["SQL","C#","MVC"]}';

SELECT *
FROM OPENJSON(@json);
```  
  
**结果**
  
|key|值|type|  
|---------|-----------|----------|  
|name|John|1|  
|surname|Doe|1|  
|age|45|2|  
|技能|["SQL","C#","MVC"]|4|

### <a name="more-info-about-openjson-with-the-default-schema"></a>有关具有默认架构的 OPENJSON 的详细信息

有关详细信息和更多示例，请参阅[使用具有默认架构的 OPENJSON (SQL Server)](../../relational-databases/json/use-openjson-with-the-default-schema-sql-server.md)。

有关语法和用法的信息，请参阅 [OPENJSON (Transact-SQL)](../../t-sql/functions/openjson-transact-sql.md)下可用。 

## <a name="option-2---openjson-output-with-an-explicit-structure"></a>选项 2 - 具有显式结构的 OPENJSON 输出

如果使用 **OPENJSON** 函数的 **WITH** 子句指定结果的架构，该函数返回的表只包含 **WITH** 子句中定义的列。 在可选的 WITH 子句中，指定一组输出列、列类型和每个输出值的 JSON 源属性的路径。 **OPENJSON** 循环访问 JSON 对象的数组，读取每一列的指定路径上的值，并将值转换为指定类型。  

下面是使用具有 WITH 子句中显式指定的输出架构的 OPENJSON 快捷示例。  
  
**示例**
  
```sql  
DECLARE @json NVARCHAR(MAX)
SET @json =   
  N'[  
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
   
SELECT * FROM  
 OPENJSON ( @json )  
WITH (   
              Number   varchar(200) '$.Order.Number' ,  
              Date     datetime     '$.Order.Date',  
              Customer varchar(200) '$.AccountNumber',  
              Quantity int          '$.Item.Quantity'  
 ) 
```  
  
**结果**
  
|Number|Date|客户|数量|  
|------------|----------|--------------|--------------|  
|SO43659|2011-05-31T00:00:00|AW29825|1|  
|SO43661|2011-06-01T00:00:00|AW73565|3|  
  
此函数返回 JSON 数组的元素并将其格式化。  
  
-   对于 JSON 数组中的每个元素， **OPENJSON** 会在输出表中生成新的一行。 JSON 数组中的两个元素将在返回的表中转换为两行。  
  
-   对于通过使用 `colName type json_path` 语法指定的每一列，OPENJSON 将指定路径上的每个数组元素中找到的值转换为指定类型。 在此示例中，`Date` 列的值获取自路径 `$.Order.Date` 上的每个元素，并被转换为日期时间值。  
  
### <a name="more-info-about-openjson-with-an-explicit-schema"></a>有关具有显式架构的 OPENJSON 的详细信息

有关详细信息和更多示例，请参阅[使用具有显示架构的 OPENJSON (SQL Server)](../../relational-databases/json/use-openjson-with-an-explicit-schema-sql-server.md)。

有关语法和用法的信息，请参阅 [OPENJSON (Transact-SQL)](../../t-sql/functions/openjson-transact-sql.md)下可用。

## <a name="openjson-requires-compatibility-level-130"></a>OPENJSON 要求兼容性级别 130

**OPENJSON** 函数仅在 **兼容级别 130** 下可用。 如果数据库兼容级别低于 130，SQL Server 将无法找到并运行 OPENJSON 函数。 其他内置 JSON 函数在所有兼容级别均可用。

可以在 `sys.databases` 视图或数据库属性中查看兼容级别。

可以使用以下命令更改数据库的兼容级别：   
`ALTER DATABASE <DatabaseName> SET COMPATIBILITY_LEVEL = 130`  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>详细了解 SQL Server 和 Azure SQL 数据库中的 JSON  
  
### <a name="microsoft-videos"></a>Microsoft 视频

有关 SQL Server 和 Azure SQL 数据库中内置 JSON 支持的视频介绍，请观看以下视频：

- [SQL Server 2016 和 JSON 支持](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

- [在 SQL Server 2016 和 Azure SQL 数据库中使用 JSON](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

- [JSON 充当 NoSQL 和关系环境之间的桥梁](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
  
## <a name="see-also"></a>另请参阅  
 [OPENJSON (Transact-SQL)](../../t-sql/functions/openjson-transact-sql.md)  
  

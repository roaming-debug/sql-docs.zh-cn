---
description: JSON_QUERY (Transact-SQL)
title: JSON_QUERY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- JSON_QUERY
- JSON_QUERY_TSQL
helpviewer_keywords:
- JSON, extracting
- JSON, querying
- JSON_QUERY function
ms.assetid: 1ab0d90f-19b6-4988-ab4f-22fdf28b7c79
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: ec281dac44455369bc1d7e8f53b2b5b2a7ff5bb0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102550"
---
# <a name="json_query-transact-sql"></a>JSON_QUERY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

 从 JSON 字符串中提取对象或数组。  
  
 若要从 JSON 字符串提取标量值而不是对象或数组，请参阅 [JSON_VALUE (Transact SQL)](../../t-sql/functions/json-value-transact-sql.md)。 有关 JSON_VALUE 和 JSON_QUERY 之间差异的信息，请参阅[比较 JSON_VALUE 和 JSON_QUERY](../../relational-databases/json/validate-query-and-change-json-data-with-built-in-functions-sql-server.md#JSONCompare) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
JSON_QUERY ( expression [ , path ] )  
```  
  
## <a name="arguments"></a>参数

 *expression*  
 一个表达式。 通常是包含 JSON 文本的变量或列的名称。  
  
 如果 JSON_QUERY 在找到由 path 标识的值之前，找到在 expression 中无效的 JSON，则函数会返回错误 。 如果 JSON_QUERY 找不到由 path 标识的值，则它会扫描整个文本，并且会在找到在 expression 中任何位置无效的 JSON 时返回错误 。  
  
 *路径*  
 指定要提取的对象或数组的 JSON 路径。

在 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 和 [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)] 中，可提供变量作为 path 的值。

JSON 路径可以为分析指定宽松或严格模式。 如果未指定分析模式，则宽松模式是默认值。 有关详细信息，请参阅 [JSON 路径表达式 (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)。  

path 的默认值是“$”。 因此，如果没有为 path 提供值，则 JSON_QUERY 会返回输入 expression。

如果 path 格式无效，则 JSON_QUERY 返回错误。  
  
## <a name="return-value"></a>返回值

 返回类型为 nvarchar(max) 的 JSON 片段。 返回值的排序规则与输入表达式的排序规则相同。  
  
 如果值不是对象或数组：  
  
- 在宽松模式下，**JSON_QUERY** 返回 NULL。  
  
- 在严格模式下，**JSON_QUERY** 返回错误。  
  
## <a name="remarks"></a>备注  

### <a name="lax-mode-and-strict-mode"></a>宽松模式和严格模式

 请参考以下 JSON 文本：  
  
```json  
{
   "info": {
      "type": 1,
      "address": {
         "town": "Bristol",
         "county": "Avon",
         "country": "England"
      },
      "tags": ["Sport", "Water polo"]
   },
   "type": "Basic"
} 
```  
  
 下表对宽松模式和严格模式下 **JSON_QUERY** 的行为进行了比较。 有关可选路径模式规范（宽松或严格）的详细信息，请参阅 [JSON 路径表达式 (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)。  
  
|路径|宽松模式下的返回值|严格模式下的返回值|更多信息|  
|----------|------------------------------|---------------------------------|---------------|  
|$|返回整个 JSON 文本。|返回整个 JSON 文本。|N/A|  
|$.info.type|Null|错误|不是对象或数组。<br /><br /> 改用 **JSON_VALUE**。|  
|$.info.address.town|Null|错误|不是对象或数组。<br /><br /> 改用 **JSON_VALUE**。|  
|$.info."address"|N'{ "town":"Bristol", "county":"Avon", "country":"England" }'|N'{ "town":"Bristol", "county":"Avon", "country":"England" }'|N/A|  
|$.info.tags|N'[ "Sport", "Water polo"]'|N'[ "Sport", "Water polo"]'|N/A|  
|$.info.type[0]|Null|错误|不是数组。|  
|$.info.none|Null|错误|属性不存在。|  

### <a name="using-json_query-with-for-json"></a>将 JSON_QUERY 与 FOR JSON 结合使用

**JSON_QUERY** 返回有效 JSON 片段。 因此，**FOR JSON** 不对 **JSON_QUERY** 返回值中的特殊字符进行转义。

如果在使用 FOR JSON 返回结果，并且包含已采用 JSON 格式（在列中或作为表达式的结果）的数据，则使用不带 path 参数的 JSON_QUERY 对数据进行包装。

## <a name="examples"></a>示例  
  
### <a name="example-1"></a>示例 1

 下面的示例演示如何在查询结果中从 `CustomFields` 列返回 JSON 片段。  
  
```sql  
SELECT PersonID,FullName,
  JSON_QUERY(CustomFields,'$.OtherLanguages') AS Languages
FROM Application.People
```  
  
### <a name="example-2"></a>示例 2

下面的示例演示如何在 FOR JSON 子句的输出中包含 JSON 片段。  
  
```sql  
SELECT StockItemID, StockItemName,
         JSON_QUERY(Tags) as Tags,
         JSON_QUERY(CONCAT('["',ValidFrom,'","',ValidTo,'"]')) ValidityPeriod
FROM Warehouse.StockItems
FOR JSON PATH
```  
  
## <a name="see-also"></a>另请参阅

- [JSON 路径表达式 (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)   
- [JSON 数据 (SQL Server)](../../relational-databases/json/json-data-sql-server.md)  

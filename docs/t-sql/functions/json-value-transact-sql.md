---
description: JSON_VALUE (Transact-SQL)
title: JSON_VALUE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- JSON_VALUE
- JSON_VALUE_TSQL
helpviewer_keywords:
- JSON_VALUE function
- JSON, extracting
- JSON, querying
ms.assetid: cd016e14-11eb-4eaf-bf05-c7cfcc820a10
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 9c6571a5938c503b99378003323168a695de58a9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98086740"
---
# <a name="json_value-transact-sql"></a>JSON_VALUE (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

 从 JSON 字符串中提取标量值。  
  
 若要从 JSON 字符串而不是标量值中提取对象或数组，请参阅 [JSON_QUERY (Transact-SQL)](../../t-sql/functions/json-query-transact-sql.md)。 有关 JSON_VALUE 和 JSON_QUERY 之间差异的信息，请参阅[比较 JSON_VALUE 和 JSON_QUERY](../../relational-databases/json/validate-query-and-change-json-data-with-built-in-functions-sql-server.md#JSONCompare) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
JSON_VALUE ( expression , path )  
```  
  
## <a name="arguments"></a>参数

 *expression*  
 一个表达式。 通常是包含 JSON 文本的变量或列的名称。  

 如果 JSON_VALUE 在找到由 path 标识的值之前，找到在 expression 中无效的 JSON，则函数会返回错误。 如果 JSON_VALUE 找不到路径标识的值，则它将扫描整个文本，如果在表达式中的任何位置发现无效的 JSON，则返回错误。
  
 *路径*  
 指定要提取属性的 JSON 路径。 有关详细信息，请参阅 [JSON 路径表达式 (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)。  

在 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 和 [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)] 中，可提供变量作为 path 的值。
  
 如果 path 格式无效，则 JSON_VALUE 返回错误。  
  
## <a name="return-value"></a>返回值

 返回 nvarchar(4000) 类型的单个文本值。 返回值的排序规则与输入表达式的排序规则相同。  
  
 如果值大于 4000 个字符：  
  
- 在宽松模式下，JSON_VALUE 返回 NULL。  
  
- 在严格模式下，JSON_VALUE 返回错误。  
  
 如果必须返回大于 4000 个字符的标量值，请使用 OPENJSON 而不是 JSON_VALUE。 有关详细信息，请参阅 [OPENJSON (Transact-SQL)](../../t-sql/functions/openjson-transact-sql.md)。  
  
## <a name="remarks"></a>备注

### <a name="lax-mode-and-strict-mode"></a>宽松模式和严格模式

 请参考以下 JSON 文本：  
  
```json  
DECLARE @jsonInfo NVARCHAR(MAX)

SET @jsonInfo=N'{  
     "info":{    
       "type":1,  
       "address":{    
         "town":"Bristol",  
         "county":"Avon",  
         "country":"England"  
       },  
       "tags":["Sport", "Water polo"]  
    },  
    "type":"Basic"  
 }'  
```  
  
 下表对宽松模式和严格模式下 JSON_VALUE 的行为进行了比较。 有关可选路径模式规范（宽松或严格）的详细信息，请参阅 [JSON 路径表达式 (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)。  
  
|路径|宽松模式下的返回值|严格模式下的返回值|更多信息|  
|----------|------------------------------|---------------------------------|---------------|  
|$|Null|错误|不是标量值。<br /><br /> 改用 JSON_QUERY。|  
|$.info.type|N'1'|N'1'|不适用|  
|$.info.address.town|N'Bristol'|N'Bristol'|不适用|  
|$.info."address"|Null|错误|不是标量值。<br /><br /> 改用 JSON_QUERY。|  
|$.info.tags|Null|错误|不是标量值。<br /><br /> 改用 JSON_QUERY。|  
|$.info.type[0]|Null|错误|不是数组。|  
|$.info.none|Null|错误|属性不存在。|  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; |
  
## <a name="examples"></a>示例  
  
### <a name="example-1"></a>示例 1
 以下示例在查询结果中使用 JSON 属性 `town` 和 `state` 的值。 由于 JSON_VALUE 保留了源的排序规则，因此结果的排序顺序取决于 `jsonInfo` 列的排序规则。 

> [!NOTE]
> （此示例假定名为 `Person.Person` 的表包含 JSON 文本的 `jsonInfo` 列，且此列具有先前宽松模式和严格模式讨论中所示的结构。 在 AdventureWorks 示例数据库中，`Person` 表实际上不包含 `jsonInfo` 列。）
  
```sql  
SELECT FirstName, LastName,
 JSON_VALUE(jsonInfo,'$.info.address[0].town') AS Town
FROM Person.Person
WHERE JSON_VALUE(jsonInfo,'$.info.address[0].state') LIKE 'US%'
ORDER BY JSON_VALUE(jsonInfo,'$.info.address[0].town')
```  
  
### <a name="example-2"></a>示例 2
 下面的示例将 JSON 属性 `town` 的值提取到本地变量。  
  
```sql
DECLARE @jsonInfo NVARCHAR(MAX)
DECLARE @town NVARCHAR(32)

SET @jsonInfo=N'{"info":{"address":[{"town":"Paris"},{"town":"London"}]}}';

SET @town=JSON_VALUE(@jsonInfo,'$.info.address[0].town'); -- Paris
SET @town=JSON_VALUE(@jsonInfo,'$.info.address[1].town'); -- London
```  
  
### <a name="example-3"></a>示例 3
 下面的示例基于 JSON 属性的值创建计算列。  
  
```sql  
CREATE TABLE dbo.Store
 (
  StoreID INT IDENTITY(1,1) NOT NULL,
  Address VARCHAR(500),
  jsonContent NVARCHAR(4000),
  Longitude AS JSON_VALUE(jsonContent, '$.address[0].longitude'),
  Latitude AS JSON_VALUE(jsonContent, '$.address[0].latitude')
 )
```  
  
## <a name="see-also"></a>另请参阅
 [JSON 路径表达式 (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)   
 [JSON 数据 (SQL Server)](../../relational-databases/json/json-data-sql-server.md)  
  

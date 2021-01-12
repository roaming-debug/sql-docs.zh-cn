---
description: JSON_MODIFY (Transact-SQL)
title: JSON_MODIFY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
ms.assetid: 96bc8255-a037-4907-aec4-1a9c30814651
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 547a304b4f5a9b6069d023a751f73d99e8456bbc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092219"
---
# <a name="json_modify-transact-sql"></a>JSON_MODIFY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  更新 JSON 字符串中属性的值，并返回已更新的 JSON 字符串。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
JSON_MODIFY ( expression , path , newValue )  
```  
  
## <a name="arguments"></a>参数

 *expression*  
 一个表达式。 通常是包含 JSON 文本的变量或列的名称。  
  
 如果 expression 不包含有效 JSON，则 JSON_MODIFY 返回错误。  
  
 *路径*  
 指定要更新的属性的 JSON 路径表达式。

 path 具有以下语法：  
  
 `[append] [ lax | strict ] $.<json path>`  
  
- *append*  
    指定应将新值追加到通过 \<json path> 引用的数组的可选修饰符。  
  
- *lax*  
    指定通过 \<json path> 引用的属性不是必须存在。 如果该属性不存在，则 JSON_MODIFY 尝试在指定路径上插入新值。 如果无法在路径上插入属性，则插入可能会失败。 如果未指定 lax 或 strict，则 lax 是默认模式。  
  
- *strict*  
    指定通过 \<json path> 引用的属性必须处于 JSON 表达式中。 如果该属性不存在，则 JSON_MODIFY 返回错误。  
  
- *\<json path>*  
    为要更新的属性指定路径。 有关详细信息，请参阅 [JSON 路径表达式 (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)。  
  
在 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 和 [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)] 中，可提供变量作为 path 的值。

如果 path 格式无效，则 JSON_MODIFY 返回错误。  
  
 *newValue*  
 path 指定的属性的新值。  
 新值必须是 [n]varchar 或 text。
  
 在宽松模式下，如果新值为 NULL，则 JSON_MODIFY 会删除指定键。  
  
如果值的类型是 NVARCHAR 或 VARCHAR，则 JSON_MODIFY 会对新值中的所有特殊字符进行转义。 如果文本值是由 FOR JSON、JSON_QUERY 或 JSON_MODIFY 生成的正确格式化 JSON，则它不会进行转义。  
  
## <a name="return-value"></a>返回值

 以正确格式化 JSON 文本的形式返回 expression 的更新值。  
  
## <a name="remarks"></a>备注

 通过 JSON_MODIFY 函数可以基于模式和所提供值的组合，更新现有属性的值、插入新的键:值对或删除键。  
  
 下表对宽松模式和严格模式下 **JSON_MODIFY** 的行为进行了比较。 有关可选路径模式规范（宽松或严格）的详细信息，请参阅 [JSON 路径表达式 (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)。  
  
|现有的值|路径存在|宽松模式|严格模式|  
|--------------------|-----------------|--------------|-----------------|  
|不为 NULL|是|更新现有值。|更新现有值。|  
|不为 NULL|否|尝试在指定路径上创建新的键:值对。<br /><br /> 这可能会失败。 例如，如果指定路径 `$.user.setting.theme`，则在 `$.user` 或 `$.user.settings` 对象不存在，或者设置是数组或标量值时，JSON_MODIFY 不会插入键 `theme`。|错误 - INVALID_PROPERTY|  
|Null|是|删除现有属性。|将现有值设置为 NULL。|  
|Null|否|无操作。 返回第一个参数作为结果。|错误 - INVALID_PROPERTY|  
  
 在宽松模式下，JSON_MODIFY 会尝试创建新的键:值对，但在某些情况下可能会失败。  
  
## <a name="examples"></a>示例  
  
### <a name="example---basic-operations"></a>示例 - 基本操作

 下面的示例演示可以对 JSON 文本进行的基本操作。  
  
 **查询**
  
```sql

DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Update name  

SET @info=JSON_MODIFY(@info,'$.name','Mike')

PRINT @info

-- Insert surname  

SET @info=JSON_MODIFY(@info,'$.surname','Smith')

PRINT @info

-- Delete name  

SET @info=JSON_MODIFY(@info,'$.name',NULL)

PRINT @info

-- Add skill  

SET @info=JSON_MODIFY(@info,'append $.skills','Azure')

PRINT @info
```  
  
 **结果**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "Mike",
    "skills": ["C#", "SQL"]
} {
    "name": "Mike",
    "skills": ["C#", "SQL"],
    "surname": "Smith"
} {
    "skills": ["C#", "SQL"],
    "surname": "Smith"
} {
    "skills": ["C#", "SQL", "Azure"],
    "surname": "Smith"
}
```  
  
### <a name="example---multiple-updates"></a>示例 - 多个更新

 使用 JSON_MODIFY 时，只能更新一个属性。 如果必须执行多个更新，则可以使用多个 JSON_MODIFY 调用。  
  
 **查询**
  
```sql
DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Multiple updates  

SET @info=JSON_MODIFY(JSON_MODIFY(JSON_MODIFY(@info,'$.name','Mike'),'$.surname','Smith'),'append $.skills','Azure')

PRINT @info
```  
  
 **结果**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "Mike",
    "skills": ["C#", "SQL", "Azure"],
    "surname": "Smith"
}
```  
  
### <a name="example---rename-a-key"></a>示例 - 重命名键  
 下面的示例演示如何使用 JSON_MODIFY 函数重命名 JSON 文本中的属性。 首先，可以获取现有属性的值，并将它作为新的键:值对插入。 随后可以通过将旧属性的值设置为 NULL 来删除旧键。  
  
 **查询**
  
```sql  
DECLARE @product NVARCHAR(100)='{"price":49.99}'

PRINT @product

-- Rename property  

SET @product=
 JSON_MODIFY(
  JSON_MODIFY(@product,'$.Price',CAST(JSON_VALUE(@product,'$.price') AS NUMERIC(4,2))),
  '$.price',
  NULL
 )

PRINT @product
```  
  
 **结果**
  
```json  
{
    "price": 49.99
} {
    "Price": 49.99
}
```  
  
 如果不将新值强制转换为数值类型，则 JSON_MODIFY 会将它视为文本，并用双引号将它括起。  
  
### <a name="example---increment-a-value"></a>示例 - 递增值

 下面的示例演示如何使用 JSON_MODIFY 函数递增 JSON 文本中的属性值。 首先，可以获取现有属性的值，并将它作为新的键:值对插入。 随后可以通过将旧属性的值设置为 NULL 来删除旧键。  
  
 **查询**
  
```sql  
DECLARE @stats NVARCHAR(100)='{"click_count": 173}'

PRINT @stats

-- Increment value  

SET @stats=JSON_MODIFY(@stats,'$.click_count',
 CAST(JSON_VALUE(@stats,'$.click_count') AS INT)+1)

PRINT @stats
```  
  
 **结果**
  
```json  
{
    "click_count": 173
} {
    "click_count": 174
}
```  
  
### <a name="example---modify-a-json-object"></a>示例 - 修改 JSON 对象

 JSON_MODIFY 将 newValue 参数视为纯文本（即使它包含正确格式化 JSON 文本）。 因此，函数的 JSON 输出会使用双引号括起，并且所有特殊字符都会进行转义，如下面的示例中所示。  
  
 **查询**  
  
```sql
DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Update skills array

SET @info=JSON_MODIFY(@info,'$.skills','["C#","T-SQL","Azure"]')

PRINT @info
```  
  
 **结果**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "John",
    "skills": "["C#","T-SQL","Azure"]"
}
```  
  
 若要避免自动转义，请使用 JSON_QUERY 函数提供 newValue。 JSON_MODIFY 知道 JSON_MODIFY 返回的值是正确格式化 JSON，因此它不会对值进行转义。  
  
 **查询**  
  
```sql
DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Update skills array  

SET @info=JSON_MODIFY(@info,'$.skills',JSON_QUERY('["C#","T-SQL","Azure"]'))

PRINT @info
```  
  
 **结果**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "John",
    "skills": ["C#", "T-SQL", "Azure"]
}
```  
  
### <a name="example---update-a-json-column"></a>示例 - 更新 JSON 列

 下面的示例更新包含 JSON 的表列中属性的值。  
  
```sql  
UPDATE Employee
SET jsonCol=JSON_MODIFY(jsonCol,'$.info.address.town','London')
WHERE EmployeeID=17
```  
  
## <a name="see-also"></a>另请参阅

- [JSON 路径表达式 (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)   
- [JSON 数据 (SQL Server)](../../relational-databases/json/json-data-sql-server.md)  
  

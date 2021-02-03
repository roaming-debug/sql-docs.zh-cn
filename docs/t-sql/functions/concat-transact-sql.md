---
description: CONCAT (Transact-SQL)
title: CONCAT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CONCAT
- CONCAT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CONCAT function
ms.assetid: fce5a8d4-283b-4c47-95e5-4946402550d5
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 68589c27d999bdddc0998773f80dcf17c4dcfec8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188781"
---
# <a name="concat-transact-sql"></a>CONCAT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

此函数以端到端的方式返回从串联或联接的两个或更多字符串值生成的字符串。 （若要在串联过程中添加分隔值，请参阅 [CONCAT_WS](../../t-sql/functions/concat-ws-transact-sql.md)。）
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CONCAT ( string_value1, string_value2 [, string_valueN ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
string_value  
要与其他值串联的字符串值。 `CONCAT` 函数需要至少两个 string_value 自变量，并且不得超过 254 个 string_value 自变量。
  
## <a name="return-types"></a>返回类型  
string_value  
长度和类型取决于输入的字符串值。
  
## <a name="remarks"></a>备注  
`CONCAT` 采用可变数量的字符串自变量，并将它们串联（或联接）成单个字符串。 需要至少两个输入值；否则 `CONCAT` 将引发错误。 `CONCAT` 在串联前会将所有自变量隐式转换为字符串类型。 `CONCAT` 会将 Null 值隐式转换为空字符串。 如果 `CONCAT` 接收到全部为 NULL 值的自变量，它将返回类型为 varchar(1) 的空字符串 。 隐式转换为字符串的过程遵循现有的数据类型转换规则。 有关数据类型转换的详细信息，请参阅 [CAST 和 CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)。
  
返回类型取决于参数的类型。 此表将说明这一映射：
  
|输入类型|输出类型和长度|  
|---|---|
|1.以下类型的任何自变量<br><br />SQL-CLR 系统类型<br><br />SQL-CLR UDT<br><br />或<br><br />`nvarchar(max)`|**nvarchar(max)**|  
|2.或者为以下类型的任何自变量<br><br />**varbinary(max)**<br><br />或<br><br />**varchar(max)**|varchar(max)，除非其中一个参数是任意长度的 nvarchar 。 在这种情况下，`CONCAT` 将返回类型 nvarchar(max) 的结果。|  
|3.或者为类型 nvarchar 的任何自变量，最多为 4000 个字符<br><br />(nvarchar(<= 4000) )|**nvarchar**(<= 4000)|  
|4.在所有其他情况下|varchar(<= 8000)（最多为 8000 个字符的 varchar），除非其中一个参数是任意长度的 nvarchar。 在那种情况下，`CONCAT` 将返回类型 nvarchar(max) 的结果。|  
  
当 `CONCAT` 接收到长度小于或等于 4000 个字符的 nvarchar 输入自变量或长度小于或等于 8000 个字符的 varchar 输入自变量时，隐式转换可能会影响结果的长度。 当将其他数据类型隐式转换为字符串时，它们具有不同长度。 例如，int (14) 的字符串长度为 12，而 float 的长度为 32。 因此，两个整数的串联将返回长度小于 24 的结果。
  
如果没有任何输入自变量具有支持的大型对象 (LOB) 类型，返回类型的长度则截断为 8000 个字符，而不考虑返回类型。 这种截断可节约空间并支持计划生成的效率。
  
可以在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和更高版本的链接服务器上远程执行 CONCAT 函数。 对于较低版本的链接服务器，在链接服务器返回非串联值后，将在本地执行 CONCAT 操作。
  
## <a name="examples"></a>示例  
  
### <a name="a-using-concat"></a>A. 使用 CONCAT  
  
```sql
SELECT CONCAT ( 'Happy ', 'Birthday ', 11, '/', '25' ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
-------------------------  
Happy Birthday 11/25  
  
(1 row(s) affected)  
```  
  
### <a name="b-using-concat-with-null-values"></a>B. 使用 CONCAT 以及 NULL 值  
  
```sql
CREATE TABLE #temp (  
    emp_name NVARCHAR(200) NOT NULL,  
    emp_middlename NVARCHAR(200) NULL,  
    emp_lastname NVARCHAR(200) NOT NULL  
);  
INSERT INTO #temp VALUES( 'Name', NULL, 'Lastname' );  
SELECT CONCAT( emp_name, emp_middlename, emp_lastname ) AS Result  
FROM #temp;  
```  

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
------------------  
NameLastname  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅
 [CONCAT_WS (Transact-SQL)](../../t-sql/functions/concat-ws-transact-sql.md)   
 [FORMATMESSAGE (Transact-SQL)](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME (Transact-SQL)](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE (Transact-SQL)](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE (Transact-SQL)](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG (Transact-SQL)](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE (Transact-SQL)](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF (Transact-SQL)](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE (Transact-SQL)](../../t-sql/functions/translate-transact-sql.md)  
 [字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)  
  



---
description: CONCAT_WS (Transact-SQL)
title: CONCAT_WS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/25/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- CONCAT_WS
- CONCAT_WS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CONCAT_WS function
ms.assetid: f1375fd7-a2fd-48bf-922a-4f778f0deb1f
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: f471f29f14a1d1d2044ffc82874836d0c5d17db0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440318"
---
# <a name="concat_ws-transact-sql"></a>CONCAT_WS (Transact-SQL)
[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

此函数以端到端的方式返回从串联或联接的两个或更多字符串值生成的字符串。 它会用第一个函数参数中指定的分隔符分隔连接的字符串值。 （`CONCAT_WS` 指示使用分隔符连接。） 

##  <a name="syntax"></a>语法   
```syntaxsql
CONCAT_WS ( separator, argument1, argument2 [, argumentN]... )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
分隔符：任何字符类型的表达式（`char`、`nchar``nvarchar` 或 `varchar`）。

argument1、argument2、argumentN：任何类型的表达式。 `CONCAT_WS` 函数需要至少两个自变量，并且不得超过 254 个自变量。

## <a name="return-types"></a>返回类型
长度和类型取决于输入的字符串值。

## <a name="remarks"></a>备注   
`CONCAT_WS` 采用可变数量的字符串自变量，并将它们串联（或联接）成单个字符串。 它会用第一个函数参数中指定的分隔符分隔连接的字符串值。 `CONCAT_WS` 需要分隔符参数和两个其他字符串值参数的最小值，否则，`CONCAT_WS` 将引发错误。 `CONCAT_WS` 在串联前会将所有自变量隐式转换为字符串类型。 

隐式转换为字符串的过程遵循现有的数据类型转换规则。 请参阅 [CONCAT (Transact SQL)](../../t-sql/functions/concat-transact-sql.md) 获取有关行为和数据类型转换的详细信息。

### <a name="treatment-of-null-values"></a>NULL 值处理方式

`CONCAT_WS` 忽略 `SET CONCAT_NULL_YIELDS_NULL {ON|OFF}` 设置。

如果 `CONCAT_WS` 接收到全部为 NULL 值的自变量，它将返回类型为 varchar(1) 的空字符串。

串联过程中 `CONCAT_WS` 会忽略 Null 值，并且不会在 null 值之间添加分隔符。 因此，`CONCAT_WS` 可以完全处理可能具有“空”值（例如，次要地址字段）的字符串串联。 请参阅示例 B 获取详细信息。

如果方案涉及由分隔符分隔的 null 值，请考虑 `ISNULL` 函数。 请参阅示例 C 获取详细信息。

## <a name="examples"></a>示例   

### <a name="a--concatenating-values-with-separator"></a>A.  使用分隔符连接值
此示例串联 sys.databases 表中的三列，并使用 `-` 分隔这些值。   

```sql
SELECT CONCAT_WS( ' - ', database_id, recovery_model_desc, containment_desc) AS DatabaseInfo
FROM sys.databases;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   

|DatabaseInfo |  
|---------|
|1 - SIMPLE - NONE |
|2 - SIMPLE - NONE |
|3 - FULL - NONE |
|4 - SIMPLE - NONE |


### <a name="b--skipping-null-values"></a>B.  跳过 NULL 值
此示例忽略参数列表中的 `NULL` 值。

```sql
SELECT CONCAT_WS(',','1 Microsoft Way', NULL, NULL, 'Redmond', 'WA', 98052) AS Address;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   

```
Address
------------   
1 Microsoft Way,Redmond,WA,98052
```

### <a name="c--generating-csv-file-from-table"></a>C.  从表中生成 CSV 文件
此示例使用逗号 `,` 作为分隔符，并在结果集的列分隔值格式中添加回车符 `char(13)`。

```sql
SELECT 
STRING_AGG(CONCAT_WS( ',', database_id, recovery_model_desc, containment_desc), char(13)) AS DatabaseInfo
FROM sys.databases
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   

```
DatabaseInfo
------------   
1,SIMPLE,NONE
2,SIMPLE,NONE
3,FULL,NONE 
4,SIMPLE,NONE 
```

CONCAT_WS 会忽略列中的 NULL 值。 用 `ISNULL` 函数包装可以为 null 的列，并提供默认值。 参阅此示例获取详细信息：

```sql
SELECT 
STRING_AGG(CONCAT_WS( ',', database_id, ISNULL(recovery_model_desc,''), ISNULL(containment_desc,'N/A')), char(13)) AS DatabaseInfo
FROM sys.databases;
```

## <a name="see-also"></a>另请参阅
 [CONCAT (Transact-SQL)](../../t-sql/functions/concat-transact-sql.md)  
 [FORMATMESSAGE (Transact-SQL)](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME (Transact-SQL)](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE (Transact-SQL)](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE (Transact-SQL)](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG (Transact-SQL)](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE (Transact-SQL)](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF (Transact-SQL)](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE (Transact-SQL)](../../t-sql/functions/translate-transact-sql.md)  
 [字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)  


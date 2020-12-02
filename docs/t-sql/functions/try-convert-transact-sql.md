---
description: TRY_CONVERT (Transact-SQL)
title: TRY_CONVERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- TRY_CONVERT_TSQL
- TRY_CONVERT
dev_langs:
- TSQL
helpviewer_keywords:
- TRY_CONVERT function
ms.assetid: 3e6e7825-6482-4cb2-a8c2-9abc99e265a6
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current||>= sql-server-2016 ||>= sql-server-linux-2017||= sqlallproducts-allversions||>= aps-pdw-2016||= azure-sqldw-latest
ms.openlocfilehash: 82c8807aef206867a8f50eed507e7a3a4cb48e59
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91379502"
---
# <a name="try_convert-transact-sql"></a>TRY_CONVERT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回转换为指定数据类型的值（如果转换成功）；否则返回 Null。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
TRY_CONVERT ( data_type [ ( length ) ], expression [, style ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 data_type [ ( length ) ]  
 要将 expression 强制转换为的数据类型。  
  
 *expression*  
 要强制转换的值。  
  
 style  
 一个可选的整数表达式，指定 TRY_CONVERT 函数如何转换 expression。  
  
 style 接受与 CONVERT 函数的 style 参数相同的值。 有关详细信息，请参阅 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)。  
  
 可接受值的范围由 data_type 的值确定。 如果 style 为 NULL，则 TRY_CONVERT 返回 NULL。  
  
## <a name="return-types"></a>返回类型  
 返回转换为指定数据类型的值（如果转换成功）；否则返回 Null。  
  
## <a name="remarks"></a>注解  
 TRY_CONVERT 接收传递给它的值，并尝试将该值转换为指定的 data_type。 如果强制转换成功，TRY_CONVERT 按指定的 data_type 返回值；如果发生错误，则返回 NULL。 但是，如果请求的转换是显式不允许执行的转换，则 TRY_CONVERT 失败并显示错误。  
  
 TRY_CONVERT 是兼容级别 110 和更高级别中的保留关键字。  
  
 此功能可以在某个 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 版本以及更高版本的服务器上远程执行。 但在低于 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 的服务器版本中无法远程执行。  
  
## <a name="examples"></a>示例  
  
### <a name="a-try_convert-returns-null"></a>A. TRY_CONVERT 返回 null  
 下面的示例演示转换失败时 TRY_CONVERT 返回 null。  
  
```sql  
SELECT   
    CASE WHEN TRY_CONVERT(float, 'test') IS NULL   
    THEN 'Cast failed'  
    ELSE 'Cast succeeded'  
END AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
------------  
Cast failed  
  
(1 row(s) affected)  
```  
  
 以下示例演示表达式必须采用所需的格式。  
  
```sql  
SET DATEFORMAT dmy;  
SELECT TRY_CONVERT(datetime2, '12/31/2010') AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
----------------------  
NULL  
  
(1 row(s) affected)  
```  
  
### <a name="b-try_convert-fails-with-an-error"></a>B. TRY_CONVERT 将失败，并出现错误  
 下面的示例演示明确不允许转换时 TRY_CONVERT 返回错误。  
  
```sql  
SELECT TRY_CONVERT(xml, 4) AS Result;  
GO  
```  
  
 此语句的结果是一个错误，因为整数无法转换为 xml 数据类型。  
  
```  
Explicit conversion from data type int to xml is not allowed.  
```  
  
### <a name="c-try_convert-succeeds"></a>C. TRY_CONVERT 成功  
 此示例演示表达式必须采用所需的格式。  
  
```sql
SET DATEFORMAT mdy;  
SELECT TRY_CONVERT(datetime2, '12/31/2010') AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
----------------------------------  
2010-12-31 00:00:00.0000000  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅  
 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
  

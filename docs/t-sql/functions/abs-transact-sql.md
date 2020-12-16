---
description: ABS (Transact-SQL)
title: ABS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ABS_TSQL
- ABS
dev_langs:
- TSQL
helpviewer_keywords:
- values [SQL Server], positive
- values [SQL Server], absolute
- ABS function
- absolute positive value
ms.assetid: e2ea7a6d-3e2f-472c-afbc-437d3b835c03
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bc1b312febf7f9718f7a6b2737d25e7a638577eb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472168"
---
# <a name="abs-transact-sql"></a>ABS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

返回指定数值表达式的绝对值（正值）的数学函数。 （`ABS` 将负值更改为正值。 `ABS` 对零或正值没有影响。）
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ABS ( numeric_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*numeric_expression*  
精确数值或近似数值数据类型类别的表达式。
  
## <a name="return-types"></a>返回类型  
返回与 numeric_expression 相同的类型。
  
## <a name="examples"></a>示例  
此示例显示了对三个不同数字使用 `ABS` 函数所得的结果。
  
```sql
SELECT ABS(-1.0), ABS(0.0), ABS(1.0);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---- ---- ----  
1.0  .0   1.0  
```  
  
当一个数的绝对值超出指定数据类型所能表示的最大数时，`ABS` 函数可能产生溢出错误。 例如，`int` 数据类型的值范围是 `-2,147,483,648` 到 `2,147,483,647`。 计算有符号整数 `-2,147,483,648` 的绝对值将导致溢出错误，因为其绝对值已超出 `int` 数据类型的正值范围限制。
  
```sql
DECLARE @i INT;  
SET @i = -2147483648;  
SELECT ABS(@i);  
GO  
```  
  
返回以下错误消息：
  
“Msg 8115，级别 16，状态 2，第 3 行”
  
“将表达式转换为数据类型 int 时发生算术溢出错误。”

  
## <a name="see-also"></a>另请参阅
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)  
[数学函数 (Transact-SQL)](../../t-sql/functions/mathematical-functions-transact-sql.md)  
[内置函数 (Transact-SQL)](../../t-sql/functions/functions.md)
  
  


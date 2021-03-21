---
description: FLOOR (Transact-SQL)
title: FLOOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: synapse-analytics, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FLOOR_TSQL
- FLOOR
dev_langs:
- TSQL
helpviewer_keywords:
- integers [SQL Server]
- largest integers
- FLOOR function [Transact-SQL]
ms.assetid: 4f26c784-9240-491f-b854-754be3fccae4
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 191d1148b0e582cbbaa62497d0489032ac6b3fcb
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747267"
---
# <a name="floor-transact-sql"></a>FLOOR (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回小于或等于指定数值表达式的最大整数。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
FLOOR ( numeric_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *numeric_expression*  
 是精确数值或近似数值数据类型类别（bit 数据类型除外）的表达式。  
  
## <a name="return-types"></a>返回类型  
 返回与 numeric_expression 相同的类型。  
  
## <a name="examples"></a>示例  
 以下示例显示正数、负数和货币值在 `FLOOR` 函数中的运用。  
  
```sql  
SELECT FLOOR(123.45), FLOOR(-123.45), FLOOR($123.45);  
```  
  
 结果是与 numeric_expression数据类型相同的计算所得值的整数部分。  
  
```  
---------      ---------     -----------  
123            -124          123.0000     
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 以下示例显示正数、负数和值在 `FLOOR` 函数中的运用。  
  
```sql  
SELECT FLOOR(123.45), FLOOR(-123.45), FLOOR($123.45);  
```  
  
 结果是与 numeric_expression数据类型相同的计算所得值的整数部分。  
  
 ```
 -----   ---------    -----------  
  
 123     -124         123
 ```  
  
## <a name="see-also"></a>另请参阅  
 [数学函数 (Transact-SQL)](../../t-sql/functions/mathematical-functions-transact-sql.md)  
  
  


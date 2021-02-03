---
description: 排序规则函数 - TERTIARY_WEIGHTS (Transact-SQL)
title: TERTIARY_WEIGHTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TERTIARY_WEIGHTS_TSQL
- TERTIARY_WEIGHTS
dev_langs:
- TSQL
helpviewer_keywords:
- weights [SQL Server]
- SQL tertiary collations
- TERTIARY_WEIGHTS function
ms.assetid: 7e1f5350-260b-4c61-8c84-69bb1a214f1f
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6d5b6e88f49b2452184a77ebd103e5050efe33a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210001"
---
# <a name="collation-functions---tertiary_weights-transact-sql"></a>排序规则函数 - TERTIARY_WEIGHTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

对于非 Unicode 字符串表达式中的每个字符（使用 SQL 第三排序规则定义），此函数返回一个表示权重的二进制字符串。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
TERTIARY_WEIGHTS( non_Unicode_character_string_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
non_Unicode_character_string_expression  
根据 SQL 第三排序规则定义的 char、varchar 或 varchar(max) 类型的字符串[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 有关上述排序规则的列表，请参阅注释。
  
## <a name="return-types"></a>返回类型
non_Unicode_character_string_expression 是 char 或 varchar 时，`TERTIARY_WEIGHTS` 返回 varbinary；non_Unicode_character_string_expression 具有 varchar(max) 数据类型时，返回 varbinary(max)。
  
## <a name="remarks"></a>备注  
SQL 第三排序规则未定义 non_Unicode_character_string_expression 时，`TERTIARY_WEIGHTS` 返回 NULL。 下表显示了 SQL 第三排序规则：
  
|排序次序 ID|SQL 排序规则|  
|---|---|
|33|SQL_Latin1_General_Pref_CP437_CI_AS|  
|34|SQL_Latin1_General_CP437_CI_AI|  
|43|SQL_Latin1_General_Pref_CP850_CI_AS|  
|44|SQL_Latin1_General_CP850_CI_AI|  
|49|SQL_1xCompat_CP850_CI_AS|  
|53|SQL_Latin1_General_Pref_CP1_CI_AS|  
|54|SQL_Latin1_General_CP1_CI_AI|  
|56|SQL_AltDiction_Pref_CP850_CI_AS|  
|57|SQL_AltDiction_CP850_CI_AI|  
|58|SQL_Scandinavian_Pref_CP850_CI_AS|  
|82|SQL_Latin1_General_CP1250_CI_AS|  
|84|SQL_Czech_CP1250_CI_AS|  
|86|SQL_Hungarian_CP1250_CI_AS|  
|88|SQL_Polish_CP1250_CI_AS|  
|90|SQL_Romanian_CP1250_CI_AS|  
|92|SQL_Croatian_CP1250_CI_AS|  
|94|SQL_Slovak_CP1250_CI_AS|  
|96|SQL_Slovenian_CP1250_CI_AS|  
|106|SQL_Latin1_General_CP1251_CI_AS|  
|108|SQL_Ukrainian_CP1251_CI_AS|  
|113|SQL_Latin1_General_CP1253_CS_AS|  
|114|SQL_Latin1_General_CP1253_CI_AS|  
|130|SQL_Latin1_General_CP1254_CI_AS|  
|146|SQL_Latin1_General_CP1256_CI_AS|  
|154|SQL_Latin1_General_CP1257_CI_AS|  
|156|SQL_Estonian_CP1257_CI_AS|  
|158|SQL_Latvian_CP1257_CI_AS|  
|160|SQL_Lithuanian_CP1257_CI_AS|  
|183|SQL_Danish_Pref_CP1_CI_AS|  
|184|SQL_SwedishPhone_Pref_CP1_CI_AS|  
|185|SQL_SwedishStd_Pref_CP1_CI_AS|  
|186|SQL_Icelandic_Pref_CP1_CI_AS|  
  
使用 `TERTIARY_WEIGHTS` 来定义根据 char、varchar 或 varchar(max) 列的值定义的计算列。 当查询的 ORDER BY 子句指定 char、varchar 或 varchar(max) 列时，计算列和 char、varchar 或 varchar(max) 列上的索引定义可以提升性能。
  
## <a name="examples"></a>示例  
本示例在将 `TERTIARY_WEIGHTS` 函数应用于 `char` 列的值的表中创建计算列：
  
```sql
CREATE TABLE TertColTable  
(Col1 char(15) COLLATE SQL_Latin1_General_Pref_CP437_CI_AS,  
Col2 AS TERTIARY_WEIGHTS(Col1));  
GO   
```  
  
## <a name="see-also"></a>另请参阅
[ORDER BY 子句 (Transact-SQL)](../../t-sql/queries/select-order-by-clause-transact-sql.md)
  
  

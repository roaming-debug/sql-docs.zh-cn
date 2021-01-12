---
description: +=（加法赋值）(Transact-SQL)
title: +=（加法赋值）(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- +=
- +=_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- += (add equals)
- compound operators, +=
- assignment operators, +=
- augmented operators, +=
ms.assetid: 9ea52519-80d1-473f-b988-0572f0e2c92f
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1c140365e95935719653805c1ce3e7a631fdbacf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101062"
---
# <a name="-addition-assignment-transact-sql"></a>+=（加法赋值）(Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  将两个数字相加并将值设置为该运算的结果。 例如，如果变量 @x 等于 35，则 @x += 2 会将 @x 的原始值加上 2 并将 @x 设置为该新值 (37)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
expression += expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *expression*  
 数值类别中任何数据类型（bit 数据类型除外）的任何有效[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
## <a name="result-types"></a>结果类型  
 返回优先级较高的参数的数据类型。 有关详细信息，请参阅[数据类型优先级 (Transact-SQL)](../../t-sql/data-types/data-type-precedence-transact-sql.md)。  
  
## <a name="remarks"></a>备注  
 有关详细信息，请参阅 [+（加法）(Transact-SQL)](../../t-sql/language-elements/add-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [复合运算符 (Transact-SQL)](../../t-sql/language-elements/compound-operators-transact-sql.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [+=（字符串串联赋值）(Transact-SQL)](../../t-sql/language-elements/string-concatenation-equal-transact-sql.md)  
  
  

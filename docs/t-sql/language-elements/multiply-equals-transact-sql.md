---
description: '*=（乘法赋值）(Transact-SQL)'
title: '*=（乘法赋值）(Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '*=_TSQL'
- '*='
dev_langs:
- TSQL
helpviewer_keywords:
- compound operators, *=
- assignment operators, *=
- augmented operators, *=
- '*= (multiply equals)'
- '*= (multiplication assignment)'
ms.assetid: 816ff5dc-9a40-4c07-8351-39c194dbc079
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 96b003987e52dbb6bf20f3606833f9d33355e727
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128224"
---
# <a name="-multiplication-assignment-transact-sql"></a>*=（乘法赋值）(Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

将两个数字相乘并将值设置为该运算的结果。 例如，如果变量 @x 等于 35，则 @x *= 2 会将 @x 的原始值乘以 2 并将 @x 设置为该新值 (70)。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
expression *= expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
_expression_  
数值类别中的任意有效数据类型[表达式](../../t-sql/language-elements/expressions-transact-sql.md)（bit 类型除外）。  
  
## <a name="result-types"></a>结果类型  
返回优先级较高的参数的数据类型。 有关详细信息，请参阅[数据类型优先级 (Transact-SQL)](../../t-sql/data-types/data-type-precedence-transact-sql.md)。  
  
## <a name="remarks"></a>备注  
有关详细信息，请参阅[（乘法）(Transact-SQL)](../../t-sql/language-elements/multiply-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
[复合运算符 (Transact-SQL)](../../t-sql/language-elements/compound-operators-transact-sql.md)   
[表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
[运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)  
  
  

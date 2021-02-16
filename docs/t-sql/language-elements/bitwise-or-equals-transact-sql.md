---
description: '|=（位异或赋值）(Transact-SQL)'
title: '|=（位异或）'
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 01/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '|='
- '|=_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- compound operators, |=
- assignment operators, |=
- augmented operators, |=
- '|= (bitwise OR equals)'
ms.assetid: bd746a4f-6498-4196-bf2e-b6f457a15d44
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 17019a138128c092a7855a7f3e3587a14edee82a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345275"
---
# <a name="-bitwise-or-assignment-transact-sql"></a>|=（位异或赋值）(Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  将 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中的两个指定整数值转换为二进制表达式后执行“逻辑位或”运算，并将一个值设置为运算的结果。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
expression |= expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *expression*  
 数值类别中任意数据类型（bit 数据类型除外）的任何有效[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
## <a name="result-types"></a>结果类型  
 返回优先级较高的参数的数据类型。 有关详细信息，请参阅[数据类型优先级 (Transact-SQL)](../../t-sql/data-types/data-type-precedence-transact-sql.md)。  
  
## <a name="remarks"></a>备注  
 有关详细信息，请参阅 [&#124;（位异或）(Transact-SQL)](../../t-sql/language-elements/bitwise-or-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [复合运算符 (Transact-SQL)](../../t-sql/language-elements/compound-operators-transact-sql.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [位运算符 (Transact-SQL)](../../t-sql/language-elements/bitwise-operators-transact-sql.md)  
  
  

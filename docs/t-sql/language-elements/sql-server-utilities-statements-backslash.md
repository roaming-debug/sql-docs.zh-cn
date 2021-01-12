---
description: 反斜杠（行继续符）(Transact SQL)
title: 反斜杠（行继续符）(Transact SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '\_TSQL'
- '\'
dev_langs:
- TSQL
helpviewer_keywords:
- backwhack
- backslash
- escape character
- hack character
- '\ (backslash)'
- backslant
- bash
- reverse slant
- slosh
- reversed virgule
- line continuation character
- reverse solidus
ms.assetid: c97fbb20-3d12-4d0b-9b52-62a229bc83c0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3451d6a9346fd1988cdfc509635967017a0b6cb9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095908"
---
# <a name="backslash-line-continuation-transact-sql"></a>反斜杠（行继续符）(Transact SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

`\` 将一个长字符串常量、字符或二进制分成两行或更多行，以方便阅读。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
<first section of string> \  
<continued section of string>  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 \<first section of string>  
 是字符串的开头部分。  
  
 \<continued section of string>  
 是字符串的后面部分。  
  
## <a name="remarks"></a>备注  
该命令将字符串的第一部分和后续部分作为一个字符串返回，中间没有反斜杠。 反斜杠后的新行必须是换行符 (U + 000A) 或回车符 (U + 000D) 和换行符 (U + 000A) 的组合。 

## <a name="examples"></a>示例  

### <a name="a-splitting-a-character-string"></a>A. 拆分字符串  

以下示例使用反斜杠和回车符将字符串分成两行。  
  
```sql  
SELECT 'abc\  
def' AS [ColumnResult];  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 ColumnResult  
 ------------  
 abcdef
 ```    

### <a name="b-splitting-a-binary-string"></a>B. 拆分二进制字符串  

以下示例使用反斜杠和回车符将二进制字符串分成两行。  

```sql  
SELECT 0xabc\
def AS [ColumnResult];  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 ColumnResult  
 ------------  
 0xABCDEF
 ```    

## <a name="see-also"></a>另请参阅  
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [(Division) (Transact-SQL)](../../t-sql/language-elements/divide-transact-sql.md)   
 [（除法赋值）(Transact-SQL)](../../t-sql/language-elements/divide-equals-transact-sql.md)   
 [复合运算符 (Transact-SQL)](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  

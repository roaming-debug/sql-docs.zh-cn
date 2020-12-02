---
description: 位运算符 (Transact-SQL)
title: 位运算符 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- operators [Transact-SQL], bitwise
- bitwise operators
- bit manipulations
ms.assetid: 2b994cf5-2daa-438a-b8c7-4bd8d451ac8d
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 15649f0ff6a9695b17af629f28156ea4860e4aa6
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128325"
---
# <a name="bitwise-operators-transact-sql"></a>位运算符 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  位运算符在两个表达式之间执行位操作，这两个表达式可以为整数数据类型类别中的任何数据类型。  
  位运算符将两个整数值转换为二进制位，对每个位执行 AND、OR 或 NOT 操作并得出结果。 然后将结果转换为整数。  
  
  例如，整数 170 转换为二进制是 1010 1010。
整数 75 转换为二进制是 0100 1011。

|运算符后的表达式|位运算|
|---- |---- |
|AND <br> 如果两个位置上的位均为 1，则结果为 1。 |1010 1010 = 170 <br>0100 1011 =  75 <br>-----------------  <br> 0000 1010 =  10 |
|或 <br> 如果两个位置上任意一个位置的位为 1，则结果为 1。 |1010 1010 = 170 <br>0100 1011 =  75 <br>-----------------  <br> 1110 1011 = 235|
|NOT  <br> 对每个位位置上的位值取反。 |1010 1010 = 170 <br>----------------- <br>  0101 0101 =   85 |
  
请参阅下列主题：   
* [&（位与）](../../t-sql/language-elements/bitwise-and-transact-sql.md)  
* [&=（位与赋值）](../../t-sql/language-elements/bitwise-and-equals-transact-sql.md)   
* [&#124;（位或）](../../t-sql/language-elements/bitwise-or-transact-sql.md)  
* [&#124;=（位或赋值）](../../t-sql/language-elements/bitwise-or-equals-transact-sql.md)   
* [^（位异或）](../../t-sql/language-elements/bitwise-exclusive-or-transact-sql.md)  
* [^=（位异或赋值）](../../t-sql/language-elements/bitwise-exclusive-or-equals-transact-sql.md)  
* [~（位非）](../../t-sql/language-elements/bitwise-not-transact-sql.md)  
  
 位运算符的操作数可以是整数或二进制字符串数据类型类别中的任何数据类型（image 数据类型除外），但两个操作数不能同时是二进制字符串数据类型类别中的某种数据类型。 下表显示所支持的操作数数据类型。  
  
|左操作数|右操作数|  
|------------------|-------------------|  
|[binary](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)|int、smallint 或 tinyint  |  
|[bit](../../t-sql/data-types/bit-transact-sql.md)|int、smallint、tinyint 或 bit|  
|[bigint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|bigint、int、smallint、tinyint、binary 或 varbinary|  
|[int](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|int、smallint、tinyint、binary 或 varbinary|  
|[smallint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|int、smallint、tinyint、binary 或 varbinary|  
|[tinyint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|int、smallint、tinyint、binary 或 varbinary|  
|[varbinary](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)|int、smallint 或 tinyint  |  
  
## <a name="see-also"></a>另请参阅  
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [复合运算符 (Transact-SQL)](../../t-sql/language-elements/compound-operators-transact-sql.md)

---
title: int、bigint、smallint 和 tinyint (Transact-SQL) | Microsoft Docs
description: int、bigint、smallint 和 tinyint 数据类型的 Transact-SQL 参考。 这些数据类型用于表示整数数据。
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- bigint_TSQL
- smallint
- bigint
- smallint_TSQL
- tinyint_TSQL
- int_TSQL
- int
- tinyint
dev_langs:
- TSQL
helpviewer_keywords:
- exact numeric data [SQL Server]
- numeric data
- tinyint data type
- int data type
- smallint data type
ms.assetid: 9bda5b0b-2380-4931-a1c8-f362fdefa99b
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a388e1cf13ce0de99a51b099e79e94dafa9dc0a4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466178"
---
# <a name="int-bigint-smallint-and-tinyint-transact-sql"></a>int、bigint、smallint 和 tinyint (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

使用整数数据的精确数字数据类型。 若要节省数据库空间，请使用能够可靠包含所有可能值的最小数据类型。 例如，对于一个人的年龄，tinyint 就足够了，因为没人活到 255 岁以上。 但对于建筑物的年龄，tinyint 就不再适应，因为建筑物的年龄可能超过 255 年。
  
|数据类型|范围|存储|  
|---|---|---|
|**bigint**|-2^63 (-9,223,372,036,854,775,808) 到 2^63-1 (9,223,372,036,854,775,807)|8 字节|  
|**int**|-2^31 (-2,147,483,648) 到 2^31-1 (2,147,483,647)|4 个字节|  
|**smallint**|-2^15 (-32,768) 到 2^15-1 (32,767)|2 字节|  
|**tinyint**|0 到 255|1 字节|  
  
## <a name="remarks"></a>备注  
int 数据类型是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的主要整数数据类型  。 bigint 数据类型用于整数值可能超过 int 数据类型支持范围的情况   。
  
在数据类型优先次序表中，bigint 介于 smallmoney 和 int 之间    。
  
仅当参数表达式为 bigint 数据类型时，函数才返回 bigint   。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不会自动将其他整数数据类型（tinyint、smallint 和 int）提升到 bigint     。
  
> [!CAUTION]  
>  使用 +、-、\*、/ 或 % 等算术运算符将 int、smallint、tinyint 或 bigint 常量值隐式或显式转换为 float、real、decimal 或 numeric 数据类型时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算数据类型和表达式结果的精度时应用的规则有所不同，这取决于查询是否是自动参数化的         。  
>   
>  因此，查询中的类似表达式有时可能会生成不同的结果。 如果查询不是自动参数化的，则将常量值转换为指定的数据类型之前，首先将其转换为 numeric，该数据类型的精度很大，足以保存常量值  。 例如，常量值 1 转换为 numeric (1, 0)，常量值 250 转换为 numeric (3, 0)   。  
>   
>  如果查询是自动参数化的，则将常量值转换为最终数据类型之前，始终先将其转换为 numeric (10, 0)  。 如果涉及 / 运算符，则对于类似查询而言，不仅结果类型的精度可能不同，而且结果值也可能不同。 例如，包含表达式 `SELECT CAST (1.0 / 7 AS float)` 的自动参数化查询的结果值将不同于非自动参数化的同一查询的结果值，因为自动参数化查询的结果将被截断以适合 numeric (10, 0) 数据类型  。  
  
## <a name="converting-integer-data"></a>转换整数数据
将整数隐式转换为字符数据类型时，如果整数太大而无法容纳到字符字段中，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会输入 ASCII 字符 42，即星号 (*)。
  
大于 2,147,483,647 的整数常量将转换为 decimal 数据类型，而不是 bigint 数据类型   。 下面的示例显示当超过此阈值时，结果的数据类型将从 int 变为 decimal   。
  
```sql
SELECT 2147483647 / 2 AS Result1, 2147483649 / 2 AS Result2 ;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
Result1      Result2  
1073741823   1073741824.500000  
```  
  
## <a name="examples"></a>示例  
以下示例将使用 bigint、int、smallint 和 tinyint 数据类型创建一个表     。 值插入到每列中并在 SELECT 语句中返回。
  
```sql
CREATE TABLE dbo.MyTable  
(  
  MyBigIntColumn BIGINT  
,MyIntColumn  INT
,MySmallIntColumn SMALLINT
,MyTinyIntColumn TINYINT
);  
  
GO  
  
INSERT INTO dbo.MyTable VALUES (9223372036854775807, 2147483647,32767,255);  
 GO  
SELECT MyBigIntColumn, MyIntColumn, MySmallIntColumn, MyTinyIntColumn  
FROM dbo.MyTable;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
MyBigIntColumn       MyIntColumn MySmallIntColumn MyTinyIntColumn  
-------------------- ----------- ---------------- ---------------  
9223372036854775807  2147483647  32767            255  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅
[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)  
[DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable (Transact-SQL)](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[sys.types (Transact-SQL)](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)
  
  

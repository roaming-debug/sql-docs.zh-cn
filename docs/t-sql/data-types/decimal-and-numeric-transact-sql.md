---
title: decimal 和 numeric (Transact-SQL) | Microsoft Docs
description: Decimal 和 numeric 数据类型的 Transact-SQL 参考。 Decimal 和 numeric 是具有固定精度和小数位数的 numeric 数据类型的同义词。
ms.date: 09/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- decimal
- decimal_TSQL
- numeric
- numeric_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- decimal data type
- decimal data type, about decimal data type
- numeric data type
- numeric data type, about numeric data type
ms.assetid: 9d862a90-e6b7-4692-8605-92358dccccdf
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a0ff8f34468e4186700d6942cbbfcbf5d6f2b09b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747717"
---
# <a name="decimal-and-numeric-transact-sql"></a>decimal 和 numeric (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

带固定精度和小数位数的数值数据类型。 Decimal 和 numeric 是同义词，可互换使用。
  
## <a name="arguments"></a>参数
decimal[ (p[ ,s] )] 和 numeric[ (p[ ,s] )]              
固定精度和小数位数。 使用最大精度时，有效值的范围为 - 10^38 +1 到 10^38 - 1。 decimal 的 ISO 同义词为 dec 和 dec(p,s)       。 numeric  在功能上完全等同于 decimal  。
  
p（精度）  
要存储的十进制数字的总数上限。 此数目包括小数点的左右两侧。 该精度必须是从 1 到最大精度 38 之间的值。 默认精度为 18。
  
> [!NOTE]  
>  Informatica 仅支持 16 位有效位数，无论指定精度和小数位数如何。  
  
s（小数位数）   
小数点右侧存储的十进制数字位数。 从 p 中减去此数字可确定小数点左边的最大位数  。 确定位数值必须介于 0 和 p  之间，只能在指定了精度的情况下指定此值。 默认的确定位数为 0；因此，0 <= s \<= p   。 最大存储大小基于精度而变化。
  
|Precision|存储字节数|  
|---|---|
|1 - 9|5|  
|10-19|9|  
|20-28|13|  
|29-38|17|  
  
> [!NOTE]  
>  Informatica（通过 SQL Server PDW Informatica 连接器连接）仅支持 16 位有效位数，无论指定精度和小数位数如何。  
  
## <a name="converting-decimal-and-numeric-data"></a>转换 decimal 和 numeric 数据
对于 decimal  和 numeric  数据类型，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将精度和确定位数的每个组合视为不同的数据类型。 例如，将 decimal(5,5) 和 decimal(5,0) 视为不同的数据类型   。
  
在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中，带有小数点的常量将自动转换为 numeric 数据值，而且使用必需的最小精度和小数位数  。 例如，常量 12.345 将被转换为精度为 5，小数位数为 3 的 numeric 值  。
  
从 decimal 或 numeric 转换为 float 或 real 可能会导致精度的一些丢失     。 从 int、smallint、tinyint、float、real、money 或 smallmoney 转换为 decimal 或 numeric 可能导致溢出          。
  
默认情况下，将数字转换为精度和小数位数较低的 decimal 或 numeric 值时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会进行舍入   。 反过来说，如果 SET ARITHABORT 选项为 ON，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在发生溢出时抛出错误。 如果仅降低精度和确定位数，不足以抛出错误。
  
在进行 [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 之前，浮点值到 decimal 或 numeric 的转换仅限于精度为 17 位数字的值    。 小于 5E-18 的任何浮点  值（使用 5E-18 的科学计数法或 0.0000000000000000050000000000000005 十进制表示法设置时）都舍入为 0。 从 [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 起，不再具有该限制。
  
## <a name="examples"></a>示例  
下面的示例使用 decimal 和 numeric 数据类型创建一个表   。  值插入每个列中。 结果使用 SELECT 语句返回。
  
```sql
CREATE TABLE dbo.MyTable  
(  
  MyDecimalColumn DECIMAL(5,2)  
,MyNumericColumn NUMERIC(10,5)
  
);  
  
GO  
INSERT INTO dbo.MyTable VALUES (123, 12345.12);  
GO  
SELECT MyDecimalColumn, MyNumericColumn  
FROM dbo.MyTable;  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
MyDecimalColumn                         MyNumericColumn  
--------------------------------------- ---------------------------------------  
123.00                                  12345.12000  
  
(1 row(s) affected)  
  
```  
  
## <a name="see-also"></a>另请参阅
[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)  
[DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable (Transact-SQL)](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[sys.types (Transact-SQL)](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)
  

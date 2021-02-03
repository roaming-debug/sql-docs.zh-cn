---
description: DATALENGTH (Transact-SQL)
title: DATALENGTH (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/20/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATALENGTH_TSQL
- DATALENGTH
dev_langs:
- TSQL
helpviewer_keywords:
- number of bytes representing expression
- data types [SQL Server], length
- DATALENGTH function
- expressions [SQL Server], length
- lengths [SQL Server], data
ms.assetid: 00f377f1-cc3e-4eac-be47-b3e3f80267c9
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6a047c6178f9defea3abca3a1526c4b11a8fce88
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183563"
---
# <a name="datalength-transact-sql"></a>DATALENGTH (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

此函数返回用于表示任何表达式的字节数。

> [!NOTE]
> 若要返回字符串表达式中的字符数，请使用 [LEN](../../t-sql/functions/len-transact-sql.md) 函数。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DATALENGTH ( expression )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*expression*  
任何数据类型的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。
  
## <a name="return-types"></a>返回类型
如果 expression 具有一个 nvarchar(max)、varbinary(max) 或 varchar(max) 数据类型，则为 bigint；否则为 int。
  
## <a name="remarks"></a>备注  
当与可以存储长度可变数据的数据类型一起使用时，`DATALENGTH` 会非常有用，如：
- **图像**
- **ntext**
- **nvarchar**
- **text**
- **varbinary**
- **varchar**
  
对于 NULL 值，`DATALENGTH` 返回 NULL。
  
> [!NOTE]  
> 兼容级别可能影响返回值。 有关兼容性级别的详细信息，请参阅 [ALTER DATABASE 兼容性级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。  

> [!NOTE]
> 使用 [LEN](../../t-sql/functions/len-transact-sql.md) 返回编码到给定字符串表达式中的字符数，使用 [DATALENGTH](../../t-sql/functions/datalength-transact-sql.md) 返回给定字符串表达式的字节大小。 这些输出可能会因列中使用的数据类型和编码类型而异。 若要详细了解不同编码类型的存储区别，请参阅[排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。

## <a name="examples"></a>示例  
此示例查找 `Product` 表中的 `Name` 列的长度：
  
```sql
USE AdventureWorks2016  
GO
SELECT length = DATALENGTH(EnglishProductName), EnglishProductName  
FROM dbo.DimProduct  
ORDER BY EnglishProductName;  
GO  
```  
  
## <a name="see-also"></a>另请参阅
[LEN (Transact-SQL)](../../t-sql/functions/len-transact-sql.md)  
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)  
[系统函数 (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)

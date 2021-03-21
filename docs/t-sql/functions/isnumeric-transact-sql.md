---
description: ISNUMERIC (Transact-SQL)
title: ISNUMERIC (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ISNUMERIC
- ISNUMERIC_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], valid numeric type
- numeric data
- ISNUMERIC function
- verifying valid numeric type
- valid numeric type [SQL Server]
- checking valid numeric type
ms.assetid: 7aa816de-529a-4f6c-a99f-4d5a9ef599eb
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8ac57fb4dcb2e76797a8fb705ba97064cfbbd0e7
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746877"
---
# <a name="isnumeric-transact-sql"></a>ISNUMERIC (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  确定表达式是否为有效的数值类型。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql 
ISNUMERIC ( expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *expression*  
 是要计算的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
## <a name="return-types"></a>返回类型  
 **int**  
  
## <a name="remarks"></a>备注  
 当输入表达式的计算结果为有效的 numeric 数据类型时，ISNUMERIC 返回 1；否则返回 0。 有效的 [numeric 数据类型](../../t-sql/data-types/numeric-types.md)包括以下类型：  

| 区域 | 数字数据类型 |
|-|-|
| [精确数字](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md) | **bigint**、**int**、**smallint**、**tinyint**、**bit** |
| [固定精度](../../t-sql/data-types/decimal-and-numeric-transact-sql.md) | **decimal**、**numeric** |
| [近似](../../t-sql/data-types/float-and-real-transact-sql.md) | **float**、**real** |
| [货币值](../../t-sql/data-types/money-and-smallmoney-transact-sql.md) | **money**、 **smallmoney** |

> [!NOTE]  
> 对于不是数字的字符（如加号 (+)、减号 (-)）和有效货币符号（如美元符号 ($)）字符，ISNUMERIC 将返回 1。 有关货币符号的完整列表，请参阅 [money 和 smallmoney (Transact-SQL)](../../t-sql/data-types/money-and-smallmoney-transact-sql.md)。  
  
## <a name="examples"></a>示例  
 以下示例使用 `ISNUMERIC` 返回所有非数值的邮政编码。  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT City, PostalCode  
FROM Person.Address   
WHERE ISNUMERIC(PostalCode) <> 1;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 以下示例使用 `ISNUMERIC` 返回所有非数值的邮政编码。  
  
```sql
USE master;  
GO  
SELECT name, ISNUMERIC(name) AS IsNameANumber, database_id, ISNUMERIC(database_id) AS IsIdANumber   
FROM sys.databases;  
GO  
```  
  
## <a name="see-also"></a>另请参阅

- [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)
- [系统函数 (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)
- [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)

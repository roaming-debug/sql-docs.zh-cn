---
description: 数据类型优先级 (Transact-SQL)
title: 数据类型优先级 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- precedence [SQL Server]
- data types [SQL Server], converting
- data types [SQL Server], precedence
- converting data types [SQL Server], precedence
- precedence [SQL Server], data types
ms.assetid: f4c804ab-ed3f-43b1-a024-c9ac6944b66b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 02d0152ed3876d3f41c2cc9da9bdfb137d3d4a33
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211700"
---
# <a name="data-type-precedence-transact-sql"></a>数据类型优先级 (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-all-md.md](../../includes/tsql-appliesto-ss2012-all-md.md)]

当两个不同数据类型的表达式用运算符组合后，优先级较低的数据类型首先转换为优先级较高的数据类型。 如果此转换不是所支持的隐式转换，则返回错误。 对于组合具有相同数据类型的操作数表达式的运算符时，运算的结果便为该数据类型。
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对数据类型使用以下优先级顺序：
  
1.  用户定义数据类型（最高）  
1.  **sql_variant**  
1.  **xml**  
1.  **datetimeoffset**  
1.  **datetime2**  
1.  **datetime**  
1.  **smalldatetime**  
1.  **date**  
1. **time**  
1. **float**  
1. **real**  
1. **decimal**  
1. **money**  
1. **smallmoney**  
1. **bigint**  
1. **int**  
1. **smallint**  
1. **tinyint**  
1. **bit**  
1. **ntext**  
1. **text**  
1. **图像**  
1. **timestamp**  
1. **uniqueidentifier**  
1. nvarchar（包括 nvarchar(max)）  
1. **nchar**  
1. varchar（包括 varchar(max)）  
1. **char**  
1. varbinary（包括 varbinary(max)）  
1. binary（最低）  
  
## <a name="see-also"></a>另请参阅
[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)  
[表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)  
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
  

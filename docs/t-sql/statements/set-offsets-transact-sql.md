---
description: SET OFFSETS (Transact-SQL)
title: SET OFFSETS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_OFFSETS_TSQL
- OFFSETS_TSQL
- SET OFFSETS
- OFFSETS
dev_langs:
- TSQL
helpviewer_keywords:
- position relative to start of statement [SQL Server]
- OFFSETS option
- offsets [SQL Server]
- SET OFFSETS statement
ms.assetid: c7bcc697-0930-4630-acae-d8ccbfa4414c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7226c450ed3c854847d609e2ddadf2f250ea5691
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189485"
---
# <a name="set-offsets-transact-sql"></a>SET OFFSETS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  将 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中指定关键字的偏移量（相对于语句起始点的位置）返回给 DB-Library 应用程序。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
 
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
SET OFFSETS keyword_list { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 keyword_list  
 用逗号分隔的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 构造列表，包括 SELECT、FROM、ORDER、TABLE、PROCEDURE、STATEMENT、PARAM 和 EXECUTE。  
  
## <a name="remarks"></a>备注  
 SET OFFSETS 只用在 DB-Library 应用程序中。  
  
 SET OFFSETS 的设置是在分析时设置，而不是在执行或运行时设置。 在分析时进行设置意味着：SET 语句只要出现在批处理或存储过程中，该设置即生效，与代码执行实际上是否到达该点无关；并且 SET 语句在任何语句执行之前生效。 例如，假设 SET 语句在 IF...ELSE 语句块中，而在执行过程中从未到达过该语句块，但由于分析了 IF...ELSE 语句块，因此 SET 语句仍生效。  
  
 如果在存储过程中设置 SET OFFSETS，则从存储过程返回控制后将还原 SET OFFSETS 的值。 因此，动态 SQL 中指定的 SET OFFSETS 语句对动态 SQL 语句之后的任何语句无效。  
  
 如果 OFFSETS 选项为 ON 而且没有出现错误，则 SET PARSEONLY 返回偏移量。  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="see-also"></a>另请参阅  
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET PARSEONLY (Transact-SQL)](../../t-sql/statements/set-parseonly-transact-sql.md)  
  
  

---
description: cursor (Transact-SQL)
title: cursor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- cursor data type
ms.assetid: fbea16ef-f2cc-4734-9149-ec2598fd3cca
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 858a6200e1e1d74f5610d61e85a2b5dde3248e61
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205486"
---
# <a name="cursor-transact-sql"></a>cursor (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

这是变量或存储过程 OUTPUT 参数的一种数据类型，这些参数包含对游标的引用。
  
## <a name="remarks"></a>备注  
有些操作可以引用那些具有 cursor 数据类型的变量和参数，这些操作包括：
-   DECLARE \@local_variable 和 SET \@local_variable 语句 。  
-   OPEN、FETCH、CLOSE 及 DEALLOCATE 游标语句。  
-   存储过程输出参数。  
-   CURSOR_STATUS 函数。  
-   sp_cursor_list、sp_describe_cursor、sp_describe_cursor_tables 以及 sp_describe_cursor_columns 系统存储过程。  
  
sp_cursor_list 和 sp_describe_cursor 的 cursor_name 输出列返回游标变量的名称。
  
使用 cursor 数据类型创建的所有变量都可以为 Null。
  
对于 CREATE TABLE 语句中的列，不能使用 cursor 数据类型。
  
## <a name="see-also"></a>另请参阅
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CURSOR_STATUS (Transact-SQL)](../../t-sql/functions/cursor-status-transact-sql.md)  
[数据类型转换（数据库引擎）](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE CURSOR (Transact-SQL)](../../t-sql/language-elements/declare-cursor-transact-sql.md)  
[DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable (Transact-SQL)](../../t-sql/language-elements/set-local-variable-transact-sql.md)
  
  

---
description: MIN_ACTIVE_ROWVERSION (Transact-SQL)
title: MIN_ACTIVE_ROWVERSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- MIN_ACTIVE_ROWVERSION
- MIN_ACTIVE_ROWVERSION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MIN_ACTIVE_ROWVERSION function [Transact-SQL]
ms.assetid: 87c89547-8ea1-4820-b75e-36be683e4e10
author: cawrites
ms.author: chadam
ms.openlocfilehash: f28737a094a5d42b6d0e28c06793aa227c14795b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195941"
---
# <a name="min_active_rowversion-transact-sql"></a>MIN_ACTIVE_ROWVERSION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回当前数据库中最低的活动 **rowversion** 值。 如果 **rowversion** 值用在尚未提交的事务中，则它处于活动状态。 有关详细信息，请参阅 [rowversion (Transact-SQL)](../../t-sql/data-types/rowversion-transact-sql.md)。  
  
> [!NOTE]  
>  **rowversion** 数据类型也称为 **timestamp**。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
MIN_ACTIVE_ROWVERSION ( ) 
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 返回 binary(8) 值。  
  
## <a name="remarks"></a>备注  
 MIN_ACTIVE_ROWVERSION 是一个非确定性函数，可返回当前数据库中最低的活动 rowversion 值。 对包含 **rowversion** 类型的列的表执行插入或更新操作时，通常会生成一个新的 **rowversion** 值。 如果数据库中没有活动的值，则 MIN_ACTIVE_ROWVERSION 会返回与 @@DBTS + 1 相同的值。  
  
 对于诸如使用 rowversion 值将多组更改组合在一起的数据同步等情况，MIN_ACTIVE_ROWVERSION 很有用。 如果应用程序使用 @@DBTS 而不是 MIN_ACTIVE_ROWVERSION，则在进行同步时可能会丢失处于活动状态的更改。  
  
 MIN_ACTIVE_ROWVERSION 函数不受事务隔离级别中的更改影响。  
  
## <a name="examples"></a>示例  
 下例使用 `MIN_ACTIVE_ROWVERSION` 和 `@@DBTS` 返回 rowversion 值。 请注意，当数据库中没有活动事务时，值会有所不同。  
  
```sql  
-- Create a table that has a ROWVERSION column in it.  
CREATE TABLE RowVersionTestTable (rv ROWVERSION)  
GO  
  
-- Print the current values for the database.  
PRINT ''  
PRINT 'DBTS'  
PRINT @@DBTS  
PRINT 'MIN_ACTIVE_ROWVERSION'  
PRINT MIN_ACTIVE_ROWVERSION()   
GO  
---------------- Results ----------------  
--DBTS  
--0x00000000000007E2  
--MIN_ACTIVE_ROWVERSION  
--0x00000000000007E3  
  
-- Insert a row.  
INSERT INTO RowVersionTestTable VALUES (DEFAULT)  
SELECT * FROM RowVersionTestTable  
GO  
---------------- Results ----------------  
--rv  
--0x00000000000007E3  
  
-- Print the current values for the database.  
PRINT ''  
PRINT 'DBTS'  
PRINT @@DBTS  
PRINT 'MIN_ACTIVE_ROWVERSION'  
PRINT MIN_ACTIVE_ROWVERSION()  
GO  
---------------- Results ----------------  
--DBTS  
--0x00000000000007E3  
--MIN_ACTIVE_ROWVERSION  
--0x00000000000007E4  
  
-- Insert a new row inside a transaction but do not commit.  
BEGIN TRAN  
INSERT INTO RowVersionTestTable VALUES (DEFAULT)  
SELECT * FROM RowVersionTestTable  
GO  
---------------- Results ----------------  
--rv  
--0x00000000000007E3  
--0x00000000000007E4  
  
-- Print the current values for the database.  
PRINT ''  
PRINT 'DBTS'  
PRINT @@DBTS  
PRINT 'MIN_ACTIVE_ROWVERSION'  
PRINT MIN_ACTIVE_ROWVERSION()   
GO  
---------------- Results ----------------  
--DBTS  
--0x00000000000007E4  
--MIN_ACTIVE_ROWVERSION  
--0x00000000000007E4  
  
-- Commit the transaction.  
COMMIT  
GO  
  
-- Print the current values for the database.  
PRINT ''  
PRINT 'DBTS'  
PRINT @@DBTS  
PRINT 'MIN_ACTIVE_ROWVERSION'  
PRINT MIN_ACTIVE_ROWVERSION()  
GO  
---------------- Results ----------------  
--DBTS  
--0x00000000000007E4  
--MIN_ACTIVE_ROWVERSION  
--0x00000000000007E5  
```  
  
## <a name="see-also"></a>另请参阅  
 [@@DBTS (Transact-SQL)](../../t-sql/functions/dbts-transact-sql.md)   
 [rowversion (Transact-SQL)](../../t-sql/data-types/rowversion-transact-sql.md)  
  
  

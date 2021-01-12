---
description: SET LOCK_TIMEOUT (Transact-SQL)
title: SET LOCK_TIMEOUT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- LOCK_TIMEOUT_TSQL
- SET_LOCK_TIMEOUT_TSQL
- SET LOCK_TIMEOUT
- LOCK_TIMEOUT
dev_langs:
- TSQL
helpviewer_keywords:
- timeout options [SQL Server], locks
- releasing locks
- LOCK_TIMEOUT option
- SET LOCK_TIMEOUT statement
- locking [SQL Server], time-outs
- wait time for lock releases [SQL Server]
ms.assetid: dd0c389e-956d-435e-bf71-e16624a0a215
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a006850f731e78628275f3558975c95e12c64dc6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093363"
---
# <a name="set-lock_timeout-transact-sql"></a>SET LOCK_TIMEOUT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  指定语句等待锁释放的毫秒数。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
SET LOCK_TIMEOUT timeout_period  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 timeout_period  
 在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回锁定错误前经过的毫秒数。 值为 -1（默认值）时表示没有超时期限（即无限期等待）。  
  
 当锁等待超过超时值时，将返回错误。 值为 0 时表示根本不等待，一遇到锁就返回消息。  
  
## <a name="remarks"></a>注解  
 在连接开始时，此设置的值为 -1。 设置更改后，新设置在其余的连接时间里一直有效。  
  
 SET LOCK_TIMEOUT 的设置是在执行或运行时设置，而不是在分析时设置。  
  
 READPAST 锁提示为该 SET 选项提供了另一种方式。  
  
 CREATE DATABASE、ALTER DATABASE 和 DROP DATABASE 语句不使用 SET LOCK_TIMEOUT 设置。  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="examples"></a>示例  
  
### <a name="a-set-the-lock-timeout-to-1800-milliseconds"></a>A：将锁超时设置为 1800 毫秒  
 下面的示例将锁超时期限设置为 `1800` 毫秒。  
  
```sql  
SET LOCK_TIMEOUT 1800;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-set-the-lock-timeout-to-wait-forever-for-a-lock-to-be-released"></a>B. 将锁超时设置为一直等待锁释放。  
 以下示例将锁超时设置为一直等待，永不过期。 这是在每次连接开始时便已设置的默认行为。  
  
```sql  
SET LOCK_TIMEOUT -1;  
```  
  
 下面的示例将锁超时期限设置为 `1800` 毫秒。 在此版本中，[!INCLUDE[ssDW](../../includes/ssdw-md.md)] 将成功分析该语句，但会忽略值 1800 并继续使用默认行为。  
  
```sql  
SET LOCK_TIMEOUT 1800;  
```  
  
## <a name="see-also"></a>另请参阅  
 [@@LOCK_TIMEOUT (Transact-SQL)](../../t-sql/functions/lock-timeout-transact-sql.md)   
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)  
  
  


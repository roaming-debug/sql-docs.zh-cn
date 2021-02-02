---
description: '&#x40;&#x40;TOTAL_WRITE (Transact-SQL)'
title: '@@TOTAL_WRITE (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@TOTAL_WRITE'
- '@@TOTAL_WRITE_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- write activity since last started [SQL Server]
- number of disk writes
- '@@TOTAL_WRITE function'
- disks [SQL Server], number of disk writes
- total write [SQL Server]
ms.assetid: cd528126-51ee-4aa4-a21f-f32ce5c80fac
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 59271a43cf66316e56e97866761113b7c513dc72
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158657"
---
# <a name="x40x40total_write-transact-sql"></a>&#x40;&#x40;TOTAL_WRITE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回自上次启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以来 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所执行的磁盘写入数。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
@@TOTAL_WRITE  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 **integer**  
  
## <a name="remarks"></a>备注  
 要显示包含多项 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 统计信息（包括读写活动）的报表，请运行 sp_monitor。  
  
## <a name="examples"></a>示例  
 以下示例显示了如何返回到当前日期和时间为止总的磁盘读写操作的次数。  
  
```sql
SELECT @@TOTAL_READ AS 'Reads', @@TOTAL_WRITE AS 'Writes', GETDATE() AS 'As of'  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Reads       Writes      As of                   
----------- ----------- ----------------------  
7760        97263       12/5/2006 10:23:00 PM   
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_monitor (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)   
 [系统统计函数 (Transact-SQL)](../../t-sql/functions/system-statistical-functions-transact-sql.md)   
 [@@TOTAL_READ (Transact-SQL)](../../t-sql/functions/total-read-transact-sql.md)  
  
  

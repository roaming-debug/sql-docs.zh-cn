---
description: '&#x40;&#x40;CPU_BUSY (Transact-SQL)'
title: CPU_BUSY (Transact-SQL)
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@CPU_BUSY_TSQL'
- '@@CPU_BUSY'
dev_langs:
- TSQL
helpviewer_keywords:
- CPU [SQL Server]
- status information [SQL Server], CPU
- ticks [SQL Server]
- time [SQL Server], CPU activity
- '@@CPU_BUSY function'
- statistical information [SQL Server], CPU
- CPU [SQL Server], activity
ms.assetid: 81ae0e64-79fa-4a74-9aa5-37045c4cd211
author: cawrites
ms.author: chadam
ms.openlocfilehash: edb5daccc0054ce98d0ebc4bb7568835adc46d80
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98087954"
---
# <a name="x40x40cpu_busy-transact-sql"></a>&#x40;&#x40;CPU_BUSY (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

此函数返回自最近一次开始以来，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在活动操作中所花的时间。 `@@CPU_BUSY` 返回一个以 CPU 时间增量或“滴答数”计算的结果。 此值为所有 CPU 时间的累积，因此，可能会超出实际占用的时间。 若要转换为微秒，请乘以 [@@TIMETICKS](./timeticks-transact-sql.md)。
  
> [!NOTE]  
>  如果 @@CPU_BUSY 或 @@IO_BUSY 中返回的时间超过 49 天（大约）的累积 CPU 时间，则会收到算术溢出警告。 在这种情况下，`@@CPU_BUSY``@@IO_BUSY` 和 `@@IDLE` 变量值并不精确。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
@@CPU_BUSY  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]


## <a name="return-types"></a>返回类型
**integer**
  
## <a name="remarks"></a>注解  
若要查看包含若干个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 统计信息（包括 CPU 活动）的报表，请运行 [sp_monitor](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)。
  
## <a name="examples"></a>示例  
此示例显示了返回当前日期和时间之前的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CPU 活动。 此示例将其中一个值转换为 `float` 数据类型。 这样就避免了计算以微秒为单位的值时的算术溢出问题。
  
```sql
SELECT @@CPU_BUSY * CAST(@@TIMETICKS AS FLOAT) AS 'CPU microseconds',   
   GETDATE() AS 'As of' ;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
CPU microseconds As of
---------------- -----------------------
18406250         2006-12-05 17:00:50.600
```
  
## <a name="see-also"></a>另请参阅
[sys.dm_os_sys_info (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)  
[@@IDLE (Transact-SQL)](../../t-sql/functions/idle-transact-sql.md)  
[@@IO_BUSY (Transact-SQL)](../../t-sql/functions/io-busy-transact-sql.md)  
[sp_monitor (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)  
[系统统计函数 (Transact-SQL)](../../t-sql/functions/system-statistical-functions-transact-sql.md)
  
  

---
description: '&#x40;&#x40;IDLE (Transact-SQL)'
title: '@@IDLE (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@IDLE_TSQL'
- '@@IDLE'
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], idle
- ticks [SQL Server]
- '@@IDLE function'
- status information [SQL Server], idle time
- idle time [SQL Server]
ms.assetid: 8f49c62a-8da5-4afd-a5eb-4df8ef8be755
author: cawrites
ms.author: chadam
ms.openlocfilehash: 96fd2277d19f8610b265c16942196585951eb5a9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101097"
---
# <a name="x40x40idle-transact-sql"></a>&#x40;&#x40;IDLE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自上次启动后的空闲时间。 结果以 CPU 时间增量或“时钟周期”表示，并且是所有 CPU 的累积，因此该值可能超过实际经过的时间。 乘以 @@TIMETICKS 可转换为微秒。  
  
> [!NOTE]  
>  如果 @@CPU_BUSY 或 @@IO_BUSY 中返回的时间超过累积的 CPU 时间约 49 天，则会收到算术溢出警告。 在这种情况下，@@CPU_BUSY、@@IO_BUSY 和 @@IDLE 变量值并不精确。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
@@IDLE  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 **integer**  
  
## <a name="remarks"></a>注解  
 若要显示包含几种 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 统计信息的报告，请运行 sp_monitor。  
  
## <a name="examples"></a>示例  
 以下示例将返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自启动到当前时间的空闲毫秒数。 为了避免将值转换为微秒时出现算术溢出，此示例将其中一个值转换为 `float` 数据类型。  
  
```sql  
SELECT @@IDLE * CAST(@@TIMETICKS AS float) AS 'Idle microseconds',  
   GETDATE() AS 'as of';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
I  
Idle microseconds  as of                   
----------------- ----------------------  
8199934           12/5/2006 10:23:00 AM   
```  
  
## <a name="see-also"></a>另请参阅  
 [@@CPU_BUSY (Transact-SQL)](../../t-sql/functions/cpu-busy-transact-sql.md)   
 [sp_monitor (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)   
 [@@IO_BUSY (Transact-SQL)](../../t-sql/functions/io-busy-transact-sql.md)   
 [系统统计函数 (Transact-SQL)](../../t-sql/functions/system-statistical-functions-transact-sql.md)  
  
  

---
description: Errors and Warnings 事件类别（数据库引擎）
title: 错误和警告事件类别
ms.date: 06/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Errors and Warnings event category [SQL Server]
- SQL Server event classes, Errors and Warnings event category
- event classes [SQL Server], Errors and Warnings event category
ms.assetid: 249c19b5-af68-4433-80f6-337395176641
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-lt-2019
ms.openlocfilehash: d40fee49969f019bc325b9a65a28b04f162560f8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469778"
---
# <a name="errors-and-warnings-event-category-database-engine"></a>Errors and Warnings 事件类别（数据库引擎）
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  **Errors and Warnings** 事件类别包含常规错误和警告事件。  
  
## <a name="in-this-section"></a>本节内容  
  
|主题|说明|  
|-----------|-----------------|  
|[Attention 事件类](../../relational-databases/event-classes/attention-event-class.md)|指示出现了 **Attention** 事件。|  
|[Background Job Error 事件类](../../relational-databases/event-classes/background-job-error-event-class.md)|指示后台作业异常终止。|  
|[Bitmap Warning 事件类](../../relational-databases/event-classes/bitmap-warning-event-class.md)|指示已在查询中禁用位图筛选。|  
|[Blocked Process Report 事件类](../../relational-databases/event-classes/blocked-process-report-event-class.md)|指示任务被阻塞的时间超过了指定的时间。|  
|[CPU Threshold Exceeded 事件类](../../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md)|指示资源调控器检测到超出指定 CPU 阈值的查询。|  
|[ErrorLog 事件类](../../relational-databases/event-classes/errorlog-event-class.md)|指示已将错误事件记录到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志中。|  
|[EventLog 事件类](../../relational-databases/event-classes/eventlog-event-class.md)|指示已将事件记录到 Windows 事件日志中。|  
|[Exception 事件类](../../relational-databases/event-classes/exception-event-class.md)|指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中出现了异常。|  
|[Exchange Spill 事件类](../../relational-databases/event-classes/exchange-spill-event-class.md)|指示并行查询计划中的通信缓冲区已写入 tempdb 数据库。|  
|[Execution Warnings 事件类](../../relational-databases/event-classes/execution-warnings-event-class.md)|指示在执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语句或存储过程期间出现了内存授予警告。|  
|[Hash Warning 事件类](../../relational-databases/event-classes/hash-warning-event-class.md)|指示在哈希操作过程中发生哈希递归或哈希援助。|  
|[Missing Column Statistics 事件类](../../relational-databases/event-classes/missing-column-statistics-event-class.md)|指示缺少可能对优化器有用的列统计信息。|  
|[Missing Join Predicate 事件类](../../relational-databases/event-classes/missing-join-predicate-event-class.md)|指示正在执行没有联接谓词的查询。|  
|[Sort Warnings 事件类](../../relational-databases/event-classes/sort-warnings-event-class.md)|指示不适合内存的排序操作。|  
|[User Error Message 事件类](../../relational-databases/event-classes/user-error-message-event-class.md)|显示用户可见的错误消息。|  
  
## <a name="see-also"></a>另请参阅  
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  

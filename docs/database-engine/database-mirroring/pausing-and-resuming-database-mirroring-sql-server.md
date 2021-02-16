---
title: 暂停和恢复数据库镜像
description: 了解如何暂停并随后恢复 SQL Server 数据库镜像会话，以在暂停镜像时保留会话状态。
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- sessions [SQL Server], database mirroring
- resuming database mirroring
- database mirroring [SQL Server], pausing
- database mirroring [SQL Server], resuming
- pausing database mirroring
ms.assetid: c67802c6-ee8c-4cbd-a6d4-f7b80413a4ab
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c565c8ca660c0616c061ba70f3c24d3a7d347191
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343731"
---
# <a name="pausing-and-resuming-database-mirroring-sql-server"></a>暂停和恢复数据库镜像 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  数据库所有者可以暂停并在以后随时恢复数据库镜像会话。 执行暂停操作将保留在挂起镜像时的会话状态。 当出现瓶颈时，暂停可能有利于提高主体服务器的性能。  
  
 会话暂停后，主体数据库仍然可用。 暂停操作将镜像会话的状态设置为 SUSPENDED，并且镜像数据库不再与主体数据库保持一致，从而导致主体数据库公开运行。  
  
 由于在数据库镜像会话处于暂停时无法截断事务日志，因此建议您尽快恢复暂停的会话。 因此，如果数据库镜像会话暂停的时间太长，事务日志将填满，导致数据库不可用。 有关此现象产生原因的解释，请参阅本主题后面的“暂停和恢复如何影响日志截断”。  
  
> [!IMPORTANT]  
>  执行强制服务之后，当重新连接原始主体服务器时，镜像便会挂起。 在这种情况下，恢复镜像可能会导致原始主体服务器上的数据丢失。 有关管理潜在的数据丢失的信息，请参阅 [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)。  
  
 **本主题内容：**  
  
-   [暂停和恢复如何影响日志截断](#EffectOnLogTrunc)  
  
-   [避免出现已满事务日志](#AvoidFullLog)  
  
-   [相关任务](#RelatedTasks)  
  
##  <a name="how-pausing-and-resuming-affect-log-truncation"></a><a name="EffectOnLogTrunc"></a> 暂停和恢复如何影响日志截断  
 通常，在数据库上执行自动检查点操作时，事务日志将在下一个日志备份后截断到该检查点。 当数据库镜像会话处于暂停时，当前所有日志记录都保持为活动状态，因为主体服务器正等待将这些记录发送到镜像服务器。 未发送的日志记录将堆积在主体数据库的事务日志中，直到会话恢复并且主体服务器将它们发送到镜像服务器为止。  
  
 会话恢复时，主体服务器立即开始将堆积的日志记录发送到镜像服务器。 当镜像服务器确认与最早的自动检查点相对应的日志记录已排队后，主体服务器便会将主体数据库的日志截断到该检查点。 镜像服务器会截断同一个日志记录的重做队列。 随着对每个连续的检查点重复此过程，日志将对检查点逐个分阶段地截断。  
  
> [!NOTE]  
>  有关检查点和日志截断的详细信息，请参阅[数据库检查点 (SQL Server)](../../relational-databases/logs/database-checkpoints-sql-server.md)。  
  
##  <a name="avoid-a-full-transaction-log"></a><a name="AvoidFullLog"></a> 避免出现已满事务日志  
 如果填满该日志（因为它达到其最大大小或服务器实例耗尽空间），则数据库将无法再执行任何更新。 若要避免出现这种问题，有两种选择：  
  
-   在该日志填满之前恢复数据库镜像会话，或添加更多的日志空间。 恢复数据库镜像会使主体服务器将其累积的活动日志发送到镜像服务器，并将镜像数据库设置为 SYNCHRONIZING 状态。 然后镜像服务器可将日志镜像到磁盘并开始重做。  
  
-   通过删除镜像来停止数据库镜像会话。  
  
     和暂停会话不同，删除镜像将删除有关镜像会话的所有信息。 每个伙伴服务器实例将保留其自己的数据库副本。 如果前一个镜像副本已恢复，则它将与前一个主体副本分离，且滞后时间等于此会话暂停的时间。 有关详细信息，请参阅 [删除数据库镜像 (SQL Server)](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md)。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **暂停或恢复数据库镜像**  
  
-   [暂停或恢复数据库镜像会话 (SQL Server)](../../database-engine/database-mirroring/pause-or-resume-a-database-mirroring-session-sql-server.md)  
  
 **停止数据库镜像**  
  
-   [删除数据库镜像 (SQL Server)](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [删除数据库镜像 (SQL Server)](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md)  
  
  

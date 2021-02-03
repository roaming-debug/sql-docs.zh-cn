---
description: MSSQLSERVER_9004
title: MSSQLSERVER_9004 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 9004 (Database Engine error)
ms.assetid: b528bb49-340c-4a81-9c8d-cefce6562f16
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: b4706982db7c2c121af0658142344c0558321b52
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161965"
---
# <a name="mssqlserver_9004"></a>MSSQLSERVER_9004
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|9004|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|LOG_CORRUPT|  
|消息正文|处理数据库 '%.*ls' 的日志时出错。  如果可能，请从备份还原。 如果没有可用备份，可能需要重新生成日志。|  
  
## <a name="explanation"></a>说明  
在回滚、恢复或复制期间处理日志时出错。 这可能表明操作系统检测到错误，或者 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 检测到内部一致性错误。  
[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 在读取和处理事务日志内容时对其一致性执行逻辑检查。 对日志标头、日志块和日志记录的检查并不涉及所有方面。 状态编号提供有关故障类型的更多信息：

 - 状态 1：虚拟日志文件 (VLF) 的日志文件标头已损坏。  在服务启动时，如果在启动数据库期间找到了已损坏的日志文件标头，则在错误日志中只能看到错误 9004。 “日志文件标头”是事务日志中每个 VLF 的第一个部分。 日志文件标头不等同于日志文件的单个文件头，也不是前 8 KB。 如果日志文件的文件头已损坏，你可能会收到消息 5172，类似于数据库文件标头页损坏。
 - 状态 2 和 3：在 RESTORE 操作过程中执行恢复时日志块无效。
 - 状态 4 到状态 12：在处理日志记录时，这些是对日志块进行的所有检查。 其中包括奇偶校验、扇区和其他对事务日志一致性的逻辑检查

在大多数情况下，此错误仅在错误日志或 Windows 应用程序事件日志中出现，其 EventID = 9004，因为处理日志的操作不是基于直接用户命令的（如在 SQL Server 引擎启动时运行恢复）。 在这些情况下，错误 9004 通常会与错误 3414 同时出现。 但是，某些查询（如 ALTER DATABASE）可能需要处理日志，因此会看到这些错误。 由于错误的严重性为 21，用户会话已断开连接。

## <a name="cause"></a>原因
错误 9004 是一个一般性错误，它指示事务日志的内容已损坏。 日志变得不一致的原因与 SQL Server 引擎检测到任何数据库损坏问题的原因类似。 若要查明日志损坏的原因，应遵循针对数据库损坏所用的类似技术，其中包括对可能的硬件、文件系统和 I/O 问题的分析。 请注意，DBCC CHECKDB 在操作中不会检查事务日志，并且无法检测出日志损坏错误。 SQL Server 引擎本身会引发错误 9004。

## <a name="user-action"></a>用户操作  
执行以下操作之一将更正此错误：  
  
-   从备份还原：从已知完好的备份还原以解决此问题。 如果数据库或日志备份的日志部分包含损坏的内容，则在还原时可能会遇到错误 9004。 在这种情况下，备份中的事务日志已损坏。
  
-   重新生成日志：如果无法从备份还原，可以通过重新生成事务日志使数据库联机。 应仔细了解重建事务日志的后果。 其中包括“数据库中可能出现事务一致性缺失”。 有关如何重新生成事务日志的更多信息，请参阅[在数据库紧急模式下纠正错误](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md#resolving-errors-in-database-emergency-mode)。
  
-   检查日志中是否存在系统问题：另外，查看系统事件日志和错误日志，以识别系统内可能导致错误的问题。  
  

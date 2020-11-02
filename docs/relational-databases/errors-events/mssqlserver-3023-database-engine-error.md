---
description: MSSQLSERVER_3023
title: MSSQLSERVER_3023
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 3023 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 0197c7e5b75164615572e4041a5b348a4d5abcc4
ms.sourcegitcommit: ead0b8c334d487a07e41256ce5d6acafa2d23c9d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "92418685"
---
# <a name="mssqlserver_3023"></a>MSSQLSERVER_3023
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|3023|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|DB_IN_USE_DUMP|
|消息正文|数据库上的备份和文件操作（例如 ALTER DATABASE ADD FILE）必须串行化。 请在当前备份或文件操作完成后重新发出该语句|
||

## <a name="explanation"></a>说明

尝试在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中运行备份、收缩或更改数据库命令时，会看到以下消息：

> 消息 3023，级别 16，状态 2，行 1  
数据库上的备份和文件操作（例如 ALTER DATABASE ADD FILE）必须串行化。 请在当前备份或文件操作完成后重新发出该语句。

> 消息 3013，级别 16，状态 1，行 1  
备份数据库异常终止。

此外，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志包含如下消息：

> \<Datetime> 备份 错误：3041，严重级别：16，状态：1。  
\<Datetime> 备份，备份未能完成 BACKUP DATABASE MyDatabase WITH DIFFERENTIAL 命令。 有关详细消息，请查看备份应用程序日志。

你可能还会注意到，当从各种动态管理视图 (DMV)（如 `sys.dm_exec_requests` 或 `sys.dm_os_waiting_tasks`）查看这些命令的状态时，这些命令会遇到 `wait_type = LCK_M_U` 和 `wait_resource = DATABASE: <id> [BULKOP_BACKUP_DB]`。

## <a name="possible-causes"></a>可能的原因

当完整数据库正在针对某数据库进行操作时，有一些规则来规定允许或禁止的操作。 以下是一些示例：

- 一次只能进行一个数据备份（进行完整数据库备份时，差异备份和增量备份不能同时进行）。
- 一次只能进行一个日志备份（进行完整数据库备份时，可进行日志备份）。
- 在备份过程中，无法将文件添加或拖至数据库。
- 进行数据库备份时，无法收缩文件。
- 进行备份时，可进行的恢复模式更改是有限的。

执行上述任一冲突操作时，命令将遇到“说明”部分中提到的锁定等待，然后你会收到 3023 和 3041 消息。

## <a name="user-action"></a>用户操作

检查各种数据库维护活动的计划，然后调整计划，使这些操作或命令不会相互冲突。

## <a name="more-information"></a>详细信息

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 记录了 `msdb` 数据库中备份的开始时间和结束时间。 可检查备份历史记录，确定在尝试增量备份时是否正在进行完整数据库备份并因而导致出现错误。 可使用以下查询来帮助你执行此过程：

```sql
select database_name, type, backup_start_date, backup_finish_date
from msdb.dbo.backupset
order by database_name, type, backup_start_date, backup_finish_date
go
```

还可使用 SQL 探查器跟踪中的“使用错误消息”事件或扩展事件中的 error_reported 事件来跟踪将 3023 消息报告回启动了备份或其他维护命令的应用程序的过程 。

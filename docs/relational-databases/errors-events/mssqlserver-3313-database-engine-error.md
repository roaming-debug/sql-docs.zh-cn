---
description: MSSQLSERVER_3313
title: MSSQLSERVER_3313 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3313 (Database Engine error)
ms.assetid: a244227b-8553-42df-9435-034f906c4c74
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 54d19e8b37d0cf4154bb14b3909c1af60afb0743
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208383"
---
# <a name="mssqlserver_3313"></a>MSSQLSERVER_3313
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|3313|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|ERR_LOG_RID1|  
|消息正文|在重做数据库 '%.*ls' 的日志中记录的操作时，日志记录 ID %S_LSN 出错。 通常，特定故障以前会在 Windows 事件日志服务中记录为错误。 请利用完整备份还原数据库，或者修复该数据库。|  
  
## <a name="explanation"></a>说明  
这是重做恢复的累积错误。 此错误使数据库进入 SUSPECT 状态。 主文件组以及可能其他文件组可疑并可能受损。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动过程中无法恢复数据库，因此无法使用该数据库。 需要用户执行操作来解决问题。  
  
请注意，如果对于 **tempdb** 发生此错误，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例关闭。  
  
## <a name="user-action"></a>用户操作  
此错误可能是由在某次尝试启动服务器实例或恢复数据库的过程中系统上存在的暂时性条件导致的。 此错误也可能是由当您每次尝试启动数据库时发生的永久性错误导致的。 有关原因的信息，请检查 Windows 事件日志以了解有关指示特定故障的先前错误。  
  
请注意，当遇到此错误状况时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通常在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] LOG 文件夹中生成三个文件。 SQLDump *nnnn*.txt 文件包含与故障相关的高级诊断信息，包括有关事务的详细信息和遇到问题的页面。 此信息通常由产品支持小组用来分析故障的原因。  
  
有关错误 3313 出现原因的信息，请检查 Windows 事件日志以了解有关指示特定故障的先前错误。 相应的用户操作取决于 Windows 事件日志中的信息是否指示该 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误由暂时条件或永久性故障导致。 有关排除 3313 错误的用户操作的信息，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书。  
  
## <a name="see-also"></a>另请参阅  
[ALTER DATABASE (Transact-SQL)](~/t-sql/statements/alter-database-transact-sql-set-options.md)  
[DBCC CHECKDB (Transact-SQL)](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[完整数据库还原（简单恢复模式）](~/relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
[MSSQLSERVER_824](~/relational-databases/errors-events/mssqlserver-824-database-engine-error.md)  
[sys.databases (Transact-SQL)](~/relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
  

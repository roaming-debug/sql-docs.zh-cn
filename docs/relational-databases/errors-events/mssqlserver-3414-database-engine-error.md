---
description: MSSQLSERVER_3414
title: MSSQLSERVER_3414 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3414 (Database Engine error)
ms.assetid: f25852f9-b91c-4356-b817-78bec9ec8db4
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: fe68125b8b7b5d98e707a9e38d956d1d114db74e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195171"
---
# <a name="mssqlserver_3414"></a>MSSQLSERVER_3414
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|3414|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|REC_GIVEUP|  
|消息正文|恢复期间出错，导致数据库 '%.*ls' (数据库 ID %d)无法重新启动。 请诊断并纠正这些恢复错误，或者从已知的正确备份中还原。 如果无法更正错误，或者为意外错误，请与技术支持人员联系。|  
  
## <a name="explanation"></a>说明  
已恢复指定的数据库，但无法启动它，因为在恢复期间出现了错误。 此错误使数据库进入 SUSPECT 状态。 主文件组以及可能其他文件组可疑并可能受损。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动过程中无法恢复数据库，因此无法使用该数据库。 需要用户执行操作来解决问题。 在 SQL Server Management Studio（数据库图标旁边）中以及查看 sys.databases.state_desc 列时，你将看到 SUSPECT 状态。 在此状态下尝试使用数据库会导致以下错误：

```
Msg 926, Level 14, State 1, Line 1 
Database 'mydb' cannot be opened. It has been marked SUSPECT by recovery. See the SQL Server errorlog for more information
```
  
请注意，tempdb 中发生此错误时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例将关闭。  

## <a name="cause"></a>原因
此错误可能是由在某次尝试启动服务器实例或恢复数据库的过程中系统上存在的暂时性条件导致的。 此错误也可能是由当您每次尝试启动数据库时发生的永久性错误导致的。 通常，可以在错误日志或事件日志中错误 3414 之前的错误中找到恢复失败的原因。 日志文件中之前的错误包含相同的 spid<n> 值。 例如，以下恢复失败的原因是尝试读取日志块时发生校验和错误。 请注意，spid15s 存在于所有行中：

```
2020-03-31 17:33:13.00 spid15s     Error: 824, Severity: 24, State: 4.  
2020-03-31 17:33:13.00 spid15s     SQL Server detected a logical consistency-based I/O error: (bad checksum). It occurred during a read of page (0:-1) in database ID 13 at offset 0x0000000000b800 in file 'C:\Program Files\Microsoft SQL Server\MSSQL10.SQL2008\MSSQL\DATA\mydb_log.LDF'.  Additional messages in the SQL Server error log or system event log may provide more detail. This is a severe error condition that threatens database integrity and must be corrected immediately. Complete a full database consistency check (DBCC CHECKDB). This error can be caused by many factors; for more information, see SQL Server Books Online.   
2020-03-31 17:33:13.16 spid15s     Error: 3414, Severity: 21, State: 1.  
2020-03-31 17:33:13.16 spid15s     An error occurred during recovery, preventing the database 'mydb' (database ID 13) from restarting. Diagnose the recovery errors and fix them, or restore from a known good backup. If errors are not corrected or expected, contact Technical Support
```


各种错误都可能导致数据库恢复失败。 尽管必须逐一评估每个错误，但解决数据库恢复失败的方法通常如下文“用户操作”部分中所述。

## <a name="user-action"></a>用户操作  
 
有关错误 3414 出现原因的信息，请检查 Windows 事件日志或错误日志以了解有关指示特定故障的先前错误。 相应的用户操作取决于 Windows 事件日志中的信息是否指示该 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误由暂时条件或永久性故障导致。 错误消息指示“诊断并纠正这些恢复错误，或者从已知的正确备份中还原”。 因此，你可以尝试纠正遇到的错误以完成恢复（请参阅[可纠正的错误和延迟的事务](#correctable-errors-and-deferred-transactions)）。

如果无法纠正错误，则解决此问题的最优先选择是从完好的备份中还原。 如果无法从备份中恢复，你还有两种其他方法可以选择，但它们无法保证完整地恢复数据：将紧急修复与 [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) 结合使用，或尝试将尽可能多的数据复制到另一个数据库。 

 1. 从上一个已知完好的数据库备份中还原
 1. 使用 [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) 提供的紧急修复方法
 1. 尝试将尽可能多的数据复制到另一个数据库。

第一种方法是还原完好的数据库备份，这是使数据库进入已知一致状态的最佳选择。  

如果备份不可用，次优选择是使数据库处于联机且可访问的状态。 但必须意识到，由于恢复失败，无法保证事务性一致性。 你无法得知哪些事务应该回滚或前滚，但由于恢复失败而未回滚或前滚。 有关紧急修复的步骤，请参阅 DBCC CHECKDB 文档中的[在紧急模式下解决数据库错误](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md#resolving-errors-in-database-emergency-mode)部分。 

如果紧急修复不起作用，并且你想尝试将某些数据抢救到另一个数据库，则可以通过 ALTER DATABASE <dbname> SET EMERGENCY 命令将数据库设置为紧急模式来访问数据库。 然后，你便可以尝试从表中复制数据。

### <a name="correctable-errors-and-deferred-transactions"></a>可纠正的错误和延迟的事务
并非数据库恢复期间遇到的所有错误都会导致恢复失败和可疑数据库：

首次打开数据库和/或事务日志文件时的错误发生在恢复之前。 此类错误的示例为 [17204](mssqlserver-17204-database-engine-error.md) 和 [17207](mssqlserver-17207-database-engine-error.md)。 纠正这些错误后，可以继续进行恢复（但如果发生其他恢复错误，则不保证能完成恢复）。 17204 和 17207 等错误不会导致可疑数据库。 实际上，发生这些问题时，数据库的状态为 RECOVERY_PENDING。 

即使发生页面级别错误，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 也可以完成恢复，并仍保持事务一致性。 此过程减少了导致可疑数据库的场景数量。 这种概念称为[延迟的事务](../backup-restore/deferred-transactions-sql-server.md)。

如果恢复期间遇到的错误表明数据库页面存在问题（例如校验和错误或消息 824），则可以在错误待处理的情况下完成恢复。 如果未提交事务，页面上的错误可能会引发[延迟的事务](../backup-restore/deferred-transactions-sql-server.md)，从而允许完成恢复。  

以下错误日志条目展示了这样一个示例：在恢复期间遇到消息 [824](mssqlserver-824-database-engine-error.md) 错误，但由于延迟的事务，恢复得以完成。 请注意，这种情况下不存在错误 3414，并且请留意数据库恢复已完成的消息：

```2010-03-31 19:17:18.45 spid7s      Error: 824, Severity: 24, State: 2.   
2010-03-31 19:17:18.45 spid7s      SQL Server detected a logical consistency-based I/O error: incorrect checksum (expected: 0xb2c87a0a; actual: 0xb6c0a5e2). It occurred during a read of page (1:153) in database ID 13 at offset 0x00000000132000 in file 'C:\Program Files\Microsoft SQL Server\MSSQL10.SQL2008\MSSQL\DATA\mydb.mdf'.  Additional messages in the SQL Server error log or system event log may provide more detail. This is a severe error condition that threatens database integrity and must be corrected immediately. Complete a full database consistency check (DBCC CHECKDB). This error can be caused by many factors; for more information, see SQL Server Books Online.   
2010-03-31 19:17:18.45 spid7s      Error: 3314, Severity: 21, State: 1.   
2010-03-31 19:17:18.45 spid7s      During undoing of a logged operation in database 'mydb', an error occurred at log record ID (25:100:19). Typically, the specific failure is logged previously as an error in the Windows Event Log service. Restore the database or file from a backup, or repair the database.
2010-03-31 19:17:18.45 spid7s      Errors occurred during recovery while rolling back a transaction. The transaction was deferred. Restore the bad page or file, and re-run recovery.   
2010-03-31 19:17:18.45 spid7s      Recovery completed for database mydb (database ID 13) in 2 second(s) (analysis 204 ms, redo 25 ms, undo 1832 ms.) This is an informational message only. No user action is required.   
```

如果要前滚提交的事务，可以将页面标记为不可访问（后续尝试访问该页面会导致消息 829），并且可以完成恢复。 在这种情况下，若要纠正错误，必须通过从备份中还原页面或结合使用 DBCC CHECKDB 和修复来取消分配页面。


  
## <a name="see-also"></a>另请参阅  
[ALTER DATABASE (Transact-SQL)](~/t-sql/statements/alter-database-transact-sql-set-options.md)  
[DBCC CHECKDB (Transact-SQL)](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[完整数据库还原（简单恢复模式）](~/relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
[延迟的事务](../backup-restore/deferred-transactions-sql-server.md)  
[MSSQLSERVER_824](~/relational-databases/errors-events/mssqlserver-824-database-engine-error.md)  
[sys.databases (Transact-SQL)](~/relational-databases/system-catalog-views/sys-databases-transact-sql.md)

  

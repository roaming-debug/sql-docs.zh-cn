---
description: MSSQLSERVER_3314
title: MSSQLSERVER_3314 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3314 (Database Engine error)
ms.assetid: f3a5ca6a-b502-4cab-b3b1-4bc753763fa9
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7b18cf411c241b3c0dc3549bf4503d93782c6f91
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190895"
---
# <a name="mssqlserver_3314"></a>MSSQLSERVER_3314
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|3314|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|ERR_LOG_RID2|  
|消息正文|在数据库 '%.*ls' 中撤消日志记录下的操作时，在日志记录 %S_LSN 处出错。 通常，这一特定故障以前在 Windows 事件日志服务中会记录为错误。 请利用备份还原数据库或文件，或者修复该数据库。|  
  
## <a name="explanation"></a>说明  
这是撤消恢复的累积错误。 此错误使数据库进入 SUSPECT 状态。 主文件组以及可能其他文件组可疑并可能受损。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动过程中无法恢复数据库，因此无法使用该数据库。 需要用户执行操作来解决问题。  
  
请注意，如果对于 **tempdb** 发生此错误，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例关闭。  
  
## <a name="user-action"></a>用户操作  
此错误可能是由在某次尝试启动服务器实例或恢复数据库的过程中系统上存在的暂时性条件导致的。 此错误也可能是由当您每次尝试启动数据库时发生的永久性错误导致的。 有关原因的信息，请检查 Windows 事件日志以了解有关指示特定故障的先前错误。  
  
有关错误 3314 出现原因的信息，请检查 Windows 事件日志以了解有关指示特定故障的先前错误。 相应的用户操作取决于 Windows 事件日志中的信息是否指示该 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误由暂时条件或永久性故障导致。 有关排除 3314 错误的用户操作的信息，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书。  
  
## <a name="see-also"></a>另请参阅  
[ALTER DATABASE (Transact-SQL)](~/t-sql/statements/alter-database-transact-sql-set-options.md)  
[DBCC CHECKDB (Transact-SQL)](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[完整数据库还原（简单恢复模式）](~/relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
[MSSQLSERVER_824](~/relational-databases/errors-events/mssqlserver-824-database-engine-error.md)  
[sys.databases (Transact-SQL)](~/relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
  

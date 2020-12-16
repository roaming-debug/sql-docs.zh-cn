---
description: 文件状态
title: 文件状态 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- restoring file state [SQL Server]
- verifying file states
- current file states
- verifying filegroup states
- file states [SQL Server]
- online file state
- offline file state [SQL Server]
- viewing filegroup states
- viewing file states
- suspect file state
- recovering file state [SQL Server]
- current filegroup state
- recovery pending file state [SQL Server]
- displaying file states
- states [SQL Server], files
- displaying filegroup states
- defunct file state
ms.assetid: b426474d-8954-4df0-b78b-887becfbe8d6
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 261969b4be7d1ea6edfc2a661dbc5782593ccd8f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474098"
---
# <a name="file-states"></a>文件状态
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，数据库文件的状态独立于数据库的状态。 文件始终处于一个特定状态，例如 ONLINE 或 OFFLINE。 若要查看文件的当前状态，请使用 [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md) 或 [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) 目录视图。 如果数据库处于离线状态，则可以从 [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md) 目录视图中查看文件的状态。  
  
 文件组中文件的状态决定整个文件组的可用性。 文件组中的所有文件都必须联机，文件组才可用。 若要查看文件组的当前状态，请使用 [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md) 目录视图。 如果文件组处于离线状态，而您尝试使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句访问该文件组，则操作将失败并显示一条错误。 当查询优化器生成 SELECT 语句的查询计划时，它将避免使用位于离线文件组中的非聚集索引和索引视图，从而使这些语句成功。 但是，如果脱机文件组包含目标表的堆或聚集索引，SELECT 语句将失败。 此外，如果 INSERT、UPDATE 或 DELETE 语句修改的表的索引包含在脱机文件组中，这些语句将失败。  
  
## <a name="file-state-definitions"></a>文件状态定义  
 下表定义了文件状态。  
  
|状态|定义|  
|-----------|----------------|  
|ONLINE|文件可用于所有操作。 如果数据库本身处于在线状态，则主文件组中的文件始终处于在线状态。 如果主文件组中的文件处于离线状态，则数据库将处于离线状态，并且辅助文件的状态未定义。|  
|OFFLINE|文件不可访问，并且可能不显示在磁盘中。 文件通过显式用户操作变为离线，并在执行其他用户操作之前保持离线状态。<br /><br /> **\*\*警告\*\*** 如果文件损坏但可还原，则文件状态可设置为离线。 设置为离线的文件只能通过从备份还原才能设置为在线。 有关还原单个文件的详细信息，请参阅 [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md)。 <br /><br /> 如果数据库处于完全或批量记录恢复状态且文件已删除时，数据库文件也设置为离线。 sys.master_files 中的条目继续存在，直到超过 drop_lsn 值截断事务日志。 有关详细信息，请参阅[事务日志截断](../../relational-databases/logs/the-transaction-log-sql-server.md#Truncation)。 |  
|RESTORING|正在还原文件。 文件处于还原状态（因为还原命令会影响整个文件，而不仅是页还原），并且在还原完成及文件恢复之前，一直保持此状态。|  
|RECOVERY PENDING|文件恢复被推迟。 由于在段落还原过程中未还原和恢复文件，因此文件将自动进入此状态。 需要用户执行其他操作来解决该错误，并允许完成恢复过程。 有关详细信息，请参阅[段落还原 (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)。|  
|SUSPECT|联机还原过程中，恢复文件失败。 如果文件位于主文件组，则数据库还将标记为可疑。 否则，仅文件处于可疑状态，而数据库仍处于在线状态。<br /><br /> 在通过以下方法之一将文件变为可用之前，该文件将保持可疑状态：<br /><br /> 还原和恢复<br /><br /> 包含 REPAIR_ALLOW_DATA_LOSS 的 BCC CHECKDB|  
|DEFUNCT|当文件不处于在线状态时被删除。 删除离线文件组后，文件组中的所有文件都将失效。|  
  
## <a name="related-content"></a>相关内容  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)  
  
 [数据库状态](../../relational-databases/databases/database-states.md)  
  
 [镜像状态 (SQL Server)](../../database-engine/database-mirroring/mirroring-states-sql-server.md)  
  
 [DBCC CHECKDB (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
  
 [数据库文件和文件组](../../relational-databases/databases/database-files-and-filegroups.md)  

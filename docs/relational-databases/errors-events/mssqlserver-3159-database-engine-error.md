---
description: MSSQLSERVER_3159
title: MSSQLSERVER_3159 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3159 (Database Engine error)
ms.assetid: c93c1003-0e3a-40aa-9873-44a0f5b8b57e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d83e149a8f17c509f2c34c9284c43b92828091dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194228"
---
# <a name="mssqlserver_3159"></a>MSSQLSERVER_3159
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|事件 ID|3159|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|LDDB_LOGNOTBACKEDUP|  
|消息正文|数据库 "%ls" 的日志尾部尚未备份。 如果该日志包含您不希望丢失的工作，则使用 BACKUP LOG WITH NORECOVERY 对其进行备份。 使用 RESTORE 语句的 WITH REPLACE 或 WITH STOPAT 子句覆盖该日志的内容。|  
  
## <a name="explanation"></a>说明  
在大多数情况下，在完整恢复模式或大容量日志恢复模式下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 要求您备份日志尾部以捕获尚未备份的日志记录。 还原操作之前对日志尾部执行的日志备份称为“结尾日志备份”。  
  
将数据库恢复到故障点时，结尾日志备份是恢复计划中的最后一个相关备份。 如果无法备份日志尾部，则只能将数据库恢复为发生故障前创建的最后一个备份。  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通常要求您在开始还原数据库前执行结尾日志备份。 结尾日志备份可以防止工作丢失并确保日志链的完整性。 但是，并非所有还原方案都要求执行结尾日志备份。 如果先前的日志备份中包含恢复点，或者您准备移动或替换（覆盖）数据库，并且在最新备份后不需要将该数据库恢复到某一时间点，则不一定需要结尾日志备份。 并且，如果日志文件受损且无法创建结尾日志备份，则必须在不使用结尾日志备份的情况下还原数据库。 最新日志备份后提交的任何事务都将丢失。 有关详细信息，请参阅本主题下文中的“不使用结尾日志备份执行还原操作”。  
  
> [!CAUTION]  
> 应尽可能避免使用 REPLACE，在使用该选项之前必须慎重考虑。  
  
## <a name="user-action"></a>用户操作  
执行结尾日志备份，然后重试还原操作。  
  
如果不能备份日志尾部，则使用 RESTORE 语句中的 WITH STOPAT 或 WITH REPLACE 子句。  
  
## <a name="see-also"></a>另请参阅  
[将 SQL Server 数据库还原到某个时间点（完整恢复模式）](~/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
[在数据库损坏时备份事务日志 (SQL Server)](~/relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)  
[备份事务日志 (SQL Server)](~/relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
[结尾日志备份 (SQL Server)](~/relational-databases/backup-restore/tail-log-backups-sql-server.md)  
  

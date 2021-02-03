---
title: 还原、恢复和管理备份
description: 用于还原、恢复和管理备份的 Transact-SQL RESTORE 语句。
ms.custom: seo-lt-2019
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- restoring files [SQL Server], RESTORE statement
- RESTORE statement, about restore operations
- database restores [SQL Server], RESTORE statement
- restoring databases [SQL Server], RESTORE statement
- database backups [SQL Server], RESTORE statement
- backing up databases [SQL Server], RESTORE statement
- file restores [SQL Server], RESTORE statement
- transaction log backups [SQL Server], RESTORE statement
ms.assetid: fb29a151-f312-4f85-b857-5deeca0de8ce
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 533840c991966b40d27f5069e9a14ee1e2a638e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159320"
---
# <a name="restore-statements-for-restoring-recovering-and-managing-backups-transact-sql"></a>用于还原、恢复和管理备份的 RESTORE 语句 (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md.md )]

  本节说明用于备份的 RESTORE 语句。 除了用于还原和恢复备份的主要语句 RESTORE {DATABASE | LOG} 以外，还可以使用多个辅助 RESTORE 语句来管理备份和计划还原序列。 辅助 RESTORE 命令包括：RESTORE FILELISTONLY、RESTORE HEADERONLY、RESTORE LABELONLY、RESTORE REWINDONLY 和 RESTORE VERIFYONLY。  
  
> [!IMPORTANT]  
>  在以前的 SQL Server 版本中，任何用户都能够使用 RESTORE FILELISTONLY、RESTORE HEADERONLY、RESTORE LABELONLY 和 RESTORE VERIFYONLY Transact-SQL 语句来获取有关备份集和备份设备的信息。 由于它们会泄露有关备份文件内容的信息，因此，在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本中，这些语句需要 CREATE DATABASE 权限。 与以前的版本相比，这项新要求为您的备份文件提高了安全性，并更周全地保护了您的备份信息。 有关此权限的信息，请参阅 [GRANT 数据库权限 (Transact-SQL)](../../t-sql/statements/grant-database-permissions-transact-sql.md)。  
  
## <a name="in-this-section"></a>本节内容  
  
|语句|说明|  
|---------------|-----------------|  
|[RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)|说明 RESTORE DATABASE 和 RESTORE LOG Transact-SQL 语句，这些语句用于从使用 BACKUP 命令创建的备份还原和恢复数据库。 RESTORE DATABASE 可用于任何恢复模式下的数据库。 RESTORE LOG 仅用于完全恢复模式和大容量日志记录恢复模式。 RESTORE DATABASE 也可用于将数据库恢复为数据库快照。|  
|[RESTORE 参数 (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md)|介绍在 RESTORE 语句以及下面的关联辅助语句集的“语法”部分中说明的参数：RESTORE FILELISTONLY、RESTORE HEADERONLY、RESTORE LABELONLY、RESTORE REWINDONLY 和 RESTORE VERIFYONLY。 大多数参数都仅由这六个语句中的一部分支持。 每个参数的说明中都指示了相应的支持信息。|  
|[RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)|说明 RESTORE FILELISTONLY Transact-SQL 语句，该语句用于返回一个结果集，其中包括备份集中包含的一组数据库和日志文件。|  
|[RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)|说明 RESTORE HEADERONLY Transact-SQL 语句，该语句用于返回一个结果集，其中包含特定备份设备上所有备份集的所有备份标头信息。|  
|[RESTORE LABELONLY (Transact-SQL)](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)|说明 RESTORE LABELONLY Transact-SQL 语句，该语句用于返回一个结果集，其中包含有关给定备份设备标识的备份介质的信息。|  
|[RESTORE REWINDONLY (Transact-SQL)](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)|说明 RESTORE REWINDONLY Transact-SQL 语句，该语句用于倒带和关闭由 NOREWIND 选项执行 BACKUP 或 RESTORE 语句而使其保持打开的磁盘设备。|  
|[RESTORE VERIFYONLY (Transact-SQL)](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)|说明 RESTORE VERIFYONLY Transact-SQL 语句，该语句用于验证但不还原备份，并检查备份集是否已完成以及整个备份是否可读；不会尝试验证数据的结构。|  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 数据库的备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)  
  
  

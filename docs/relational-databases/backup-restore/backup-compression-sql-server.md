---
title: 备份压缩 (SQL Server) | Microsoft Docs
description: 了解 SQL Server 备份的压缩，包括限制、性能折中、配置备份压缩以及压缩率。
ms.custom: ''
ms.date: 07/08/2020
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], backup compression
- backup compression [SQL Server], about backup compression
- compression [SQL Server], backup compression
- backups [SQL Server], compression
- backing up [SQL Server], backup compression
- backup compression [SQL Server]
ms.assetid: 05bc9c4f-3947-4dd4-b823-db77519bd4d2
author: cawrites
ms.author: chadam
ms.openlocfilehash: a98aaaea7d243853f31d19c0e7c3aa824ee2cc4e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171469"
---
# <a name="backup-compression-sql-server"></a>备份压缩 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份的压缩，包括限制、压缩备份时的性能折中、备份压缩的配置以及压缩率。  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本支持备份压缩：企业版、标准版和开发人员版。  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 的每个版本和更高版本都可以还原已压缩的备份。 
 
  
##  <a name="benefits"></a><a name="Benefits"></a> 优势  
  
-   因为相同数据的压缩的备份比未压缩备份小，所以压缩备份所需的设备 I/O 通常较少，因此通常可大大提高备份速度。  
  
     有关详细信息，请参阅本主题后面的 [压缩备份的性能影响](#PerfImpact)。  
  
  
##  <a name="restrictions"></a><a name="Restrictions"></a> 限制  
 压缩的备份具有以下限制条件：  
  
-   压缩的备份和未压缩的备份不能共存于一个介质集中。  
  
-   早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法读取压缩的备份。  
  
-   NTbackup 无法共享包含压缩的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份的磁带。  
  
  
##  <a name="performance-impact-of-compressing-backups"></a><a name="PerfImpact"></a> 压缩备份的性能影响  
 默认情况下，压缩会显著增加 CPU 的使用，并且压缩进程所消耗的额外 CPU 可能会对并发操作产生不利影响。 因此，你可能需要在会话中创建低优先级的压缩备份，其 CPU 使用率受[资源调控器](../../relational-databases/resource-governor/resource-governor.md)限制。 有关详细信息，请参阅本主题后面的 [使用资源调控器限制备份压缩的 CPU 使用量 (Transact-SQL)](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md)限制。  
  
 若要很好地了解备份 I/O 的性能表现，可以通过评估以下类型的性能计数器来分别考察进入设备或来自设备的备份 I/O：  
  
-   Windows I/O 性能计数器，例如物理磁盘计数器  
  
-   **SQLServer:备份设备** 对象的 [设备吞吐量（字节/秒）](../../relational-databases/performance-monitor/sql-server-backup-device-object.md) 计数器  
  
-   **SQLServer:数据库** 对象的 [备份/还原吞吐量/秒](../../relational-databases/performance-monitor/sql-server-databases-object.md) 计数器  
  
 有关 Windows 计数器的信息，请参阅 Windows 帮助。 有关如何使用 SQL Server 计数器的信息，请参阅 [使用 SQL Server 对象](../../relational-databases/performance-monitor/use-sql-server-objects.md)。  
  
   
##  <a name="calculate-the-compression-ratio-of-a-compressed-backup"></a><a name="CompressionRatio"></a> 计算压缩的备份的压缩率  
 若要计算备份的压缩率，请使用 **backupset** 历史记录表的 **backup_size** 列和 [compressed_backup_size](../../relational-databases/system-tables/backupset-transact-sql.md) 列中有关此备份的值，如下所示：  
  
 **backup_size**:**compressed_backup_size**  
  
 例如，3:1 的压缩率表明您可以节省大约 66% 的磁盘空间。 若要查询这些列，可以使用以下 Transact-SQL 语句：  
  
```sql  
SELECT backup_size/compressed_backup_size FROM msdb..backupset;  
```  
  
 压缩备份的压缩率取决于所压缩的数据。 有多种因素会影响所获得的压缩率。 主要因素包括：  
  
-   数据类型。  
  
     字符数据的压缩率要高于其他类型的数据。  
  
-   页面上各行间数据的一致性。  
  
     通常，如果某页包含多个行，而其中的某个字段包含相同的值，则该值可获得较大的压缩。 相反，对于包含随机数据或者每页只有一个很大的行的数据库，压缩备份的大小几乎与未压缩的备份相同。  
  
-   数据是否加密  
  
     与未加密数据相比，同样的加密数据的压缩率要小得多。 例如，如果使用 Always Encrypted 或其他应用程序级别的加密在列级别上对数据进行加密，则压缩备份后大小可能变化不大。

     要详细了解如何压缩使用透明数据加密 (TDE) 进行加密的数据库，请参阅[压缩使用 TDE 的备份](#backup-compression-with-tde)。

-   数据库是否压缩。  
  
     如果压缩数据库，则压缩备份不会将大小减小很多，甚至根本不会减小。  

## <a name="backup-compression-with-tde"></a>压缩使用 TDE 的备份

从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 开始，`MAXTRANSFERSIZE` 大于 65536 (64 KB) 的设置为[透明数据加密 (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) 加密数据库启用优化的压缩算法，该算法先解密页面，然后将其压缩并再次对其进行加密。 如果未指定 `MAXTRANSFERSIZE` 或者使用了 `MAXTRANSFERSIZE = 65536` (64 KB)，对使用 TDE 加密的数据库执行备份压缩时会直接压缩加密的页面，且可能不会得到良好的压缩比率。 有关详细信息，请参阅[支持 TDE 的数据库的备份压缩](/archive/blogs/sqlcat/sqlsweet16-episode-1-backup-compression-for-tde-enabled-databases)。

从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] CU5 开始，不再需要 `MAXTRANSFERSIZE` 设置来对 TDE 启用此优化的压缩算法。 如果备份命令指定 `WITH COMPRESSION`，或者将备份压缩默认服务器配置设置为 1，则 `MAXTRANSFERSIZE` 将自动增加到 128K 以启用优化算法。 如果备份命令指定 `MAXTRANSFERSIZE` 的值为 > 64K，则将采用提供的值。 换句话说，SQL Server 绝不会自动减小该值，只会增加它。 如果需要使用 `MAXTRANSFERSIZE = 65536` 备份 TDE 加密的数据库，则必须指定 `WITH NO_COMPRESSION`，或者确保将备份压缩默认服务器配置设置为 0。

有关详细信息，请参阅 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)。

##  <a name="allocation-of-space-for-the-backup-file"></a><a name="Allocation"></a> 为备份文件分配空间  
 对于压缩的备份，最终备份文件的大小取决于数据可压缩程度，这在备份操作完成之前是未知的。  因此，默认情况下，在使用压缩备份数据库时，数据库引擎将预先分配算法用于备份文件。 此算法为备份文件预先分配数据库大小的预定义的百分比。 如果在备份操作过程中需要更多空间，则数据库引擎会增大该文件。 如果最终大小小于分配的空间，则在备份操作结束时，数据库引擎会将该文件收缩到备份的实际的最终大小。  
  
 若要允许备份文件仅在需要时增大以便达到其最终大小，则使用跟踪标志 3042。 跟踪标志 3042 导致备份操作绕过默认的备份压缩预先分配算法。 如果您需要仅分配压缩的备份所需的实际大小以便节约空间，则此跟踪标志将很有用。 但是，使用此跟踪标志可能会导致轻微的性能损失（在备份操作期间损失可能会增加）。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [配置备份压缩 (SQL Server)](../../relational-databases/backup-restore/configure-backup-compression-sql-server.md)  
  
-   [查看或配置 backup compression default 服务器配置选项](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)  
  
-   [使用资源调控器限制备份压缩的 CPU 使用量 (Transact-SQL)](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md)  
  
-   [DBCC TRACEON (Transact-SQL)](../../t-sql/database-console-commands/dbcc-traceon-transact-sql.md)  
  
-   [DBCC TRACEOFF (Transact-SQL)](../../t-sql/database-console-commands/dbcc-traceoff-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
 [备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [跟踪标志 (Transact-SQL)](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)  
  

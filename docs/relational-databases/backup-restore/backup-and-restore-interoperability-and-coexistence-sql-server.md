---
title: 备份和还原：功能互操作性
description: 本文介绍 SQL Server 中的备份和还原功能，包括数据库启动、联机还原和禁用的索引以及数据库镜像。
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- file restores [SQL Server], related features
- restoring [SQL Server], files
- restoring files [SQL Server], related features
- backups [SQL Server], files or filegroups
- file backups [SQL Server], related features
ms.assetid: 69f212b8-edcd-4c5d-8a8a-679ced33c128
author: cawrites
ms.author: chadam
ms.openlocfilehash: 106773df7a5e9f88c123b614688ca19722613d7f
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130533"
---
# <a name="backup-and-restore-interoperability-and-coexistence-sql-server"></a>备份和还原：互操作性和共存 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题描述 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]中若干功能的备份和还原注意事项。 这些功能包括：文件还原和数据库启动、联机还原和禁用的索引、数据库镜像以及段落还原和全文索引。  
  
 **本主题内容：**  
  
-   [文件还原和数据库启动](#FileRestoreAndDbStartup)  
  
-   [联机还原和禁用的索引](#OnlineRestoreAndDisabledIndexes)  
  
-   [数据库镜像以及备份和还原](#DbMandBnR)  
  
-   [段落还原和全文索引](#PiecemealAndFTIndexes)  
  
-   [文件备份、还原和压缩](#FileBnRandCompression)  
  
-   [相关任务](#RelatedTasks)  
  
##  <a name="file-restore-and-database-startup"></a><a name="FileRestoreAndDbStartup"></a> 文件还原和数据库启动  
 本节仅与包含多个文件组的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库相关。  
  
> [!NOTE]  
>  数据库启动时，只有其文件在数据库关闭时处于联机状态的文件组能够恢复并处于联机状态。  
  
 如果数据库启动过程中出现问题，恢复将失败且数据库被标记为 SUSPECT。 如果可以将问题隔离到单个文件或多个文件，则数据库管理员可以使文件脱机并尝试重新启动数据库。 若要使文件脱机，可以使用下列 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) 语句：  
  
 ALTER DATABASE *database_name* MODIFY FILE (NAME **='** _filename_*_'_*, OFFLINE)  
  
 如果数据库成功启动，则任何包含脱机文件的文件组将保持脱机状态。  
  
##  <a name="online-restore-and-disabled-indexes"></a><a name="OnlineRestoreAndDisabledIndexes"></a> 联机还原和禁用的索引  
 本节仅适用于包含多个文件组的数据库；在简单恢复模式下，适用于至少包含一个只读文件组的数据库。  
  
 在此类情况下，当数据库处于联机状态时，仅当包含索引任意部分的所有文件组均联机时，才能创建、删除、启用或禁用索引。  
  
 有关还原脱机文件组的信息，请参阅[联机还原 (SQL Server)](../../relational-databases/backup-restore/online-restore-sql-server.md)。  
  
##  <a name="database-mirroring-and-backup-and-restore"></a><a name="DbMandBnR"></a> 数据库镜像以及备份和还原  
 本节仅与包含多个文件组的完整模式数据库有关。  
  
> [!NOTE]  
>  后续版本的 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]将删除数据库镜像功能。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 请改用 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]。  
  
 “数据库镜像”是一种提高数据库可用性的解决方案。 镜像基于每个数据库实现，并且只适用于使用完整恢复模式的数据库。 有关详细信息，请参阅[数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)。  
  
> [!NOTE]  
>  若要分发数据库中部分文件组的副本，请使用复制：仅复制文件组中您要复制到其他服务器的那些对象。 有关复制的详细信息，请参阅 [SQL Server 复制](../../relational-databases/replication/sql-server-replication.md)。  
  
### <a name="creating-the-mirror-database"></a>创建镜像数据库  
 镜像数据库是通过还原 (WITH NORECOVERY) 镜像服务器上主体数据库的备份创建的。 还原后的数据库必须保持相同的数据库名称。 有关详细信息，请参阅 [为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)的各版本中均未提供见证服务器实例。  
  
 您可以使用段落还原顺序在支持镜像数据库的地方创建镜像数据库。 但是，在开始镜像之前，必须还原所有文件组后才能，通常还必须还原日志备份以保证镜像数据库的时间与原始数据库的时间足够接近。 有关详细信息，请参阅[段落还原 (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)。  
  
### <a name="restrictions-on-backup-and-restore-during-mirroring"></a>镜像期间备份和还原的限制  
 数据库镜像会话活动时，将应用以下限制：  
  
-   不允许备份和还原镜像数据库。  
  
-   允许备份主体数据库，但不允许 BACKUP LOG WITH NORECOVERY。  
  
-   不允许还原主体数据库。  
  
##  <a name="piecemeal-restore-and-full-text-indexes"></a><a name="PiecemealAndFTIndexes"></a> 段落还原和全文索引  
 本节仅与包含多个文件组的数据库有关；对于简单模式数据库，仅与只读文件组有关。  
  
 全文索引存储在数据库文件组中，受段落还原的影响。 如果全文索引与任何关联的表数据位于同一文件组中，则段落还原将按预期的方式工作。  
  
> [!NOTE]  
>  若要查看包含全文索引的文件组的文件组 ID，请选择 [sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)的 data_space_id 列。  
  
### <a name="full-text-indexes-and-tables-in-separate-filegroups"></a>全文索引和表在不同的文件组中  
 如果全文索引与所有关联的表数据位于不同的文件组中，则段落还原的行为取决于哪个文件组首先还原并变为联机：  
  
-   如果包含全文索引的文件组先于包含关联表数据的文件组还原并变为联机，则在全文索引联机后，全文搜索将按预期的方式工作。  
  
-   如果包含表数据的文件组先于包含全文索引的文件组还原并变为联机，则全文行为可能会受到影响。 这是因为在索引联机之前，触发填充、重新生成目录或者重新组织目录的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句会失败。 这些语句包括 CREATE FULLTEXT INDEX、ALTER FULLTEXT INDEX、DROP FULLTEXT INDEX 和 ALTER FULLTEXT CATALOG。  
  
     在这种情况下，下列因素就至关重要：  
  
    -   如果全文索引具有更改跟踪，则在索引文件组联机之前，用户 DML 会失败。 在索引文件组联机之前，删除操作也会失败。  
  
    -   不管是否有跟踪更改，由于索引不可用，全文查询都会失败。 如果在包含全文索引的文件组脱机时尝试全文查询，则会返回错误。  
  
    -   状态函数（如 FULLTEXTCATALOGPROPERTY）只有在不必访问全文索引时才会成功。 例如，访问任何联机的全文元数据都会成功，但 **uniquekeycount, itemcount** 会失败。  
  
     在全文索引文件组还原并变为联机之后，目录数据和表数据将一致。  
  
 在基表文件组和全文索引文件组均联机后，将立即恢复所有暂停的全文填充。  
  
##  <a name="file-backup-and-restore-and-compression"></a><a name="FileBnRandCompression"></a> 文件备份、还原和压缩  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持对只读文件组和只读数据库进行 NTFS 文件系统数据压缩。  
  
 压缩的 NTFS 文件支持还原只读文件组中的文件。 这些文件组的备份和还原实质上与任何只读文件组相同，但下列情况除外：  
  
-   将读写文件（包括读写数据库的主文件或日志文件）还原为压缩卷会失败并显示错误。  
  
-   但是，将只读数据库还原为压缩卷是允许的。  
  
> [!NOTE]  
>  读/写数据库的日志文件决不要放在压缩文件系统中。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [备份和还原全文目录和索引](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 数据库的备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [备份和还原复制的数据库](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)   
[活动次要副本：备份次要副本 \(Always On 可用性组\)](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)  
  
  

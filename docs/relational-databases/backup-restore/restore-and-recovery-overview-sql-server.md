---
title: 还原与恢复概述 (SQL Server) | Microsoft Docs
description: 本文主要介绍了通过依次还原一组 SQL Server 备份，从故障中恢复 SQL Server 数据库所涉及的操作。
ms.custom: ''
ms.date: 04/23/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- restoring tables [SQL Server]
- backups [SQL Server], restore scenarios
- database backups [SQL Server], restore scenarios
- database restores [SQL Server]
- restoring [SQL Server]
- restores [SQL Server]
- table restores [SQL Server]
- restoring databases [SQL Server], about restoring databases
- database restores [SQL Server], scenarios
- accelerated database recovery
ms.assetid: e985c9a6-4230-4087-9fdb-de8571ba5a5f
author: cawrites
ms.author: chadam
ms.openlocfilehash: ef3d409ae656776b870119ccd14cb211cc16b32c
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125561"
---
# <a name="restore-and-recovery-overview-sql-server"></a>还原与恢复概述 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  若要从故障中恢复 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库，数据库管理员必须按照逻辑正确并且有意义的还原顺序还原一组 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 还原和恢复支持从整个数据库、数据文件或数据页的备份还原数据，如下所示：  
  
-   数据库（“数据库完整还原”  ）  
  
     还原和恢复整个数据库，并且数据库在还原和恢复操作期间处于脱机状态。  
  
-   数据文件（“文件还原”  ）  
  
     还原和恢复一个数据文件或一组文件。 在文件还原过程中，包含相应文件的文件组在还原过程中自动变为脱机状态。 访问脱机文件组的任何尝试都会导致错误。  
  
-   数据页（“页面还原”  ）  
  
     在完整恢复模式或大容量日志恢复模式下，可以还原单个数据库。 可以对任何数据库执行页面还原，而不管文件组数为多少。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份和还原对所有支持的操作系统都有效。 有关支持的操作系统的信息，请参阅 [安装 SQL Server 2016 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)。 有关支持从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的早期版本进行备份的信息，请参阅 [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md)中的“兼容性支持”部分。  
  
##  <a name="overview-of-restore-scenarios"></a><a name="RestoreScenariosOv"></a> 还原方案概述  
 *中的“还原方案*[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ”是从一个或多个备份还原数据、继而恢复数据库的过程。 支持的还原方案取决于数据库的恢复模式和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的版本。  
  
 下表介绍了不同恢复模式所支持的可行还原方案。  
  
|还原方案|在简单恢复模式下|在完整/大容量日志恢复模式下|  
|----------------------|---------------------------------|----------------------------------------------|  
|数据库完整还原|这是基本的还原策略。 数据库完整还原可能涉及完整数据库备份的简单还原和恢复。 另外，完整的数据库还原还可能涉及还原完整数据库备份，以及还原和恢复差异备份。<br /><br /> 有关详细信息，请参阅[完整数据库还原（简单恢复模式）](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)。|这是基本的还原策略。 数据库完整还原涉及还原完整数据库备份或差异备份（如果有），以及还原所有后续日志备份（按顺序）。 通过恢复并还原上一次日志备份 (RESTORE WITH RECOVERY) 完成数据库完整还原。<br /><br /> 有关详细信息，请参阅[完整数据库还原（完整恢复模式）](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md)。|  
|文件还原 * *\** _|还原损坏的只读文件，但不还原整个数据库。 仅在数据库至少有一个只读文件组时才可以进行文件还原。|还原一个或多个文件，而不还原整个数据库。 可以在数据库处于脱机状态时执行文件还原，对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的某些版本，也可以在数据库仍处于联机状态时执行。 在文件还原过程中，包含正在还原的文件的文件组一直处于脱机状态。|  
|页面还原|不适用|还原损坏的页面。 可以在数据库处于脱机状态时执行页面还原，对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的某些版本，也可以在数据库仍处于联机状态时执行。 在页面还原过程中，正在还原的页面一直处于脱机状态。<br /><br /> 必须具有完整的日志备份链（包含当前日志文件），并且必须应用所有这些日志备份以使页面与当前日志文件保持一致。<br /><br /> 有关详细信息，请参阅[还原页 (SQL Server)](../../relational-databases/backup-restore/restore-pages-sql-server.md)。|  
|段落还原 _*\**_|按文件组级别并从主文件组和所有读写辅助文件组开始，分阶段还原和恢复数据库。|按文件组级别并从主文件组开始，分阶段还原和恢复数据库。<br /><br /> 有关详细信息，请参阅[段落还原 (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)。|  
  
 _*\**_ 仅在 Enterprise Edition 中支持联机还原。  

### <a name="steps-to-restore-a-database"></a>还原数据库的步骤
若要执行文件还原，[!INCLUDE[ssde_md](../../includes/ssde_md.md)] 执行两个步骤： 

-   创建所有丢失的数据库文件。

-   将数据从备份设备复制到数据库文件。

若要执行数据库还原，[!INCLUDE[ssde_md](../../includes/ssde_md.md)] 执行三个步骤： 

-   创建数据库和事务日志文件（如果它们不存在）。

-   从数据库的备份介质将所有数据、日志和索引页复制到数据库文件中。 

-   在所谓的[恢复过程](#TlogAndRecovery)中应用事务日志。

 无论以何种方式还原数据，在恢复数据库前， [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 都会保证整个数据库在逻辑上的一致性。 例如，若要还原一个文件，则必须将该文件前滚足够长度，以便与数据库保持一致，才能恢复该文件并使其联机。  
  
### <a name="advantages-of-a-file-or-page-restore"></a>文件还原或页面还原的优点  
 只还原和恢复个别文件或页面（而非整个数据库）的方法具有以下优点：  
  
-   还原少量数据可以缩短复制和恢复数据的时间。  
  
-   在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，还原文件或页面的操作可能会允许数据库中的其他数据在还原操作期间仍保持联机状态。  

## <a name="recovery-and-the-transaction-log"></a><a name="TlogAndRecovery"></a> 恢复和事务日志
对于大多数还原方案，需要应用事务日志备份并允许 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 运行恢复过程才能使数据库联机。 恢复是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用于让每个数据库以事务一致状态或干净状态启动的进程。

如果故障转移或其他非干净关闭，数据库可能处于这样的状态：某些修改从未从缓冲区缓存写入数据文件，且在数据文件内可能有未完成事务所做的某些修改。 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例启动时，将根据最后一个[数据库检查点](../../relational-databases/logs/database-checkpoints-sql-server.md)运行每个数据库的恢复，其中包含三个阶段：

-   “分析阶段”  分析事务日志以确定最后一个检查点，并创建脏页表 (DPT) 和活动事务表 (ATT)。 DPT 包含在数据库关闭时处于脏状态的页面的记录。 ATT 包含在数据库未正常关闭时处于活动状态的事务的记录。

-   “重做阶段”  前滚日志中记录的且在数据库关闭时可能尚未写入数据文件的每个修改。 成功进行数据库范围内恢复所需的[最小日志序列号](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#minlsn) (minLSN) 在 DPT 中找到，并标记了所有脏页上所需的重做操作的开始时间。 在此阶段中，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 会将属于提交的事务的所有脏页写入磁盘。

-   “撤消阶段”  回滚 ATT 中找到的未完成的事务，以确保数据库的完整性。 回滚后，数据库将进入联机状态，不能再将其他事务日志备份应用到数据库。

有关每个数据库恢复阶段的进度的信息记录在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [错误日志](../../tools/configuration-manager/viewing-the-sql-server-error-log.md)中。 还可以使用扩展事件跟踪数据库恢复进度。 有关详细信息，请参阅博客文章[数据库恢复进度的新扩展事件](/archive/blogs/sql_server_team/new-extended-events-for-database-recovery-progress)。

> [!NOTE]
> 对于段落还原方案，如果在文件备份创建之前，只读文件组就已处于只读状态，则该文件组无需应用日志备份，并且文件还原会跳过日志备份的应用过程。 

<a name="FastRecovery"></a>
> [!NOTE]
> 若要在最大程度上使用企业环境中的数据库，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise Edition 可以在重做阶段后使数据库联机，而撤消阶段仍在执行。 这称为“快速恢复”。

##  <a name="recovery-models-and-supported-restore-operations"></a><a name="RMsAndSupportedRestoreOps"></a> 恢复模式和支持的还原操作  
 可用于数据库的还原操作取决于所用的恢复模式。 下表简要说明了每种恢复模式是否支持给定的还原方案以及适用范围。  
  
|还原操作|完整恢复模式|大容量日志恢复模式|简单恢复模式|  
|-----------------------|-------------------------|---------------------------------|---------------------------|  
|数据恢复|完整还原（如果日志可用）。|某些数据将丢失。|自上次完整备份或差异备份后的任何数据将丢失。|  
|时点还原|日志备份所涵盖的任何时间。|日志备份包含任何大容量日志更改时不允许。|不支持。|  
|文件还原 * *\** _|完全支持。|不完全支持。_ *\*\** *|仅对只读辅助文件可用。|  
|页面还原 * *\** _|完全支持。|不完全支持。_ *\*\** *|无。|  
|段落（文件组级）还原 * *\** _|完全支持。|不完全支持。_ *\*\** *|仅对只读辅助文件可用。|  
  
 * *\** _ 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 Enterprise Edition 中可用  
  
 _ *\*\** * 对于所需条件，请参阅本主题后面的[简单恢复模式下的还原限制](#RMsimpleScenarios)。  
  
> [!IMPORTANT]  
> 无论数据库的恢复模式如何，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份都无法还原到早于创建该备份的版本的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 版本。  
  
## <a name="restore-scenarios-under-the-simple-recovery-model"></a><a name="RMsimpleScenarios"></a> 简单恢复模式下的还原方案  
 简单恢复模式对还原操作有下列限制：  
  
-   文件还原和段落还原仅对只读辅助文件组可用。 有关这些还原方案的信息，请参阅 [文件还原（简单恢复模式）](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md) 和[段落还原 (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)。  
  
-   不允许进行页面还原。  
  
-   不支持时点还原。  
  
 如果这些限制中有任何不适合于恢复要求的内容，我们建议您考虑使用完整恢复模式。 有关详细信息，请参阅 [备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)。  
  
> [!IMPORTANT]  
> 无论数据库的恢复模式如何， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份都无法从早于创建该备份的版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本还原。  
  
##  <a name="restore-under-the-bulk-logged-recovery-model"></a><a name="RMblogRestore"></a> 在大容量日志恢复模式下进行还原  
 本节讨论特定于大容量日志恢复模式的还原注意事项，大容量日志恢复模式专门用于补充完整恢复模式。  
  
> [!NOTE]  
> 有关大容量日志恢复模式的介绍，请参阅[事务日志 (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)。  
  
 通常，大容量日志恢复模式与完整恢复模式相似，针对完整恢复模式的说明信息对两者都适用。 但是，大容量日志恢复模式对时点恢复和联机还原存在影响。  
  
### <a name="restrictions-for-point-in-time-recovery"></a>对时点恢复的限制  
 如果在大容量恢复模式下执行的日志备份包含大容量日志更改，则不允许时点恢复。 试图对包含大容量更改的日志备份执行时点恢复将导致还原操作失败。  
  
### <a name="restrictions-for-online-restore"></a>对联机还原的限制  
 仅在满足下列条件时，联机还原顺序才有效：  
  
-   在启动还原顺序之前必须执行所有必要的日志备份。  
  
-   在启动联机还原顺序之前必须备份大容量更改。  
  
-   如果数据库中存在大容量更改，则所有文件必须处于联机或[失效](../../relational-databases/backup-restore/remove-defunct-filegroups-sql-server.md)状态。 （这意味着它不再是数据库的一部分。）  
  
 如果这些条件不满足，则联机还原顺序失败。  
  
> [!NOTE]  
>  建议在启动联机还原之前切换为完整恢复模式。 有关详细信息，请参阅[恢复模式 (SQL Server)](../../relational-databases/backup-restore/recovery-models-sql-server.md)。  
  
 有关如何执行联机还原的信息，请参阅[联机还原 (SQL Server)](../../relational-databases/backup-restore/online-restore-sql-server.md)。  
  
##  <a name="database-recovery-advisor-sql-server-management-studio"></a><a name="DRA"></a> 数据库恢复顾问 (SQL Server Management Studio)  
数据库恢复顾问简化了制定还原计划的过程，可以很轻松地实现最优的正确还原顺序。 很多已知数据库还原问题和客户所要求的增强功能已得到解决。 数据库恢复顾问引入的主要增强功能包括：  
  
-   **还原计划算法：** 用于制定还原计划的算法已得到明显改进，特别是对于复杂的还原方案。 对于许多边缘案例（包括时点还原中存在分支的情形），处理效率要比以前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]更高。  
  
-   **时间点还原：** 数据库恢复顾问极大地简化了将数据库还原到给定时间点的过程。 可视备份时间线明显增强了对时点还原的支持。 此可视时间线允许您将合适的时点标识为还原数据库的目标恢复点。 时间线简化了遍历有分支恢复路径（跨恢复分支的路径）的过程。 给定时点还原计划自动包括与还原到目标时点（日期和时间）相关的备份。 有关详细信息，请参阅[将 SQL Server 数据库还原到某个时间点（完整恢复模式）](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)。  
  
有关详细信息，请参阅下列 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可管理性博客中有关数据库恢复顾问的信息：  
  
-   [恢复顾问：简介](/archive/blogs/managingsql/recovery-advisor-an-introduction)  
  
-   [恢复顾问：使用 SSMS 创建/还原拆分备份](/archive/blogs/managingsql/recovery-advisor-using-ssms-to-createrestore-split-backups)  

## <a name="accelerated-database-recovery"></a><a name="adr"></a> 加速数据库恢复
[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中提供了[加速数据库恢复](/azure/sql-database/sql-database-accelerated-database-recovery/) 通过重新设计 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] [恢复过程](#TlogAndRecovery)，加速数据库恢复极大地提高了数据库可用性，尤其是存在长时间运行的事务时。 启用了加速数据库恢复的数据库在故障转移或其他非干净关闭后完成恢复过程的速度显著加快。 启用加速数据库恢复后，回滚取消长时间运行的事务的速度也显著加快。

可使用以下语法对 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 按数据库启用加速数据库恢复：

```sql
ALTER DATABASE <db_name> SET ACCELERATED_DATABASE_RECOVERY = ON;
```

> [!NOTE]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 上将默认启用加速数据库恢复。

## <a name="see-also"></a><a name="RelatedContent"></a> 另请参阅  
 [备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)      
 [事务日志 (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)     
 [SQL Server 事务日志体系结构和管理指南](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)     
 [SQL Server 数据库的备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)     
 [应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)
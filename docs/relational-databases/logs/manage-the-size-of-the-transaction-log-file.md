---
title: 管理事务日志文件大小 | Microsoft Docs
description: 了解如何监视 SQL Server 事务日志大小、收缩日志、扩大日志、优化 tempdb 日志增长率以及控制事务日志的增长。
ms.custom: ''
ms.date: 01/05/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- transaction logs [SQL Server], size management
- manage log size
- log size, manage
ms.assetid: 3a70e606-303f-47a8-96d4-2456a18d4297
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e0d5c070d961e71711189b9b32ad12508812b277
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171539"
---
# <a name="manage-the-size-of-the-transaction-log-file"></a>管理事务日志文件的大小
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
本主题介绍如何监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事务日志大小、收缩事务日志、添加或扩大事务日志文件、优化 tempdb 事务日志增长率以及控制事务日志文件的增长。  

##  <a name="monitor-log-space-use"></a><a name="MonitorSpaceUse"></a>监视日志空间使用情况  
使用 [sys.dm_db_log_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md) 监视日志空间使用情况。 此 DMV 返回有关当前使用的日志空间量信息，并指示何时需要截断事务日志。 

若要了解有关日志文件的当前大小、最大大小以及文件的自动增长选项的信息，还可以在 [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) 中针对此日志文件使用 **size**、**max_size** 和 **growth** 等列。  
  
> [!IMPORTANT]
> 避免日志磁盘重载。 请确保日志存储可以承受 [IOPS](https://wikipedia.org/wiki/IOPS) 和事务加载的低延迟需求。 
  
##  <a name="shrink-log-file-size"></a><a name="ShrinkSize"></a> 收缩日志文件大小  
 若要减少物理日志文件的物理大小，则必须收缩日志文件。 知道事务日志文件包含未使用空间时，此方法很有用。 仅当数据库处于联机状态，而且至少一个[虚拟日志文件 (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) 可用时，才能收缩日志文件。 在某些情况下，直到下一个日志截断后，才能收缩日志。  
  
> [!NOTE]
> 能够延长 [VLF](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) 活动时间的因素（如长时间运行的事务）可以限制甚至阻止日志收缩。 有关详细信息，请参阅[可能延迟日志截断的因素](../../relational-databases/logs/the-transaction-log-sql-server.md#FactorsThatDelayTruncation)。  
  
收缩日志文件可删除一个或多个不包含逻辑日志任何部分的 [VLF](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch)（即不活动的 VLF）。 收缩事务日志文件时，将从日志文件末端删除不活动的 VLF，以将日志减小到接近目标大小。 

> [!IMPORTANT]
> 收缩事务日志前，请记住[可能延迟日志截断的因素](../../relational-databases/logs/the-transaction-log-sql-server.md#FactorsThatDelayTruncation)。 如果在日志收缩后还需要存储空间，则会再次增加事务日志，导致在增加日志操作期间产生性能开销。 有关详细信息，请参阅本主题中的[建议](#Recommendations)。
  
 **收缩日志文件（而不收缩数据库文件）**  
  
-   [DBCC SHRINKFILE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)  
  
-   [收缩文件](../../relational-databases/databases/shrink-a-file.md)  
  
 **监视日志文件收缩事件**  
  
-   [Log File Auto Shrink Event Class](../../relational-databases/event-classes/log-file-auto-shrink-event-class.md)。  
  
 **监视日志空间**  
  
-   [sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)  
  
-   [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)（请参阅日志文件或文件的 **size**、**max_size** 和 **growth** 列。）  
  
##  <a name="add-or-enlarge-a-log-file"></a><a name="AddOrEnlarge"></a> 添加或扩大日志文件  
可以通过扩大现有日志文件（如果磁盘空间允许）或将日志文件添加至数据库（通常是其他磁盘上的数据库）来获得空间。 一个事务日志文件就足够了，除非日志空间不足且保留日志文件的卷上的磁盘空间也不足。   
  
-   要将日志文件添加到数据库，请使用 `ALTER DATABASE` 语句的 `ADD LOG FILE` 子句。 添加日志文件可以使日志获得空间。  
-   要扩大日志文件，请使用 `ALTER DATABASE` 语句的 `MODIFY FILE` 子句，指定 `SIZE` 和 `MAXSIZE` 语法。 有关详细信息，请参阅 [ALTER DATABASE (Transact-SQL) 文件和文件组选项](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)。  

有关详细信息，请参阅本主题中的[建议](#Recommendations)。
    
##  <a name="optimize-tempdb-transaction-log-size"></a><a name="tempdbOptimize"></a> 优化 tempdb 事务日志的大小  
 重新启动服务器实例可以将 **tempdb** 数据库的事务日志调整到自动增长之前的原始大小。 这会降低 **tempdb** 事务日志的性能。 
 
 您可以通过在启动或重新启动服务器实例之后增加 **tempdb** 事务日志的大小来避免此开销。 有关详细信息，请参阅 [tempdb Database](../../relational-databases/databases/tempdb-database.md)。  
  
##  <a name="control-transaction-log-file-growth"></a><a name="ControlGrowth"></a> 控制事务日志文件的增长  
 使用 [ALTER DATABASE (Transact-SQL) 文件和文件组选项](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)语句管理事务日志文件的增长。 注意以下事项：  
  
-   要更改当前文件大小（以 KB、MB、GB 和 TB 为单位），请使用 `SIZE` 选项。  
-   要更改增量，请使用 `FILEGROWTH` 选项。 如果值为 0，则表明自动增长已设置为关闭，且不允许增加空间。  
-   要控制日志文件的最大大小（以 KB、MB、GB 和 TB 为单位）或将增长设置为 UNLIMITED，请使用 `MAXSIZE` 选项。  

有关详细信息，请参阅本主题中的[建议](#Recommendations)。

## <a name="recommendations"></a><a name="Recommendations"></a> 建议
下面是使用事务日志文件时的一些一般建议：

-   `FILEGROWTH` 选项设置的事务日志的自动增长 (autogrow) 增量必须足够大，以领先于工作负载事务的需求。 因此，为了避免经常向日志文件中扩充内容，应该采用足够大的文件增量。 要正确设置事务日志的大小，建议监视以下时间内所占用的日志数量：
    -  执行完整备份所需的时间，因为日志备份在其完成后才能进行。
    -  最大型索引维护操作所需的时间。
    -  在数据库中执行最大批操作所需的时间。

-   使用 `FILEGROWTH` 选项设置数据和日志文件的 autogrow 时，建议首选使用 size 而不是使用 percentage 进行设置，以便更好地控制增长比，因为 percentage 表示的是日益增长量  。
    -  请记住，事务日志不能利用[即时文件初始化](../../relational-databases/databases/database-instant-file-initialization.md)，因此延长的日志增长时间尤其重要。 
    -  最佳做法是，针对日志事务，请勿将 `FILEGROWTH` 选项值设置为超过 1,024 MB。 `FILEGROWTH` 选项的默认值为：  
  
      |版本|默认值|  
      |-------------|--------------------|  
      |自 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 起|数据 64 MB。 日志文件 64 MB。|  
      |自 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 起|数据 1 MB。 日志文件 10%。|  
      |[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 之前|数据 10%。 日志文件 10%。|  

-   小型的增长增量可能生成过多的 [VLF](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) 并且可能降低性能。 若要确定给定实例中所有数据库的当前事务日志大小的最佳 VLF 分发，以及实现所需大小需要的增长量，请参阅此[脚本](https://github.com/Microsoft/tigertoolbox/tree/master/Fixing-VLFs)。

-   大型的增长增量可能生成过少的大型 [VLF](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) 并且也可能影响性能。 若要确定给定实例中所有数据库的当前事务日志大小的最佳 VLF 分发，以及实现所需大小需要的增长量，请参阅此[脚本](https://github.com/Microsoft/tigertoolbox/tree/master/Fixing-VLFs)。 

-   即使启用自动增长，如果增长速度不能满足查询需求，也可能收到提示事务日志已满的消息。 有关更改增长增量的详细信息，请参阅 [ALTER DATABASE (Transact-SQL) 文件和文件组选项](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)

-   在数据库中有多个日志文件不会以任何方式提升性能，因为事务日志文件不会像同一文件组中的数据文件一样使用[比例填充](../../relational-databases/pages-and-extents-architecture-guide.md#ProportionalFill)。  

-   日志文件可以设为自动收缩。 但是，不建议这样做，auto_shrink 数据库属性默认设为 FALSE 。 如果 auto_shrink 设置为 TRUE，则仅当其空间的 25% 以上未使用时，自动收缩才会减少文件的大小。 
    -   文件将收缩至未使用空间占文件 25% 的大小，或者收缩至文件的原始大小，以两者中较大者为准。 
    -   有关更改 auto_shrink 属性设置的详细信息，请参阅[查看或更改数据库的属性](../../relational-databases/databases/view-or-change-the-properties-of-a-database.md)和 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)。 
  
## <a name="see-also"></a>另请参阅  
[BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
[解决事务日志已满的问题（SQL Server 错误 9002）](../../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)    
[SQL Server 事务日志体系结构和管理指南中的事务日志备份](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Backups)    
[事务日志备份 (SQL Server)](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)    
[ALTER DATABASE (Transact-SQL) 文件和文件组选项](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)

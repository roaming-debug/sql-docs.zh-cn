---
description: 跟踪数据更改 (SQL Server)
title: 跟踪数据更改
ms.custom: seo-dt-2019
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- change data capture [SQL Server], compared to change tracking
- change data capture vs. change tracking
- change data capture [SQL Server], data type behaviors
- Sync Services for ADO.NET
- change tracking [SQL Server], Sync Services for ADO.NET
- change tracking [SQL Server]
- change data capture [SQL Server], security
- change data capture [SQL Server], other SQL Server features and
ms.assetid: 7a34be46-15b4-4b6b-8497-cfd8f9f14234
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b38293d4b60168028a5669a9f5dd2e01ccbde012
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466748"
---
# <a name="track-data-changes-sql-server"></a>跟踪数据更改 (SQL Server)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 提供两个用于在数据库中跟踪数据更改的功能： [变更数据捕获](#Capture) 和 [更改跟踪](#Tracking)。 这两个功能使应用程序能够确定对数据库中的用户表所做的 DML 更改（插入、更新和删除操作）。 可对同一个数据库启用变更数据捕获和更改跟踪；没有特殊的注意事项。 有关支持变更数据捕获和更改跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。 [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]支持更改跟踪。 仅支持在 SQL Server 和 Azure SQL 托管实例中使用变更数据捕获功能。
  
## <a name="benefits-of-using-change-data-capture-or-change-tracking"></a>使用变更数据捕获或更改跟踪的好处  
 对于某些注重效能的应用程序来说，查询数据库中已更改的数据的能力是一项很重要的要求。 通常，为了确定数据更改，应用程序开发人员必须在其应用程序中使用触发器、时间戳列和其他表的组合来实现自定义跟踪方法。 创建这些应用程序通常涉及多项工作，导致架构更新，并且通常带来较高的性能开销。  
  
 在应用程序中使用变更数据捕获或更改跟踪而不开发自定义解决方案来跟踪数据库中的更改具有以下好处：  
  
-   减少了开发时间。 由于 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]中提供了功能，因此无需开发自定义解决方案。  
  
-   不需要架构更改。 您无需添加列、添加触发器或创建要在其中跟踪已删除的行或存储更改跟踪信息的端表（如果无法将列添加到用户表）。  
  
-   具有内置清除机制。 更改跟踪的清除操作在后台自动执行。 不需要端表中存储的数据的自定义清除。  
  
-   提供功能的目的是获取更改信息。  
  
-   降低了 DML 操作的开销。 同步更改跟踪始终会有一些开销。 但是，使用更改跟踪有助于使开销最小化。 开销通常会低于使用其他解决方案，对于需要使用触发器的解决方案，尤其如此。  
  
-   更改跟踪是基于提交的事务进行的。 更改的顺序基于事务提交时间。 在存在长时间运行和重叠事务的情况下，这样可获得可靠的结果。 必须专门设计使用 **timestamp** 值的自定义解决方案，以处理这些情况。  
  
-   提供可用于配置和管理的标准工具。 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 提供标准的 DDL 语句、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、目录视图和安全权限。  
  
## <a name="feature-differences-between-change-data-capture-and-change-tracking"></a>变更数据捕获与更改跟踪之间的功能差异  
 下表列出了变更数据捕获与更改跟踪之间的功能差异。 变更数据捕获中的跟踪机制涉及从事务日志中异步捕获更改，因此，可以在执行 DML 操作后获得更改信息。 更改跟踪中的跟踪机制涉及在执行 DML 操作的同时同步跟踪更改，因此，可以立即获得更改信息。  
  
|Feature|更改数据捕获|更改跟踪|  
|-------------|-------------------------|---------------------|  
|**跟踪的更改**|||  
|DML 更改|是|是|  
|**跟踪的信息**|||  
|历史数据|是|否|  
|是否更改了列|是|是|  
|DML 类型|是|是|  
  
##  <a name="change-data-capture"></a><a name="Capture"></a> 变更数据捕获  
 变更数据捕获通过获取进行 DML 更改的方面和更改的实际数据，提供用户表的历史更改信息。 更改是使用异步进程捕获的，此进程读取事务日志，并且对系统造成的影响很小。  
  
 正如下图所示，对用户表所做的更改是在相应更改表中捕获的。 这些更改表提供了更改随时间变化的历史视图。 借助于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供的[变更数据捕获](../../relational-databases/system-functions/change-data-capture-functions-transact-sql.md)功能，可以方便且系统地使用更改数据。  
  
 ![变更数据捕获的概念图示](../../relational-databases/track-changes/media/cdcart1.gif "变更数据捕获的概念图示")  
  
### <a name="security-model"></a>安全模式  
 本部分说明了变更数据捕获安全模式。  
  
 **配置和管理**  
 若要为数据库启用或禁用变更数据捕获，[sys.sp_cdc_enable_db (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-db-transact-sql.md) 或 [sys.sp_cdc_disable_db (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-db-transact-sql.md) 的调用者必须是 **sysadmin** 固定服务器角色的成员。 若要在表级启用或禁用变更数据捕获，要求 [sys.sp_cdc_enable_table (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md) 和 [sys.sp_cdc_disable_table (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-table-transact-sql.md) 的调用者必须是 sysadmin 角色成员或数据库 **db_owner** 角色的成员。  
  
 仅限服务器 **sysadmin** 角色和 **database db_owner** 角色成员能够使用存储过程来支持变更数据捕获作业管理。  
  
 **更改枚举和元数据查询**  
 若要获取对与捕获实例关联的更改数据的访问，必须为用户授予关联源表中的所有捕获列的选择访问权限。 此外，如果在创建捕获实例时指定了访问控制角色，调用者还必须是指定访问控制角色的成员。 所有数据库用户可通过 public 角色访问用于访问元数据的其他常规变更数据捕获功能，但返回的元数据访问通常也是使用基础源表的选择访问权限以及任何定义的访问控制角色成员控制的。  
  
 **对启用了变更数据捕获的源表执行的 DDL 操作**  
 为表启用变更数据捕获后，只能由固定服务器角色 **sysadmin**、 **database role db_owner** 成员或 **database role db_ddladmin** 成员将 DDL 操作应用于该表。 如果为用户显式授予了对表执行 DDL 操作的权限，这些用户在尝试执行这些操作时将收到错误 22914。  
  
### <a name="data-type-considerations-for-change-data-capture"></a>变更数据捕获的数据类型注意事项  
 变更数据捕获支持所有基列类型。 下表列出了几个列类型的行为和限制。  
  
|列类型|在更改表中捕获更改|限制|  
|--------------------|---------------------------------------|-----------------|  
|稀疏列|是|不支持在使用列集时捕获更改。|  
|计算列|否|不跟踪对计算列的更改。 在更改表中该列将显示为相应类型，不过其值为 NULL。|  
|XML|是|不跟踪对单个 XML 元素的更改。|  
|时间戳|是|更改表中的数据类型将转换为 binary。|  
|BLOB 数据类型|是|仅当 BLOB 列本身更改时才存储该列的上一映像。|  
  
### <a name="change-data-capture-and-other-sql-server-features"></a>变更数据捕获和其他 SQL Server 功能  
 本节说明下列功能如何与变更数据捕获交互：  
  
-   数据库镜像  
  
-   事务复制  
  
-   数据库还原或附加  
  
#### <a name="database-mirroring"></a>数据库镜像  
 可以对启用变更数据捕获的数据库进行镜像。 为确保捕获和清除操作可在镜像服务器上自动发生，请执行以下步骤：  
  
1.  确保 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理正在镜像上运行。  
  
2.  在主体服务器因故障而转由镜像服务器接替其工作后，在镜像服务器上创建捕获作业和清除作业。 若要创建作业，请使用 [sys.sp_cdc_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md) 存储过程。  

 有关数据库镜像的详细信息，请参阅[数据库镜像 (SQL Server) ](../../database-engine/database-mirroring/database-mirroring-sql-server.md)。  
  
#### <a name="transactional-replication"></a>事务复制  
 变更数据捕获和事务复制可以共存于同一数据库中，但在启用这两项功能后，更改表的填充处理方式将发生变化。 变更数据捕获和事务复制始终使用相同的过程 [sp_replcmds](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)从事务日志读取更改。 当单独启用变更数据捕获时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业会调用 **sp_replcmds**。 在同一数据库中启用这两项功能时，日志读取器代理会调用 **sp_replcmds**。 此代理将填充更改表和分发数据库表。 有关详细信息，请参阅 [Replication Log Reader Agent](../../relational-databases/replication/agents/replication-log-reader-agent.md)。  
  
 假设为 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库启用了变更数据捕获，并为捕获启用了两个表。 为了填充更改表，捕获作业将调用 **sp_replcmds**。 此外，还为该数据库启用了事务复制，并会创建发布。 此时，将为该数据库创建日志读取器代理，并删除捕获作业。 日志读取器代理继续从提交到更改表的最后一个日志序列号开始扫描日志。 这样将确保更改表中的数据一致性。 如果在此数据库中禁用事务复制，则会删除日志读取器代理，并重新创建捕获作业。  
  
> [!NOTE]  
>  当日志读取器代理同时用于变更数据捕获和事务复制时，复制的更改将首先写入分发数据库。 然后，捕获的更改会写入更改表。 两项操作会一起提交。 如果在写入分发数据库时有任何滞后时间，则在更改显示在更改表中之前，将有对应的滞后时间。  
  
#### <a name="restoring-or-attaching-a-database-enabled-for-change-data-capture"></a>还原或附加启用了变更数据捕获的数据库  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用以下逻辑确定还原或附加数据库后变更数据捕获是否继续保持启用状态：  
  
-   如果数据库以同一数据库名称还原到同一服务器，变更数据捕获将保持启用状态。  
  
-   如果数据库还原到其他服务器，默认情况下将禁用变更数据捕获，并删除所有相关的元数据。  
  
     若要保留变更数据捕获，请在还原数据库时使用 **KEEP_CDC** 选项。 有关此选项的详细信息，请参阅 [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)。  
  
-   如果数据库在分离后附加到同一服务器或其他服务器，变更数据捕获将保持启用状态。  
  
-   如果数据库是使用 KEEP_CDC  选项附加或还原到除标准版或企业版以外的任何版本，操作会遭阻止，因为变更数据捕获需要 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标准版或企业版。 将显示错误消息 932：  
  
     `SQL Server cannot load database '%.*ls' because change data capture is enabled. The currently installed edition of SQL Server does not support change data capture. Either disable change data capture in the database by using a supported edition of SQL Server, or upgrade the instance to one that supports change data capture.`  
  
 可以使用 [sys.sp_cdc_disable_db](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-db-transact-sql.md) 从还原或附加的数据库中删除变更数据捕获。  
  
##  <a name="change-tracking"></a><a name="Tracking"></a> 更改跟踪  
 更改跟踪捕获更改了表行这一事实，但不会捕获更改的数据。 这样，应用程序就可以确定使用从用户表中直接获取的最新行数据更改的行。 因此，与变更数据捕获相比，更改跟踪可以解答的历史问题比较有限。 但是，对于不需要历史信息的那些应用程序，更改跟踪产生的存储开销要小得多，因为它不需要捕获更改的数据。 它使用同步跟踪机制来跟踪更改。 此功能旨在最大限度地减少 DML 操作开销。  
  
 下图显示了从使用更改跟踪中受益的同步方案。 在此方案中，应用程序需要以下信息：在上次表同步后更改的所有表行以及仅当前行数据。 由于使用同步机制来跟踪更改，因此，应用程序可以执行双向同步，并且可靠地检测到可能发生的任何冲突。  
  
 ![更改跟踪的概念图示](../../relational-databases/track-changes/media/cdcart2.gif "变更跟踪的概念图示")  
  
### <a name="change-tracking-and-sync-services-for-adonet"></a>ADO.NET 的更改跟踪和同步服务  
 [!INCLUDE[sql_sync_long](../../includes/sql-sync-long-md.md)] 支持数据库之间的同步，并且提供了一个直观且灵活的 API，使您能够生成面向脱机和协作应用场景的应用程序。 [!INCLUDE[sql_sync_long](../../includes/sql-sync-long-md.md)] 提供了一个 API 用于同步更改，但并不实际跟踪服务器或对等数据库中的更改。 您可以创建自定义的更改跟踪系统，但这通常会大大增加复杂性和性能开销。 若要跟踪服务器或对等数据库中的更改，建议您使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中的更改跟踪功能，因为它易于配置并提供高性能的跟踪操作。  
  
 有关更改跟踪和 [!INCLUDE[sql_sync_long](../../includes/sql-sync-long-md.md)]的详细信息，请使用以下链接：  
  
-   [关于更改跟踪 (SQL Server)](../../relational-databases/track-changes/about-change-tracking-sql-server.md)  
  
     介绍更改跟踪，提供更改跟踪工作方式的概要说明，并描述更改跟踪如何与其他 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 功能进行交互。  
  
-   [Microsoft Sync Framework 开发人员中心](/previous-versions/sql/synchronization/mt490616(v=msdn.10))  
  
     提供 [!INCLUDE[ssSyncFrameLong](../../includes/sssyncframelong-md.md)] 和 [!INCLUDE[sql_sync_short](../../includes/sql-sync-short-md.md)]的完整文档。 在 [!INCLUDE[sql_sync_short](../../includes/sql-sync-short-md.md)]的文档中，“如何使用 SQL Server 更改跟踪”主题包含了详细信息和代码示例。  
  
## <a name="related-tasks-required"></a>相关任务（必需）  
  
|任务|主题|  
|-|-|  
|提供变更数据捕获的概述。|[关于变更数据捕获 (SQL Server)](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)|  
|介绍如何启用和禁用针对数据库或表的变更数据捕获。|[启用和禁用变更数据捕获 (SQL Server)](../../relational-databases/track-changes/enable-and-disable-change-data-capture-sql-server.md)|  
|介绍如何管理和监视变更数据捕获。|[管理和监视变更数据捕获 (SQL Server)](../../relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server.md)|  
|介绍如何使用可用于变更数据捕获使用者的变更数据。 此主题介绍如何验证 LSN 边界、查询函数和查询函数应用场景。|[处理变更数据 (SQL Server)](../../relational-databases/track-changes/work-with-change-data-sql-server.md)|  
|提供更改跟踪的概述。|[关于更改跟踪 (SQL Server)](../../relational-databases/track-changes/about-change-tracking-sql-server.md)|  
|介绍如何启用和禁用针对数据库或表的更改跟踪。|[启用和禁用更改跟踪 (SQL Server)](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)|  
|介绍如何管理更改跟踪、配置安全性和确定使用更改跟踪时对存储和性能的影响。|[管理更改跟踪 (SQL Server)](../../relational-databases/track-changes/manage-change-tracking-sql-server.md)|  
|介绍使用更改跟踪的应用程序如何获取跟踪的更改、将这些更改应用到其他数据存储区和更新源数据库。 此主题还介绍了在发生故障转移且必须从备份还原数据库时，角色更改跟踪如何进行。|[处理更改跟踪 (SQL Server)](../../relational-databases/track-changes/work-with-change-tracking-sql-server.md)|  
  
## <a name="see-also"></a>另请参阅  
 [变更数据捕获函数 (Transact-SQL)](../../relational-databases/system-functions/change-data-capture-functions-transact-sql.md)   
 [变更跟踪函数 (Transact-SQL)](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [更改数据捕获存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/change-data-capture-stored-procedures-transact-sql.md)   
 [变更数据捕获表 (Transact-SQL)](../../relational-databases/system-tables/change-data-capture-tables-transact-sql.md)   
 [与变更数据捕获相关的动态管理视图 (Transact-SQL)](../system-dynamic-management-views/system-dynamic-management-views.md)  
  

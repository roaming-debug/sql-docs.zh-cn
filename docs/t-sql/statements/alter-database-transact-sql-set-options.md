---
title: ALTER DATABASE SET 选项 (Transact-SQL) | Microsoft Docs
description: 了解如何在 SQL Server 和 Azure SQL 数据库中设置自动优化、加密和查询存储等数据库选项。
ms.custom: ''
ms.date: 09/04/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- online database state [SQL Server]
- database options [SQL Server]
- emergency database state [SQL Server]
- databases [SQL Server], options
- read-only databases
- recovery models [SQL Server], switching
- ALTER DATABASE statement, SET options
- offline database state [SQL Server]
- snapshot isolation framework option
- checksums [SQL Server]
- Automatic tuning
- " Data Retention Policy"
- query plan regression correction
- auto_create_statistics
- auto_update_statistics
- Query Store options
ms.assetid: f76fbd84-df59-4404-806b-8ecb4497c9cc
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||=azuresqldb-mi-current
ms.openlocfilehash: 652a4b13db3fdd98b774a5c884e68848a3b0b847
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099563"
---
# <a name="alter-database-set-options-transact-sql"></a>ALTER DATABASE SET 选项 (Transact-SQL)

设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 和 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 中的数据库选项。 有关其他 ALTER DATABASE 选项，请参阅 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)。

选择以下选项卡之一，了解所使用的特定 SQL 版本的语法、参数、注解、权限和示例。

有关语法约定的详细信息，请参阅 [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。

> [!NOTE]
> 若要使用 ALTER DATABASE 设置一些选项，可能需要独占数据库访问权限。 如果 ALTER DATABASE 语句没有及时完成，请检查数据库中的其他会话是否正在阻止 ALTER DATABASE 会话。

## <a name="select-a-product"></a>选择一个产品

在下一行中，选择你感兴趣的产品名称。 这样做会在此网页上的此位置显示适合你所选择的任何产品的不同内容。

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL 数据库](alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 托管实例](alter-database-transact-sql-set-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

数据库镜像、[!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 和兼容性级别属于 `SET` 选项，考虑到这些选项的长度，将在单独的文章中介绍它们。 有关详细信息，请参阅 [ALTER DATABASE 数据库镜像](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)、[ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md) 和 [ALTER DATABASE 兼容性级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。

数据库范围配置用于在单个数据库级别设置多个数据库配置。 有关详细信息，请参阅 [ALTER DATABASE SCOPED CONFIGURATION](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)。

> [!NOTE]
> 可以使用 [SET 语句](../../t-sql/statements/set-statements-transact-sql.md) 来为当前会话配置很多数据库 SET 选项，当它们连接时通常通过应用程序来配置。 会话级 SET 选项覆盖 ALTER DATABASE SET 值。 下面各节中所述的数据库选项是你可以为未明确提供其他 SET 选项值的会话设置的值。

## <a name="syntax"></a>语法

```syntaxsql
ALTER DATABASE { database_name | CURRENT }
SET
{
    <option_spec> [ ,...n ] [ WITH <termination> ]
}

<option_spec> ::=
{
    <accelerated_database_recovery>
  | <auto_option>
  | <automatic_tuning_option>
  | <change_tracking_option>
  | <containment_option>
  | <cursor_option>
  | <database_mirroring_option>
  | <date_correlation_optimization_option>
  | <db_encryption_option>
  | <db_state_option>
  | <db_update_option>
  | <db_user_access_option>
  | <delayed_durability_option>
  | <external_access_option>
  | FILESTREAM ( <FILESTREAM_option> )
  | <HADR_options>
  | <mixed_page_allocation_option>
  | <parameterization_option>
  | <query_store_options>
  | <recovery_option>
  | <remote_data_archive_option>
  | <service_broker_option>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
  | <data_retention_policy>
}
;

<accelerated_database_recovery> ::=
{
    ACCELERATED_DATABASE_RECOVERY = { ON | OFF }
     [ ( PERSISTENT_VERSION_STORE_FILEGROUP = { filegroup name } ) ];
}

<auto_option> ::=
{
    AUTO_CLOSE { ON | OFF }
  | AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] }
  | AUTO_SHRINK { ON | OFF }
  | AUTO_UPDATE_STATISTICS { ON | OFF }
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<automatic_tuning_option> ::=
{
    AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = { ON | OFF } )
}

<change_tracking_option> ::=
{
    CHANGE_TRACKING
   {
       = OFF
     | = ON [ ( <change_tracking_option_list > [,...n] ) ]
     | ( <change_tracking_option_list> [,...n] )
   }
}

<change_tracking_option_list> ::=
{
   AUTO_CLEANUP = { ON | OFF }
 | CHANGE_RETENTION = retention_period { DAYS | HOURS | MINUTES }
}

<containment_option> ::=
   CONTAINMENT = { NONE | PARTIAL }

<cursor_option> ::=
{
    CURSOR_CLOSE_ON_COMMIT { ON | OFF }
  | CURSOR_DEFAULT { LOCAL | GLOBAL }
}

<database_mirroring_option>
  ALTER DATABASE Database Mirroring

<date_correlation_optimization_option> ::=
    DATE_CORRELATION_OPTIMIZATION { ON | OFF }
  
<db_encryption_option> ::=
    ENCRYPTION { ON | OFF | SUSPEND | RESUME }

<db_state_option> ::=
    { ONLINE | OFFLINE | EMERGENCY }

<db_update_option> ::=
    { READ_ONLY | READ_WRITE }

<db_user_access_option> ::=
    { SINGLE_USER | RESTRICTED_USER | MULTI_USER }

<delayed_durability_option> ::=
    DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }

<external_access_option> ::=
{
    DB_CHAINING { ON | OFF }
  | TRUSTWORTHY { ON | OFF }
  | DEFAULT_FULLTEXT_LANGUAGE = { <lcid> | <language name> | <language alias> }
  | DEFAULT_LANGUAGE = { <lcid> | <language name> | <language alias> }
  | NESTED_TRIGGERS = { OFF | ON }
  | TRANSFORM_NOISE_WORDS = { OFF | ON }
  | TWO_DIGIT_YEAR_CUTOFF = { 1753, ..., 2049, ..., 9999 }
}

<FILESTREAM_option> ::=
{
    NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL
  | DIRECTORY_NAME = <directory_name>
}

<HADR_options> ::=
    ALTER DATABASE SET HADR

<mixed_page_allocation_option> ::=
    MIXED_PAGE_ALLOCATION { OFF | ON }

<parameterization_option> ::=
    PARAMETERIZATION { SIMPLE | FORCED }

<query_store_options> ::=
{
    QUERY_STORE
    {
          = OFF [ ( FORCED ) ] 
        | = ON [ ( <query_store_option_list> [,...n] ) ]
        | ( < query_store_option_list> [,...n] )
        | CLEAR [ ALL ]
    }
}

<query_store_option_list> ::=
{
      OPERATION_MODE = { READ_WRITE | READ_ONLY }
    | CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = number )
    | DATA_FLUSH_INTERVAL_SECONDS = number
    | MAX_STORAGE_SIZE_MB = number
    | INTERVAL_LENGTH_MINUTES = number
    | SIZE_BASED_CLEANUP_MODE = { AUTO | OFF }
    | QUERY_CAPTURE_MODE = { ALL | AUTO | CUSTOM | NONE }
    | MAX_PLANS_PER_QUERY = number
    | WAIT_STATS_CAPTURE_MODE = { ON | OFF }
    | QUERY_CAPTURE_POLICY = ( <query_capture_policy_option_list> [,...n] )
}

<query_capture_policy_option_list> :: =
{
      STALE_CAPTURE_POLICY_THRESHOLD = number { DAYS | HOURS }
    | EXECUTION_COUNT = number
    | TOTAL_COMPILE_CPU_TIME_MS = number
    | TOTAL_EXECUTION_CPU_TIME_MS = number
}

<recovery_option> ::=
{
    RECOVERY { FULL | BULK_LOGGED | SIMPLE }
  | TORN_PAGE_DETECTION { ON | OFF }
  | PAGE_VERIFY { CHECKSUM | TORN_PAGE_DETECTION | NONE }
}

<remote_data_archive_option> ::=
{
    REMOTE_DATA_ARCHIVE =
    {
        ON ( SERVER = <server_name> ,
             { 
                  CREDENTIAL = <db_scoped_credential_name>
                  | FEDERATED_SERVICE_ACCOUNT = ON | OFF
             }
        )
        | OFF
    }
}

<service_broker_option> ::=
{
    ENABLE_BROKER
  | DISABLE_BROKER
  | NEW_BROKER
  | ERROR_BROKER_CONVERSATIONS
  | HONOR_BROKER_PRIORITY { ON | OFF}
}

<snapshot_option> ::=
{
    ALLOW_SNAPSHOT_ISOLATION { ON | OFF }
  | READ_COMMITTED_SNAPSHOT { ON | OFF }
  | MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = { ON | OFF }
}
<sql_option> ::=
{
    ANSI_NULL_DEFAULT { ON | OFF }
  | ANSI_NULLS { ON | OFF }
  | ANSI_PADDING { ON | OFF }
  | ANSI_WARNINGS { ON | OFF }
  | ARITHABORT { ON | OFF }
  | COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }
  | CONCAT_NULL_YIELDS_NULL { ON | OFF }
  | NUMERIC_ROUNDABORT { ON | OFF }
  | QUOTED_IDENTIFIER { ON | OFF }
  | RECURSIVE_TRIGGERS { ON | OFF }
}

<target_recovery_time_option> ::=
    TARGET_RECOVERY_TIME = target_recovery_time { SECONDS | MINUTES }

<termination>::=
{
    ROLLBACK AFTER number [ SECONDS ]
  | ROLLBACK IMMEDIATE
  | NO_WAIT
}

<temporal_history_retention> ::=
    TEMPORAL_HISTORY_RETENTION { ON | OFF }

<data_retention_policy> ::=
    DATA_RETENTION { ON | OFF }

```

## <a name="arguments"></a>参数

database_name     
要修改的数据库的名称。

CURRENT     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）

运行当前数据库中的操作。 并不是所有上下文中的所有选项都支持 `CURRENT`。 如果 `CURRENT` 失败，则提供数据库名称。

\<accelerated_database_recovery> ::=     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）

按数据库启用[加速数据库恢复](../../relational-databases/accelerated-database-recovery-management.md) (ADR)。 默认情况下，[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 中的 ADR 设置为 OFF。 通过使用此语法，可选择为永久版本存储 (PVS) 数据指定特定的文件组。 如果未指定文件组，则 PVS 将存储在 PRIMARY 文件组中。 有关示例和详细信息，请参阅[加速数据库恢复](../../relational-databases/accelerated-database-recovery-management.md)。

\<auto_option> ::=     
控制自动选项。

<a name="auto_close"></a> AUTO_CLOSE { ON | OFF }     
ON     
在最后一个用户退出后，数据库完全关闭，其资源得到释放。

当用户尝试再次使用该数据库时，该数据库将自动重新打开。 例如，用户发出 `USE database_name` 语句时，就会发生这种重新打开行为。 在 AUTO_CLOSE 设置为 ON 时，数据库可以完全关闭。 如果是这样，则在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 下次重新启动时，数据库在用户尝试使用数据库之前不会重新打开。

OFF     
在最后一个用户退出后，数据库仍然保持打开状态。

AUTO_CLOSE 选项允许将数据库文件作为常规文件进行管理，因此，该选项对于桌面数据库很有用。 它们可以移动、复制以制作备份，甚至可以通过电子邮件发送给其他用户。 AUTO_CLOSE 进程为异步进程；反复打开和关闭数据库不会降低性能。

> [!NOTE]
> AUTO_CLOSE 选项在包含的数据库或 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 中不可用。
> 可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_auto_close_on` 列或 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAutoClose` 属性来确定此选项的状态。
>
> 当 AUTO_CLOSE 设置为 ON 时，由于该数据库不可用于检索数据，因此 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的某些列和 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数将返回 NULL。 若要解决此问题，请运行 USE 语句打开数据库。
>
> 数据库镜像要求将 AUTO_CLOSE 设置为 OFF。

数据库设置为 `AUTOCLOSE = ON` 时，启动数据库自动关闭的操作将清除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计划缓存。 清除计划缓存将导致对所有后续执行计划进行重新编译，并可能导致查询性能暂时性地突然降低。 从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 开始，对于计划缓存中每个已清除的缓存存储区，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志包含以下信息性消息：`SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to some database maintenance or reconfigure operations`。 每隔五分钟，只要缓存在这段时间间隔内得到刷新，此消息就记录一次。

<a name="auto_create_statistics"></a> AUTO_CREATE_STATISTICS { ON | OFF }     
ON     
查询优化器根据需要在查询谓词中的单列上创建统计信息，以便改进查询计划和查询性能。 在查询优化器编译查询时创建这些单列统计信息。 这些单列统计信息只在尚不是现有统计信息对象的第一列的列上创建。

默认设置为 ON。 建议您对于大多数数据库使用默认设置。

OFF     
查询优化器在编译查询时不在查询谓词中的单列上创建统计信息。 将此选项设置为 OFF 可能导致并非最佳的查询计划以及查询性能下降。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_auto_create_stats_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAutoCreateStatistics` 属性来确定状态。

有关详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)中的“使用数据库范围的统计信息选项”部分。

INCREMENTAL = ON | OFF     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

将 AUTO_CREATE_STATISTICS 设置为 ON，并将 INCREMENTAL 设置为 ON。 只要支持增量统计信息，就会将自动创建的统计信息设置为增量统计信息。 默认值为 OFF。 有关详细信息，请参阅 [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md)。

<a name="auto_shrink"></a> AUTO_SHRINK { ON | OFF }     
ON     
数据库文件是定期收缩的候选项。

数据文件和日志文件都可以自动收缩。 只有在将数据库设置为 SIMPLE 恢复模式时，或备份事务日志时，AUTO_SHRINK 才可减小事务日志的大小。 将 AUTO_SHRINK 设置为 OFF 时，在定期检查未使用空间的过程中，数据库文件不自动收缩。

当文件中超过百分之二十五的部分包含未使用的空间时，AUTO_SHRINK 选项将导致收缩文件。 它将文件缩小为两个大小之一（以较大者为准）：

- 其中 25% 的文件不包含任何内容时的大小
- 文件创建时的大小

不能收缩只读数据库。

OFF     
在定期检查未使用空间时不会自动收缩数据库文件。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_auto_shrink_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAutoShrink` 属性来确定状态。

> [!NOTE]
> AUTO_SHRINK 选项在包含数据库中不可用。

<a name="auto_update_statistics"></a> AUTO_UPDATE_STATISTICS { ON | OFF }     
ON     
指定在统计信息由查询使用并且可能过期时，查询优化器更新统计信息。 统计信息将在插入、更新、删除或合并操作更改表或索引视图中的数据分布后过期。 查询优化器通过计算自最后统计信息更新后数据修改的次数并且将这一修改次数与某一阈值进行比较，确定统计信息何时可能过期。 该阈值基于表中或索引视图中的行数。

查询优化器在编译查询和运行缓存查询计划前，检查是否存在过期的统计信息。 查询优化器使用查询谓词中的列、表和索引视图确定哪些统计信息可能过期。 查询优化器在编译查询之前确定此信息。 在运行缓存查询计划前，[!INCLUDE[ssDE](../../includes/ssde-md.md)] 确认该查询计划引用最新的统计信息。

AUTO_UPDATE_STATISTICS 选项适用于为索引创建的统计信息、查询谓词中的单列以及使用 CREATE STATISTICS 语句创建的统计信息。 此选项也适用于筛选的统计信息。

默认值为 ON。 建议您对于大多数数据库使用默认设置。

使用 AUTO_UPDATE_STATISTICS_ASYNC 选项可以指定统计信息是同步更新还是异步更新。

OFF     
指定在查询使用统计信息时，查询优化器不更新统计信息。 查询优化器在统计信息可能过期时，也不会更新统计信息。 将此选项设置为 OFF 可能导致并非最佳的查询计划以及查询性能下降。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_auto_update_stats_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAutoUpdateStatistics` 属性来确定状态。

有关详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)中的“使用数据库范围的统计信息选项”部分。

<a name="auto_update_statistics_async"></a> AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }     
ON     
指定针对 AUTO_UPDATE_STATISTICS 选项的统计信息更新是异步的。 查询优化器不等待统计信息更新完成即编译查询。

除非已将 AUTO_UPDATE_STATISTICS 设置为 ON，否则将此选项设置为 ON 不会产生任何影响。

默认情况下，AUTO_UPDATE_STATISTICS_ASYNC 选项为 OFF，并且查询优化器以同步方式更新统计信息。

OFF     
指定针对 AUTO_UPDATE_STATISTICS 选项的统计信息更新是同步的。 查询优化器在编译查询前等待统计信息更新完成。

> [!NOTE]
> 除非已将 AUTO_UPDATE_STATISTICS 设置为 ON，否则将此选项设置为 OFF 不会产生任何影响。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_auto_update_stats_async_on` 列来确定此选项的状态。

有关描述何时使用同步统计信息更新或异步统计信息更新的详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md#statistics-options)中的“统计信息选项”部分。

<a name="auto_tuning"></a> \<automatic_tuning_option> ::=     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 开始）

启用或禁用 `FORCE_LAST_GOOD_PLAN` [自动优化](../../relational-databases/automatic-tuning/automatic-tuning.md)选项。

FORCE_LAST_GOOD_PLAN = { ON | OFF }     
ON     
[!INCLUDE[ssde_md](../../includes/ssde_md.md)] 在新查询计划导致性能回归的 [!INCLUDE[tsql-md](../../includes/tsql-md.md)] 查询中自动强制执行上一个已知完好的计划。 [!INCLUDE[ssde_md](../../includes/ssde_md.md)]通过该强制计划持续监视 [!INCLUDE[tsql-md](../../includes/tsql-md.md)] 查询的查询性能。

如果性能有所提升，[!INCLUDE[ssde_md](../../includes/ssde_md.md)]将继续使用上一个已知完好的计划。 如果未检测到性能提升，[!INCLUDE[ssde_md](../../includes/ssde_md.md)]将生成新的查询计划。 如果查询存储未启用或者查询存储不处于读写模式，该语句将失败。

OFF     
[!INCLUDE[ssde_md](../../includes/ssde_md.md)]报告由 [sys.dm_db_tuning_recommendations](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md) 视图中的查询计划更改引起的潜在查询性能回归。 但是，不会自动应用这些建议。 用户可以通过应用视图中显示的 [!INCLUDE[tsql-md](../../includes/tsql-md.md)] 脚本来监视正在应用的建议和修复已识别的问题。 默认值为 OFF。

\<change_tracking_option> ::=     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssSDSFull](../../includes/sssds-md.md)]

控制更改跟踪选项。 可以启用更改跟踪、设置选项、更改选项以及禁用更改跟踪。 有关示例，请参阅本文后面的“示例”一节。

ON     
对数据库启用更改跟踪。 启用更改跟踪时，还可以设置 AUTO CLEANUP 和 CHANGE RETENTION 选项。

AUTO_CLEANUP = { ON | OFF }     
ON     
在经过指定的保持期后会自动删除更改跟踪信息。

OFF     
不会从数据库中自动删除更改跟踪数据。

CHANGE_RETENTION =retention_period { DAYS | HOURS | MINUTES }     
指定在数据库中保留更改跟踪信息的最短期限。 只有在 AUTO_CLEANUP 值为 ON 时，才会删除数据。

*retention_period* 是一个整数，用于指定保留期的数值部分。

默认保持期为 2 天。 最短保持期为 1 分钟。 默认保留类型为 DAYS。

OFF     
对数据库禁用更改跟踪。 先对所有表禁用更改跟踪，然后才能对数据库禁用更改跟踪。

\<containment_option> ::=     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）

控制数据库包含选项。

CONTAINMENT = { NONE | PARTIAL}     
无     
该数据库不是包含数据库。

PARTIAL     
该数据库是包含数据库。 如果数据库已启用复制、更改数据捕获或更改跟踪功能，则将数据库包含关系设置为 partial 将失败。 错误检查将在一次失败后停止。 有关包含的数据库的详细信息，请参阅 [Contained Databases](../../relational-databases/databases/contained-databases.md)。

\<cursor_option> ::=     

控制游标选项。

CURSOR_CLOSE_ON_COMMIT { ON | OFF }     
ON     
在提交或回滚事务时打开的所有游标都会关闭。

OFF     
在提交事务时游标保持打开状态；回滚事务则会关闭除了定义为 INSENSITIVE 或 STATIC 的游标以外的所有游标。

连接级设置（使用 SET 语句设置）覆盖 CURSOR_CLOSE_ON_COMMIT 的默认数据库设置。 默认情况下，ODBC 和 OLE DB 客户端会发出连接级别的 SET 语句，将会话的 CURSOR_CLOSE_ON_COMMIT 设置为 OFF。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md)。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_cursor_close_on_commit_on` 列或 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsCloseCursorsOnCommitEnabled` 属性来确定此选项的状态。

CURSOR_DEFAULT { LOCAL | GLOBAL }     
适用于：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

控制游标作用域是使用 LOCAL 还是 GLOBAL。

LOCAL     
如果指定 LOCAL 并且在创建游标时未将游标定义为 GLOBAL，则游标的作用域是局部的。 具体而言，作用域对在其中创建游标的批处理、存储过程或触发器是局部的。 游标名仅在该作用域内有效。

在批处理、存储过程、触发器或存储过程 OUTPUT 参数中，该游标可由局部游标变量引用。 当批处理、存储过程或触发器结束时，游标将被隐式释放。 游标会被释放，除非它在一个 OUTPUT 参数中传递回来。 游标可以在 OUTPUT 参数中传递回来。 如果采用此方式将游标传递回来，则游标将在引用它的最后一个变量释放或离开作用域时释放。

GLOBAL     
如果指定了 GLOBAL，而创建游标时没有将其定义为 LOCAL，那么游标的作用域将是相应连接的全局范围。 在由此连接执行的任何存储过程或批处理中，都可以引用该游标名称。

该游标仅在断开连接时才被隐式释放。 有关详细信息，请参阅 [DECLARE CURSOR](../../t-sql/language-elements/declare-cursor-transact-sql.md)。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_local_cursor_default` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsLocalCursorsDefault` 属性来确定状态。

**\<data_retention_policy> ::=**

**适用对象**：仅限 Azure SQL Edge

DATA_RETENTION { ON | OFF }   
ON    
对数据库启用基于数据保留策略的清理。

OFF   
对数据库禁用基于数据保留策略的清理。

**\<database_mirroring>**     
适用于：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

有关参数说明，请参阅 [ALTER DATABASE 数据库镜像](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)。

\<date_correlation_optimization_option> ::=     
适用于：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

控制 date_correlation_optimization 选项。

DATE_CORRELATION_OPTIMIZATION { ON | OFF }     
ON     
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 维护关联统计信息，其中 FOREIGN KEY 约束链接数据库中的任意两个表，并且这些表具有 datetime 列。

OFF     
不维护相关性统计信息。

若要将 DATE_CORRELATION_OPTIMIZATION 设置为 ON，则除执行 ALTER DATABASE 语句的连接以外，该数据库还必须没有其他活动连接。 以后会支持多个连接。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_date_correlation_on` 列确定此选项的当前设置。

\<db_encryption_option> ::=     
控制数据库加密状态。

ENCRYPTION { ON \| OFF \| SUSPEND \| RESUME }     
ON     
设置要加密的数据库。

OFF     
将数据库设置为不加密。

SUSPEND     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）     

可用于在启用或禁用透明数据加密后或更改了加密密钥后暂停和恢复加密扫描。

RESUME     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）

可用于恢复先前暂停的加密扫描。

有关数据加密的详细信息，请参阅[透明数据加密](../../relational-databases/security/encryption/transparent-data-encryption.md)和[借助 Azure SQL 数据库实现透明数据加密](/azure/azure-sql/database/transparent-data-encryption-tde-overview)。

在数据库级别启用加密时，所有文件组都将进行加密。 任何新的文件组都将继承加密的属性。 如果数据库中的任何文件组设置为 READ ONLY，则数据库加密操作将失败。

可以通过使用 [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md) 动态管理视图来查看数据库的加密状态和加密扫描的状态。

\<db_state_option> ::=     
适用于：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

控制数据库的状态。

OFFLINE     
数据库已关闭、完全关闭并标记为脱机。 数据库脱机时，不能进行修改。

ONLINE     
该数据库已打开且可用。

EMERGENCY     
数据库标记为 READ_ONLY，禁用日志记录，并且仅限 sysadmin 固定服务器角色的成员访问。 EMERGENCY 主要用于故障排除。 例如，可以将由于损坏了日志文件而标记为可疑的数据库设置为 EMERGENCY 状态。 此设置可以使系统管理员能够对数据库进行只读访问。 只有 sysadmin 固定服务器角色的成员才可以将数据库设置为 EMERGENCY 状态。

需要主题数据库的 `ALTER DATABASE` 权限将数据库更改为脱机或紧急状态，还需要服务器级别的 `ALTER ANY DATABASE` 权限将数据库从脱机移动到联机状态。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `state` 和 `state_desc` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `Status` 属性来确定状态。 有关详细信息，请参阅 [Database States](../../relational-databases/databases/database-states.md)。

无法将标记为 RESTORING 的数据库设置为 OFFLINE、ONLINE 或 EMERGENCY。 在活动还原操作期间，或者当数据库还原操作或日志文件还原操作由于备份文件损坏而失败时，数据库可以处于 RESTORING 状态。

\<db_update_option> ::=

控制是否允许更新数据库。

READ_ONLY     
用户可以从数据库读取数据，但不能修改数据库。

> [!NOTE]
> 若要改进查询优化器，请在将数据库设置为 READ_ONLY 之前更新统计信息。 如果在将数据库设置为 READ_ONLY 之后需要其他统计信息，则[!INCLUDE[ssDE](../../includes/ssde-md.md)]将在 tempdb 中创建统计信息。 有关只读数据库的统计信息的详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)。

READ_WRITE     
允许对数据库执行读写操作。

若要更改此状态，您必须对数据库有独占访问权限。 有关详细信息，请参阅 SINGLE_USER 子句。

> [!NOTE]
> 在 [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] 联合数据库中，`SET { READ_ONLY | READ_WRITE }` 处于禁用状态。

\<db_user_access_option> ::=     
控制用户对数据库的访问。

SINGLE_USER     
适用于：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

指定一次只能有一个用户可以访问数据库。 如果指定了 SINGLE_USER，但已有其他用户连接到数据库，则 ALTER DATABASE 语句会被阻止，直到所有用户都与指定的数据库断开连接为止。 若要替代此行为，请参阅 WITH \<termination> 子句。

即使设置此选项的用户已注销，数据库仍保持 SINGLE_USER 模式。这时，其他用户（但只能是一个）可以连接到数据库。

在将数据库设置为 SINGLE_USER 之前，应验证 AUTO_UPDATE_STATISTICS_ASYNC 选项是否设置为 OFF。 设置为 ON 时，用于更新统计信息的后台线程将与数据库建立连接，你将无法以单用户模式访问数据库。 若要查看此选项的状态，请查询 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_auto_update_stats_async_on` 列。 如果此选项设置为 ON，请执行以下任务：

1. 将 AUTO_UPDATE_STATISTICS_ASYNC 设置为 OFF。

2. 通过查询 [sys.dm_exec_background_job_queue](../../relational-databases/system-dynamic-management-views/sys-dm-exec-background-job-queue-transact-sql.md) 动态管理视图来检查活动的异步统计信息作业。

如果存在活动的作业，可以允许作业完成或通过使用 [KILL STATS JOB](../../t-sql/language-elements/kill-stats-job-transact-sql.md) 来手动终止这些作业。

RESTRICTED_USER     
只允许 `db_owner` 固定数据库角色的成员以及 `dbcreator` 和 `sysadmin` 固定服务器角色的成员连接到数据库。 RESTRICTED_USER 对连接数没有限制。 使用 ALTER DATABASE 语句的终止子句所指定的时间范围，断开所有数据库连接。 在数据库转换到 RESTRICTED_USER 状态后，不合格用户所做的连接尝试将被拒绝。

MULTI_USER     
所有拥有连接到数据库的相应权限的用户，都允许进行连接。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `user_access` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `UserAccess` 属性来确定状态。

\<delayed_durability_option> ::=     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）

控制提交的事务是完全持久事务还是延迟持久事务。

DISABLED     
`SET DISABLED` 之后的所有事务都是完全持久事务。 将忽略在原子块或 commit 语句中设置的任何持续性选项。

ALLOWED     
`SET ALLOWED` 之后的所有事务都是完全持久事务或都是延迟持久事务，具体取决于在原子块或 commit 语句中设置的持续性选项。

FORCED     
`SET FORCED` 之后的所有事务都是延迟持久事务。 将忽略在原子块或 commit 语句中设置的任何持续性选项。

\<external_access_option> ::=     
适用于：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

控制是否允许外部资源（例如另一个数据库中的对象）访问数据库。

DB_CHAINING { ON | OFF }     
ON     
数据库可以作为跨数据库所有权链的源或目标。

OFF     
数据库不能参与建立跨数据库所有权链。

> [!IMPORTANT]
> 如果 cross db ownership chaining 服务器选项为 0 (OFF)，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例将可以识别此设置。 如果 cross db ownership chaining 为 1 (ON)，则不论此选项为何值，所有用户数据库都可以参与跨数据库所有权链。 此选项是使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 来设置的。

要设置此选项，要求对数据库拥有 `CONTROL SERVER` 权限。

不能针对 master、model 和 tempdb 系统数据库设置 DB_CHAINING 选项。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_db_chaining_on` 列来确定此选项的状态。

TRUSTWORTHY { ON | OFF }     
ON     
使用模拟上下文的数据库模块（例如，用户定义函数或存储过程）可以访问数据库外部的资源。

OFF     
模拟上下文中的数据库模块不能访问数据库外部的资源。

只要附加数据库，TRUSTWORTHY 就会设置为 OFF。

默认情况下，除 msdb 数据库之外的所有系统数据库都将 TRUSTWORTHY 设置为 OFF。 对于 model 和 tempdb 数据库，不能更改此值。 建议在任何情况下都不要将 master 数据库的 TRUSTWORTHY 选项设置为 ON。

要设置此选项，要求对数据库拥有 `CONTROL SERVER` 权限。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_trustworthy_on` 列来确定此选项的状态。

DEFAULT_FULLTEXT_LANGUAGE     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）

指定全文检索列的默认语言值。

> [!IMPORTANT]
> 仅在将 CONTAINMENT 设置为 PARTIAL 之后，才允许使用此选项。 如果将 CONTAINMENT 设置为 NONE，将发生错误。

DEFAULT_LANGUAGE     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）

指定所有新建登录名的默认语言。 可以通过提供本地 ID (lcid)、语言名称或语言别名来指定语言。 有关可接受的语言名称和别名的列表，请参阅 [sys.syslanguages](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md)。 仅在将 CONTAINMENT 设置为 PARTIAL 之后，才允许使用此选项。 如果将 CONTAINMENT 设置为 NONE，将发生错误。

NESTED_TRIGGERS     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）

指定 AFTER 触发器是否可级联；级联是指执行某项操作将启动另一个触发器，而该触发器又将启动另外一个，依此类推。 仅在将 CONTAINMENT 设置为 PARTIAL 之后，才允许使用此选项。 如果将 CONTAINMENT 设置为 NONE，将发生错误。

TRANSFORM_NOISE_WORDS     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）

用于取消干扰词（或非索引字）导致全文查询的布尔操作失败时所产生的错误消息。 仅在将 CONTAINMENT 设置为 PARTIAL 之后，才允许使用此选项。 如果将 CONTAINMENT 设置为 NONE，将发生错误。

TWO_DIGIT_YEAR_CUTOFF     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）

指定一个介于 1753 到 9999 之间的整数，表示用于将两位数年份解释为四位数年份的截止年份。 仅在将 CONTAINMENT 设置为 PARTIAL 之后，才允许使用此选项。 如果将 CONTAINMENT 设置为 NONE，将发生错误。

\<FILESTREAM_option> ::=     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）

控制 FileTable 的设置。

NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL }     
OFF     
禁用对 FileTable 数据的非事务性访问。

READ_ONLY     
可以通过非事务性进程读取此数据库的 FileTable 中的 FILESTREAM 数据。

FULL     
启用对 FileTable 中 FILESTREAM 数据的完全非事务性访问。

DIRECTORY_NAME = \<directory_name>     
与 Windows 兼容的目录名称。 此名称应在该 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的所有数据库级目录名称中保持唯一。 无论排序规则如何设置，唯一性比较都不区分大小写。 在此数据库中创建 FileTable 之前，必须设置此选项。

\<HADR_options> ::=     
适用于：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

请参阅 [ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)。

\<mixed_page_allocation_option> ::=     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始）

控制数据库能否使用混合区为表或索引的前 8 页创建初始页面。

MIXED_PAGE_ALLOCATION { OFF | ON }     
OFF     
数据库始终使用统一区创建初始页面。 OFF 是默认值。

ON     
数据库可以使用混合区创建初始页面。

对于所有系统数据库，此设置均为 ON。 **tempdb** 是唯一支持 OFF 的系统数据库。

\<PARAMETERIZATION_option> ::=     
控制参数化选项。 有关详细信息，请参阅[查询处理体系结构指南](../../relational-databases/query-processing-architecture-guide.md#SimpleParam)。

PARAMETERIZATION { SIMPLE | FORCED }     
SIMPLE     
查询的参数化是根据数据库的默认行为进行的。

FORCED     
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对数据库中的所有查询进行参数化。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_parameterization_forced column` 确定此选项的当前设置。

<a name="query-store"></a> \<query_store_options> ::=     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始）

ON | **OFF** [ ( FORCED )  ] | CLEAR [ ALL ]     
控制查询存储是否在此数据库中启用，同时控制是否删除查询存储的内容。 有关详细信息，请参阅 [查询存储使用方案](../../relational-databases/performance/query-store-usage-scenarios.md)。

ON     
启用查询存储。

OFF [ ( FORCED ) ]      
禁用查询存储。 OFF 是默认值。 FORCED 是可选项。 FORCED 会中止所有正在运行的查询存储后台任务，并在查询存储关闭时跳过同步刷新。 使查询存储尽快关闭。 FORCED 适用于 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 CU14、[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU21、[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] CU6 和更高内部版本。

> [!NOTE]  
> 无法在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]的单一数据库和弹性池中禁用查询存储。 执行 `ALTER DATABASE [database] SET QUERY_STORE = OFF` 将返回警告 `'QUERY_STORE=OFF' is not supported in this version of SQL Server.`。 

CLEAR [ ALL ]     
从查询存储中删除与查询相关的数据。 ALL 是可选项。 ALL 将从查询存储中删除与查询相关的数据和元数据。

OPERATION_MODE { READ_ONLY | READ_WRITE }     
描述查询存储的操作模式。

READ_WRITE     
查询存储将收集并保留查询计划和运行时执行统计信息。

READ_ONLY     
可以从查询存储读取信息，但不会添加新信息。 如果已用尽查询存储的最大分配空间，查询存储的操作模式将更改为 READ_ONLY。

CLEANUP_POLICY     
描述查询存储的数据保留策略。 STALE_QUERY_THRESHOLD_DAYS 可确定查询信息在查询存储中保留的天数。 STALE_QUERY_THRESHOLD_DAYS 的类型为 **bigint**。 默认值为 30。

DATA_FLUSH_INTERVAL_SECONDS     
确定写入到查询存储的数据保留到磁盘的频率。 为了优化性能，由查询存储收集的数据应以异步方式写入到磁盘。 通过使用 DATA_FLUSH_INTERVAL_SECONDS 参数，配置此异步传输发生的频率。 DATA_FLUSH_INTERVAL_SECONDS 的类型为 **bigint**。 默认值为 900（15 分钟）。

MAX_STORAGE_SIZE_MB     
确定分配给查询存储的空间。 MAX_STORAGE_SIZE_MB 的类型为 **bigint**。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 到 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]）的默认值为 100 MB。 从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始，默认值为 1 GB。

> [!NOTE]
> 没有严格执行 `MAX_STORAGE_SIZE_MB` 限制。 仅当查询存储将数据写入磁盘时才检查存储大小。 此间隔由 `DATA_FLUSH_INTERVAL_SECONDS` 选项或 [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] 查询存储对话框选项“数据刷新时间间隔”设置。 间隔时间默认值为 900 秒（或 15 分钟）。
> 如果查询存储已违反存储大小检查之间的 `MAX_STORAGE_SIZE_MB` 限制，则它将转换为只读模式。 如果启用了 `SIZE_BASED_CLEANUP_MODE`，则也会触发强制实施 `MAX_STORAGE_SIZE_MB` 限制的清理机制。
> 清除足够的空间后，查询存储模式将自动切换回读写模式。

> [!IMPORTANT]
> 如果你认为工作负载捕获将需要 10 GB 以上的磁盘空间，则可能应重新考虑和优化工作负载以重用查询计划（例如，使用[强制参数化](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)），或调整查询存储配置。    
> 从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始，在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，可以将 `QUERY_CAPTURE_MODE` 设置为“CUSTOM”，以进一步控制查询捕获策略。

INTERVAL_LENGTH_MINUTES     
确定运行时执行统计数据聚合到查询存储中的时间间隔。 为了优化空间使用情况，将在固定时间窗口上聚合运行时统计信息存储中的运行时执行统计信息。 此固定时间窗口使用 INTERVAL_LENGTH_MINUTES 参数进行配置。 INTERVAL_LENGTH_MINUTES 的类型为 bigint。 默认值是 **60** 秒。

SIZE_BASED_CLEANUP_MODE { AUTO | OFF }     
控制当数据总量接近最大大小时是否自动激活清除。

AUTO     
当磁盘上的大小达到 MAX_STORAGE_SIZE_MB 的 90% 时，将自动激活基于大小的清除。 基于大小的清除首先会删除成本最低和最旧的查询。 它在达到 MAX_STORAGE_SIZE_MB 的大约 80% 时停止。 此值是默认配置值。

OFF     
不自动激活基于大小的清除。

SIZE_BASED_CLEANUP_MODE 的类型为 **nvarchar**。

QUERY_CAPTURE_MODE { ALL \| AUTO \| CUSTOM \| NONE }     
指定当前处于活动状态的查询捕获模式。 每个模式都定义了特定的查询捕获策略。

> [!NOTE]
> 当查询捕获模式设置为“全部”、“自动”或“自定义”时，始终捕获游标、存储过程中的查询和本机编译的查询。

ALL     
捕获所有查询。 ALL 是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 至 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]）的默认配置值。

AUTO     
根据执行计数和资源消耗捕获相关查询。 这是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（以 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开头）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 的默认配置值。

无     
停止捕获新查询。 查询存储将继续为已经捕获的查询收集编译和运行时统计信息。 请谨慎使用此配置，因为你可能会错过捕获重要的查询。

CUSTOM     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）

可控制 QUERY_CAPTURE_POLICY 选项。

QUERY_CAPTURE_MODE 的类型为 **nvarchar**。 

max_plans_per_query     
定义为每个查询保留的最大计划数。 MAX_PLANS_PER_QUERY 的类型为 int。默认值为 200。

WAIT_STATS_CAPTURE_MODE { ON | OFF }     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）

控制是否按查询捕获等待统计信息。

ON    
捕获每个查询的等待统计信息。 此值是默认配置值。

OFF    
不捕获每个查询的等待统计信息。

\<query_capture_policy_option_list> :: =     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）

控制查询存储捕获策略选项。 除 STALE_CAPTURE_POLICY_THRESHOLD 外，这些选项定义 OR 条件，需要满足这些条件，才能在定义的“过时捕获策略阈值”中捕获查询。

STALE_CAPTURE_POLICY_THRESHOLD = *number* { DAYS | HOURS }     
定义评估间隔时段以确定是否应捕获查询。 默认值为 1 天，可以设置为 1 小时到 7 天。 number 的类型为 int。

EXECUTION_COUNT     
定义在评估期间执行查询的次数。 默认值为 30，这意味着对于默认的过时捕获策略阈值，查询必须在一天内至少执行 30 次才能在查询存储中保留。 EXECUTION_COUNT 的类型为 int。

TOTAL_COMPILE_CPU_TIME_MS     
定义查询在评估期间使用的总编译 CPU 时间。 默认值为 1000，这意味着对于默认的过时捕获策略阈值，查询必须在一天内在查询编译期间总共花费至少一秒钟的 CPU 时间，才能持久存储在查询存储中。 TOTAL_COMPILE_CPU_TIME_MS 的类型为 int。

TOTAL_EXECUTION_CPU_TIME_MS     
定义查询在评估期间使用的总执行 CPU 时间。 默认值为 100，这意味着对于默认的过时捕获策略阈值，查询必须在一天内在执行期间总共花费至少100 ms 的 CPU 时间，才能持久存储在查询存储中。 TOTAL_EXECUTION_CPU_TIME_MS 的类型为 int。

\<recovery_option> ::=     
适用于：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

控制数据库恢复选项和磁盘 I/O 错误检查。

FULL     
通过使用事务日志备份，在介质发生故障后提供完整恢复。 如果数据文件损坏，介质恢复可以还原所有已提交的事务。 有关详细信息，请参阅[恢复模式](../../relational-databases/backup-restore/recovery-models-sql-server.md)。

BULK_LOGGED     
在发生媒体故障之后提供恢复。 对于某些大规模或批量操作，将最佳性能与最少日志空间使用量相结合。 有关能以最低限度记录的操作的信息，请参阅[事务日志](../../relational-databases/logs/the-transaction-log-sql-server.md)。 在 BULK_LOGGED 恢复模式下，这些操作的日志记录最少。 有关详细信息，请参阅[恢复模式](../../relational-databases/backup-restore/recovery-models-sql-server.md)。

SIMPLE     
系统将提供占用日志空间最小的简单备份策略。 服务器故障恢复不再需要的日志空间可被自动重用。 有关详细信息，请参阅[恢复模式](../../relational-databases/backup-restore/recovery-models-sql-server.md)。

> [!IMPORTANT]
> 简单恢复模式比其他两种模式更容易管理，但代价是数据文件损坏时丢失数据的风险也较大。 最近的数据库备份或差异数据库备份之后的所有更改都将丢失，必须手动重新输入。

默认恢复模式由 **model** 数据库的恢复模式决定。 有关选择适当恢复模式的详细信息，请参阅[恢复模式](../../relational-databases/backup-restore/recovery-models-sql-server.md)。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 recovery_model 和 recovery_model_desc 列来确定此选项的状态 。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `Recovery` 属性来确定状态。

TORN_PAGE_DETECTION { ON | OFF }     
ON     
[!INCLUDE[ssDE](../../includes/ssde-md.md)]可以检测不完整页。

OFF     
[!INCLUDE[ssDE](../../includes/ssde-md.md)] 不能检测不完整页。

> [!IMPORTANT]
> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中，将删除语法结构 TORN_PAGE_DETECTION ON | OFF。 请避免在新的开发工作中使用此语法结构，并计划修改当前使用此语法结构的应用程序。 请改用 PAGE_VERIFY 选项。

<a name="page_verify"></a> PAGE_VERIFY { CHECKSUM | TORN_PAGE_DETECTION | NONE }     
发现磁盘 I/O 路径错误引起的损坏的数据库页面。 磁盘 I/O 路径错误可能是数据库损坏问题的原因。 这些错误通常由在将页写入磁盘时发生的电源故障或磁盘硬件故障所导致。

CHECKSUM     
在向磁盘中写入页面时，计算整个页面内容的校验并将该值存储在页眉中。 从磁盘中读取页时，将重新计算校验和，并与存储在页头中的校验和值进行比较。 如果两个值不匹配，将同时在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志和 Windows 事件日志中报告错误消息 824（指示校验和失败）。 校验和失败指示存在 I/O 路径问题。 若要确定其根本原因，需要调查硬件、固件驱动程序、BIOS、筛选器驱动程序（如防病毒软件）和其他 I/O 路径组件。

TORN_PAGE_DETECTION     
将页面写入磁盘时，将每个 512 字节扇区的特定 2 位模式保存在 8 KB 数据库页面中并存储在数据库页头中。 从磁盘中读取页时，页头中存储的残缺位将与实际的页扇区信息进行比较。

如果值不匹配，表明只有页面的一部分被写入磁盘。 在这种情况下，将同时在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志和 Windows 事件日志中报告错误消息 824（指示页撕裂错误）。 如果页面写入确实不完整，则数据库恢复通常会检测到页撕裂。 不过，其他 I/O 路径故障可能随时导致页撕裂。

无     
数据库页写入不会生成 CHECKSUM 或 TORN_PAGE_DETECTION 值。 在读取过程中，即使页头中存在 CHECKSUM 或 TORN_PAGE_DETECTION 值，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 也不会验证校验和或页撕裂。

使用 PAGE_VERIFY 选项时，请考虑下列重要事项：

- 默认值为 CHECKSUM。
- 用户数据库或系统数据库升级到 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 或更高版本后，PAGE_VERIFY 值（NONE 或 TORN_PAGE_DETECTION）不会更改。 建议更改为 CHECKSUM。

    > [!NOTE]
    > 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本中，TempDB 数据库的 PAGE_VERIFY 数据库选项设置为 NONE 且不能修改。 从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 开始，对于新安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，TempDB 数据库的这一默认值为 CHECKSUM。 如果是升级安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，则默认值仍为 NONE。 可以修改该选项。 我们建议为 tempdb 数据库使用 CHECKSUM。

- TORN_PAGE_DETECTION 可能使用较少资源，但提供的 CHECKSUM 保护最少。
- 无需使数据库脱机、锁定数据库或以其他方式阻止对数据库的并发访问，即可设置 PAGE_VERIFY。
- CHECKSUM 与 TORN_PAGE_DETECTION 互相排斥。 不能同时启用这两个选项。

检测到页撕裂或校验和失败时，如果失败仅限于索引页，则可通过还原数据进行恢复，可能还需要重建索引进行恢复。 如果要在校验和失败的情况下确定受影响的一个或多个数据库页面的类型，请运行 DBCC CHECKDB。 有关还原选项的详细信息，请参阅 [RESTORE 参数](../../t-sql/statements/restore-statements-arguments-transact-sql.md)。 虽然还原数据可解决数据损坏问题，但应尽快诊断并纠正根本原因（如磁盘硬件故障），以防止继续出错。

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将对因校验和、页撕裂或其他 I/O 错误而失败的任何读取都重试四次。 如果在任何一次重试中读取成功，则会向错误日志写入消息。 触发读取的命令会继续执行。 如果重试失败，则该命令失败，且显示错误消息 824。

有关错误消息 823、824 和 825 的详细信息，请参阅：

- [如何解决 SQL Server 中的消息 823 错误](https://support.microsoft.com/help/2015755)
- [如何解决 SQL Server 中的消息 824](https://support.microsoft.com/help/2015756)
- [如何解决消息“重试读取”的问题](https://support.microsoft.com/help/2015757)。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `page_verify_option` 列或 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsTornPageDetectionEnabled` 属性，确定此选项的当前设置。

\<remote_data_archive_option> ::=     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始）

为数据库启用或禁用 Stretch Database。 有关详细信息，请参阅 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)。

REMOTE_DATA_ARCHIVE = { ON ( SERVER = \<server_name> , { CREDENTIAL = \<db_scoped_credential_name> \| FEDERATED_SERVICE_ACCOUNT = ON \| OFF } )\| OFF     
ON     
为数据库启用 Stretch Database。 有关详细信息，包括附加的先决条件，请参阅[为数据库启用 Stretch Database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)。

若要为表启用 Stretch Database，需要 `db_owner` 权限。 若要为数据库启用 Stretch Database，需要 `db_owner` 和 `CONTROL DATABASE` 权限。

SERVER = \<server_name>     
指定 Azure 服务器的地址。 包括名称的 `.database.windows.net` 部分。 例如，`MyStretchDatabaseServer.database.windows.net`。

CREDENTIAL = \<db_scoped_credential_name>     
指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例用于连接到 Azure 服务器的数据库作用域凭据。 在运行此命令之前，确保存在凭据。 有关详细信息，请参阅 [CREATE DATABASE SCOPED CREDENTIAL](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)。

FEDERATED_SERVICE_ACCOUNT = { ON | OFF }     
当下列条件全数成立时，可以使用联合服务帐户让本地 SQL Server 与远程 Azure 服务器通信。

- 用来运行 SQL Server 实例的服务帐户为域帐户。
- 域帐户属于其 Active Directory 与 Azure Active Directory 联合的域。
- 远程 Azure 服务器已配置为支持 Azure Active Directory 身份验证。
- 用来运行 SQL Server 实例的服务帐户在远程 Azure 服务器上必须配置为 `dbmanager` 或 `sysadmin` 帐户。

如果将联合服务帐户指定为“ON”，则不能同时指定 CREDENTIAL 参数。 如果指定 OFF，则提供 CREDENTIAL 参数。

OFF     
为数据库禁用 Stretch Database。 有关详细信息，请参阅 [禁用 Stretch Database 并恢复远程数据](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md)。

只有在数据库不再包含为 Stretch Database 启用的任何表后，才能为数据库禁用 Stretch Database。 禁用 Stretch Database 之后，数据迁移会停止。 此外，查询结果不再包括来自远程表的结果。

禁用 Stretch 不会删除远程数据库。 若要删除远程数据库，请使用 Azure 门户将其删除。

\<service_broker_option> ::=     
适用于：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

控制下列 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 选项：启用或禁用消息传递，设置新的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 标识符，或者将会话优先级设置为 ON 或 OFF。

ENABLE_BROKER     
指定对指定的数据库启用 [!INCLUDE[ssSB](../../includes/sssb-md.md)]。 消息传递已启动，is_broker_enabled 标志在 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中设置为 True。 数据库保留现有的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 标识符。 当数据库是数据库镜像配置中的主体时，无法启用 Service broker。

> [!NOTE]
> ENABLE_BROKER 要求排他数据库锁。 如果其他会话已锁定了数据库中的资源，ENABLE_BROKER 将等待其他会话释放其锁。 若要在用户数据库中启用 [!INCLUDE[ssSB](../../includes/sssb-md.md)]，请确保在运行 ALTER DATABASE SET ENABLE_BROKER 语句之前其他会话没有使用该数据库，例如，将该数据库置于单用户模式。 若要在 msdb 数据库中启用 [!INCLUDE[ssSB](../../includes/sssb-md.md)]，请首先停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理，这样 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 便可获得必要的锁。

DISABLE_BROKER     
指定对指定的数据库禁用 [!INCLUDE[ssSB](../../includes/sssb-md.md)]。 消息传递已停止，is_broker_enabled 标志在 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中设置为 False。 数据库保留现有的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 标识符。

NEW_BROKER     
指定数据库应接收新的 Broker 标识符。 数据库充当新 Service Broker。 因此，将立即删除数据库中的所有现有对话，而不生成结束对话框消息。 必须使用新标识符重新创建任何引用旧 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 标识符的路由。

ERROR_BROKER_CONVERSATIONS     
指定启用 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 消息传递。 此设置会保留数据库的现有 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 标识符。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 将结束数据库中的所有会话，并显示错误消息。 此设置使应用程序可以为现有对话运行定期清理。

HONOR_BROKER_PRIORITY {ON | OFF}     
ON     
发送操作考虑到分配给会话的优先级别。 先发送来自优先级别高的对话的消息，再发送来自所分配优先级别低的对话的消息。

OFF     
发送操作就像在所有会话都具有默认优先级别的情况下一样运行。

对于新的对话框或没有等待发送的消息的对话框，对 HONOR_BROKER_PRIORITY 选项的更改会立即生效。 在 ALTER DATABASE 运行时具有要发送的消息的对话框在其部分消息完成发送前，不会接受新的设置。 在所有对话框都开始使用新设置前等待的时间可能相差迥异。

此属性的当前设置在 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_broker_priority_honored` 列中进行报告。

\<snapshot_option> ::=     
计算事务隔离级别。

ALLOW_SNAPSHOT_ISOLATION { ON | OFF }     
ON     
在数据库级别启用快照选项。 启用该选项后，DML 语句将开始生成行版本，即使没有事务使用快照隔离也是如此。 一旦启用此选项，事务即可指定 SNAPSHOT 事务隔离级别。 当事务在 SNAPSHOT 隔离级别运行时，所有的语句都将数据快照视为位于事务的开头。 如果在 SNAPSHOT 隔离级别运行的事务要访问多个数据库中的数据，则必须将所有数据库中的 ALLOW_SNAPSHOT_ISOLATION 都设置为 ON，或者事务中的每个语句都必须对 FROM 子句中的所有引用（引用 ALLOW_SNAPSHOT_ISOLATION 设置为 OFF 的数据库中的表）使用锁提示。

OFF     
在数据库级别禁用快照选项。 事务不能指定 SNAPSHOT 事务隔离级别。

在将 ALLOW_SNAPSHOT_ISOLATION 设置为新状态（从 ON 设置为 OFF，或从 OFF 设置为 ON）时，在数据库中的所有现有事务均已提交之前，ALTER DATABASE 不会将控制权返回给调用方。 如果数据库已处于 ALTER DATABASE 语句所指定的状态，则控制权会立刻返回给调用方。 如果 ALTER DATABASE 语句未迅速返回，请使用 [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) 确定是否存在长期运行的事务。 如果 ALTER DATABASE 语句被取消，则数据库仍保持 ALTER DATABASE 开始时所处的状态。 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图指示数据库中快照隔离事务的状态。 如果 **snapshot_isolation_state_desc** = IN_TRANSITION_TO_ON，ALTER DATABASE ALLOW_SNAPSHOT_ISOLATION OFF 将暂停六秒钟，然后重试操作。

如果数据库处于 OFFLINE 状态，则不能更改 ALLOW_SNAPSHOT_ISOLATION 的状态。

如果在 READ_ONLY 数据库中设置 ALLOW_SNAPSHOT_ISOLATION，则以后将数据库设置为 READ_WRITE 时，仍将保留该设置。

可以为 master、model、msdb 和 tempdb 数据库更改 ALLOW_SNAPSHOT_ISOLATION 设置。 如果为 tempdb 更改该设置，则每次停止并重新启动[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例时会保留该设置。 如果为 model 更改该设置，则该设置将成为除 tempdb 以外的所有新建数据库的默认设置。

对于 master 和 msdb 数据库，默认情况下该选项设置为 ON。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `snapshot_isolation_state` 列确定此选项的当前设置。

READ_COMMITTED_SNAPSHOT { ON | OFF }     
ON     
在数据库级别启用已提交读快照选项。 启用该选项后，DML 语句将开始生成行版本，即使没有事务使用快照隔离也是如此。 一旦启用此选项，指定已提交读隔离级别的事务将使用行版本控制而不是锁定。 当事务在 READ COMMITTED 隔离级别运行时，所有的语句都将数据快照视为位于语句的开头。

OFF     
在数据库级别禁用已提交读快照选项。 指定 READ COMMITTED 隔离级别的事务使用锁定。

若要将 READ_COMMITTED_SNAPSHOT 设置为 ON 或 OFF，不应存在任何活动的数据库连接，运行 ALTER DATABASE 命令的连接除外。 但是，数据库不必一定要处于单用户模式下。 当数据库处于 OFFLINE 状态时，不能更改此选项的状态。

如果在 READ_ONLY 数据库中设置 READ_COMMITTED_SNAPSHOT，则以后将数据库设置为 READ_WRITE 时，仍将保留该设置。

对于 master、tempdb 或 msdb 系统数据库，不能将 READ_COMMITTED_SNAPSHOT 设置为 ON。 如果为 model 更改该设置，则该设置会成为除 tempdb 以外的所有新建数据库的默认设置。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_read_committed_snapshot_on` 列确定此选项的当前设置。

> [!WARNING]
> 当使用 DURABILITY = SCHEMA_ONLY 创建表，随后使用 ALTER DATABASE 更改 READ_COMMITTED_SNAPSHOT 时，表中的数据将丢失  。

MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT { ON | OFF }     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）

ON     
当事务隔离级别设置为任何低于 SNAPSHOT 的隔离级别时，内存优化表中所有经过解释的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 操作将在 SNAPSHOT 隔离下运行。 低于快照的隔离级别示例有 READ COMMITTED 或 READ UNCOMMITTED。 无论是在会话级别显式设置事务隔离级别还是隐式使用默认值，这些操作都会运行。

OFF     
不提升内存优化表中经过解释的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 操作的事务隔离级别。

如果数据库处于 OFFLINE 状态，不能更改 MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT 的状态。

默认选项为 OFF。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_memory_optimized_elevate_to_snapshot_on` 列确定此选项的当前设置。

\<sql_option> ::=     
在数据库级别控制 ANSI 遵从选项。

ANSI_NULL_DEFAULT { ON | OFF }     
确定在 CREATE TABLE 或 ALTER TABLE 语句中未显式定义为 Null 性的列或 [CLR 用户定义类型](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)的默认值（NULL 或 NOT NULL）。 无论此设置如何，使用约束定义的列都将遵循约束规则。

ON     
未定义列的默认值为 NULL。

OFF     
未定义列的默认值不为 NULL。

连接级设置（使用 SET 语句设置）覆盖 ANSI_NULL_DEFAULT 的默认数据库级别设置。 默认情况下，ODBC 和 OLE DB 客户端发出连接级 SET 语句，将会话的 ANSI_NULL_DEFAULT 设置为 ON。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md)。

对于 ANSI 兼容性，数据库选项 ANSI_NULL_DEFAULT 设置为 ON 将使数据库默认设置改为 NULL。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_ansi_null_default_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAnsiNullDefault` 属性来确定状态。

ANSI_NULLS { ON | OFF }     
ON     
与 Null 值的所有比较的结果均为 UNKNOWN。

OFF     
将非 UNICODE 值与 Null 值比较时，如果这两个值都为 NULL，则结果为 TRUE。

> [!IMPORTANT]
> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中，ANSI_NULLS 将始终为 ON，将该选项显式设置为 OFF 的任何应用程序都将产生错误。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。

连接级设置（使用 SET 语句设置）覆盖 ANSI_NULLS 的默认数据库设置。 默认情况下，ODBC 和 OLE DB 客户端发出连接级 SET 语句，将会话的 ANSI_NULLS 设置为 ON。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md)。

> [!IMPORTANT]
> 创建或更改计算列或索引视图的索引时，SET ANSI_NULLS 必须设置为 ON。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_ansi_nulls_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAnsiNullsEnabled` 属性来确定状态。

ANSI_PADDING { ON | OFF }     
ON     
在进行转换之前，将字符串填充到同一长度。 在插入到 varchar 或 nvarchar 数据类型之前，也填充到同一长度。

OFF     
将字符值中的尾随空格插入 varchar 或 nvarchar 列中。 也保留插入 varbinary 列中的二进制值的尾随零。 不将值填充到列的长度。

如果指定了 OFF，该设置只影响新列的定义。

> [!IMPORTANT]
> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中，ANSI_PADDING 将始终为 ON，将该选项显式设置为 OFF 的任何应用程序都将产生错误。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 建议始终将 ANSI_PADDING 设置为 ON。 创建或操作计算列或索引视图的索引时，ANSI_PADDING 必须为 ON。

当 ANSI_PADDING 设置为 ON 时，会将允许为 Null 值的 char(_n_) 和 binary(_n_) 列填充到列长度。 当 ANSI_PADDING 为 OFF 时，会剪裁尾随空格和零。 始终将不允许为 Null 值的 char(_n_) 和 binary(_n_) 列填充到列长度 。

连接级设置（使用 SET 语句设置）覆盖 ANSI_PADDING 的默认数据库级别设置。 默认情况下，ODBC 和 OLE DB 客户端发出连接级 SET 语句，将会话的 ANSI_PADDING 设置为 ON。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md)。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_ansi_padding_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAnsiPaddingEnabled` 属性来确定状态。

ANSI_WARNINGS { ON | OFF }     
ON     
当出现被零除这类情况时，将发出错误或警告。 当聚合函数中出现 Null 值时，也会发出错误和警告。

OFF     
出现被零除等情况时不会引发警告，而是返回 Null 值。

> [!IMPORTANT]
> 创建或更改计算列或索引视图的索引时，SET ANSI_WARNINGS 必须设置为 ON。

连接级设置（使用 SET 语句设置）覆盖 ANSI_WARNINGS 的默认数据库设置。 默认情况下，ODBC 和 OLE DB 客户端发出连接级 SET 语句，将会话的 ANSI_WARNINGS 设置为 ON。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md)。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_ansi_warnings_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAnsiWarningsEnabled` 属性来确定状态。

ARITHABORT { ON | OFF }     
ON     
在查询执行过程中出现溢出或被零除等错误时，结束查询。

OFF     
在出现其中一个错误时显示警告消息。 即使显示警告，查询、批处理或事务也会继续进行处理，就像没有发生错误一样。

> [!IMPORTANT]
> 创建或更改计算列或索引视图的索引时，SET ARITHABORT 必须设置为 ON。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_arithabort_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsArithmeticAbortEnabled` 属性来确定状态。

COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }     

有关详细信息，请参阅 [ALTER DATABASE 兼容级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。

CONCAT_NULL_YIELDS_NULL { ON | OFF }     
ON     
当串联运算的两个操作数中任意一个为 NULL 时，结果也为 NULL。 例如，将字符串“This is”和 NULL 串联将返回 NULL 值，而不是“This is”值。

OFF     
Null 值被视为空字符串进行处理。

> [重要] 创建或更改计算列或索引视图的索引时，CONCAT_NULL_YIELDS_NULL 必须设置为 ON。
>
> 在即将推出的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本中，CONCAT_NULL_YIELDS_NULL 将始终为 ON，而且将该选项显式设置为 OFF 的任何应用程序都将触发错误。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。

连接级设置（使用 SET 语句设置）覆盖 CONCAT_NULL_YIELDS_NULL 的默认数据库设置。 默认情况下，当连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时，ODBC 和 OLE DB 客户端发出连接级别 SET 语句，将会话的 CONCAT_NULL_YIELDS_NULL 设置为 ON。 有关详细信息，请参阅 [SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_concat_null_yields_null_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsNullConcat` 属性来确定状态。

QUOTED_IDENTIFIER { ON | OFF }     
ON     
可以将分隔标识符包含在双引号中。

所有用双引号分隔的字符串都被解释为对象标识符。 加引号的标识符不必遵守 [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符规则。 它们可以是关键字，并且可以包含 [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符中不允许的字符。 如果单引号 (') 是文字字符串的一部分，则可以用双引号 (") 表示它。

OFF     
标识符不能包含在引号中，而且必须遵循所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符规则。 文字可以由单引号或双引号分隔。

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 还允许使用方括号 ([ ]) 分隔标识符。 无论 QUOTED_IDENTIFIER 设置如何，始终都可以使用用方括号括起来的标识符。 有关详细信息，请参阅 [Database Identifiers](../../relational-databases/databases/database-identifiers.md)。

创建表后，QUOTED IDENTIFIER 选项在表的元数据中始终存储为 ON。 即使在创建表时将该选项设置为 OFF 也会存储该选项。

连接级设置（使用 SET 语句设置）覆盖 QUOTED_IDENTIFIER 的默认数据库设置。 默认情况下，ODBC 和 OLE DB 客户端发出连接级 SET 语句，将 QUOTED_IDENTIFIER 设置为 ON。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md)。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_quoted_identifier_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsQuotedIdentifiersEnabled` 属性来确定状态。

NUMERIC_ROUNDABORT { ON | OFF }     
ON     
当表达式中发生精度损失时生成错误。

OFF     
精度的降低不会生成错误消息，会根据存储结果的列或变量的精度，将结果舍入。

> [!IMPORTANT]
> 创建或更改计算列或索引视图的索引时，NUMERIC_ROUNDABORT 必须设置为 OFF。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_numeric_roundabort_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsNumericRoundAbortEnabled` 属性来确定状态。

RECURSIVE_TRIGGERS { ON | OFF }     
ON     
允许递归激发 AFTER 触发器。

OFF     
可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_recursive_triggers_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsRecursiveTriggersEnabled` 属性来确定状态。

> [!NOTE]
> 当 RECURSIVE_TRIGGERS 设置为 OFF 时，只禁止直接递归触发。 若要禁用间接递归触发，还必须将 nested triggers 服务器选项设置为 0。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_recursive_triggers_on` 列或 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsRecursiveTriggersEnabled` 属性来确定此选项的状态。

\<target_recovery_time_option> ::=     
**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）

指定每个数据库上间接检查点的频率。 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始，新数据库的默认值为 1 分钟，表示数据库使用间接检查点。 较旧版本的默认值为 0，表示数据库使用自动检查点，其频率依赖于服务器实例的恢复间隔设置。 对于大多数系统，[!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议设置为 1 分钟。

TARGET_RECOVERY_TIME **=** *target_recovery_time* { SECONDS | MINUTES }     
*target_recovery_time*     
指定在发生崩溃的情况下恢复指定数据库的最大上限时间。 target_recovery_time 的类型为 int。

SECONDS     
指示 *target_recovery_time* 表示为秒数。

MINUTES     
指示 *target_recovery_time* 表示为分钟数。

有关间接检查点的详细信息，请参阅[数据库检查点](../../relational-databases/logs/database-checkpoints-sql-server.md)。

WITH \<termination> ::=     
指定当数据库从一种状态转换到另一种状态时，何时回滚未完成的事务。 如果终止子句被忽略，则当数据库中存在任何锁时，ALTER DATABASE 语句将无限期等待。 只能指定一条终止子句，而且该子句应跟在 SET 子句后面。

> [!NOTE]
> 并非所有数据库选项都使用 WITH \<termination> 子句。 有关详细信息，请参阅本文的“备注”部分中的[设置选项](#SettingOptions)下面的表。

ROLLBACK AFTER *number* [SECONDS] | ROLLBACK IMMEDIATE     
指定是在指定秒数之后回滚还是立即回滚。 number 的类型为 int。

NO_WAIT     
指定：如果请求的数据库状态或选项更改无法立即完成，则请求失败。 立即完成意味着不会等待事务自己提交或回滚。

## <a name="setting-options"></a><a name="SettingOptions"></a> 设置选项
若要检索数据库选项的当前设置，请使用 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图或 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)

设置数据库选项后，新的设置将立即生效。

可以为所有新建数据库更改任意一个数据库选项的默认值。 为此，请更改 model 数据库中的相应数据库选项。

并非所有数据库选项都使用 WITH \<termination> 子句，也不是所有数据库选项都能结合其他选项指定。 下表列出这些选项以及它们的选项和终止状态。

|选项类别|可与其他选项一起指定|可以使用 WITH \<termination> 子句|
|----------------------|-----------------------------------------|---------------------------------------------|
|\<db_state_option>|是|是|
|\<db_user_access_option>|是|是|
|\<db_update_option>|是|是|
|\<delayed_durability_option>|是|是|
|\<external_access_option>|是|否|
|\<cursor_option>|是|否|
|\<auto_option>|是|否|
|\<sql_option>|是|否|
|\<recovery_option>|是|否|
|\<target_recovery_time_option>|否|是|
|\<database_mirroring_option>|否|否|
|ALLOW_SNAPSHOT_ISOLATION|否|否|
|READ_COMMITTED_SNAPSHOT|否|是|
|MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT|是|是|
|\<service_broker_option>|是|否|
|DATE_CORRELATION_OPTIMIZATION|是|是|
|\<parameterization_option>|是|是|
|\<change_tracking_option>|是|是|
|\<db_encryption_option>|是|否|

通过设置以下选项之一来清除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计划缓存：

:::row:::
    :::column:::
        OFFLINE
    :::column-end:::
    :::column:::
        ONLINE
    :::column-end:::
    :::column:::
        MODIFY_NAME
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        COLLATE
    :::column-end:::
    :::column:::
        READ_ONLY
    :::column-end:::
    :::column:::
        READ_WRITE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        MODIFY FILEGROUP DEFAULT
    :::column-end:::
    :::column:::
        MODIFY FILEGROUP READ_WRITE
    :::column-end:::
    :::column:::
        MODIFY FILEGROUP READ_ONLY
    :::column-end:::
:::row-end:::

在下列情况下，也会刷新计划缓存。

- 数据库的 AUTO_CLOSE 数据库选项设置为 ON。 在没有用户连接引用或使用该数据库时，后台任务将尝试关闭并自动关闭数据库。
- 针对具有默认选项的数据库运行多个查询。 然后，删除数据库。
- 删除源数据库的数据库快照。
- 您已成功重新生成数据库的事务日志。
- 还原数据库备份。
- 分离数据库。

清除计划缓存将导致对所有后续执行计划进行重新编译，并可能导致查询性能暂时性地突然降低。 对于计划缓存中每个已清除的缓存存储区，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志包含以下信息性消息：`SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to some database maintenance or reconfigure operations`。 每隔五分钟，只要缓存在这段时间间隔内得到刷新，此消息就记录一次。

## <a name="examples"></a>示例

### <a name="a-setting-options-on-a-database"></a>A. 设置数据库选项

下面的示例设置 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库的恢复模式和数据页面验证选项。

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET RECOVERY FULL PAGE_VERIFY CHECKSUM;
GO

```

### <a name="b-setting-the-database-to-read_only"></a>B. 将数据库设置为 READ_ONLY

将数据库或文件组的状态改为 READ_ONLY 或 READ_WRITE 需要具有数据库的专有访问权限。 下面的示例将数据库设置为 `SINGLE_USER` 模式，以获得独占访问权。 然后，该示例将 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的状态设置为 `READ_ONLY` ，并将对数据库的访问权返回给所有用户。

> [!NOTE]
> 此示例在第一个 `WITH ROLLBACK IMMEDIATE` 语句中使用终止选项 `ALTER DATABASE` 。 所有未完成事务都将被回滚，并将立刻断开 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的所有其他连接。

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET SINGLE_USER
WITH ROLLBACK IMMEDIATE;
GO
ALTER DATABASE [database_name]
SET READ_ONLY
GO
ALTER DATABASE [database_name]
SET MULTI_USER;
GO
```

### <a name="c-enabling-snapshot-isolation-on-a-database"></a>C. 对数据库启用快照隔离

下面的示例为 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库启用快照隔离框架选项。

```sql
USE [database_name];
USE master;
GO
ALTER DATABASE [database_name]
SET ALLOW_SNAPSHOT_ISOLATION ON;
GO
-- Check the state of the snapshot_isolation_framework
-- in the database.
SELECT name, snapshot_isolation_state,
    snapshot_isolation_state_desc AS description
FROM sys.databases
WHERE name = N'[database_name]';
GO

```

结果集显示快照隔离框架已启用。

|name |snapshot_isolation_state |description|
|-------------------- |------------------------|----------|
|[database_name] |1| ON |

### <a name="d-enabling-modifying-and-disabling-change-tracking"></a>D. 启用、修改和禁用更改跟踪

下面的示例对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库启用更改跟踪并将保持期设置为 `2` 天。

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = ON
(AUTO_CLEANUP = ON, CHANGE_RETENTION = 2 DAYS);
```

下面的示例说明如何将保持期更改为 `3` 天。

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING (CHANGE_RETENTION = 3 DAYS);
```

下面的示例说明如何对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库禁用更改跟踪。

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = OFF;
```

### <a name="e-enabling-the-query-store"></a>E. 启用查询存储

**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始）

下面的示例启用查询存储并配置其参数。

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

### <a name="f-enabling-the-query-store-with-wait-statistics"></a>F. 使用等待统计信息启用查询存储

**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）

下面的示例启用查询存储并配置其参数。

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
    );
```

### <a name="g-enabling-the-query-store-with-custom-capture-policy-options"></a>G. 使用自定义捕获策略选项启用查询存储

**适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）

下面的示例启用查询存储并配置其参数。

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
      QUERY_CAPTURE_MODE = CUSTOM,
      QUERY_CAPTURE_POLICY = (
        STALE_CAPTURE_POLICY_THRESHOLD = 24 HOURS,
        EXECUTION_COUNT = 30,
        TOTAL_COMPILE_CPU_TIME_MS = 1000,
        TOTAL_EXECUTION_CPU_TIME_MS = 100
      )
    );
```

## <a name="see-also"></a>另请参阅

- [ALTER DATABASE 兼容级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)
- [ALTER DATABASE 数据库镜像](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)
- [ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)
- [统计信息](../../relational-databases/statistics/statistics.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [启用和禁用更改跟踪](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [Query Store 最佳实践](../../relational-databases/performance/best-practice-with-the-query-store.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-set-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        \* SQL 数据库 \*&nbsp;
    :::column-end:::
    :::column:::
        [SQL 托管实例](alter-database-transact-sql-set-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>SQL 数据库

兼容性级别是 `SET` 选项，但在 [ALTER DATABASE 兼容性级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)中进行了说明。

> [!NOTE]
> 可以使用 [SET Statements](../../t-sql/statements/set-statements-transact-sql.md) 来为当前会话配置很多数据库 SET 选项，当它们连接时通常通过应用程序来配置。 会话级 SET 选项覆盖 ALTER DATABASE SET 值。 下面各节中所述的数据库选项是你可以为未明确提供其他 SET 选项值的会话设置的值。

## <a name="syntax"></a>语法

```syntaxsql
ALTER DATABASE { database_name | Current }
SET
{
    <option_spec> [ ,...n ] [ WITH <termination> ]
}
;

<option_spec> ::=
{
    <auto_option>
  | <automatic_tuning_option>
  | <change_tracking_option>
  | <cursor_option>
  | <db_encryption_option>
  | <db_update_option>
  | <db_user_access_option>
  | <delayed_durability_option>
  | <parameterization_option>
  | <query_store_options>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
}
;

<auto_option> ::=
{
    AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] }
  | AUTO_SHRINK { ON | OFF }
  | AUTO_UPDATE_STATISTICS { ON | OFF }
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<automatic_tuning_option> ::=
{
    AUTOMATIC_TUNING = { AUTO | INHERIT | CUSTOM }
  | AUTOMATIC_TUNING ( CREATE_INDEX = { DEFAULT | ON | OFF } )
  | AUTOMATIC_TUNING ( DROP_INDEX = { DEFAULT | ON | OFF } )
  | AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = { DEFAULT | ON | OFF } )
}

<change_tracking_option> ::=
{
    CHANGE_TRACKING
    {
        = OFF
      | = ON [ ( <change_tracking_option_list > [,...n] ) ]
      | ( <change_tracking_option_list> [,...n] )
    }
}

<change_tracking_option_list> ::=
   {
       AUTO_CLEANUP = { ON | OFF }
     | CHANGE_RETENTION = retention_period { DAYS | HOURS | MINUTES }
   }

<cursor_option> ::=
{
    CURSOR_CLOSE_ON_COMMIT { ON | OFF }
}

<db_encryption_option> ::=
  ENCRYPTION { ON | OFF }

<db_update_option> ::=
  { READ_ONLY | READ_WRITE }

<db_user_access_option> ::=
  { RESTRICTED_USER | MULTI_USER }

<delayed_durability_option> ::= DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }

<parameterization_option> ::=
  PARAMETERIZATION { SIMPLE | FORCED }

<query_store_options> ::=
{
  QUERY_STORE
  {
      = OFF
    | = ON [ ( <query_store_option_list> [,... n] ) ]
    | ( < query_store_option_list> [,... n] )
    | CLEAR [ ALL ]
  }
}

<query_store_option_list> ::=
{
  OPERATION_MODE = { READ_WRITE | READ_ONLY }
  | CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = number )
  | DATA_FLUSH_INTERVAL_SECONDS = number
  | MAX_STORAGE_SIZE_MB = number
  | INTERVAL_LENGTH_MINUTES = number
  | SIZE_BASED_CLEANUP_MODE = { AUTO | OFF }
  | QUERY_CAPTURE_MODE = { ALL | AUTO | CUSTOM | NONE }
  | MAX_PLANS_PER_QUERY = number
  | WAIT_STATS_CAPTURE_MODE = { ON | OFF }
  | QUERY_CAPTURE_POLICY = ( <query_capture_policy_option_list> [,...n] )
}

<query_capture_policy_option_list> :: =
{
    STALE_CAPTURE_POLICY_THRESHOLD = number { DAYS | HOURS }
    | EXECUTION_COUNT = number
    | TOTAL_COMPILE_CPU_TIME_MS = number
    | TOTAL_EXECUTION_CPU_TIME_MS = number
}

<snapshot_option> ::=
{
    ALLOW_SNAPSHOT_ISOLATION { ON | OFF }
  | READ_COMMITTED_SNAPSHOT {ON | OFF }
  | MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT {ON | OFF }
}
<sql_option> ::=
{
    ANSI_NULL_DEFAULT { ON | OFF }
  | ANSI_NULLS { ON | OFF }
  | ANSI_PADDING { ON | OFF }
  | ANSI_WARNINGS { ON | OFF }
  | ARITHABORT { ON | OFF }
  | COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }
  | CONCAT_NULL_YIELDS_NULL { ON | OFF }
  | NUMERIC_ROUNDABORT { ON | OFF }
  | QUOTED_IDENTIFIER { ON | OFF }
  | RECURSIVE_TRIGGERS { ON | OFF }
}

<termination>::=
{
    ROLLBACK AFTER integer [ SECONDS ]
  | ROLLBACK IMMEDIATE
  | NO_WAIT
}

<temporal_history_retention>::=TEMPORAL_HISTORY_RETENTION { ON | OFF }
```

## <a name="arguments"></a>参数

database_name     
要修改的数据库的名称。

CURRENT     
`CURRENT` 运行当前数据库中的操作。 并不是所有上下文中的所有选项都支持 `CURRENT`。 如果 `CURRENT` 失败，则提供数据库名称。

\<auto_option> ::=     
控制自动选项。

<a name="auto_create_statistics"></a> AUTO_CREATE_STATISTICS { ON | OFF }     
ON     
查询优化器根据需要在查询谓词中的单列上创建统计信息，以便改进查询计划和查询性能。 在查询优化器编译查询时创建这些单列统计信息。 这些单列统计信息只在尚不是现有统计信息对象的第一列的列上创建。

默认值为 ON。 建议您对于大多数数据库使用默认设置。

OFF     
查询优化器在编译查询时不在查询谓词中的单列上创建统计信息。 将此选项设置为 OFF 可能导致并非最佳的查询计划以及查询性能下降。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_auto_create_stats_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAutoCreateStatistics` 属性来确定状态。

有关详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md#statistics-options)中的“统计信息选项”部分。

INCREMENTAL = ON | OFF     
将 AUTO_CREATE_STATISTICS 设置为 ON，并将 INCREMENTAL 设置为 ON。 只要支持增量统计信息，此设置便会自动创建增量统计信息。 默认值为 OFF。 有关详细信息，请参阅 [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md)。

<a name="auto_shrink"></a> AUTO_SHRINK { ON | OFF }     
ON     
数据库文件是定期收缩的候选项。

数据文件和日志文件都可以自动收缩。 只有在将数据库设置为 SIMPLE 恢复模式时，或备份事务日志时，AUTO_SHRINK 才可减小事务日志的大小。 当设置为 OFF 时，在定期检查未使用空间的过程中，数据库文件不自动收缩。

当文件中超过百分之二十五的部分包含未使用的空间时，AUTO_SHRINK 选项将导致收缩文件。 该选项会导致文件收缩为两种大小之一。 它会收缩为其中较大的大小：

- 其中 25% 的文件不包含任何内容时的大小
- 文件创建时的大小

不能收缩只读数据库。

OFF     
在定期检查未使用空间时不会自动收缩数据库文件。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_auto_shrink_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAutoShrink` 属性来确定状态。

> [!NOTE]
> AUTO_SHRINK 选项在包含的数据库中不可用。

<a name="auto_update_statistics"></a> AUTO_UPDATE_STATISTICS { ON | OFF }     
ON     
指定在统计信息由查询使用并且可能过期时，查询优化器更新统计信息。 统计信息将在插入、更新、删除或合并操作更改表或索引视图中的数据分布后过期。 查询优化器通过计算自最后统计信息更新后数据修改的次数并且将这一修改次数与某一阈值进行比较，确定统计信息何时可能过期。 该阈值基于表中或索引视图中的行数。

查询优化器在编译查询和运行缓存查询计划前，检查是否存在过期的统计信息。 查询优化器使用查询谓词中的列、表和索引视图确定哪些统计信息可能过期。 查询优化器在编译查询之前确定此信息。 在运行缓存查询计划前，[!INCLUDE[ssDE](../../includes/ssde-md.md)] 确认该查询计划引用最新的统计信息。

AUTO_UPDATE_STATISTICS 选项适用于为索引创建的统计信息、查询谓词中的单列以及使用 CREATE STATISTICS 语句创建的统计信息。 此选项也适用于筛选的统计信息。

默认值为 ON。 建议您对于大多数数据库使用默认设置。

使用 AUTO_UPDATE_STATISTICS_ASYNC 选项可以指定统计信息是同步更新还是异步更新。

OFF     
指定在查询使用统计信息时，查询优化器不更新统计信息。 查询优化器在统计信息可能过期时，也不会更新统计信息。 将此选项设置为 OFF 可能导致并非最佳的查询计划以及查询性能下降。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_auto_update_stats_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAutoUpdateStatistics` 属性来确定状态。

有关详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md#statistics-options)中的“统计信息选项”部分。

<a name="auto_update_statistics_async"></a> AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }     
ON     
指定针对 AUTO_UPDATE_STATISTICS 选项的统计信息更新是异步的。 查询优化器不等待统计信息更新完成即编译查询。

除非已将 AUTO_UPDATE_STATISTICS 设置为 ON，否则将此选项设置为 ON 不会产生任何影响。

默认情况下，AUTO_UPDATE_STATISTICS_ASYNC 选项设置为 OFF，并且查询优化器以同步方式更新统计信息。

OFF     
指定针对 AUTO_UPDATE_STATISTICS 选项的统计信息更新是同步的。 查询优化器在编译查询前等待统计信息更新完成。

除非已将 AUTO_UPDATE_STATISTICS 设置为 ON，否则将此选项设置为 OFF 不会产生任何影响。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_auto_update_stats_async_on` 列来确定此选项的状态。

有关描述何时使用同步统计信息更新或异步统计信息更新的详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md#statistics-options)中的“统计信息选项”部分。

<a name="auto_tuning"></a> \<automatic_tuning_option> ::=     
适用于：[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)]

控制[自动优化](../../relational-databases/automatic-tuning/automatic-tuning.md)的自动选项。

AUTOMATIC_TUNING = { AUTO | INHERIT | CUSTOM }     
AUTO     
将自动优化值设置为 AUTO，将应用 Azure 配置默认值进行自动优化。

INHERIT     
使用 INHERIT 值将从父服务器继承默认配置。 如果想要在父服务器上自定义自动优化配置，并让该服务器上的所有数据库继承这些自定义设置，则这特别有用。 请注意，为了使继承顺利完成，需要在数据库上将三个单独优化选项 FORCE_LAST_GOOD_PLAN、CREATE_INDEX 和 DROP_INDEX 设置为 DEFAULT。

CUSTOM     
使用 CUSTOM 值，需要自定义配置数据库上可用的每个自动优化选项。

启用或禁用[自动优化](../../relational-databases/automatic-tuning/automatic-tuning.md)的自动索引管理 `CREATE_INDEX` 选项。

CREATE_INDEX = { DEFAULT | ON | OFF }     
DEFAULT     
从服务器继承默认设置。 本例中，在服务器级别定义了启用或禁用单个“自动优化”功能的选项。

ON     
启用时，将自动生成数据库上缺失的索引。 在索引创建之后，已验证工作负荷的性能提升。 此类创建的索引不再能够提升工作负荷性能时，会自动将其还原。 自动创建的索引将标记为系统生成的索引。

OFF     
不自动生成数据库上缺失的索引。

启用或禁用[自动优化](../../relational-databases/automatic-tuning/automatic-tuning.md)的自动索引管理 `DROP_INDEX` 选项。

DROP_INDEX = { DEFAULT | ON | OFF }     
DEFAULT     
从服务器继承默认设置。 本例中，在服务器级别定义了启用或禁用单个“自动优化”功能的选项。

ON     
自动删除重复或对性能工作负荷而言不再有用的索引。

OFF     
不自动删除数据库上缺失的索引。

启用或禁用[自动优化](../../relational-databases/automatic-tuning/automatic-tuning.md)的自动计划校正 `FORCE_LAST_GOOD_PLAN` 选项。

FORCE_LAST_GOOD_PLAN = { DEFAULT | ON | OFF }     
DEFAULT     
从服务器继承默认设置。 本例中，在服务器级别定义了启用或禁用单个“自动优化”功能的选项。

ON     
[!INCLUDE[ssde_md](../../includes/ssde_md.md)] 在新查询计划导致性能回归的 [!INCLUDE[tsql-md](../../includes/tsql-md.md)] 查询中自动强制执行上一个已知完好的计划。 [!INCLUDE[ssde_md](../../includes/ssde_md.md)]通过该强制计划持续监视 [!INCLUDE[tsql-md](../../includes/tsql-md.md)] 查询的查询性能。 如果性能有所提升，[!INCLUDE[ssde_md](../../includes/ssde_md.md)]将继续使用上一个已知完好的计划。 如果未检测到性能提升，[!INCLUDE[ssde_md](../../includes/ssde_md.md)]将生成新的查询计划。 如果查询存储未启用或者不处于读写模式，该语句将失败。

OFF     
[!INCLUDE[ssde_md](../../includes/ssde_md.md)]报告由 [sys.dm_db_tuning_recommendations](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md) 视图中的查询计划更改引起的潜在查询性能回归。 但是，不会自动应用这些建议。 用户可以通过应用视图中显示的 [!INCLUDE[tsql-md](../../includes/tsql-md.md)] 脚本来监视正在应用的建议和修复已识别的问题。 这是默认值。

\<change_tracking_option> ::=     
控制更改跟踪选项。 可以启用更改跟踪、设置选项、更改选项以及禁用更改跟踪。 有关示例，请参阅本文后面的“示例”一节。

ON     
对数据库启用更改跟踪。 启用更改跟踪时，还可以设置 AUTO CLEANUP 和 CHANGE RETENTION 选项。

AUTO_CLEANUP = { ON | OFF }     
ON     
在经过指定的保持期后会自动删除更改跟踪信息。

OFF     
不会从数据库中删除更改跟踪数据。

CHANGE_RETENTION =retention_period { DAYS | HOURS | MINUTES }     
指定在数据库中保留更改跟踪信息的最短期限。 只有在 AUTO_CLEANUP 值为 ON 时，才会删除数据。

*retention_period* 是一个整数，用于指定保留期的数值部分。

默认保持期为 2 天。 最短保持期为 1 分钟。 默认保留类型为 DAYS。

OFF     
对数据库禁用更改跟踪。 先对所有表禁用更改跟踪，然后才能对数据库禁用更改跟踪。

\<cursor_option> ::=     
控制游标选项。

CURSOR_CLOSE_ON_COMMIT { ON | OFF }     
ON     
在提交或回滚事务时打开的所有游标都会关闭。

OFF     
在提交事务时游标保持打开状态；回滚事务则会关闭除了定义为 INSENSITIVE 或 STATIC 的游标以外的所有游标。

连接级设置（使用 SET 语句设置）覆盖 CURSOR_CLOSE_ON_COMMIT 的默认数据库设置。 默认情况下，ODBC 和 OLE DB 客户端会发出连接级别的 SET 语句，将会话的 CURSOR_CLOSE_ON_COMMIT 设置为 OFF。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md)。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_cursor_close_on_commit_on` 列或 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsCloseCursorsOnCommitEnabled` 属性来确定此选项的状态。 该游标仅在断开连接时才被隐式释放。 有关详细信息，请参阅 [DECLARE CURSOR](../../t-sql/language-elements/declare-cursor-transact-sql.md)。

\<db_encryption_option> ::=     
控制数据库加密状态。

ENCRYPTION { ON | OFF }     
将数据库设置为加密的 (ON) 或未加密的 (OFF)。 有关数据加密的详细信息，请参阅[透明数据加密](../../relational-databases/security/encryption/transparent-data-encryption.md)和[借助 Azure SQL 数据库实现透明数据加密](/azure/azure-sql/database/transparent-data-encryption-tde-overview)。

在数据库级别启用加密时，所有文件组都将进行加密。 任何新的文件组都将继承加密的属性。 如果数据库中的任何文件组设置为 READ ONLY，则数据库加密操作将失败。

可以使用 [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md) 动态管理视图来查看数据库的加密状态。

\<db_update_option> ::=     
控制是否允许更新数据库。

READ_ONLY     
用户可以从数据库读取数据，但不能修改数据库。

> [!NOTE]
> 若要改进查询优化器，请在将数据库设置为 READ_ONLY 之前更新统计信息。 如果在将数据库设置为 READ_ONLY 之后需要其他统计信息，则[!INCLUDE[ssDE](../../includes/ssde-md.md)]将在 tempdb 中创建统计信息。 有关只读数据库的统计信息的详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)。

READ_WRITE     
允许对数据库执行读写操作。

若要更改此状态，您必须对数据库有独占访问权限。 有关详细信息，请参阅 SINGLE_USER 子句。

> [!NOTE]
> 在 [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] 联合数据库中，`SET { READ_ONLY | READ_WRITE }` 处于禁用状态。

\<db_user_access_option> ::=     
控制用户对数据库的访问。

RESTRICTED_USER     
仅允许 `db_owner` 固定数据库角色的成员以及 `dbcreator` 和 `sysadmin` 固定服务器角色的成员连接到数据库，不过对连接数没有限制。 在 ALTER DATABASE 语句的终止子句所指定的时间范围内，所有数据库连接都将被断开。 在数据库转换到 RESTRICTED_USER 状态后，不合格用户所做的连接尝试将被拒绝。 不能使用 SQL 托管实例修改 RESTRICTED_USER。

MULTI_USER     
所有拥有连接到数据库的相应权限的用户，都允许进行连接。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `user_access` 列或 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `UserAccess` 属性来确定此选项的状态。

\<delayed_durability_option> ::=     
控制提交的事务是完全持久事务还是延迟持久事务。

DISABLED     
`SET DISABLED` 之后的所有事务都是完全持久事务。 将忽略在原子块或 commit 语句中设置的任何持续性选项。

ALLOWED     
`SET ALLOWED` 之后的所有事务都是完全持久事务或都是延迟持久事务，具体取决于在原子块或 commit 语句中设置的持续性选项。

FORCED     
`SET FORCED` 之后的所有事务都是延迟持久事务。 将忽略在原子块或 commit 语句中设置的任何持续性选项。

\<PARAMETERIZATION_option> ::=     
控制参数化选项。

PARAMETERIZATION { SIMPLE | FORCED }     
SIMPLE     
查询的参数化是根据数据库的默认行为进行的。

FORCED     
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对数据库中的所有查询进行参数化。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_parameterization_forced` 列确定此选项的当前设置。

<a name="query-store"></a> \<query_store_options> ::=

打开|关闭|清除[全部]     
控制查询存储是否在此数据库中启用，同时控制是否删除查询存储的内容。

ON     
启用查询存储。 ON 是默认值。

OFF     
禁用查询存储。 

> [!NOTE]  
> 无法在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]的单一数据库和弹性池中禁用查询存储。 执行 `ALTER DATABASE [database] SET QUERY_STORE = OFF` 将返回警告 `'QUERY_STORE=OFF' is not supported in this version of SQL Server.`。 

CLEAR     
删除查询存储的内容。

OPERATION_MODE     
描述查询存储的操作模式。 有效值为 READ_ONLY 和 READ_WRITE。 在 READ_WRITE 模式下，查询存储将收集并保留查询计划和运行时执行统计信息。 在 READ_ONLY 模式下，可以从查询存储读取信息，但不会添加新信息。 如果已用尽查询存储的最大分配空间，查询存储的操作模式将更改为 READ_ONLY。

CLEANUP_POLICY     
描述查询存储的数据保留策略。 STALE_QUERY_THRESHOLD_DAYS 可确定查询信息在查询存储中保留的天数。 STALE_QUERY_THRESHOLD_DAYS 的类型为 **bigint**。 默认值为 30。 对于 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] 基本版，默认值为 7 天。

DATA_FLUSH_INTERVAL_SECONDS     
确定写入到查询存储的数据保留到磁盘的频率。 为了优化性能，由查询存储收集的数据应以异步方式写入到磁盘。 通过使用 DATA_FLUSH_INTERVAL_SECONDS 参数，配置此异步传输发生的频率。 DATA_FLUSH_INTERVAL_SECONDS 的类型为 **bigint**。 默认值为 900（15 分钟）。

MAX_STORAGE_SIZE_MB     
确定分配给查询存储的空间。 MAX_STORAGE_SIZE_MB 的类型为 **bigint**。 对于 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] 高级版，默认值为 1 GB，对于 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] 基本版，默认值为 10 MB 。

> [!NOTE]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 上的 `MAX_STORAGE_SIZE_MB` 设置限制为 10,240 MB。 

> [!NOTE]
> 没有严格执行 `MAX_STORAGE_SIZE_MB` 限制。 仅当查询存储将数据写入磁盘时才检查存储大小。 此间隔由 `DATA_FLUSH_INTERVAL_SECONDS` 选项或 [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] 查询存储对话框选项“数据刷新时间间隔”设置。 间隔时间默认值为 900 秒（或 15 分钟）。
> 如果查询存储已违反存储大小检查之间的 `MAX_STORAGE_SIZE_MB` 限制，则它将转换为只读模式。 如果启用了 `SIZE_BASED_CLEANUP_MODE`，则也会触发强制实施 `MAX_STORAGE_SIZE_MB` 限制的清理机制。
> 清除足够的空间后，查询存储模式将自动切换回读写模式。

> [!IMPORTANT]
> 如果你认为工作负载捕获将需要 10 GB 以上的磁盘空间，则可能应重新考虑和优化工作负载以重用查询计划（例如，使用[强制参数化](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)），或调整查询存储配置。    
> 从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始，在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，可以将 `QUERY_CAPTURE_MODE` 设置为“CUSTOM”，以进一步控制查询捕获策略。

INTERVAL_LENGTH_MINUTES     
确定运行时执行统计数据聚合到查询存储中的时间间隔。 为了优化空间使用情况，将在固定时间窗口上聚合运行时统计信息存储中的运行时执行统计信息。 此固定时间窗口使用 INTERVAL_LENGTH_MINUTES 参数进行配置。 INTERVAL_LENGTH_MINUTES 的类型为 bigint。 默认值是 **60** 秒。

SIZE_BASED_CLEANUP_MODE     
控制当数据总量接近最大大小时是否自动激活清除。

OFF     
不自动激活基于大小的清除。

AUTO     
AUTO 当磁盘上的大小达到 max_storage_size_mb 的 90% 时，将自动激活基于大小的清除。 基于大小的清除首先会删除成本最低和最旧的查询。 它在达到 **max_storage_size_mb** 的大约 80% 时停止。 这是默认的配置值。

SIZE_BASED_CLEANUP_MODE 的类型为 **nvarchar**。

QUERY_CAPTURE_MODE { ALL \| AUTO \| CUSTOM \| NONE }     
指定当前处于活动状态的查询捕获模式。 每个模式都定义了特定的查询捕获策略。   

> [!NOTE]
> 当查询捕获模式设置为“全部”、“自动”或“自定义”时，始终捕获游标、存储过程中的查询和本机编译的查询。

ALL     
捕获所有查询。

AUTO     
根据执行计数和资源消耗捕获相关查询。 这是 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 的默认配置值。

无     
停止捕获新查询。 查询存储将继续为已经捕获的查询收集编译和运行时统计信息。 请谨慎使用此配置，因为你可能会错过捕获重要的查询。

CUSTOM     
可控制 QUERY_CAPTURE_POLICY 选项。

QUERY_CAPTURE_MODE 的类型为 **nvarchar**。

max_plans_per_query     
定义为每个查询保留的最大计划数。 MAX_PLANS_PER_QUERY 的类型为 int。默认值为 200。

WAIT_STATS_CAPTURE_MODE { ON | OFF }     
控制是否按查询捕获等待统计信息。

ON    
捕获每个查询的等待统计信息。 此值是默认配置值。

OFF    
不捕获每个查询的等待统计信息。

\<query_capture_policy_option_list> :: =     
控制查询存储捕获策略选项。 除 STALE_CAPTURE_POLICY_THRESHOLD 外，这些选项定义 OR 条件，需要满足这些条件，才能在定义的“过时捕获策略阈值”中捕获查询。

STALE_CAPTURE_POLICY_THRESHOLD = *number* { DAYS | HOURS }     
定义评估间隔时段以确定是否应捕获查询。 默认值为 1 天，可以设置为 1 小时到 7 天。 number 的类型为 int。

EXECUTION_COUNT     
定义在评估期间执行查询的次数。 默认值为 30，这意味着对于默认的过时捕获策略阈值，查询必须在一天内至少执行 30 次才能在查询存储中保留。 EXECUTION_COUNT 的类型为 int。

TOTAL_COMPILE_CPU_TIME_MS     
定义查询在评估期间使用的总编译 CPU 时间。 默认值为 1000，这意味着对于默认的过时捕获策略阈值，查询必须在一天内在查询编译期间总共花费至少一秒钟的 CPU 时间，才能持久存储在查询存储中。 TOTAL_COMPILE_CPU_TIME_MS 的类型为 int。

TOTAL_EXECUTION_CPU_TIME_MS     
定义查询在评估期间使用的总执行 CPU 时间。 默认值为 100，这意味着对于默认的过时捕获策略阈值，查询必须在一天内在执行期间总共花费至少100 ms 的 CPU 时间，才能持久存储在查询存储中。 TOTAL_EXECUTION_CPU_TIME_MS 的类型为 int。

\<snapshot_option> ::=     
确定事务隔离级别。

ALLOW_SNAPSHOT_ISOLATION { ON | OFF }     
ON     
在数据库级别启用快照选项。 启用该选项后，DML 语句将开始生成行版本，即使没有事务使用快照隔离也是如此。 一旦启用此选项，事务即可指定 SNAPSHOT 事务隔离级别。 当事务在 SNAPSHOT 隔离级别运行时，所有的语句都将数据快照视为位于事务的开头。 如果在 SNAPSHOT 隔离级别运行的事务要访问多个数据库中的数据，则必须将所有数据库中的 ALLOW_SNAPSHOT_ISOLATION 都设置为 ON，或者事务中的每个语句都必须对 FROM 子句中的所有引用（引用 ALLOW_SNAPSHOT_ISOLATION 设置为 OFF 的数据库中的表）使用锁提示。

OFF     
在数据库级别禁用快照选项。 事务不能指定 SNAPSHOT 事务隔离级别。

在将 ALLOW_SNAPSHOT_ISOLATION 设置为新状态（从 ON 设置为 OFF，或从 OFF 设置为 ON）时，在数据库中的所有现有事务均已提交之前，ALTER DATABASE 不会将控制权返回给调用方。 如果数据库已处于 ALTER DATABASE 语句所指定的状态，则控制权会立刻返回给调用方。 如果 ALTER DATABASE 语句未迅速返回，请使用 [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) 确定是否存在长期运行的事务。 如果 ALTER DATABASE 语句被取消，则数据库仍保持 ALTER DATABASE 开始时所处的状态。 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图指示数据库中快照隔离事务的状态。 如果 **snapshot_isolation_state_desc** = IN_TRANSITION_TO_ON，ALTER DATABASE ALLOW_SNAPSHOT_ISOLATION OFF 将暂停六秒钟，然后重试操作。

如果数据库处于 OFFLINE 状态，则不能更改 ALLOW_SNAPSHOT_ISOLATION 的状态。

如果在 READ_ONLY 数据库中设置 ALLOW_SNAPSHOT_ISOLATION，则以后将数据库设置为 READ_WRITE 时，仍将保留该设置。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `snapshot_isolation_state` 列确定此选项的当前设置。

READ_COMMITTED_SNAPSHOT { ON | OFF }     
ON     
在数据库级别启用已提交读快照选项。 启用该选项后，DML 语句将开始生成行版本，即使没有事务使用快照隔离也是如此。 启用此选项后，指定 READ COMMITTED 隔离级别的事务将使用行版本控制而不是锁定。 当事务在 READ COMMITTED 隔离级别运行时，所有的语句都将数据快照视为位于语句的开头。

OFF     
在数据库级别禁用已提交读快照选项。 指定 READ COMMITTED 隔离级别的事务使用锁定。

若要将 READ_COMMITTED_SNAPSHOT 设置为 ON 或 OFF，不应存在任何活动的数据库连接，运行 ALTER DATABASE 命令的连接除外。 但是，数据库不必一定要处于单用户模式下。 当数据库处于 OFFLINE 状态时，不能更改此选项的状态。

如果在 READ_ONLY 数据库中设置 READ_COMMITTED_SNAPSHOT，则以后将数据库设置为 READ_WRITE 时，仍将保留该设置。

对于 master、tempdb 或 msdb 系统数据库，不能将 READ_COMMITTED_SNAPSHOT 设置为 ON。 如果为 model 更改该设置，则该设置会成为除 tempdb 以外的所有新建数据库的默认设置。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_read_committed_snapshot_on` 列确定此选项的当前设置。

> [!WARNING]
> 如果使用 `DURABILITY = SCHEMA_ONLY` 创建表，并随后使用 `ALTER DATABASE` 对 READ_COMMITTED_SNAPSHOT 进行更改，则表中的数据会丢失。

MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT { ON | OFF }     
ON     
当事务隔离级别设置为任何低于 SNAPSHOT 的隔离级别时，内存优化表中所有经过解释的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 操作将在 SNAPSHOT 隔离下运行。 低于快照的隔离级别示例有 READ COMMITTED 或 READ UNCOMMITTED。 无论是在会话级别显式设置事务隔离级别还是隐式使用默认值，这些操作都会运行。

OFF     
不提升内存优化表中经过解释的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 操作的事务隔离级别。

如果数据库处于 OFFLINE 状态，不能更改 MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT 的状态。

默认值为 OFF。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_memory_optimized_elevate_to_snapshot_on` 列确定此选项的当前设置。

\<sql_option> ::=     
在数据库级别控制 ANSI 遵从选项。

ANSI_NULL_DEFAULT { ON | OFF }     
确定在 CREATE TABLE 或 ALTER TABLE 语句中未显式定义为 Null 性的列或 [CLR 用户定义类型](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)的默认值（NULL 或 NOT NULL）。 无论此设置如何，使用约束定义的列都将遵循约束规则。

ON     
默认值为 NULL。

OFF     
默认值为 NOT NULL。

连接级设置（使用 SET 语句设置）覆盖 ANSI_NULL_DEFAULT 的默认数据库级别设置。 默认情况下，ODBC 和 OLE DB 客户端发出连接级 SET 语句，将会话的 ANSI_NULL_DEFAULT 设置为 ON。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md)。

对于 ANSI 兼容性，数据库选项 ANSI_NULL_DEFAULT 设置为 ON 将使数据库默认设置改为 NULL。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_ansi_null_default_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAnsiNullDefault` 属性来确定状态。

ANSI_NULLS { ON | OFF }     
ON     
与 Null 值的所有比较的结果均为 UNKNOWN。

OFF     
将非 UNICODE 值与 Null 值比较时，如果这两个值都为 NULL，则结果为 TRUE。

> [!IMPORTANT]
> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中，ANSI_NULLS 将始终为 ON，将该选项显式设置为 OFF 的任何应用程序都将产生错误。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。

连接级设置（使用 SET 语句设置）覆盖 ANSI_NULLS 的默认数据库设置。 默认情况下，ODBC 和 OLE DB 客户端发出连接级 SET 语句，将会话的 ANSI_NULLS 设置为 ON。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md)。

> [!NOTE]
> 创建或更改计算列或索引视图的索引时，SET ANSI_NULLS 必须设置为 ON。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_ansi_nulls_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAnsiNullsEnabled` 属性来确定状态。

ANSI_PADDING { ON | OFF }     
ON     
在进行转换之前，将字符串填充到同一长度。 在插入到 varchar 或 nvarchar 数据类型之前，也填充到同一长度。

OFF     
将字符值中的尾随空格插入 varchar 或 nvarchar 列中。 也保留插入 varbinary 列中的二进制值的尾随零。 不将值填充到列的长度。

如果指定了 OFF，该设置只影响新列的定义。

> [!IMPORTANT]
> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中，ANSI_PADDING 将始终为 ON，将该选项显式设置为 OFF 的任何应用程序都将产生错误。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 建议始终将 ANSI_PADDING 设置为 ON。 创建或操作计算列或索引视图的索引时，ANSI_PADDING 必须为 ON。

当 ANSI_PADDING 设置为 ON 时，会将允许为 Null 值的 char(_n_) 和 binary(_n_) 列填充到列长度。 当 ANSI_PADDING 为 OFF 时，会剪裁尾随空格和零。 始终将不允许为 Null 值的 char(_n_) 和 binary(_n_) 列填充到列长度 。

连接级设置（使用 SET 语句设置）覆盖 ANSI_PADDING 的默认数据库级别设置。 默认情况下，ODBC 和 OLE DB 客户端发出连接级 SET 语句，将会话的 ANSI_PADDING 设置为 ON。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md)。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_ansi_padding_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAnsiPaddingEnabled` 属性来确定状态。

ANSI_WARNINGS { ON | OFF }     
ON     
当出现被零除这类情况时，将发出错误或警告。 当聚合函数中出现 Null 值时，也会发出错误和警告。

OFF     
出现被零除等情况时不会引发警告，而是返回 Null 值。

> [!NOTE]
> 创建或更改计算列或索引视图的索引时，SET ANSI_WARNINGS 必须设置为 ON。

连接级设置（使用 SET 语句设置）覆盖 ANSI_WARNINGS 的默认数据库设置。 默认情况下，ODBC 和 OLE DB 客户端发出连接级 SET 语句，将会话的 ANSI_WARNINGS 设置为 ON。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md)。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_ansi_warnings_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAnsiWarningsEnabled` 属性来确定状态。

ARITHABORT { ON | OFF }     
ON     
在查询执行过程中出现溢出或被零除等错误时，结束查询。

OFF     
在出现其中一个错误时显示警告消息。 即使显示警告，查询、批处理或事务也会继续进行处理，就像没有发生错误一样。

> [!NOTE]
> 创建或更改计算列或索引视图的索引时，SET ARITHABORT 必须设置为 ON。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_arithabort_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsArithmeticAbortEnabled` 属性来确定状态。

COMPATIBILITY_LEVEL = { 150 \| 140 \| 130 \| 120 \| 110 \| 100 }     
有关详细信息，请参阅 [ALTER DATABASE 兼容级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。

CONCAT_NULL_YIELDS_NULL { ON | OFF }     
ON     
当串联运算的两个操作数中任意一个为 NULL 时，结果也为 NULL。 例如，将字符串“This is”和 NULL 串联将得到 NULL 值，而不是值“This is”。

OFF     
Null 值被视为空字符串进行处理。

> [!NOTE]
> 创建或更改计算列或索引视图的索引时，CONCAT_NULL_YIELDS_NULL 必须设置为 ON。
>
> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中，CONCAT_NULL_YIELDS_NULL 将始终为 ON，而且将该选项显式设置为 OFF 的任何应用程序都将产生一个错误。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。

连接级设置（使用 SET 语句设置）覆盖 CONCAT_NULL_YIELDS_NULL 的默认数据库设置。 默认情况下，当连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时，ODBC 和 OLE DB 客户端发出连接级别 SET 语句，将会话的 CONCAT_NULL_YIELDS_NULL 设置为 ON。 有关详细信息，请参阅 [SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_concat_null_yields_null_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsNullConcat` 属性来确定状态。

QUOTED_IDENTIFIER { ON | OFF }     
ON     
可以将分隔标识符包含在双引号中。

所有用双引号分隔的字符串都被解释为对象标识符。 加引号的标识符不必遵守 [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符规则。 它们可以是关键字，并且可以包含 [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符中不允许的字符。 如果单引号 (') 是文字字符串的一部分，则可以用双引号 (") 表示它。

OFF     
标识符不能包含在引号中，而且必须遵循所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符规则。 文字可以由单引号或双引号分隔。

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 还允许使用方括号 ([ ]) 分隔标识符。 无论 QUOTED_IDENTIFIER 设置如何，始终都可以使用用方括号括起来的标识符。 有关详细信息，请参阅 [Database Identifiers](../../relational-databases/databases/database-identifiers.md)。

创建表后，QUOTED IDENTIFIER 选项在表的元数据中始终存储为 ON。 即使在创建表时将该选项设置为 OFF 也会存储该选项。

连接级设置（使用 SET 语句设置）覆盖 QUOTED_IDENTIFIER 的默认数据库设置。 默认情况下，ODBC 和 OLE DB 客户端发出连接级 SET 语句，将 QUOTED_IDENTIFIER 设置为 ON。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md)。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_quoted_identifier_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsQuotedIdentifiersEnabled` 属性来确定状态。

NUMERIC_ROUNDABORT { ON | OFF }     
ON     
当表达式中发生精度损失时生成错误。

OFF     
精度的降低不会生成错误消息，会根据存储结果的列或变量的精度，将结果舍入。

> [!IMPORTANT]
> 创建或更改计算列或索引视图的索引时，NUMERIC_ROUNDABORT 必须设置为 OFF。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_numeric_roundabort_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsNumericRoundAbortEnabled` 属性来确定状态。

RECURSIVE_TRIGGERS { ON | OFF }     
ON     
允许递归激发 AFTER 触发器。

OFF     
可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_recursive_triggers_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsRecursiveTriggersEnabled` 属性来确定状态。

> [!NOTE]
> 当 RECURSIVE_TRIGGERS 设置为 OFF 时，只禁止直接递归触发。 若要禁用间接递归触发，还必须将 nested triggers 服务器选项设置为 0。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_recursive_triggers_on` 列或 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsRecursiveTriggersEnabled` 属性来确定此选项的状态。

\<target_recovery_time_option> ::=     
指定每个数据库上间接检查点的频率。 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始，新数据库的默认值为 1 分钟，表示数据库使用间接检查点。 较旧版本的默认值为 0，表示数据库使用自动检查点，其频率依赖于服务器实例的恢复间隔设置。 对于大多数系统，[!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议设置为 1 分钟。

TARGET_RECOVERY_TIME = target_recovery_time { SECONDS | MINUTES }     
*target_recovery_time*     
指定在发生崩溃的情况下恢复指定数据库的最大上限时间。

SECONDS     
指示 *target_recovery_time* 表示为秒数。

MINUTES     
指示 *target_recovery_time* 表示为分钟数。

有关间接检查点的详细信息，请参阅[数据库检查点](../../relational-databases/logs/database-checkpoints-sql-server.md)。

WITH \<termination> ::=     
指定当数据库从一种状态转换到另一种状态时，何时回滚未完成的事务。 如果终止子句被忽略，则当数据库中存在任何锁时，ALTER DATABASE 语句将无限期等待。 只能指定一条终止子句，而且该子句应跟在 SET 子句后面。

> [!NOTE]
> 并非所有数据库选项都使用 WITH \<termination> 子句。 有关详细信息，请参阅本文的“备注”部分中的[设置选项](#SettingOptions)下面的表。

ROLLBACK AFTER *integer* [SECONDS] | ROLLBACK IMMEDIATE     
指定是在指定秒数之后回滚还是立即回滚。

NO_WAIT     
指定：如果请求的数据库状态或选项更改无法立即完成，则请求失败。 立即完成意味着不会等待事务自己提交或回滚。

## <a name="setting-options"></a><a name="SettingOptions"></a> 设置选项

若要检索数据库选项的当前设置，请使用 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图或 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)

设置数据库选项后，新的设置将立即生效。

可以为所有新建数据库更改任意一个数据库选项的默认值。 为此，请更改 model 数据库中的相应数据库选项。

并非所有数据库选项都使用 WITH \<termination> 子句，也不是所有数据库选项都能结合其他选项指定。 下表列出这些选项以及它们的选项和终止状态。

|选项类别|可与其他选项一起指定|可以使用 WITH \<termination> 子句|
|----------------------|-----------------------------------------|---------------------------------------------|
|\<auto_option>|是|否|
|\<change_tracking_option>|是|是|
|\<cursor_option>|是|否|
|\<db_encryption_option>|是|否|
|\<db_update_option>|是|是|
|\<db_user_access_option>|是|是|
|\<delayed_durability_option>|是|是|
|\<parameterization_option>|是|是|
|ALLOW_SNAPSHOT_ISOLATION|否|否|
|READ_COMMITTED_SNAPSHOT|否|是|
|MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT|是|是|
|DATE_CORRELATION_OPTIMIZATION|是|是|
|\<sql_option>|是|否|
|\<target_recovery_time_option>|否|是|

## <a name="examples"></a>示例

### <a name="a-setting-the-database-to-read_only"></a>A. 将数据库设置为 READ_ONLY
将数据库或文件组的状态改为 READ_ONLY 或 READ_WRITE 需要具有数据库的专有访问权限。 下面的示例将数据库设置为 `RESTRICTED_USER` 模式，以限制访问。 然后，该示例将 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的状态设置为 `READ_ONLY` ，并将对数据库的访问权返回给所有用户。

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET RESTRICTED_USER;
GO
ALTER DATABASE [database_name]
SET READ_ONLY
GO
ALTER DATABASE [database_name]
SET MULTI_USER;
GO

```

### <a name="b-enabling-snapshot-isolation-on-a-database"></a>B. 对数据库启用快照隔离
下面的示例为 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库启用快照隔离框架选项。

```sql
USE [database_name];
USE master;
GO
ALTER DATABASE [database_name]
SET ALLOW_SNAPSHOT_ISOLATION ON;
GO
-- Check the state of the snapshot_isolation_framework
-- in the database.
SELECT name, snapshot_isolation_state,
    snapshot_isolation_state_desc AS description
FROM sys.databases
WHERE name = N'[database_name]';
GO
```

结果集显示快照隔离框架已启用。

|name |snapshot_isolation_state |description|
|-------------------- |------------------------|----------|
|[database_name] |1| ON |

### <a name="c-enabling-modifying-and-disabling-change-tracking"></a>C. 启用、修改和禁用更改跟踪
下面的示例对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库启用更改跟踪并将保持期设置为 `2` 天。

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = ON
(AUTO_CLEANUP = ON, CHANGE_RETENTION = 2 DAYS);
```

下面的示例说明如何将保持期更改为 3 天。

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING (CHANGE_RETENTION = 3 DAYS);
```

下面的示例说明如何对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库禁用更改跟踪。

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = OFF;
```

### <a name="d-enabling-the-query-store"></a>D. 启用查询存储
下面的示例启用查询存储并配置查询存储参数。

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

### <a name="e-enabling-the-query-store-with-wait-statistics"></a>E. 使用等待统计信息启用查询存储
下面的示例启用查询存储并配置其参数。

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
    );
```

### <a name="f-enabling-the-query-store-with-custom-capture-policy-options"></a>F. 使用自定义捕获策略选项启用查询存储
下面的示例启用查询存储并配置其参数。

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
      QUERY_CAPTURE_MODE = CUSTOM,
      QUERY_CAPTURE_POLICY = (
        STALE_CAPTURE_POLICY_THRESHOLD = 24 HOURS,
        EXECUTION_COUNT = 30,
        TOTAL_COMPILE_CPU_TIME_MS = 1000,
        TOTAL_EXECUTION_CPU_TIME_MS = 100
      )
    );
```

## <a name="see-also"></a>另请参阅

- [ALTER DATABASE 兼容级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)
- [ALTER DATABASE 数据库镜像](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)
- [统计信息](../../relational-databases/statistics/statistics.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [启用和禁用更改跟踪](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [Query Store 最佳实践](../../relational-databases/performance/best-practice-with-the-query-store.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-set-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 数据库](alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL 托管实例 \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL 托管实例

兼容性级别是 `SET` 选项，但在 [ALTER DATABASE 兼容性级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)中进行了说明。

> [!NOTE]
> 可以使用 [SET Statements](../../t-sql/statements/set-statements-transact-sql.md) 来为当前会话配置很多数据库 SET 选项，当它们连接时通常通过应用程序来配置。 会话级 SET 选项覆盖 ALTER DATABASE SET 值。 下面各节中所述的数据库选项是你可以为未明确提供其他 SET 选项值的会话设置的值。

## <a name="syntax"></a>语法

```syntaxsql
ALTER DATABASE { database_name | Current }
SET
{
    <optionspec> [ ,...n ]
}
;

<optionspec> ::=
{
    <auto_option>
  | <change_tracking_option>
  | <cursor_option>
  | <db_encryption_option>
  | <delayed_durability_option>
  | <parameterization_option>
  | <query_store_options>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
}
;
<auto_option> ::=
{
    AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] }
  | AUTO_SHRINK { ON | OFF }
  | AUTO_UPDATE_STATISTICS { ON | OFF }
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<automatic_tuning_option> ::=
{
    AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = { ON | OFF } )
}

<change_tracking_option> ::=
{
    CHANGE_TRACKING
    {
       = OFF
     | = ON [ ( <change_tracking_option_list > [,...n] ) ]
     | ( <change_tracking_option_list> [,...n] )
    }
}

<change_tracking_option_list> ::=
   {
       AUTO_CLEANUP = { ON | OFF }
     | CHANGE_RETENTION = retention_period { DAYS | HOURS | MINUTES }
   }

<cursor_option> ::=
{
    CURSOR_CLOSE_ON_COMMIT { ON | OFF }
}

<db_encryption_option> ::=
  ENCRYPTION { ON | OFF }

<delayed_durability_option> ::=DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }

<parameterization_option> ::=
  PARAMETERIZATION { SIMPLE | FORCED }

<query_store_options> ::=
{
  QUERY_STORE
  {
    = OFF
    | = ON [ ( <query_store_option_list> [,... n] ) ]
    | ( < query_store_option_list> [,... n] )
    | CLEAR [ ALL ]
  }
}

<query_store_option_list> ::=
{
  OPERATION_MODE = { READ_WRITE | READ_ONLY }
  | CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = number )
  | DATA_FLUSH_INTERVAL_SECONDS = number
  | MAX_STORAGE_SIZE_MB = number
  | INTERVAL_LENGTH_MINUTES = number
  | SIZE_BASED_CLEANUP_MODE = { AUTO | OFF }
  | QUERY_CAPTURE_MODE = { ALL | AUTO | CUSTOM | NONE }
  | MAX_PLANS_PER_QUERY = number
  | WAIT_STATS_CAPTURE_MODE = { ON | OFF }
  | QUERY_CAPTURE_POLICY = ( <query_capture_policy_option_list> [,...n] )
}

<query_capture_policy_option_list> :: =
{
    STALE_CAPTURE_POLICY_THRESHOLD = number { DAYS | HOURS }
    | EXECUTION_COUNT = number
    | TOTAL_COMPILE_CPU_TIME_MS = number
    | TOTAL_EXECUTION_CPU_TIME_MS = number
}

<snapshot_option> ::=
{
    ALLOW_SNAPSHOT_ISOLATION { ON | OFF }
  | READ_COMMITTED_SNAPSHOT {ON | OFF }
  | MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT {ON | OFF }
}
<sql_option> ::=
{
    ANSI_NULL_DEFAULT { ON | OFF }
  | ANSI_NULLS { ON | OFF }
  | ANSI_PADDING { ON | OFF }
  | ANSI_WARNINGS { ON | OFF }
  | ARITHABORT { ON | OFF }
  | COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }
  | CONCAT_NULL_YIELDS_NULL { ON | OFF }
  | NUMERIC_ROUNDABORT { ON | OFF }
  | QUOTED_IDENTIFIER { ON | OFF }
  | RECURSIVE_TRIGGERS { ON | OFF }
}

<temporal_history_retention>::= TEMPORAL_HISTORY_RETENTION { ON | OFF }
```

## <a name="arguments"></a>参数

database_name     
要修改的数据库的名称。

CURRENT     
`CURRENT` 运行当前数据库中的操作。 并不是所有上下文中的所有选项都支持 `CURRENT`。 如果 `CURRENT` 失败，则提供数据库名称。

\<auto_option> ::=     
控制自动选项。

<a name="auto_create_statistics"></a> AUTO_CREATE_STATISTICS { ON | OFF }     
ON     
查询优化器根据需要在查询谓词中的单列上创建统计信息，以便改进查询计划和查询性能。 在查询优化器编译查询时创建这些单列统计信息。 这些单列统计信息只在尚不是现有统计信息对象的第一列的列上创建。

默认值为 ON。 建议您对于大多数数据库使用默认设置。

OFF     
查询优化器在编译查询时不在查询谓词中的单列上创建统计信息。 将此选项设置为 OFF 可能导致并非最佳的查询计划以及查询性能下降。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_auto_create_stats_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAutoCreateStatistics` 属性来确定状态。

有关详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md#statistics-options)中的“统计信息选项”部分。

INCREMENTAL = ON | OFF     
将 AUTO_CREATE_STATISTICS 设置为 ON，并将 INCREMENTAL 设置为 ON。 只要支持增量统计信息，此设置便会自动创建增量统计信息。 默认值为 OFF。 有关详细信息，请参阅 [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md)。

<a name="auto_shrink"></a> AUTO_SHRINK { ON | OFF }     
ON     
数据库文件是定期收缩的候选项。

数据文件和日志文件都可以自动收缩。 只有在将数据库设置为 SIMPLE 恢复模式时，或备份事务日志时，AUTO_SHRINK 才可减小事务日志的大小。 当设置为 OFF 时，在定期检查未使用空间的过程中，数据库文件不自动收缩。

当文件中超过百分之二十五的部分包含未使用的空间时，AUTO_SHRINK 选项将导致收缩文件。 该选项会导致文件收缩为两种大小之一。 它会收缩为其中较大的大小：

- 其中 25% 的文件不包含任何内容时的大小
- 文件创建时的大小

不能收缩只读数据库。

OFF     
在定期检查未使用空间时不会自动收缩数据库文件。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_auto_shrink_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAutoShrink` 属性来确定状态。

> [!NOTE]
> AUTO_SHRINK 选项在包含数据库中不可用。

<a name="auto_update_statistics"></a> AUTO_UPDATE_STATISTICS { ON | OFF }     
ON     
指定在统计信息由查询使用并且可能过期时，查询优化器更新统计信息。 统计信息将在插入、更新、删除或合并操作更改表或索引视图中的数据分布后过期。 查询优化器通过计算自最后统计信息更新后数据修改的次数并且将这一修改次数与某一阈值进行比较，确定统计信息何时可能过期。 该阈值基于表中或索引视图中的行数。

查询优化器在编译查询和运行缓存查询计划前，检查是否存在过期的统计信息。 查询优化器使用查询谓词中的列、表和索引视图确定哪些统计信息可能过期。 查询优化器在编译查询之前确定此信息。 在运行缓存查询计划前，[!INCLUDE[ssDE](../../includes/ssde-md.md)] 确认该查询计划引用最新的统计信息。

AUTO_UPDATE_STATISTICS 选项适用于为索引创建的统计信息、查询谓词中的单列以及使用 CREATE STATISTICS 语句创建的统计信息。 此选项也适用于筛选的统计信息。

默认值为 ON。 建议您对于大多数数据库使用默认设置。

使用 AUTO_UPDATE_STATISTICS_ASYNC 选项可以指定统计信息是同步更新还是异步更新。

OFF     
指定在查询使用统计信息时，查询优化器不更新统计信息。 查询优化器在统计信息可能过期时，也不会更新统计信息。 将此选项设置为 OFF 可能导致并非最佳的查询计划以及查询性能下降。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 is_auto_update_stats_on 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAutoUpdateStatistics` 属性来确定状态。

有关详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)中的“使用数据库范围的统计信息选项”部分。

<a name="auto_update_statistics_async"></a> AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }     
ON     
指定针对 AUTO_UPDATE_STATISTICS 选项的统计信息更新是异步的。 查询优化器不等待统计信息更新完成即编译查询。

除非已将 AUTO_UPDATE_STATISTICS 设置为 ON，否则将此选项设置为 ON 不会产生任何影响。

默认情况下，AUTO_UPDATE_STATISTICS_ASYNC 选项设置为 OFF，并且查询优化器以同步方式更新统计信息。

OFF     
指定针对 AUTO_UPDATE_STATISTICS 选项的统计信息更新是同步的。 查询优化器在编译查询前等待统计信息更新完成。

除非已将 AUTO_UPDATE_STATISTICS 设置为 ON，否则将此选项设置为 OFF 不会产生任何影响。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 is_auto_update_stats_async_on 列来确定此选项的状态。

有关描述何时使用同步统计信息更新或异步统计信息更新的详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)中的“使用数据库范围的统计信息选项”部分。

<a name="auto_tuning"></a> \<automatic_tuning_option> ::=     
适用于：[!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)]

启用或禁用 `FORCE_LAST_GOOD_PLAN` [自动优化](../../relational-databases/automatic-tuning/automatic-tuning.md)选项。

FORCE_LAST_GOOD_PLAN = { ON | OFF }     
ON     
[!INCLUDE[ssde_md](../../includes/ssde_md.md)] 在新查询计划导致性能回归的 [!INCLUDE[tsql-md](../../includes/tsql-md.md)] 查询中自动强制执行上一个已知完好的计划。 [!INCLUDE[ssde_md](../../includes/ssde_md.md)]通过该强制计划持续监视 [!INCLUDE[tsql-md](../../includes/tsql-md.md)] 查询的查询性能。 如果性能有所提升，[!INCLUDE[ssde_md](../../includes/ssde_md.md)]将继续使用上一个已知完好的计划。 如果未检测到性能提升，[!INCLUDE[ssde_md](../../includes/ssde_md.md)]将生成新的查询计划。 如果查询存储未启用或者不处于读写模式，该语句将失败。

OFF     
[!INCLUDE[ssde_md](../../includes/ssde_md.md)]报告由 [sys.dm_db_tuning_recommendations](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md) 视图中的查询计划更改引起的潜在查询性能回归。 但是，不会自动应用这些建议。 用户可以通过应用视图中显示的 [!INCLUDE[tsql-md](../../includes/tsql-md.md)] 脚本来监视正在应用的建议和修复已识别的问题。 这是默认值。

\<change_tracking_option> ::=     
控制更改跟踪选项。 可以启用更改跟踪、设置选项、更改选项以及禁用更改跟踪。 有关示例，请参阅本文后面的“示例”一节。

ON 对数据库启用更改跟踪。 启用更改跟踪时，还可以设置 AUTO CLEANUP 和 CHANGE RETENTION 选项。

AUTO_CLEANUP = { ON | OFF }     
ON     
在经过指定的保持期后会自动删除更改跟踪信息。

OFF     
不会从数据库中删除更改跟踪数据。

CHANGE_RETENTION =retention_period { DAYS | HOURS | MINUTES }     
指定在数据库中保留更改跟踪信息的最短期限。 只有在 AUTO_CLEANUP 值为 ON 时，才会删除数据。

*retention_period* 是一个整数，用于指定保留期的数值部分。

默认保持期为 2 天。 最短保持期为 1 分钟。 默认保留类型为 DAYS。

OFF     
对数据库禁用更改跟踪。 先对所有表禁用更改跟踪，然后才能对数据库禁用更改跟踪。

\<cursor_option> ::=     
控制游标选项。

CURSOR_CLOSE_ON_COMMIT { ON | OFF }     
ON     
在提交或回滚事务时打开的所有游标都会关闭。

OFF     
在提交事务时游标保持打开状态；回滚事务则会关闭除了定义为 INSENSITIVE 或 STATIC 的游标以外的所有游标。

连接级设置（使用 SET 语句设置）覆盖 CURSOR_CLOSE_ON_COMMIT 的默认数据库设置。 默认情况下，ODBC 和 OLE DB 客户端会发出连接级别的 SET 语句，将会话的 CURSOR_CLOSE_ON_COMMIT 设置为 OFF。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md)。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 is_cursor_close_on_commit_on 列或 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 IsCloseCursorsOnCommitEnabled 属性来确定此选项的状态。 该游标仅在断开连接时才被隐式释放。 有关详细信息，请参阅 [DECLARE CURSOR](../../t-sql/language-elements/declare-cursor-transact-sql.md)。

\<db_encryption_option> ::=     
控制数据库加密状态。

ENCRYPTION { ON | OFF }     
将数据库设置为加密的 (ON) 或未加密的 (OFF)。 有关数据加密的详细信息，请参阅[透明数据加密](../../relational-databases/security/encryption/transparent-data-encryption.md)和[借助 Azure SQL 数据库实现透明数据加密](/azure/azure-sql/database/transparent-data-encryption-tde-overview)。

在数据库级别启用加密时，所有文件组都将进行加密。 任何新的文件组都将继承加密的属性。 如果数据库中的任何文件组设置为 READ ONLY，则数据库加密操作将失败。

可以使用 [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md) 动态管理视图来查看数据库的加密状态。

\<db_update_option> ::=     
控制是否允许更新数据库。

READ_ONLY     
用户可以从数据库读取数据，但不能修改数据库。

> [!NOTE]
> 若要改进查询优化器，请在将数据库设置为 READ_ONLY 之前更新统计信息。 如果在将数据库设置为 READ_ONLY 之后需要其他统计信息，则[!INCLUDE[ssDE](../../includes/ssde-md.md)]将在 tempdb 中创建统计信息。 有关只读数据库的统计信息的详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)。

READ_WRITE     
允许对数据库执行读写操作。

若要更改此状态，您必须对数据库有独占访问权限。

\<db_user_access_option> ::=     
控制用户对数据库的访问。

RESTRICTED_USER     
仅允许 `db_owner` 固定数据库角色的成员以及 `dbcreator` 和 `sysadmin` 固定服务器角色的成员连接到数据库，不过对连接数没有限制。 在 ALTER DATABASE 语句的终止子句所指定的时间范围内，所有数据库连接都将被断开。 在数据库转换到 RESTRICTED_USER 状态后，不合格用户所做的连接尝试将被拒绝。 不能使用 SQL 托管实例修改 RESTRICTED_USER。

MULTI_USER     
所有拥有连接到数据库的相应权限的用户，都允许进行连接。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 user_access 列或 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `UserAccess` 属性来确定此选项的状态。

\<delayed_durability_option> ::=     
控制提交的事务是完全持久事务还是延迟持久事务。

DISABLED     
`SET DISABLED` 之后的所有事务都是完全持久事务。 将忽略在原子块或 commit 语句中设置的任何持续性选项。

ALLOWED     
`SET ALLOWED` 之后的所有事务都是完全持久事务或都是延迟持久事务，具体取决于在原子块或 commit 语句中设置的持续性选项。

FORCED     
`SET FORCED` 之后的所有事务都是延迟持久事务。 将忽略在原子块或 commit 语句中设置的任何持续性选项。

\<PARAMETERIZATION_option> ::=     
控制参数化选项。

PARAMETERIZATION { SIMPLE | FORCED }     
SIMPLE     
查询的参数化是根据数据库的默认行为进行的。

FORCED     
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对数据库中的所有查询进行参数化。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_parameterization_forced` 列确定此选项的当前设置。

<a name="query-store"></a> \<query_store_options> ::=

打开|关闭|清除[全部]     
控制查询存储是否在此数据库中启用，同时控制是否删除查询存储的内容。

ON     
启用查询存储。

OFF     
禁用查询存储。 这是默认值。

CLEAR     
删除查询存储的内容。

OPERATION_MODE     
描述查询存储的操作模式。 有效值为 READ_ONLY 和 READ_WRITE。 在 READ_WRITE 模式下，查询存储将收集并保留查询计划和运行时执行统计信息。 在 READ_ONLY 模式下，可以从查询存储读取信息，但不会添加新信息。 如果已用尽查询存储的最大分配空间，查询存储的操作模式将更改为 READ_ONLY。

CLEANUP_POLICY     
描述查询存储的数据保留策略。 STALE_QUERY_THRESHOLD_DAYS 可确定查询信息在查询存储中保留的天数。 STALE_QUERY_THRESHOLD_DAYS 的类型为 **bigint**。 默认值为 30。 对于 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] 基本版，默认值为 7 天。

DATA_FLUSH_INTERVAL_SECONDS     
确定写入到查询存储的数据保留到磁盘的频率。 为了优化性能，由查询存储收集的数据应以异步方式写入到磁盘。 通过使用 DATA_FLUSH_INTERVAL_SECONDS 参数，配置此异步传输发生的频率。 DATA_FLUSH_INTERVAL_SECONDS 的类型为 **bigint**。 默认值为 900（15 分钟）。

MAX_STORAGE_SIZE_MB     
确定分配给查询存储的空间。 MAX_STORAGE_SIZE_MB 的类型为 **bigint**。 默认值为 100 MB。

> [!NOTE]
> 没有严格执行 `MAX_STORAGE_SIZE_MB` 限制。 仅当查询存储将数据写入磁盘时才检查存储大小。 此间隔由 `DATA_FLUSH_INTERVAL_SECONDS` 选项或 [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] 查询存储对话框选项“数据刷新时间间隔”设置。 间隔时间默认值为 900 秒（或 15 分钟）。
> 如果查询存储已违反存储大小检查之间的 `MAX_STORAGE_SIZE_MB` 限制，则它将转换为只读模式。 如果启用了 `SIZE_BASED_CLEANUP_MODE`，则也会触发强制实施 `MAX_STORAGE_SIZE_MB` 限制的清理机制。
> 清除足够的空间后，查询存储模式将自动切换回读写模式。

> [!IMPORTANT]
> 如果你认为工作负载捕获将需要 10 GB 以上的磁盘空间，则可能应重新考虑和优化工作负载以重用查询计划（例如，使用[强制参数化](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)），或调整查询存储配置。    
> 从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始，在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，可以将 `QUERY_CAPTURE_MODE` 设置为“CUSTOM”，以进一步控制查询捕获策略。

INTERVAL_LENGTH_MINUTES     
确定运行时执行统计数据聚合到查询存储中的时间间隔。 为了优化空间使用情况，将在固定时间窗口上聚合运行时统计信息存储中的运行时执行统计信息。 此固定时间窗口使用 INTERVAL_LENGTH_MINUTES 参数进行配置。 INTERVAL_LENGTH_MINUTES 的类型为 bigint。 默认值是 **60** 秒。

SIZE_BASED_CLEANUP_MODE     
控制当数据总量接近最大大小时是否自动激活清除。

OFF     
不自动激活基于大小的清除。

AUTO     
AUTO 当磁盘上的大小达到 max_storage_size_mb 的 90% 时，将自动激活基于大小的清除。 基于大小的清除首先会删除成本最低和最旧的查询。 它在达到 **max_storage_size_mb** 的大约 80% 时停止。 这是默认的配置值。

SIZE_BASED_CLEANUP_MODE 的类型为 **nvarchar**。

QUERY_CAPTURE_MODE { ALL \| AUTO \| CUSTOM \| NONE }     
指定当前处于活动状态的查询捕获模式。

ALL     
捕获所有查询。

AUTO     
根据执行计数和资源消耗捕获相关查询。 这是 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 的默认配置值。

无     
停止捕获新查询。 查询存储将继续为已经捕获的查询收集编译和运行时统计信息。 请谨慎使用此配置，因为你可能会错过捕获重要的查询。

QUERY_CAPTURE_MODE 的类型为 **nvarchar**。

max_plans_per_query     
一个整数，表示为每个查询保留的最大计划数。 MAX_PLANS_PER_QUERY 的类型为 int。默认值为 200。

WAIT_STATS_CAPTURE_MODE { ON | OFF }     
控制是否按查询捕获等待统计信息。

ON    
捕获每个查询的等待统计信息。 此值是默认配置值。

OFF    
不捕获每个查询的等待统计信息。

\<query_capture_policy_option_list> :: =     
控制查询存储捕获策略选项。 除 STALE_CAPTURE_POLICY_THRESHOLD 外，这些选项定义 OR 条件，需要满足这些条件，才能在定义的“过时捕获策略阈值”中捕获查询。

STALE_CAPTURE_POLICY_THRESHOLD = *number* { DAYS | HOURS }     
定义评估间隔时段以确定是否应捕获查询。 默认值为 1 天，可以设置为 1 小时到 7 天。 number 的类型为 int。

EXECUTION_COUNT     
定义在评估期间执行查询的次数。 默认值为 30，这意味着对于默认的过时捕获策略阈值，查询必须在一天内至少执行 30 次才能在查询存储中保留。 EXECUTION_COUNT 的类型为 int。

TOTAL_COMPILE_CPU_TIME_MS     
定义查询在评估期间使用的总编译 CPU 时间。 默认值为 1000，这意味着对于默认的过时捕获策略阈值，查询必须在一天内在查询编译期间总共花费至少一秒钟的 CPU 时间，才能持久存储在查询存储中。 TOTAL_COMPILE_CPU_TIME_MS 的类型为 int。

TOTAL_EXECUTION_CPU_TIME_MS     
定义查询在评估期间使用的总执行 CPU 时间。 默认值为 100，这意味着对于默认的过时捕获策略阈值，查询必须在一天内在执行期间总共花费至少100 ms 的 CPU 时间，才能持久存储在查询存储中。 TOTAL_EXECUTION_CPU_TIME_MS 的类型为 int。

**\<snapshot_option> ::=**

确定事务隔离级别。

ALLOW_SNAPSHOT_ISOLATION { ON | OFF }     
ON     
在数据库级别启用快照选项。 启用该选项后，DML 语句将开始生成行版本，即使没有事务使用快照隔离也是如此。 启用此选项后，事务即可指定 SNAPSHOT 事务隔离级别。 当事务在 SNAPSHOT 隔离级别运行时，所有的语句都将数据快照视为位于事务的开头。 如果在 SNAPSHOT 隔离级别运行的事务要访问多个数据库中的数据，则必须将所有数据库中的 ALLOW_SNAPSHOT_ISOLATION 都设置为 ON，或者事务中的每个语句都必须对 FROM 子句中的所有引用（引用 ALLOW_SNAPSHOT_ISOLATION 设置为 OFF 的数据库中的表）使用锁提示。

OFF     
在数据库级别禁用快照选项。 事务不能指定 SNAPSHOT 事务隔离级别。

在将 ALLOW_SNAPSHOT_ISOLATION 设置为新状态（从 ON 设置为 OFF，或从 OFF 设置为 ON）时，在数据库中的所有现有事务均已提交之前，ALTER DATABASE 不会将控制权返回给调用方。 如果数据库已处于 ALTER DATABASE 语句所指定的状态，则控制权会立刻返回给调用方。 如果 ALTER DATABASE 语句未迅速返回，请使用 [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) 确定是否存在长期运行的事务。 如果 ALTER DATABASE 语句被取消，则数据库仍保持 ALTER DATABASE 开始时所处的状态。 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图指示数据库中快照隔离事务的状态。 如果 **snapshot_isolation_state_desc** = IN_TRANSITION_TO_ON，ALTER DATABASE ALLOW_SNAPSHOT_ISOLATION OFF 将暂停六秒钟，然后重试操作。

如果数据库处于 OFFLINE 状态，则不能更改 ALLOW_SNAPSHOT_ISOLATION 的状态。

如果在 READ_ONLY 数据库中设置 ALLOW_SNAPSHOT_ISOLATION，则以后将数据库设置为 READ_WRITE 时，仍将保留该设置。

可以为 master、model、msdb 和 tempdb 数据库更改 ALLOW_SNAPSHOT_ISOLATION 设置。 如果为 tempdb 更改该设置，则每次停止并重新启动[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例时会保留该设置。 如果为 model 更改该设置，则该设置将成为除 tempdb 以外的所有新建数据库的默认设置。

对于 master 和 msdb 数据库，默认情况下该选项设置为 ON。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `snapshot_isolation_state` 列确定此选项的当前设置。

READ_COMMITTED_SNAPSHOT { ON | OFF }     
ON     
在数据库级别启用 Read-Committed Snapshot 选项。 启用该选项后，DML 语句将开始生成行版本，即使没有事务使用快照隔离也是如此。 启用此选项后，指定 READ COMMITTED 隔离级别的事务将使用行版本控制而不是锁定。 当事务在 READ COMMITTED 隔离级别运行时，所有的语句都将数据快照视为位于语句的开头。

OFF     
在数据库级别禁用 Read-Committed Snapshot 选项。 指定 READ COMMITTED 隔离级别的事务使用锁定。

若要将 READ_COMMITTED_SNAPSHOT 设置为 ON 或 OFF，不应存在任何活动的数据库连接，运行 ALTER DATABASE 命令的连接除外。 但是，数据库不必一定要处于单用户模式下。 当数据库处于 OFFLINE 状态时，不能更改此选项的状态。

如果在 READ_ONLY 数据库中设置 READ_COMMITTED_SNAPSHOT，则以后将数据库设置为 READ_WRITE 时，仍将保留该设置。

对于 master、tempdb 或 msdb 系统数据库，不能将 READ_COMMITTED_SNAPSHOT 设置为 ON。 如果为 model 更改该设置，则该设置会成为除 tempdb 以外的所有新建数据库的默认设置。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_read_committed_snapshot_on` 列确定此选项的当前设置。

> [!WARNING]
> 当使用 DURABILITY = SCHEMA_ONLY 创建表，随后使用 ALTER DATABASE 更改 READ_COMMITTED_SNAPSHOT 时，表中的数据将丢失  。

MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT { ON | OFF }     
ON     
当事务隔离级别设置为任何低于 SNAPSHOT 的隔离级别时，内存优化表中所有经过解释的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 操作将在 SNAPSHOT 隔离下运行。 低于快照的隔离级别示例有 READ COMMITTED 或 READ UNCOMMITTED。 无论是在会话级别显式设置事务隔离级别还是隐式使用默认值，这些操作都会运行。

OFF     
不提升内存优化表中经过解释的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 操作的事务隔离级别。

如果数据库处于 OFFLINE 状态，不能更改 MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT 的状态。

默认值为 OFF。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_memory_optimized_elevate_to_snapshot_on` 列确定此选项的当前设置。

\<sql_option> ::=     
在数据库级别控制 ANSI 遵从选项。

ANSI_NULL_DEFAULT { ON | OFF }     
确定在 CREATE TABLE 或 ALTER TABLE 语句中未显式定义为 Null 性的列或 [CLR 用户定义类型](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)的默认值（NULL 或 NOT NULL）。 无论此设置如何，使用约束定义的列都将遵循约束规则。

ON     
默认值为 NULL。

OFF     
默认值为 NOT NULL。

连接级设置（使用 SET 语句设置）覆盖 ANSI_NULL_DEFAULT 的默认数据库级别设置。 默认情况下，ODBC 和 OLE DB 客户端发出连接级 SET 语句，将会话的 ANSI_NULL_DEFAULT 设置为 ON。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md)。

对于 ANSI 兼容性，数据库选项 ANSI_NULL_DEFAULT 设置为 ON 将使数据库默认设置改为 NULL。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_ansi_null_default_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAnsiNullDefault` 属性来确定状态。

ANSI_NULLS { ON | OFF }     
ON     
与 Null 值的所有比较的结果均为 UNKNOWN。

OFF     
将非 UNICODE 值与 Null 值比较时，如果这两个值都为 NULL，则结果为 TRUE。

> [!IMPORTANT]
> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中，ANSI_NULLS 将始终为 ON，将该选项显式设置为 OFF 的任何应用程序都将产生错误。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。

  连接级设置（使用 SET 语句设置）覆盖 ANSI_NULLS 的默认数据库设置。 默认情况下，ODBC 和 OLE DB 客户端发出连接级 SET 语句，将会话的 ANSI_NULLS 设置为 ON。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md)。

> [!IMPORTANT]
> 创建或更改计算列或索引视图的索引时，SET ANSI_NULLS 必须设置为 ON。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_ansi_nulls_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAnsiNullsEnabled` 属性来确定状态。

ANSI_PADDING { ON | OFF }     
ON     
在进行转换之前，将字符串填充到同一长度。 在插入到 varchar 或 nvarchar 数据类型之前，也填充到同一长度。

OFF     
将字符值中的尾随空格插入 varchar 或 nvarchar 列中。 也保留插入 varbinary 列中的二进制值的尾随零。 不将值填充到列的长度。

如果指定了 OFF，该设置只影响新列的定义。

> [!IMPORTANT]
> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中，ANSI_PADDING 将始终为 ON，将该选项显式设置为 OFF 的任何应用程序都将产生错误。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 建议始终将 ANSI_PADDING 设置为 ON。 创建或操作计算列或索引视图的索引时，ANSI_PADDING 必须为 ON。

当 ANSI_PADDING 设置为 ON 时，会将允许为 Null 值的 char(_n_) 和 binary(_n_) 列填充到列长度。 当 ANSI_PADDING 为 OFF 时，会剪裁尾随空格和零。 始终将不允许为 Null 值的 char(_n_) 和 binary(_n_) 列填充到列长度 。

  连接级设置（使用 SET 语句设置）覆盖 ANSI_PADDING 的默认数据库级别设置。 默认情况下，ODBC 和 OLE DB 客户端发出连接级 SET 语句，将会话的 ANSI_PADDING 设置为 ON。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md)。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_ansi_padding_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAnsiPaddingEnabled` 属性来确定状态。

ANSI_WARNINGS { ON | OFF }     
ON     
当出现被零除这类情况时，将发出错误或警告。 当聚合函数中出现 Null 值时，也会发出错误和警告。

OFF     
出现被零除等情况时不会引发警告，而是返回 Null 值。

> [!IMPORTANT]
> 创建或更改计算列或索引视图的索引时，SET ANSI_WARNINGS 必须设置为 ON。

  连接级设置（使用 SET 语句设置）覆盖 ANSI_WARNINGS 的默认数据库设置。 默认情况下，ODBC 和 OLE DB 客户端发出连接级 SET 语句，将会话的 ANSI_WARNINGS 设置为 ON。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md)。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_ansi_warnings_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAnsiWarningsEnabled` 属性来确定状态。

ARITHABORT { ON | OFF }     
ON     
在查询执行过程中出现溢出或被零除等错误时，结束查询。

OFF     
在出现其中一个错误时显示警告消息。 即使显示警告，查询、批处理或事务也会继续进行处理，就像没有发生错误一样。

> [!IMPORTANT]
> 创建或更改计算列或索引视图的索引时，SET ARITHABORT 必须设置为 ON。

  可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_arithabort_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsArithmeticAbortEnabled` 属性来确定状态。

COMPATIBILITY_LEVEL = { 150 \| 140 \| 130 \| 120 \| 110 \| 100 }     
有关详细信息，请参阅 [ALTER DATABASE 兼容级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。

CONCAT_NULL_YIELDS_NULL { ON | OFF }     
ON     
当串联运算的两个操作数中任意一个为 NULL 时，结果也为 NULL。 例如，将字符串“This is”和 NULL 串联将得到 NULL 值，而不是值“This is”。

OFF     
Null 值被视为空字符串进行处理。

> [!IMPORTANT]
> 创建或更改计算列或索引视图的索引时，CONCAT_NULL_YIELDS_NULL 必须设置为 ON。
>
> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中，CONCAT_NULL_YIELDS_NULL 将始终为 ON，而且将该选项显式设置为 OFF 的任何应用程序都将产生一个错误。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。

连接级设置（使用 SET 语句设置）覆盖 CONCAT_NULL_YIELDS_NULL 的默认数据库设置。 默认情况下，当连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时，ODBC 和 OLE DB 客户端发出连接级别 SET 语句，将会话的 CONCAT_NULL_YIELDS_NULL 设置为 ON。 有关详细信息，请参阅 [SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_concat_null_yields_null_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsNullConcat` 属性来确定状态。

QUOTED_IDENTIFIER { ON | OFF }     
ON     
可以将分隔标识符包含在双引号中。

所有用双引号分隔的字符串都被解释为对象标识符。 加引号的标识符不必遵守 [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符规则。 它们可以是关键字，并且可以包含 [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符中不允许的字符。 如果单引号 (') 是文字字符串的一部分，则可以用双引号 (") 表示它。

OFF     
标识符不能包含在引号中，而且必须遵循所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符规则。 文字可以由单引号或双引号分隔。

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 还允许使用方括号 ([ ]) 分隔标识符。 无论 QUOTED_IDENTIFIER 设置如何，始终都可以使用用方括号括起来的标识符。 有关详细信息，请参阅 [Database Identifiers](../../relational-databases/databases/database-identifiers.md)。

  创建表后，QUOTED IDENTIFIER 选项在表的元数据中始终存储为 ON。 即使在创建表时将该选项设置为 OFF 也会存储该选项。

连接级设置（使用 SET 语句设置）覆盖 QUOTED_IDENTIFIER 的默认数据库设置。 默认情况下，ODBC 和 OLE DB 客户端发出连接级 SET 语句，将 QUOTED_IDENTIFIER 设置为 ON。 客户端在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例时运行该语句。 有关详细信息，请参阅 [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md)。

  可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_quoted_identifier_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsQuotedIdentifiersEnabled` 属性来确定状态。

NUMERIC_ROUNDABORT { ON | OFF }     
ON     
当表达式中发生精度损失时生成错误。

OFF     
精度的降低不会生成错误消息，会根据存储结果的列或变量的精度，将结果舍入。

> [!IMPORTANT]
> 创建或更改计算列或索引视图的索引时，NUMERIC_ROUNDABORT 必须设置为 OFF。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_numeric_roundabort_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsNumericRoundAbortEnabled` 属性来确定状态。

RECURSIVE_TRIGGERS { ON | OFF }     
ON     
允许递归激发 AFTER 触发器。

OFF     
可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_recursive_triggers_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsRecursiveTriggersEnabled` 属性来确定状态。

> [!NOTE]
> 当 RECURSIVE_TRIGGERS 设置为 OFF 时，只禁止直接递归触发。 若要禁用间接递归触发，还必须将 nested triggers 服务器选项设置为 0。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 `is_recursive_triggers_on` 列或 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsRecursiveTriggersEnabled` 属性来确定此选项的状态。

\<target_recovery_time_option> ::=     
指定每个数据库上间接检查点的频率。 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始，新数据库的默认值为 1 分钟，表示数据库使用间接检查点。 较旧版本的默认值为 0，表示数据库使用自动检查点，其频率依赖于服务器实例的恢复间隔设置。 对于大多数系统，[!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议设置为 1 分钟。

TARGET_RECOVERY_TIME **=** _target_recovery_time_ { SECONDS | MINUTES }     
*target_recovery_time*     
指定在发生崩溃的情况下恢复指定数据库的最大上限时间。

SECONDS     
指示 *target_recovery_time* 表示为秒数。

MINUTES     
指示 *target_recovery_time* 表示为分钟数。

有关间接检查点的详细信息，请参阅[数据库检查点](../../relational-databases/logs/database-checkpoints-sql-server.md)。

ROLLBACK AFTER *integer* [SECONDS] | ROLLBACK IMMEDIATE     
指定是在指定秒数之后回滚还是立即回滚。

NO_WAIT     
指定：如果请求的数据库状态或选项更改无法立即完成，则请求失败。 立即完成意味着不会等待事务自己提交或回滚。

## <a name="setting-options"></a><a name="SettingOptions"></a> 设置选项
若要检索数据库选项的当前设置，请使用 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图或 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)

设置数据库选项后，新的设置将立即生效。

可以为所有新建数据库更改任意一个数据库选项的默认值。 为此，请更改 model 数据库中的相应数据库选项。

## <a name="examples"></a>示例

### <a name="a-setting-the-database-to-read_only"></a>A. 将数据库设置为 READ_ONLY
将数据库或文件组的状态改为 READ_ONLY 或 READ_WRITE 需要具有数据库的专有访问权限。 下面的示例将数据库设置为 `RESTRICTED_USER` 模式，以限制访问。 然后，该示例将 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的状态设置为 `READ_ONLY` ，并将对数据库的访问权返回给所有用户。

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET RESTRICTED_USER;
GO
ALTER DATABASE [database_name]
SET READ_ONLY
GO
ALTER DATABASE [database_name]
SET MULTI_USER;
GO
```

### <a name="b-enabling-snapshot-isolation-on-a-database"></a>B. 对数据库启用快照隔离
下面的示例为 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库启用快照隔离框架选项。

```sql
USE [database_name];
USE master;
GO
ALTER DATABASE [database_name]
SET ALLOW_SNAPSHOT_ISOLATION ON;
GO
-- Check the state of the snapshot_isolation_framework
-- in the database.
SELECT name, snapshot_isolation_state,
    snapshot_isolation_state_desc AS description
FROM sys.databases
WHERE name = N'[database_name]';
GO
```

结果集显示快照隔离框架已启用。

|name |snapshot_isolation_state |description|
|-------------------- |------------------------|----------|
|[database_name] |1| ON |

### <a name="c-enabling-modifying-and-disabling-change-tracking"></a>C. 启用、修改和禁用更改跟踪
下面的示例对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库启用更改跟踪并将保持期设置为 `2` 天。

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = ON
(AUTO_CLEANUP = ON, CHANGE_RETENTION = 2 DAYS);
```

下面的示例说明如何将保持期更改为 `3` 天。

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING (CHANGE_RETENTION = 3 DAYS);
```

下面的示例说明如何对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库禁用更改跟踪。

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = OFF;
```

### <a name="d-enabling-the-query-store"></a>D. 启用查询存储
下面的示例启用查询存储并配置查询存储参数。

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

### <a name="e-enabling-the-query-store-with-wait-statistics"></a>E. 使用等待统计信息启用查询存储
下面的示例启用查询存储并配置其参数。

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
    );
```

### <a name="f-enabling-the-query-store-with-custom-capture-policy-options"></a>F. 使用自定义捕获策略选项启用查询存储
下面的示例启用查询存储并配置其参数。

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
      QUERY_CAPTURE_MODE = CUSTOM,
      QUERY_CAPTURE_POLICY = (
        STALE_CAPTURE_POLICY_THRESHOLD = 24 HOURS,
        EXECUTION_COUNT = 30,
        TOTAL_COMPILE_CPU_TIME_MS = 1000,
        TOTAL_EXECUTION_CPU_TIME_MS = 100
      )
    );
```

## <a name="see-also"></a>另请参阅

- [ALTER DATABASE 兼容级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)
- [ALTER DATABASE 数据库镜像](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)
- [统计信息](../../relational-databases/statistics/statistics.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
- [启用和禁用更改跟踪](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [Query Store 最佳实践](../../relational-databases/performance/best-practice-with-the-query-store.md)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-set-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 数据库](alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL 托管实例](alter-database-transact-sql-set-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>语法

```syntaxsql
ALTER DATABASE { database_name }
SET
{
    <optionspec> [ ,...n ]
}
;

<option_spec>::=
{
    <auto_option>
  | <db_encryption_option>
  | <query_store_options>
  | <result_set_caching>
  | <snapshot_option>
}
;

<auto_option> ::=
{
    AUTO_CREATE_STATISTICS { OFF | ON }
}

<db_encryption_option> ::=
{
    ENCRYPTION { ON | OFF }
}

<query_store_option> ::=
{
    QUERY_STORE { OFF |  ON }
}

<result_set_caching_option> ::=
{
    RESULT_SET_CACHING {ON | OFF}
}

<snapshot_option> ::=
{
    READ_COMMITTED_SNAPSHOT {ON | OFF }
}

```

## <a name="arguments"></a>参数

database_name     
要修改的数据库的名称。

**<auto_option> ::=**      
控制自动选项。

AUTO_CREATE_STATISTICS { ON | OFF }     

ON     
查询优化器根据需要在查询谓词中的单列上创建统计信息，以便改进查询计划和查询性能。 在查询优化器编译查询时创建这些单列统计信息。 这些单列统计信息只在尚不是现有统计信息对象的第一列的列上创建。

默认值为 ON。 建议您对于大多数数据库使用默认设置。

OFF     
查询优化器在编译查询时不在查询谓词中的单列上创建统计信息。 将此选项设置为 OFF 可能导致并非最佳的查询计划以及查询性能下降。

### <a name="remarks"></a>备注
连接到用户数据库时，必须运行此命令。

可通过查看 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 i`s_auto_create_stats_on` 列来确定此选项的状态。 还可通过查看 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 `IsAutoCreateStatistics` 属性来确定状态。

有关详细信息，请参阅“统计信息”中的“使用数据库范围的统计信息选项”部分。

**<db_encryption_option> ::=**      
控制数据库加密状态。

ENCRYPTION { ON | OFF }     

ON     
设置要加密的数据库。

OFF     
将数据库设置为不加密。

有关数据加密的详细信息，请参阅“透明数据加密”和“借助 Azure SQL 数据库实现透明数据加密”。

在数据库级别启用加密时，所有文件组都将进行加密。 任何新的文件组都将继承加密的属性。 如果数据库中的任何文件组设置为 READ ONLY，则数据库加密操作将失败。

可以通过使用 sys.dm_database_encryption_keys 动态管理视图来查看数据库的加密状态和加密扫描的状态。

**<query_store_option> ::=**      
控制是否在此数据仓库中启用查询存储。

QUERY_STORE { ON |  OFF  }     

ON     
启用查询存储。

OFF     
禁用查询存储。 OFF 是默认值。

> [!NOTE]
> 对于 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]，必须在用户数据库中执行 `ALTER DATABASE SET QUERY_STORE`。 不支持从另一个数据仓库实例中执行该语句。

**<result_set_caching_option> ::=**      
**适用对象**：Azure Synapse Analytics  

控制查询结果是否缓存在数据库中。

RESULT_SET_CACHING { ON | OFF}     

ON     
指定在数据库中缓存从此数据库返回的查询结果集。

OFF     
指定不会在数据库中缓存从此数据库返回的查询结果集。

### <a name="remarks"></a>备注

连接到 `master` 数据库时，必须运行此命令。  对此数据库设置的更改立即生效。  缓存查询结果集会产生存储成本。 在你为数据库禁用结果缓存后，以前保留的结果缓存会立即从 Azure Synapse 存储中删除。

运行此命令以检查数据库的结果集缓存配置。  如果结果集缓存已打开，则 is_result_set_caching_on 将返回 1。

```sql
SELECT name, is_result_set_caching_on FROM sys.databases
WHERE name = <'Your_Database_Name'>
```

运行此命令以检查是否已使用缓存结果执行查询。  result_cache_hit 列对缓存命中返回 1，对缓存失误返回 0，而对未使用结果集缓存的原因，则返回负值。  有关详细信息，请检查 [sys.dm_pdw_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)。  

```sql
SELECT request_id, command, result_cache_hit FROM sys.dm_pdw_exec_requests
WHERE request_id = <'Your_Query_Request_ID'>
```

> [!IMPORTANT]
> 用于创建结果集缓存和从缓存中检索数据的操作发生在数据仓库实例的控制节点上。 当结果集缓存处于打开状态时，运行返回大型结果集（例如，超过 1 百万行）的查询可能会导致控制节点上 CPU 使用率较高，并降低实例上的整体查询响应速度。 这些查询通常在数据浏览或 ETL 操作过程中使用。 若要避免对控制节点造成压力并导致性能问题，用户应在运行此类查询之前关闭数据库的结果集缓存。  

有关通过结果集缓存进行性能优化的详细信息，请参阅[性能优化指南](/azure/sql-data-warehouse/performance-tuning-result-set-caching)。

### <a name="permissions"></a>权限

要设置 RESULT_SET_CACHING 选项，用户需要服务器级别主体登录名（在预配过程中创建的登录名）或者成为 `dbmanager` 数据库角色的成员。  

**<snapshot_option> ::=**      
**适用对象**：Azure Synapse Analytics

控制数据库的事务隔离级别。

READ_COMMITTED_SNAPSHOT  { ON | OFF }     

ON     
在数据库级别启用 READ_COMMITTED_SNAPSHOT 选项。

OFF     
在数据库级别关闭 READ_COMMITTED_SNAPSHOT 选项。

### <a name="remarks"></a>备注

连接到 `master` 数据库时，必须运行此命令。 为用户数据库启用和禁用 READ_COMMITTED_SNAPSHOT 将终止与该数据库的所有活跃连接。 建议在数据库维护时段进行此更改，也可等到除了运行 ALTER DATABASE 命令的连接之外与数据库没有任何活跃的连接后再进行更改。  数据库不必处于单用户模式。 不支持在会话级别更改 READ_COMMITTED_SNAPSHOT 设置。  要验证数据库的此设置，请检查 sys.databases 中的 is_read_committed_snapshot_on 列。

在启用了 READ_COMMITTED_SNAPSHOT 的数据库中，如果存在多个数据版本，则由于扫描版本，查询的性能可能会降低。 长时间打开的事务也会导致数据库的大小增加。 如果这些事务进行的数据更改会阻止版本清理，则会发生此问题。  

### <a name="permissions"></a>权限

要设置 READ_COMMITTED_SNAPSHOT 选项，用户需要对数据库具有 ALTER 权限。

## <a name="examples"></a>示例

### <a name="check-statistics-setting-for-a-database"></a>检查数据库的统计信息设置

```sql
SELECT name, is_auto_create_stats_on FROM sys.databases
```

### <a name="enable-query-store-for-a-database"></a>为数据库启用查询存储

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON;
```

### <a name="enable-result-set-caching-for-a-database"></a>为数据库启用结果集缓存

```sql
-- Run this command when connecting to the MASTER database

ALTER DATABASE [database_name]
SET RESULT_SET_CACHING ON;
```

### <a name="check-result-set-caching-setting-for-a-database"></a>检查数据库的结果集缓存设置

```sql
SELECT name, is_result_set_caching_on
FROM sys.databases;
```

### <a name="enable-the-read_committed_snapshot-option-for-a-database"></a>为数据库启用 Read_Committed_Snapshot 选项

```sql
-- Run this command when connecting to the MASTER database

ALTER DATABASE MyDatabase
SET READ_COMMITTED_SNAPSHOT ON
```

## <a name="see-also"></a>另请参阅

- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [Azure Synapse Analytics 最佳实践](/azure/sql-data-warehouse/sql-data-warehouse-best-practices#maintain-statistics)
- [在 Azure Synapse Analytics 中设计表](/azure/sql-data-warehouse/sql-data-warehouse-tables-overview#statistics)
- [Azure Synapse Analytics 语言元素](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-language-elements)

::: moniker-end

---
description: sys.databases (Transact-SQL)
title: sys.databases (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/08/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- databases
- databases_TSQL
- sys.databases
- sys.databases_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.databases catalog view
ms.assetid: 46c288c1-3410-4d68-a027-3bbf33239289
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fb80841037a9fc9b7fcb0bc48dff0dd436ea0f88
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462858"
---
# <a name="sysdatabases-transact-sql"></a>sys.databases (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例中的每个数据库都包含一行。  
  
如果数据库不存在 `ONLINE` ，或者 `AUTO_CLOSE` 设置为 `ON` 并且数据库已关闭，则某些列的值可能为 `NULL` 。 如果数据库为 `OFFLINE` ，则低权限用户看不到相应的行。 如果数据库为，则若要查看相应的行 `OFFLINE` ，用户必须至少具有 `ALTER ANY DATABASE` 服务器级权限或 `CREATE DATABASE` 数据库中的权限 `master` 。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|数据库名称，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中或在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 服务器中是唯一的。|  
|database_id|**int**|数据库的 ID，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中或在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 服务器中是唯一的。|  
|**source_database_id**|**int**|Non-NULL = 该数据库快照的源数据库 ID。<br /> NULL = 非数据库快照。|  
|**owner_sid**|**varbinary(85)**|注册到服务器的数据库外部所有者的 SID（安全标识符）。 有关谁可以拥有数据库的信息，请参阅 [ALTER authorization](../../t-sql/statements/alter-authorization-transact-sql.md)中的 **alter authorization for** database 部分。|  
|create_date|**datetime**|数据库的创建或重命名日期。 对于 **tempdb**，每次服务器重启时，此值都会发生更改。|  
|**compatibility_level**|**tinyint**|对应于兼容行为的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本的整数：<br /><br /><table border="0"><tr><td>值</td><td>**适用于**</td></tr><tr><td>70</td><td>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 到 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]</td></tr><tr><td>80</td><td>[!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 周一至 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]</td></tr><tr><td>90</td><td>[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 周一至 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]</td></tr><tr><td>100</td><td>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]</td></tr><tr><td>110</td><td>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]</td></tr><tr><td>120</td><td>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]</td></tr><tr><td>130</td><td>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]</td></tr><tr><td>140</td><td>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]</td></tr><tr><td>150</td><td>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]</td></tr></table>|  
|**collation_name**|**sysname**|数据库的排序规则。 作为数据库中的默认排序规则。<br /> NULL = 数据库不处于联机状态，或者 AUTO_CLOSE 设置为 ON 且数据库已关闭。|  
|**user_access**|**tinyint**|用户访问设置：<br /> 0 = 已指定 MULTI_USER<br /> 1 = 已指定 SINGLE_USER<br /> 2 = 已指定 RESTRICTED_USER|  
|**user_access_desc**|**nvarchar(60)**|用户访问设置的说明。|  
|**is_read_only**|**bit**|1 = 数据库为 READ_ONLY<br /> 0 = 数据库为 READ_WRITE|  
|**is_auto_close_on**|**bit**|1 = AUTO_CLOSE 为 ON<br /> 0 = AUTO_CLOSE 为 OFF|  
|**is_auto_shrink_on**|**bit**|1 = AUTO_SHRINK 为 ON<br /> 0 = AUTO_SHRINK 为 OFF|  
|**state**|**tinyint**|值<br /> 0 = ONLINE  <br /> 1 = RESTORING <br /> 2 = 正在恢复 <sup>1</sup><br /> 3 = RECOVERY_PENDING <sup>1</sup><br /> 4 = SUSPECT  <br /> 5 = 紧急 <sup>1</sup><br /> 6 = 脱机 <sup>1</sup><br /> 7 = 正在复制 <sup>2</sup> <br /> 10 = OFFLINE_SECONDARY <sup>2</sup> <br /><br /> **注意：** 对于 Always On 数据库，查询 `database_state` sys.dm_hadr_database_replica_states 的或 `database_state_desc` 列 [](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)。<br /><br /><sup>1</sup> **适用于**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从) 开始 ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)][!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]<br /><sup>2</sup> **适用** 于： [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)][!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)]|  
|**state_desc**|**nvarchar(60)**|数据库状态的说明。 请参阅状态。|  
|**is_in_standby**|**bit**|对于还原日志而言，数据库是只读的。|  
|**is_cleanly_shutdown**|**bit**|1 = 数据库完全关闭；在启动时不需要恢复<br /> 0 = 数据库并未完全关闭；在启动时需要恢复|  
|**is_supplemental_logging_enabled**|**bit**|1 = SUPPLEMENTAL_LOGGING 为 ON<br /> 0 = SUPPLEMENTAL_LOGGING 为 OFF|  
|**snapshot_isolation_state**|**tinyint**|允许的快照隔离事务状态，如 ALLOW_SNAPSHOT_ISOLATION 选项所设置：<br /> 0 = 快照隔离状态为 OFF（默认值）。 不允许使用快照隔离。<br /> 1 = 快照隔离状态为 ON。 允许使用快照隔离。<br /> 2 = 快照隔离状态正在转换到 OFF 状态。 所有事务都将其修改版本化。 无法使用快照隔离启动新的事务。 数据库仍保持向 OFF 状态转换，直到所有在执行 ALTER DATABASE 时处于活动状态的事务完成。<br /> 3 = 快照隔离状态正在转换到 ON 状态。 新事务都将其修改版本化。 在快照隔离状态变为 1 (ON) 之前，事务无法使用快照隔离。 数据库仍保持向 ON 状态转换，直到所有在执行 ALTER DATABASE 时处于活动状态的更新事务完成。|  
|**snapshot_isolation_state_desc**|**nvarchar(60)**|允许的快照隔离事务状态的说明，它由 ALLOW_SNAPSHOT_ISOLATION 选项设置。|  
|**is_read_committed_snapshot_on**|**bit**|1 = READ_COMMITTED_SNAPSHOT 选项为 ON。 read-committed 隔离级别下的读操作基于快照扫描，没有获取锁。<br /> 0 = READ_COMMITTED_SNAPSHOT 选项为 OFF（默认）。 read-committed 隔离级别下的读操作使用共享锁。|  
|**recovery_model**|**tinyint**|选定的恢复模式：<br /> 1 = FULL<br /> 2 = BULK_LOGGED<br /> 3 = SIMPLE|  
|**recovery_model_desc**|**nvarchar(60)**|选定的恢复模式的说明。|  
|**page_verify_option**|**tinyint**|PAGE_VERIFY 选项的设置：<br /> 0 = NONE<br /> 1 = TORN_PAGE_DETECTION<br /> 2 = CHECKSUM|  
|**page_verify_option_desc**|**nvarchar(60)**|PAGE_VERIFY 选项设置的说明。|  
|**is_auto_create_stats_on**|**bit**|1 = AUTO_CREATE_STATISTICS 为 ON<br /> 0 = AUTO_CREATE_STATISTICS 为 OFF|  
|**is_auto_create_stats_incremental_on**|**bit**|指示自动统计信息的增量选项的默认设置。<br /> 0 = 自动创建统计信息不是增量统计信息<br /> 1 = 如果可能，自动创建统计信息是增量统计信息<br /> 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）  。|  
|**is_auto_update_stats_on**|**bit**|1 = AUTO_UPDATE_STATISTICS 为 ON<br /> 0 = AUTO_UPDATE_STATISTICS 为 OFF|  
|**is_auto_update_stats_async_on**|**bit**|1 = AUTO_UPDATE_STATISTICS_ASYNC 为 ON<br /> 0 = AUTO_UPDATE_STATISTICS_ASYNC 为 OFF|  
|**is_ansi_null_default_on**|**bit**|1 = ANSI_NULL_DEFAULT 为 ON<br /> 0 = ANSI_NULL_DEFAULT 为 OFF|  
|**is_ansi_nulls_on**|**bit**|1 = ANSI_NULLS 为 ON<br /> 0 = ANSI_NULLS 为 OFF|  
|**is_ansi_padding_on**|**bit**|1 = ANSI_PADDING 为 ON<br /> 0 = ANSI_PADDING 为 OFF|  
|**is_ansi_warnings_on**|**bit**|1 = ANSI_WARNINGS 为 ON<br /> 0 = ANSI_WARNINGS 为 OFF|  
|**is_arithabort_on**|**bit**|1 = ARITHABORT 为 ON<br /> 0 = ARITHABORT 为 OFF|  
|**is_concat_null_yields_null_on**|**bit**|1 = CONCAT_NULL_YIELDS_NULL 为 ON<br /> 0 = CONCAT_NULL_YIELDS_NULL 为 OFF|  
|**is_numeric_roundabort_on**|**bit**|1 = NUMERIC_ROUNDABORT 为 ON<br /> 0 = NUMERIC_ROUNDABORT 为 OFF|  
|**is_quoted_identifier_on**|**bit**|1 = QUOTED_IDENTIFIER 为 ON<br /> 0 = QUOTED_IDENTIFIER 为 OFF|  
|**is_recursive_triggers_on**|**bit**|1 = RECURSIVE_TRIGGERS 为 ON<br /> 0 = RECURSIVE_TRIGGERS 为 OFF|  
|**is_cursor_close_on_commit_on**|**bit**|1 = CURSOR_CLOSE_ON_COMMIT 为 ON<br /> 0 = CURSOR_CLOSE_ON_COMMIT 为 OFF|  
|**is_local_cursor_default**|**bit**|1 = CURSOR_DEFAULT 为局部默认值<br /> 0 = CURSOR_DEFAULT 为全局默认值|  
|**is_fulltext_enabled**|**bit**|1 = 针对数据库启用全文<br /> 0 = 针对数据库禁用全文|  
|**is_trustworthy_on**|**bit**|1 = 数据库已标记为可信<br /> 0 = 数据库尚未标记为可信<br /> 默认情况下，还原的数据库或附加的数据库具有 "可信未启用"。|  
|**is_db_chaining_on**|**bit**|1 = 跨数据库所有权链接为 ON<br /> 0 = 跨数据库所有权链接为 OFF|  
|**is_parameterization_forced**|**bit**|1 = 参数化为 FORCED<br /> 0 = 参数化为 SIMPLE|  
|**is_master_key_encrypted_by_server**|**bit**|1 = 数据库具有加密的主密钥<br /> 0 = 数据库没有加密的主密钥|  
|**is_query_store_on**|**bit**|1 = 为此数据库启用查询存储。 选中 [sys.database_query_store_options](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md) 查看查询存储状态。<br /> 0 = 未启用查询存储<br /> 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始）  。|  
|is_published|**bit**|1 = 数据库为事务复制拓扑或快照复制拓扑中的发布数据库<br /> 0 = 不是发布数据库|  
|**is_subscribed**|**bit**|未使用此列。 它将始终返回 0，而与数据库的订阅服务器状态无关。|  
|**is_merge_published**|**bit**|1 = 数据库为合并复制拓扑中的发布数据库<br /> 0 = 不是合并复制拓扑中的发布数据库|  
|**is_distributor**|**bit**|1 = 数据库为复制拓扑的分发数据库<br /> 0 = 不是复制拓扑的分发数据库|  
|**is_sync_with_backup**|**bit**|1 = 数据库标记为与备份进行复制同步<br /> 0 = 没有标记为与备份进行复制同步|  
|**service_broker_guid**|**uniqueidentifier**|该数据库的服务代理标识符。 用作路由表中目标的 **broker_instance**。|  
|**is_broker_enabled**|**bit**|1 = 该数据库中的代理当前正在发送和接收消息。<br /> 0 = 所有已发送的消息都会停留在传输队列中，已接收的消息不会置于该数据库的队列中。<br /> 默认情况下，还原的数据库或附加的数据库都禁用了代理。 与此相关的例外是数据库镜像，其中 Broker 在故障转移后启用。|  
|**log_reuse_wait**|**tinyint**|事务日志空间的重复使用目前正在等待最后一个检查点的下列其中一项。 有关这些值的更多详细说明，请参阅 [事务日志](../../relational-databases/logs/the-transaction-log-sql-server.md)。<br /> 值<br /> 0 = 无<br /> 1 = 检查点 (当数据库使用恢复模式并且具有内存优化数据文件组时，应看到 `log_reuse_wait` 该列指示 `checkpoint` 或 `xtp_checkpoint`) <sup>1</sup><br /> 2 = 日志备份 <sup>1</sup><br /> 3 = 活动备份或还原 <sup>1</sup><br /> 4 = 活动事务 <sup>1</sup><br /> 5 = 数据库镜像 <sup>1</sup><br /> 6 = 复制 <sup>1</sup><br /> 7 = 数据库快照创建 <sup>1</sup><br /> 8 = 日志扫描 <br /> 9 = Always On 可用性组的辅助副本正将此数据库的事务日志记录应用到相应的辅助数据库。 <sup>2</sup><br /> 9 = 其他 (暂时性) <sup>3</sup><br /> 10 = 仅供内部使用 <sup>2</sup><br /> 11 = 仅供内部使用 <sup>2</sup><br /> 12 = 仅供内部使用 <sup>2</sup><br /> 13 = 最早第<sup>2</sup>页<br /> 14 = 其他 <sup>2</sup><br />  16 = XTP_CHECKPOINT (当数据库使用恢复模式并且具有内存优化数据文件组时，应会看到 `log_reuse_wait` 列指示 `checkpoint` 或 `xtp_checkpoint`) <sup>4</sup><br /><br /><sup>1</sup> **适用于**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从) 开始 ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]<br /><sup>2</sup> **适用于**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从) 开始 ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]<br /><sup>3</sup> **适用于**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 最多 (，包括 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)]) <br /><sup>4</sup> **适用于**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从) 开始 ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|  
|**log_reuse_wait_desc**|**nvarchar(60)**|日志空间的重复使用正在等待最后一个检查点的描述。|  
|**is_date_correlation_on**|**bit**|1 = DATE_CORRELATION_OPTIMIZATION 为 ON<br /> 0 = DATE_CORRELATION_OPTIMIZATION 为 OFF|  
|**is_cdc_enabled**|**bit**|1 = 对数据库启用变更数据捕获。 有关详细信息，请参阅 [&#40;transact-sql&#41;sys.sp_cdc_enable_db ](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-db-transact-sql.md)。|  
|**is_encrypted**|**bit**|指示是否对数据库进行加密 (反映使用子句) 上次设置的状态 `ALTER DATABASE SET ENCRYPTION` 。 可以是以下其中一个值：<br /> 1 = 已加密<br /> 0 = 未加密<br /> 有关数据库加密的详细信息，请参阅[透明数据加密 (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md)。<br /> 如果数据库正处于解密过程中，将 `is_encrypted` 显示值0。 可以通过使用 [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md) 动态管理视图来查看加密过程的状态。|  
|**is_honor_broker_priority_on**|**bit**|指示数据库是否采用会话优先级 (反映上次使用子句) 设置的状态 `ALTER DATABASE SET HONOR_BROKER_PRIORITY` 。 可以是以下其中一个值：<br /> 1 = HONOR_BROKER_PRIORITY 为 ON<br /> 0 = HONOR_BROKER_PRIORITY 为 OFF<br /> 默认情况下，还原的数据库或附加的数据库的 broker 优先级为 off。|  
|**replica_id**|**uniqueidentifier**|数据库参与的可用性组（如果有）的本地 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]可用性副本的唯一标识符。<br /> NULL = 数据库不是可用性组中的可用性副本的一部分。<br /> **适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**group_database_id**|**uniqueidentifier**|数据库参与到 Always On 可用性组（如果有）内的数据库的唯一标识符。 对于主副本上的此数据库以及已将数据库联接到可用性组的每个辅助副本， **group_database_id** 都是相同的。<br /> NULL = 数据库不是任何可用性组中的可用性副本的一部分。<br /> 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**resource_pool_id**|**int**|映射到此数据库的资源池的 ID。 此资源池控制对该数据库中的内存优化表可用的总内存。<br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从) 开始 ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|  
|**default_language_lcid**|**smallint**|指示包含数据库的默认语言的本地 ID (lcid)。<br /> **注意：** 作为的 [配置默认语言服务器配置选项](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) `sp_configure` 。 对于非包含数据库，此值为 **null** 。<br /> 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**default_language_name**|**nvarchar(128)**|指示包含数据库的默认语言。<br /> 对于非包含数据库，此值为 **null** 。<br /> **适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**default_fulltext_language_lcid**|**int**|指示包含数据库的默认全文语言 (lcid) 的区域设置 id。<br /> **注意：** 函数作为默认的默认 [全文语言服务器配置选项](../../database-engine/configure-windows/configure-the-default-full-text-language-server-configuration-option.md) `sp_configure` 。 对于非包含数据库，此值为 **null** 。<br /> 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**default_fulltext_language_name**|**nvarchar(128)**|指示包含数据库的默认全文语言。<br /> 对于非包含数据库，此值为 **null** 。<br /> 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**is_nested_triggers_on**|**bit**|指示包含数据库中是否允许使用嵌套触发器。<br /> 0 = 不允许使用嵌套触发器<br /> 1 = 允许使用嵌套触发器<br /> **注意：** 作为的 [配置嵌套触发器服务器配置选项](../../database-engine/configure-windows/configure-the-nested-triggers-server-configuration-option.md) `sp_configure` 。 对于非包含数据库，此值为 **null** 。 有关详细信息，请参阅 [sys.configurations &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) 。<br /> 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**is_transform_noise_words_on**|**bit**|指示是否应在包含数据库中转换干扰词。<br /> 0 = 不应转换干扰词。<br /> 1 = 应转换干扰词。<br /> **注意：** 作为的 [转换干扰词服务器配置选项](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md) `sp_configure` 。 对于非包含数据库，此值为 **null** 。 有关详细信息，请参阅 [sys.configurations &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) 。<br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从) 开始 ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|  
|**two_digit_year_cutoff**|**smallint**|指示 1753 到 9999 之间的数字值，以表示将两位数的年份解释为四位数的年份的截止年份。<br /> **注意：** 作为 " [配置两位数年份截止" 服务器配置选项](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) 的功能 `sp_configure` 。 对于非包含数据库，此值为 **null** 。 有关详细信息，请参阅 [sys.configurations &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) 。<br /> 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**containment**|**tinyint not null**|指示数据库的包含状态。<br />  0 = 数据库包含状态为 OFF。 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]<br /> 1 = 数据库在部分包容中 **适用** 于： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从) 开始 ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|  
|**containment_desc**|**nvarchar (60) not null**|指示数据库的包含状态。<br /> NONE = 早期数据库（零包含）<br /> PARTIAL = 部分包含的数据库<br /> **适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**target_recovery_time_in_seconds**|**int**|恢复数据库的估计时间（秒）。 可以为 NULL。<br /> 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**delayed_durability**|**int**|延迟持续性设置：<br /> 0 = 禁用<br /> 1 = 允许<br /> 2 = 强制<br /> 有关详细信息，请参阅[控制事务持续性](../../relational-databases/logs/control-transaction-durability.md)。<br /> 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|  
|**delayed_durability_desc**|**nvarchar(60)**|延迟持续性设置：<br /> DISABLED<br /> ALLOWED<br /> FORCED<br /> 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|  
|**is_memory_optimized_elevate_to_snapshot_on**|**bit**|在会话设置 TRANSACTION ISOLATION LEVEL 设置为较低的隔离级别、READ COMMITTED 或 READ UNCOMMITTED 时，使用 SNAPSHOT 隔离访问内存优化表。<br /> 1 = 最低隔离级别为 SNAPSHOT。<br /> 0 = 隔离级别未进行提升。|  
|**is_federation_member**|**bit**|指示该数据库是否为联合的成员。<br /> 适用于：[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**is_remote_data_archive_enabled**|**bit**|指示是否延伸数据库。<br /> 0 = 数据库未启用延伸。<br /> 1 = 数据库已启用延伸。<br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从) 开始 ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]<br /> 有关详细信息，请参阅 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)。|  
|**is_mixed_page_allocation_on**|**bit**|指示数据库中的表和索引是否可以从混合区分配初始页。<br /> 0 = 数据库中的表和索引始终从统一区分配初始页面。<br /> 1 = 数据库中的表和索引可以分配混合区中的初始页面。<br /> 有关详细信息，请参阅 `SET MIXED_PAGE_ALLOCATION` [ALTER DATABASE SET Options &#40;transact-sql&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)的选项。<br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从) 开始 ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]|  
|**is_temporal_history_retention_enabled**|**bit**|指示是否启用时态保留策略清理任务。<br /><br />1 = 启用时态保留<br />0 = 禁用时态保留<br />适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|**catalog_collation_type**|**int**|目录排序规则设置：<br />0 = DATABASE_DEFAULT<br />2 = SQL_Latin_1_General_CP1_CI_AS<br /> 适用于：[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|**catalog_collation_type_desc**|**nvarchar(60)**|目录排序规则设置：<br />COLLATE<br />SQL_Latin_1_General_CP1_CI_AS<br /> 适用于：[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|**physical_database_name**|**nvarchar(128)**|对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则为数据库的物理名称。 对于 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ，为服务器上的数据库提供通用 id。 <br />适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|**is_result_set_caching_on**|**bit**|指示是否启用结果集缓存。<br />1 = 已启用结果集缓存<br />0 = 禁用结果集缓存<br />**适用** 于： [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] Gen2。 尽管此功能已推出到所有区域，但请查看部署到实例的版本，以及最新的 [Azure Synapse 发行说明](/azure/synapse-analytics/sql-data-warehouse/release-notes-10-0-10106-0) 和 [Gen2 升级计划](/azure/synapse-analytics/sql-data-warehouse/gen2-migration-schedule) 以了解功能可用性。|
|**is_accelerated_database_recovery_on**|**bit**|指示是否启用加速数据库恢复 (ADR) 。<br />1 = 启用 ADR<br />0 = 禁用 ADR<br />适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|**is_tempdb_spill_to_remote_store**|**bit**|指示是否启用 tempdb 溢出到远程存储。<br />1 = 已启用<br />0 = 已禁用<br />**适用** 于： [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] Gen2。 尽管此功能已推出到所有区域，但请查看部署到实例的版本，以及最新的 [Azure Synapse 发行说明](/azure/synapse-analytics/sql-data-warehouse/release-notes-10-0-10106-0) 和 [Gen2 升级计划](/azure/synapse-analytics/sql-data-warehouse/gen2-migration-schedule) 以了解功能可用性。|
|**is_stale_page_detection_on**|**bit**|指示是否启用过时页检测。<br />1 = 已启用过时页检测<br />0 = 禁用陈旧页面检测<br />**适用** 于： [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] Gen2。 尽管此功能已推出到所有区域，但请查看部署到实例的版本，以及最新的 [Azure Synapse 发行说明](/azure/synapse-analytics/sql-data-warehouse/release-notes-10-0-10106-0) 和 [Gen2 升级计划](/azure/synapse-analytics/sql-data-warehouse/gen2-migration-schedule) 以了解功能可用性。|
|**is_memory_optimized_enabled**|**bit**|指示是否为数据库启用了某些 In-Memory 功能，如 [混合缓冲池](../../database-engine/configure-windows/hybrid-buffer-pool.md)。 不反映 [内存中 OLTP](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)的可用性或配置状态。 <br />1 = 启用了内存优化功能<br />0 = 禁用了内存优化的功能<br />适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
  
## <a name="permissions"></a>权限

 如果的调用方 `sys.databases` 不是数据库的所有者，并且数据库不是 `master` 或，则 `tempdb` 查看对应行所需的最小权限是 `ALTER ANY DATABASE` 或 `VIEW ANY DATABASE` 数据库中的服务器级别权限或 `CREATE DATABASE` 权限 `master` 。 始终可以在中查看调用方连接到的数据库 `sys.databases` 。  
  
> [!IMPORTANT]  
> 默认情况下，公共角色具有 `VIEW ANY DATABASE` 权限，允许所有登录名查看数据库信息。 阻止登录名检测数据库、 `REVOKE` `VIEW ANY DATABASE` 权限 `public` 或 `DENY` `VIEW ANY DATABASE` 单个登录名的权限。  
  
## <a name="azure-sql-database-remarks"></a>Azure SQL 数据库备注

在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 此视图中， `master` 数据库和用户数据库中提供了此视图。 在 `master` 数据库中，此视图返回 `master` 服务器上数据库和所有用户数据库的信息。 在用户数据库中，该视图仅返回有关当前数据库及 master 数据库的信息。  
  
 使用正在其中创建新数据库的 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 服务器的 `master` 数据库中的 `sys.databases` 视图。 数据库复制开始后，你可以 `sys.databases` `sys.dm_database_copies` 从目标服务器的数据库中查询和视图， `master` 以检索有关复制进度的详细信息。  
  
## <a name="examples"></a>示例  
  
### <a name="a-query-the-sysdatabases-view"></a>A. 查询 sys.databases 视图

下面的示例返回视图中可用的几个列 `sys.databases` 。  
  
```sql  
SELECT name, user_access_desc, is_read_only, state_desc, recovery_model_desc  
FROM sys.databases;  
```  
  
### <a name="b-check-the-copying-status-in-sssds"></a>B. 检查 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 中的复制状态

下面的示例查询 `sys.databases` 和 `sys.dm_database_copies` 视图以返回有关数据库复制操作的信息。  
  
适用于：[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql
-- Execute from the master database.  
SELECT a.name, a.state_desc, b.start_date, b.modify_date, b.percent_complete  
FROM sys.databases AS a  
INNER JOIN sys.dm_database_copies AS b ON a.database_id = b.database_id  
WHERE a.state = 7;  
```

### <a name="c-check-the-temporal-retention-policy-status-in-sssds"></a>C. 检查临时保留策略状态 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]

下面的示例将查询 `sys.databases` 以返回有关是否已启用时态保留清理任务的信息。 请注意，还原操作后，默认情况下会禁用临时保留。 使用 `ALTER DATABASE` 显式启用它。
  
适用于：[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
-- Execute from the master database.  
SELECT a.name, a.is_temporal_history_retention_enabled 
FROM sys.databases AS a;
```  
  
## <a name="next-steps"></a>后续步骤

- [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)
- [sys.database_mirroring_witnesses (Transact-SQL)](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)
- [sys.database_recovery_status &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-database-recovery-status-transact-sql.md)
- [数据库和文件目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)
- [sys.dm_database_copies（Azure SQL 数据库）](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md)  

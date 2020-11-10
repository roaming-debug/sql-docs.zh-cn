---
title: SQL Server 2016 发行说明 | Microsoft Docs
description: 本发行说明文档介绍了在安装 Microsoft SQL Server 2016 版本或者解决其相关问题之前，应该了解的一些已知问题。
ms.date: 04/25/2018
ms.prod: sql
ms.reviewer: ''
ms.custom: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- build notes
- release issues
ms.assetid: c64077a2-bec8-4c87-9def-3dbfb1ea1fb6
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 318dfd44b3c931b124372ce5ea60681b8c1e7e99
ms.sourcegitcommit: 442fbe1655d629ecef273b02fae1beb2455a762e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93235481"
---
# <a name="sql-server-2016-release-notes"></a>SQL Server 2016 发行说明
[!INCLUDE [SQL Server 2016](../includes/applies-to-version/sqlserver2016.md)]  
  本主题介绍 SQL Server 2016 版本的限制和问题，包括服务包。 有关新增功能的信息，请参阅 [《What's New in SQL Server 2016》](./what-s-new-in-sql-server-2016.md)（SQL Server 2016 的新增功能）。

- [![从评估中心下载](../includes/media/download2.png)](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016) 从 **[评估中心](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)下载 SQL Server 2016**
- [![Azure 虚拟机小](../includes/media/azure-vm.png)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=Overview) 是否拥有 Azure 帐户？  然后转到 **[此处](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=Overview)** ，启动装有 SQL Server 2016 SP1 的虚拟机。
- [![下载 SSMS](../includes/media/download2.png)](../ssms/download-sql-server-management-studio-ssms.md) 若要获取最新版本的 Management Studio，请参阅 **[下载 SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)** 。

## <a name="sql-server-2016-service-pack-2-sp2"></a><a name="bkmk_2016sp2"></a>SQL Server 2016 Service Pack 2 (SP2)

![info_tip](../sql-server/media/info-tip.png) SQL Server 2016 SP2 包括 2016 SP1 之后、CU8 之前（含 CU8）发布的所有累积更新。

- [![Microsoft 下载中心](../includes/media/download2.png)](https://www.microsoft.com/download/details.aspx?id=56836) [下载 SQL Server 2016 Service Pack 2 (SP2)](https://www.microsoft.com/download/details.aspx?id=56836)
- 有关更新的完整列表，请参阅 [SQL Server 2016 Service Pack 2 版本信息](https://support.microsoft.com/help/4052908/sql-server-2016-service-pack-2-release-information)

SQL Server 2016 SP2 安装可能需要在安装后重新启动。 作为最佳做法，建议在安装 SQL Server 2016 SP2 后计划并执行重新启动。

SQL Server 2016 SP2 中包括与性能和缩放相关的改进。

|Feature|说明|详细信息|
|---|---|---|
|分发数据库清除过程已改进   |   分发数据库表过大导致出现阻塞和死锁情况。 改进的清理过程旨在消除其中某些阻塞或死锁情况的发生。   |   [KB4040276](https://support.microsoft.com/help/4040276/fix-indirect-checkpoints-on-the-tempdb-database-cause-non-yielding)   |
|更改跟踪清除   |   改进了更改跟踪端表的更改跟踪清除性能和效率。   |   [KB4052129](https://support.microsoft.com//help/4052129/update-for-manual-change-tracking-cleanup-procedure-in-sql-server-2016)   |
|利用 CPU 超时来取消 Resource Governor 请求   |   如果已达到请求的 CPU 阈值，则通过实际取消请求改进查询请求的处理工作。 已在跟踪标志 2422 下启用此行为。   |   [KB4038419](https://support.microsoft.com/help/4038419/add-cpu-timeout-to-resource-governor-request-max-cpu-time-sec)   |
|用于在文件组中创建目标表的 SELECT INTO 语句   |   从 SQL Server 2016 SP2 开始，SELECT INTO T-SQL 语法支持在 T-SQL 语法中使用 ON <Filegroup name> 关键字将表加载到用户默认文件组以外的文件组。   |      |
|TempDB 间接检查点已改进   |   已对 TempDB 的间接检查点进行改进，从而最大程度地减少 DPList 上的旋转锁争用。 此项改进让 SQL Server 2016 上的 TempDB 工作负载可以在为 TempDB 启用了间接检查点功能的情况下向外扩展。   |   [KB4040276](https://support.microsoft.com/help/4040276)   |
|大内存计算机上的数据库备份性能已改进   |   SQL Server 2016 SP2 优化了备份过程中对正在进行的 I/O 的排出方式，从而显著提高了中小型数据库的备份性能。 在 2TB 计算机上进行系统数据库备份时，可发现改进后的性能较以前提高了 100 倍以上。 当备份页面和备份 I/O 占用的时间超过缓冲池迭代时，数据库大小将增加，此时性能提高的幅度会降低。 对于在具备较大内存的大型高端服务器上托管多个小型数据库的客户，此项更改有助于提升备份性能。   |      |
|为启用了 TDE 的数据库提供的 VDI 备份压缩支持   |   SQL Server 2016 SP2 添加了 VDI 支持，允许 VDI 备份解决方案将压缩功能用于启用了 TDE 的数据库。 借助此项改进，引进了新的备份格式，为启用了 TDE 的数据库提高备份压缩支持。 SQL Server 引擎将以透明的方式处理新的和旧的备份格式，从而还原备份。   |      |
|复制代理配置文件参数的动态加载   |   凭借此项增强功能，可在不重启代理的情况下动态加载复制代理参数。 此更改仅适用于最常用的代理配置文件参数。   |      |
|支持用于统计信息创建/更新的 MAXDOP 选项   |    此项增强功能允许为 CREATE/UPDATE 统计信息语句指定 MAXDOP 选项，并确保创建或重新生成所有类型索引过程中，在更新统计信息时采用的是正确的 MAXDOP 设置（如果存在 MAXDOP 选项）   |   [KB4041809](https://support.microsoft.com/help/4041809)   |
|增量统计信息的自动更新统计信息功能已改进   |    在某些情况下，如果表中的多个分区出现大量数据更改，递增统计信息的总修改计数器超过自动更新阈值，但是任何单独分区都未超过自动更新阈值，那么统计信息更新可能会推迟，直到该表中出现更多的修改。 已在跟踪标志 11024 下更正此行为。   |      |

SQL Server 2016 SP2 中包含与可支持性和诊断相关的改进。

|Feature|说明|详细信息|
|---|---|---|
|对可用性组中数据库的完整 DTC 支持   |   SQL Server 2016 目前不支持可用性组中数据库的跨数据库事务。 我们在 SQL Server 2016 SP2 中引入了对可用性组数据库的分布式事务的完整支持。   |      |
|更新到 sys.databases is_encrypted 列，准确反映 TempDB 的加密状态   |   对于 TempDB，sys.databases 中 is_encryptedcolumn 列的值为 1，即使是在关闭所有用户数据库的加密并重新启动 SQL Server 后也一样。 由于 TempDB 在这种情况下不再处于加密状态，因此该列中的值本该为 0。 从 SQL Server 2016 SP2 开始，sys.databases.is_encrypted 现可准确反映 TempDB 的加密状态。   |      |
|用于生成已验证克隆和备份的新 DBCC CLONEDATABASE 选项   |   借助 SQL Server 2016 SP2，DBCC CLONEDATABASE 允许两个新选项：生成已验证克隆，或生成备份克隆。 使用 WITH VERIFY_CLONEDB 选项创建克隆数据库时，将创建并验证一致的数据库克隆，Microsoft 会为此提供支持，以便生产使用。 引进了新的属性来验证克隆是否已通过验证 SELECT DATABASEPROPERTYEX(‘clone_database_name’, ‘IsVerifiedClone’)。 使用 BACKUP_CLONEDB 选项创建克隆时，会在数据文件所在的同一文件夹中生成备份，以便客户将克隆移至不同的服务器或将其发送至 Microsoft 客户支持部门 (CSS) 进行故障排除。   |      |
|DBCC CLONEDATABASE 的 Service Broker (SSB) 支持   |   已增强 DBCC CLONEDATABASE 命令，允许 SSB 对象的脚本编写。   |   [KB4092075](https://support.microsoft.com/help/4092075)   |
|用于监视 TempDB 版本存储空间使用情况的新 DMV   |   SQL Server 2016 SP2 中引入了新的 sys.dm_tran_version_store_space_usage DMV，用于监视 TempDB 的版本存储使用情况。 DBA 现可根据每个数据库的版本存储使用情况要求主动地规划 TempDB 大小，且在生产服务器上运行时无需任何性能开销。   |      |
|对复制代理的完全转储支持 | 现在，如果复制代理遇到未经处理的异常，则会默认创建异常现象的微型转储。 这使得难以对未经处理的异常问题进行故障排除。 通过此更改，我们引入了新的注册表项，这将允许为复制代理创建完全转储。   |      |
|针对可用性组的读取路由故障的扩展事件增强   |   在之前，如果存在路由列表但是其中没有可用于连接的服务器，则会激发 read_only_rout_fail xEvent。 SQL Server 2016 SP2 包含有助于故障排除的其他信息，并且还扩展了激发此 xEvent 的码位。   |      |
|用于监视事务日志的新 DMV |   新增了 DMV sys.dm_db_log_stats，用于返回有关数据库的事务日志文件的摘要级别属性和信息。   |      |
|用于监视 VLF 信息的新 DMV |   SQL Server 2016 SP2 引入了新的 DMV sys.dm_db_log_info，可公开类似于 DBCC LOGINFO 的 VLF 信息，用于监视、警报和避免客户遭遇潜在的 T 日志问题。   |      |
|sys.dm_os_sys_info 中的处理器信息|   向 sys.dm_os_sys_info DMV 添加了新的列，用于公开与处理器相关的信息（如 socket_count 和 cores_per_numa）。   |      |
|sys.dm_db_file_space_usage 中的盘区修改信息|   向 sys.dm_db_file_space_usage 添加了新的列，用于跟踪自上次完整备份以来修改的盘区数量。   |      |
|sys.dm_exec_query_stats 中的段信息   |   向 sys.dm_exec_query_stats 添加了新的列，用于跟踪跳过和读取的列存储段数（如 total_columnstore_segment_reads 和 total_columnstore_segment_skips）。   |   [KB4051358](https://support.microsoft.com/help/4051358)   |
|为分发数据库设置正确的兼容级别   |   安装 Service Pack 后，分发数据库的兼容级别会更改为 90。 这是由于 sp_vupgrade_replication 存储过程中的代码路径。 SP 现已改为设置分发数据库的正确兼容级别。   |      |
|公开已知的最新良好 DBCC CHECKDB 信息   |   添加了新的数据库选项，从而能以编程方式返回最新一次成功运行 DBCC CHECKDB 的日期。 用户现可查询 DATABASEPROPERTYEX([database], ‘lastgoodcheckdbtime’)，获取单个值，该值代表上一次在指定数据库上成功运行 DBCC CHECKDB 的日期/时间。   |      |
|显示计划 XML 增强|   [关于使用哪些统计信息编译查询计划的信息](/archive/blogs/sql_server_team/sql-server-2017-showplan-enhancements)，其中包括统计信息名称、修改计数器、采样百分比以及统计信息的上次更新时间。 请注意，这仅添加至 CE 模型 120 及更高版本。 例如，CE 70 并不支持。| |
| |如果查询优化器采用“行目标”逻辑，则向显示计划 XML 添加新属性 [EstimateRowsWithoutRowgoal](/archive/blogs/sql_server_team/more-showplan-enhancements-row-goal)。| |
| |实际显示计划 XML 中的新运行时属性 [UdfCpuTime and UdfElapsedTime](/archive/blogs/sql_server_team/more-showplan-enhancements-udfs)，用于跟踪标量的用户定义函数 (UDF) 所花费的时间。| |
| |在实际显示计划 XML 中将 CXPACKET 等待类型添加到[可能的前 10 等待列表](/archive/blogs/sql_server_team/new-showplan-enhancements) - 并行查询的执行经常涉及 CXPACKET 等待，但是实际显示计划 XML 中不会报告这种类型的等待。   |      |
| |扩展运行时溢出警告，从而报告在并行运算符溢出期间写入到 TempDB 的页数。| |
|对使用补充字符排序规则的数据库的复制支持   |   现已对使用补充字符排序规则的数据库提供复制支持。   |      |
|通过可用性组故障转移，对 Service Broker 进行正确的处理   |   在目前的执行情况中，如果在可用性组数据库上启用了 Service Broker，则 AG 故障转移期间，主要副本发起的所有 Service Broker 连接均保持打开状态。 在 AG 故障转移期间，这一改进会关闭所有此类打开的连接。   |      |
|通过添加   |   新的 [CXCONSUMER](/archive/blogs/sql_server_team/making-parallelism-waits-actionable) 等待，改进了并行等待故障排除。   |      |
|已针对相同信息改进 DMV 之间的一致性   |   sys.dm_exec_session_wait_stats DMV 现在跟踪与 sys.dm_os_wait_stats DMV 一致的 CXPACKET 和 CXCONSUMER 等待。   |      |
|查询内并行死锁的故障排除已改进 | 在 xEvent 字段名称 worktable_physical_writes 中添加了新的 exchange_spill 扩展事件，用于报告并行运算符溢出期间写入到 TempDB 的页数。| |
| |sys.dm_exec_query_stats、sys.dm_exec_procedure_stats 和 sys.dm_exec_trigger_stats DMV 中的溢出列（如 total_spills）现也包括并行运算符溢出的数据。| |
| |已针对并行死锁情况对 XML 死锁图进行改进，将更多属性添加到了 exchangeEvent 资源。| |
| |已针对涉及批处理模式运算符的死锁对 XML 死锁图进行改进，将更多属性添加到了 SyncPoint 资源。| |
|部分复制代理配置文件参数的动态重载   |   在目前复制代理的执行情况中，代理配置文件参数中产生的任何更改都要求停止并重启代理。 此改进允许在不重启复制代理的情况下动态重载参数。   |      |

![水平条的屏幕截图。](media/horizontal-bar.png)

## <a name="sql-server-2016-service-pack-1-sp1"></a><a name="bkmk_2016sp1"></a>SQL Server 2016 Service Pack 1 (SP1)
![info_tip](../sql-server/media/info-tip.png) SQL Server 2016 SP1 包含至 SQL Server 2016 RTM CU3 的所有累积更新，包括安全更新 MS16-136。 它包含 SQL Server 2016 累积更新（并包含最新累积更新 - CU3 和 2016 年 11 月 8 日发布的安全更新 MS16-136）中提供的解决方案的汇总。

以下功能在 SQL Server SP1 的标准、Web、Express 和 Local DB 版中均可用（另有注明的除外）：
- Always Encrypted
- 更改数据捕获（在 Express 版中不可用）
- 列存储
- 压缩
- 动态数据掩码
- 精细审核
- 内存中 OLTP（在 Local DB 版中不可用）
- 多文件流容器（在 Local DB 版中不可用）
- 分区
- PolyBase
- 行级别安全性

下表总结了 SQL Server 2016 SP1 中提供的重要改进。

|Feature|说明|详细信息|
|---|---|---|
|在 TF 715 下与自动 TABLOCK 成堆的大容量插入| 跟踪标志 715 为没有非聚集索引的堆中的大容量加载操作启用表锁。|[Migrating SAP workloads to SQL Server just got 2.5x faster](/archive/blogs/sql_server_team/migrating-sap-workloads-to-sql-server-just-got-2-5x-faster)（将 SAP 工作负荷迁移到 SQL Server 速度加快了 2.5 倍）|
|CREATE 或 ALTER|部署存储过程、触发器、用户定义的函数和视图等对象。|[SQL Server 数据库引擎博客](/archive/blogs/sqlserverstorageengine/create-or-alter-another-great-language-enhancement-in-sql-server-2016-sp1)|
|对复制的 DROP TABLE 支持|DROP TABLE DDL 支持复制，以允许删除复制文章。|[KB 3170123](https://support.microsoft.com/help/3170123/supports-drop-table-ddl-for-articles-that-are-included-in-transactiona)|
|文件流 RsFx 驱动程序签名|文件流 RsFx 驱动程序使用 Windows 硬件开发人员中心仪表板门户（开发门户）签名和认证，从而允许在 Windows Server 2016/Windows 10 上毫无问题地安装 SQL Server 2016 SP1 文件流 RsFx 驱动程序。|[Migrating SAP workloads to SQL Server just got 2.5x faster](/archive/blogs/sql_server_team/migrating-sap-workloads-to-sql-server-just-got-2-5x-faster)（将 SAP 工作负荷迁移到 SQL Server 速度加快了 2.5 倍）|
|SQL 服务帐户的 LPIM - 编程识别|允许 DBA 以编程方式识别服务启动时内存中锁定页面 (LPIM) 特权是否生效。|[Developers Choice:Programmatically identify LPIM and IFI privileges in SQL Server](/archive/blogs/sql_server_team/developers-choice-programmatically-identify-lpim-and-ifi-privileges-in-sql-server)（开发人员选择：以编程方式识别 SQL Server 中的 LPIM 和 IFI 特权）|
|手动更改跟踪清除|新存储过程根据需要清除更改跟踪内部表。| [KB 3173157](https://support.microsoft.com/help/3173157/adds-a-stored-procedure-for-the-manual-cleanup-of-the-change-tracking)|
|本地临时表的并行 INSERT...SELECT更改|INSERT..SELECT 操作中的新并行插入。|[SQL Server Customer Advisory Team](/archive/blogs/sqlcat/real-world-parallel-insert-what-else-you-need-to-know)（SQL Server 客户咨询团队）|
|Showplan XML|扩展诊断包括授予警告和针对查询启用的最大内存、启用的跟踪标志，并且还显示其他诊断信息。 | [KB 3190761](https://support.microsoft.com/help/3190761/update-to-improve-diagnostics-by-expose-data-type-of-the-parameters-fo)|
|存储类内存|在 Windows Server 2016 中使用存储类内存推进事务处理，带来的结果是事务提交时间加快了几个数量级。|[SQL Server 数据库引擎博客](/archive/blogs/sqlserverstorageengine/transaction-commit-latency-acceleration-using-storage-class-memory-in-windows-server-2016sql-server-2016-sp1)|
|USE HINT|使用查询选项 `OPTION(USE HINT('<option>'))` 通过受支持的查询级别提示更改查询优化器行为。 与 QUERYTRACEON 不同，USE HINT 选项不需要 sysadmin 特权。|[Developers Choice:USE HINT query hints](/archive/blogs/sql_server_team/developers-choice-use-hint-query-hints)（开发人员选择：USE HINT 查询提示）|
|XEvent 添加件|新 XEvent 和 Perfmon 诊断功能改进了延迟的故障排除。|[扩展事件](../relational-databases/extended-events/extended-events.md)|

此外，请注意以下修补程序：
- 根据来自 DBA 和 SQL 社区的反馈，启动 SQL 2016 SP1，Hekaton 日志记录消息会减少至最少。
- 评审新的[跟踪标志](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。
- WideWorldImporters 示例数据库的完整版本现在可用于 Standard Edition 和 Express Edition，启动 SQL Server 2016 SP1，在 [Github]( https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0) 中可用。 示例中无需任何更改。 在 RTM 中创建的 Enterprise Edition 数据库备份可用于 SP1 中的 Standard 和 Express 版。

SQL Server 2016 SP1 安装可能需要重新启动后安装。 作为最佳做法，建议在安装 SQL Server 2016 SP1 后计划并执行重新启动。

### <a name="download-pages-and-more-information"></a>下载页和详细信息

- [下载 Microsoft SQL Server 2016 Service Pack 1](https://www.microsoft.com/download/details.aspx?id=54276)
- [SQL Server 2016 Service Pack 1 (SP1) 已发布](/archive/blogs/sqlreleaseservices/sql-server-2016-service-pack-1-sp1-released)
- [SQL Server 2016 Service Pack 1 的发布信息](https://support.microsoft.com/kb/3182545)
- ![info_tip](../sql-server/media/info-tip.png) 有关所有受支持版本的链接和信息（包括 [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] 的服务包），请参阅 [SQL Server 更新中心](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

![水平条的另一个屏幕截图。](media/horizontal-bar.png)

##  <a name="sql-server-2016-release---general-availability-ga"></a><a name="bkmk_2016_ga"></a> SQL Server 2016 Release - General Availability (GA)
-   [数据库引擎 (GA)](#bkmk_ga_instalpatch)
-   [Stretch Database (GA)](#bkmk_ga_stretch)
-   [查询存储 (GA)](#bkmk_ga_query_store)
-   [产品文档 (GA)](#bkmk_ga_docs)

### <a name="repl_icon_warn--install-patch-requirement-ga"></a>![repl_icon_warn](../database-engine/availability-groups/windows/media/repl-icon-warn.gif) <a name="bkmk_ga_instalpatch"></a> 安装修补程序要求 (GA)
**问题及其对客户的影响：** Microsoft 已发现影响 Microsoft VC++ 2013 运行时二进制文件的一个问题，这些二进制文件是作为 SQL Server 2016 系统必备进行安装的。 现在有可用的更新来修复该问题。 如果未安装适用于该 VC 运行时二进制文件的更新，则在某些情况下 SQL Server 2016 可能会遇到稳定性问题。 在安装 SQL Server 2016 之前，请检查计算机是否需要 [KB 3164398](https://support.microsoft.com/kb/3164398)中所述的修补程序。 此修补程序也包含在 [SQL Server 2016 RTM 的累积更新包 1 (CU1)](https://www.microsoft.com/download/details.aspx?id=53338) 中。

**解决方法：** 使用以下任一解决方案：

- 安装 [KB 3138367 - Visual C++ 2013 和 Visual C++ 可再发行包的更新](https://support.microsoft.com/kb/3138367)。 KB 是首选的解决方案。 可以在安装 SQL Server 2016 之前或之后安装此更新。

    如果已安装 SQL Server 2016，请按顺序执行以下步骤：

    1.  下载合适的 *vcredist_\*exe* 。
    1.  停止数据库引擎的所有实例的 SQL Server 服务。
    1.  安装 **KB 3138367** 。
    1.  重新启动计算机。


 - 安装  [KB 3164398 - 适用于 SQL Server 2016 MSVCRT 系统必备的关键更新](https://support.microsoft.com/kb/3164398)。

    如果使用 **KB 3164398** ，可以在 SQL Server 安装过程中、通过 Microsoft 更新或从 Microsoft 下载中心进行安装。

    - **在 SQL Server 2016 安装过程中：** 如果运行 SQL Server 安装程序的计算机具有 Internet 访问权限，那么作为整个 SQL Server 安装的一部分，SQL Server 安装程序将检查更新。 如果接受更新，安装程序将下载二进制文件并在安装过程中对文件进行更新。

    - **Microsoft 更新：** 该更新作为 SQL Server 2016 的关键非安全更新，可从 Microsoft 更新获取。 通过 Microsoft 更新进行安装，更新后 SQL Server 2016 需要重新启动服务器。

    - **下载中心：** 最后，可从 Microsoft 下载中心获取该更新。 可以在安装 SQL Server 2016 之后下载更新软件并安装在服务器上。


### <a name="stretch-database"></a><a name="bkmk_ga_stretch"></a>Stretch Database

#### <a name="problem-with-a-specific-character-in-a-database-or-table-name"></a>与数据库或表名称中的特定字符有关的问题

**问题及其对客户的影响：** 尝试对数据库或表启用 Stretch Database 将失败，并出现错误。 将对象名称从小写转换为大写时，如果名称中包含被视为其他字符的一个字符，则会出现此问题。 导致此问题的一个示例字符是字符“ƒ”（通过键入 ALT+159 创建）。

**解决方法：** 如果想要对数据库或表启用 Stretch Database，唯一的方法是重命名对象并删除问题字符。

#### <a name="problem-with-an-index-that-uses-the-include-keyword"></a>与使用 INCLUDE 关键字的索引有关的问题

**问题及其对客户的影响：** 如果表的一个索引使用 INCLUDE 关键字以包含索引中的其他列，那么尝试对该表启用 Stretch Database 将失败并显示错误。

**解决方法：** 删除使用 INCLUDE 关键字的索引，对表启用 Stretch Database，然后重新创建索引。 如果这样做，请务必遵从组织的维护实践和策略，以确保对受影响的表的用户的影响最小或没有影响。

### <a name="query-store"></a><a name="bkmk_ga_query_store"></a>Query Store

#### <a name="problem-with-automatic-data-cleanup-on-editions-other-than-enterprise-and-developer"></a>与版本的自动数据清理有关的问题（不包括企业版和开发人员版）

 **问题及其对客户的影响：** 对版本的自动数据清理失败（不包括企业版和开发人员版）。
因此，如果不手动清除数据，查询存储使用的空间将不断增长，直至达到配置的限制为止。 如果此问题未得到修复，那么为错误日志分配的磁盘空间也将被填满，因为每次尝试执行清理都将生成一个转储文件。 清理激活期限取决于工作负荷频率，但不会超过 15 分钟。

 **解决方法：** 如果计划对企业版和开发人员版以外的版本使用查询存储，则需要显式关闭清理策略。 可通过 SQL Server Management Studio（数据库属性页）或 Transact-SQL 脚本来完成该操作：

```ALTER DATABASE <database name> SET QUERY_STORE (OPERATION_MODE = READ_WRITE, CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 0), SIZE_BASED_CLEANUP_MODE = OFF)```

此外，可以考虑手动清除以防止查询存储转变为只读模式。 例如，运行以下查询以定期清除整个数据空间：

```ALTER DATABASE <database name> SET QUERY_STORE CLEAR```

此外，定期执行下面的查询存储的存储过程，以清理运行时统计信息、特定查询或计划：

- `sp_query_store_reset_exec_stats`

- `sp_query_store_remove_plan`

- `sp_query_store_remove_query`


###  <a name="product-documentation-ga"></a><a name="bkmk_ga_docs"></a> 产品文档 (GA)
 **问题及其对客户的影响：** 尚未提供可下载的 SQL Server 2016 文档版本。 在使用帮助库管理器尝试联机安装内容时，你将看到 SQL Server 2012 和 SQL Sever 2014 文档，但没有 SQL Server 2016 文档的选项  。

 **解决方法：** 使用以下任一解决方法暂时解决此问题：

 ![管理 SQL Server 的帮助设置](../sql-server/media/docs-sql2016-managehelpsettings.png "管理 SQL Server 的帮助设置")

-   使用选项“选择联机或本地帮助”，然后配置“我想要使用联机帮助”对应的帮助选项。 

-   使用选项“联机安装内容”并下载 SQL Server 2014 Content。 

 **F1 帮助：** 按照设计，在 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 中按 F1 时，将在浏览器中显示 F1 帮助文章的联机版本。 此问题在于基于浏览器的帮助，即使已配置并安装了本地帮助也不例外。

**更新内容：** 在 SQL Server Management Studio 和 Visual Studio 中，Help Viewer 应用程序可能会在添加文档的过程中停止响应。 若要解决此问题，请完成下列步骤。 有关此问题的详细信息，请参阅 [《Visual Studio 帮助查看器冻结》](/previous-versions/mt654096(v=vs.140))。

* 在记事本中打开 %LOCALAPPDATA%\Microsoft\HelpViewer2.2\HlpViewer_SSMS16_en-US.settings |HlpViewer_VisualStudio14_en US.settings 文件，并将下面代码中的日期更改为在将来的某个日期。

```
     Cache LastRefreshed="12/31/2017 00:00:00"
```

## <a name="additional-information"></a>其他信息
+ [SQL Server 2016 安装](../database-engine/install-windows/install-sql-server.md)
+ [SQL Server 更新中心 - 链接和有关所有受支持版本的信息](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

![MS_Logo_X-Small](../sql-server/media/ms-logo-x-small.png "MS_Logo_X-Small")
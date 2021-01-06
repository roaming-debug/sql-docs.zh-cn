---
title: 服务器配置选项 (SQL Server)
description: 了解如何管理和优化 SQL Server 资源。 查看可用的配置选项、可能的设置、默认值和重启要求。
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
keywords:
- 服务器配置 (SQL Server)
helpviewer_keywords:
- surface area configuration [SQL Server], sp_configure
- configuration options [SQL Server], when take effect
- server management [SQL Server], configuration options
- SQL Server Management Studio [SQL Server], servers
- servers [SQL Server], configuring
- configuration options [SQL Server], setting
- options [SQL Server], configuration
- RECONFIGURE statement
- performance [SQL Server], servers
- configuration options [SQL Server]
- RECONFIGURE WITH OVERRIDE statement
- SQL Server, configuring
- sp_configure
- stored procedures [SQL Server], configuration options
- server configuration [SQL Server]
- administering SQL Server, configuration options
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: ''
ms.date: 07/20/2020
ms.openlocfilehash: bcadbc294414b2fe992d3864be240a1998a5e477
ms.sourcegitcommit: a81823f20262227454c0b5ce9c8ac607aaf537e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/18/2020
ms.locfileid: "97684220"
---
# <a name="server-configuration-options-sql-server"></a>服务器配置选项 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

您可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 sp_configure 系统存储过程通过配置选项来管理和优化 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 资源。 大多数常用的服务器配置选项可以通过 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]来使用；而所有配置选项都可通过 sp_configure 来访问。 在设置这些选项之前应该认真考虑这些选项对系统的影响。 有关详细信息，请参阅[查看或更改服务器属性 (SQL Server)](../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md)。

>**重要说明!!** 高级选项只能由有经验的数据库管理员或认证的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 技术人员更改。

## <a name="categories-of-configuration-options"></a>配置选项的分类

配置选项在下面两种情况下生效：

- 在设置选项并发出 **RECONFIGURE** （在某些情况下为 **RECONFIGURE WITH OVERRIDE**）语句之后立即生效。 重新配置某些选项可使计划缓存中的计划失效，并编译新计划。 有关详细信息，请参阅 [DBCC FREEPROCCACHE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md)。

  \- 或 -

- 执行以上操作并重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例之后生效。

需要重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的选项最初只在 value 列中显示更改后的值。 在重新启动后，新值将出现在 value 列和 value_in_use 列中。

但有些选项需要在重新启动服务器后，新的配置值才能生效。 如果设置了新值并在没有重新启动服务器的情况下运行 sp_configure，则新值将出现在配置选项的“value”列中，而不是出现在“value_in_use”列中。 重新启动服务器之后，新值出现在“value_in_use”列中。

自配置选项是指 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 根据系统需要进行调整的选项。 大多数情况下，这使您无需手动设置值。 例如，“最大工作线程数”选项和“用户连接数”选项。

## <a name="configuration-options-table"></a>配置选项表
 下表列出了所有可用的配置选项、可能的设置范围及其默认值。 配置选项按以下字母代码标记：

- A = 高级选项，仅应由有经验的数据库管理员或认证的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 专业人员更改，并且需要将“显示高级选项”设置为 1。

- RR = 需要重新启动 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的选项。

- RP = 需要重启 PolyBase 引擎的选项。

- SC = 自配置选项。

| 配置选项 | 最小值 | 最大值 | 默认 |
|--|--|--|--|
| [访问检查缓存桶计数](../../database-engine/configure-windows/access-check-cache-server-configuration-options.md) (A) | 0 | 16384 | 0 |
| [访问检查缓存配额](../../database-engine/configure-windows/access-check-cache-server-configuration-options.md) (A) | 0 | 2147483647 | 0 |
| [即席分布式查询](../../database-engine/configure-windows/ad-hoc-distributed-queries-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [ADR 清除程序重试超时（分钟）](../../database-engine/configure-windows/adr-cleaner-retry-timeout-configuration-option.md)<br><br> 在 SQL Server 2019 中引入 | 0 | 32767 | 15 |
| [ADR 预先分配因素](../../database-engine/configure-windows/adr-preallocation-factor-server-configuration-option.md)<br><br> 在 SQL Server 2019 中引入 | 0 | 32767 | 4 |
| [affinity I/O mask](../../database-engine/configure-windows/affinity-input-output-mask-server-configuration-option.md) （A，RR） | -2147483648 | 2147483647 | 0 |
| [affinity mask](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md) (A) | -2147483648 | 2147483647 | 0 |
| [affinity64 I/O mask](../../database-engine/configure-windows/affinity64-input-output-mask-server-configuration-option.md) （A，仅适用于 64 位版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]） | -2147483648 | 2147483647 | 0 |
| [affinity64 mask](../../database-engine/configure-windows/affinity64-mask-server-configuration-option.md) （A，RR），仅适用于 64 位版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] | -2147483648 | 2147483647 | 0 |
| [代理 XP](../../database-engine/configure-windows/agent-xps-server-configuration-option.md) (A) | 0 | 1 | 0<br /><br /> （当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理启动时，更改为 1。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理设置为在安装过程中自动启动，则默认值为 0。） |
| [允许 PolyBase 导出](../../database-engine/configure-windows/allow-polybase-export.md)<br/><br/> [!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)] 列中的一个值匹配。| 0 | 1 | 0 |
| [允许更新](../../database-engine/configure-windows/allow-updates-server-configuration-option.md) （已过时。 请勿使用。 将在重新配置期间导致错误。） | 0 | 1 | 0 |
| [自动 soft-NUMA 已禁用](soft-numa-sql-server.md) | 0 | 1 | 0 |
| [备份校验和默认值](../../database-engine/configure-windows/backup-checksum-default.md) | 0 | 1 | 0 |
| [backup compression default](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md) | 0 | 1 | 0 |
| [blocked process threshold](../../database-engine/configure-windows/blocked-process-threshold-server-configuration-option.md) (A) | 5 | 86400 | 0 |
| [c2 审核模式](../../database-engine/configure-windows/c2-audit-mode-server-configuration-option.md) （A，RR） | 0 | 1 | 0 |
| [clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) | 0 | 1 | 0 |
| [clr 严格安全性](../../database-engine/configure-windows/clr-strict-security.md) (A) <br /> [!INCLUDE [sqlserver2017](../../includes/applies-to-version/sqlserver2017.md)] 列中的一个值匹配。 | 0 | 1 | 0 |
| [列加密 enclave 类型](../../database-engine/configure-windows/configure-column-encryption-enclave-type.md)（A、RR） | 0 | 1 | 0 |
| [common criteria compliance enabled](../../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md) （A，RR） | 0 | 1 | 0 |
| [contained database authentication](../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md) | 0 | 1 | 0 |
| [并行的开销阈值](../../database-engine/configure-windows/configure-the-cost-threshold-for-parallelism-server-configuration-option.md) (A) | 0 | 32767 | 5 |
| [cross db ownership chaining](../../database-engine/configure-windows/cross-db-ownership-chaining-server-configuration-option.md) | 0 | 1 | 0 |
| [cursor threshold](../../database-engine/configure-windows/configure-the-cursor-threshold-server-configuration-option.md) (A) | -1 | 2147483647 | -1 |
| [Database Mail XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [default full-text language](../../database-engine/configure-windows/configure-the-default-full-text-language-server-configuration-option.md) (A) | 0 | 2147483647 | 2052 |
| [default language](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) | 0 | 9999 | 0 |
| [default trace enabled](../../database-engine/configure-windows/default-trace-enabled-server-configuration-option.md) (A) | 0 | 1 | 1 |
| [disallow results from triggers](../../database-engine/configure-windows/disallow-results-from-triggers-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [EKM provider enabled](../../database-engine/configure-windows/ekm-provider-enabled-server-configuration-option.md) | 0 | 1 | 0 |
| [外部脚本已启用](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md) (SC) (RR)<br /><br />[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)] 列中的一个值匹配。 | 0 | 1 | 0 |
| [filestream_access_level](../../database-engine/configure-windows/filestream-access-level-server-configuration-option.md) | 0 | 2 | 0 |
| [填充因子](../../database-engine/configure-windows/configure-the-fill-factor-server-configuration-option.md) （A，RR） | 0 | 100 | 0 |
| [FT 爬网带宽（最大值）](../../database-engine/configure-windows/ft-crawl-bandwidth-server-configuration-option.md)(A) | 0 | 32767 | 100 |
| [FT 爬网带宽(最小值)](../../database-engine/configure-windows/ft-crawl-bandwidth-server-configuration-option.md) (A) | 0 | 32767 | 0 |
| [FT 通知带宽（最大值）](../../database-engine/configure-windows/ft-notify-bandwidth-server-configuration-option.md)(A) | 0 | 32767 | 100 |
| [FT 通知带宽（最小值）](../../database-engine/configure-windows/ft-notify-bandwidth-server-configuration-option.md)(A) | 0 | 32767 | 0 |
| [Hadoop 连接](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md) (RP)<br /><br />[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)] 列中的一个值匹配。 | 0 | 7 | 0 |
| [in-doubt xact resolution](../../database-engine/configure-windows/in-doubt-xact-resolution-server-configuration-option.md) (A) | 0 | 2 | 0 |
| [index create memory](../../database-engine/configure-windows/configure-the-index-create-memory-server-configuration-option.md) （A，SC） | 704 | 2147483647 | 0 |
| [lightweight pooling](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md) （A，RR） | 0 | 1 | 0 |
| [locks](../../database-engine/configure-windows/configure-the-locks-server-configuration-option.md) （A，RR，SC） | 5000 | 2147483647 | 0 |
| [max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) (A) | 0 | 32767 | 0 |
| [max full-text crawl range](../../database-engine/configure-windows/max-full-text-crawl-range-server-configuration-option.md) (A) | 0 | 256 | 4 |
| [max server memory](../../database-engine/configure-windows/server-memory-server-configuration-options.md) （A，SC） | 16 | 2147483647 | 2147483647 |
| [max text repl size](../../database-engine/configure-windows/configure-the-max-text-repl-size-server-configuration-option.md) | 0 | 2147483647 | 65536 |
| [max worker threads](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md) (A) | 128 | 32767<br /><br /> （对于 32 位 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，建议最大为 1024；对于 64 位 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，建议最大为 2048。） 注意：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 是 32 位操作系统支持的最新版本。 | 0<br /><br /> 归零操作将根据处理器的数量，使用公式 (256 + (\<processors> -4) * 8)（适用于 32 位 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]）或 (512 + (\<processors> - 4) * 8)（适用于 64 位 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]）自动配置最大工作线程数。 注意：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 是 32 位操作系统支持的最新版本。 |
| [media retention](../../database-engine/configure-windows/configure-the-media-retention-server-configuration-option.md) （A，RR） | 0 | 365 | 0 |
| [min memory per query](../../database-engine/configure-windows/configure-the-min-memory-per-query-server-configuration-option.md) (A) | 512 | 2147483647 | 1024 |
| [min server memory](../../database-engine/configure-windows/server-memory-server-configuration-options.md) （A，SC） | 0 | 2147483647 | 0 |
| [嵌套触发器](../../database-engine/configure-windows/configure-the-nested-triggers-server-configuration-option.md) | 0 | 1 | 1 |
| [network packet size](../../database-engine/configure-windows/configure-the-network-packet-size-server-configuration-option.md) (A) | 512 | 32767 | 4096 |
| [Ole Automation Procedures](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [open objects](../../database-engine/configure-windows/open-objects-server-configuration-option.md) （A，RR，已过时） | 0 | 2147483647 | 0 |
| [针对即席工作负荷进行优化](../../database-engine/configure-windows/optimize-for-ad-hoc-workloads-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [PH_timeout](../../database-engine/configure-windows/ph-timeout-server-configuration-option.md) (A) | 1 | 3600 | 60 |
| [PolyBase 已启用](../../relational-databases/polybase/polybase-installation.md#enable) (RR) <br/><br/>[!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]| 0 | 1 | 0 |
| [polybase 网络加密](../../relational-databases/polybase/polybase-installation.md#enable) | 0 | 1 | 1 |
| [precompute rank](../../database-engine/configure-windows/precompute-rank-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [priority boost](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md) （A，RR） | 0 | 1 | 0 |
| [query governor cost limit](../../database-engine/configure-windows/configure-the-query-governor-cost-limit-server-configuration-option.md) (A) | 0 | 2147483647 | 0 |
| [查询等待](../../database-engine/configure-windows/configure-the-query-wait-server-configuration-option.md) (A) | -1 | 2147483647 | -1 |
| [恢复间隔](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md) （A，SC） | 0 | 32767 | 0 |
| [远程访问](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md) (RR) | 0 | 1 | 1 |
| [remote admin connections](../../database-engine/configure-windows/remote-admin-connections-server-configuration-option.md) | 0 | 1 | 0 |
| [远程数据存档](../../database-engine/configure-windows/configure-the-remote-data-archive-server-configuration-option.md) | 0 | 1 | 0 |
| [remote login timeout](../../database-engine/configure-windows/configure-the-remote-login-timeout-server-configuration-option.md) | 0 | 2147483647 | 10 |
| [remote proc trans](../../database-engine/configure-windows/configure-the-remote-proc-trans-server-configuration-option.md) | 0 | 1 | 0 |
| [remote query timeout](../../database-engine/configure-windows/configure-the-remote-query-timeout-server-configuration-option.md) | 0 | 2147483647 | 600 |
| [Replication XPs 选项](../../database-engine/configure-windows/replication-xps-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [scan for startup procs](../../database-engine/configure-windows/configure-the-scan-for-startup-procs-server-configuration-option.md) （A，RR） | 0 | 1 | 0 |
| [server trigger recursion](../../database-engine/configure-windows/server-trigger-recursion-server-configuration-option.md) | 0 | 1 | 1 |
| [set working set size](../../database-engine/configure-windows/set-working-set-size-server-configuration-option.md) （A，RR，已过时） | 0 | 1 | 0 |
| [show advanced options](../../database-engine/configure-windows/show-advanced-options-server-configuration-option.md) | 0 | 1 | 0 |
| [SMO 和 DMO XP](../../database-engine/configure-windows/smo-and-dmo-xps-server-configuration-option.md) (A) | 0 | 1 | 1 |
| [取消显示恢复模型错误](../../database-engine/configure-windows/suppress-recovery-model-errors-server-configuration-option.md) (A) <br/><br/>[!INCLUDE [asdbmi](../../includes/applies-to-version/_asdbmi.md)]| 0 | 1 | 0 |
| [tempdb 元数据内存优化](../../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata) (A) <br/><br/> [!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)].| 0 | 1 | 0 |
| [transform noise words](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [两位数年份截止](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) (A) | 1753 | 9999 | 2049 |
| [user connections](../../database-engine/configure-windows/configure-the-user-connections-server-configuration-option.md) （A，RR，SC） | 0 | 32767 | 0 |
| [user options](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md) | 0 | 32767 | 0 |
| [xp_cmdshell](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md) (A) | 0 | 1 | 0 |  |

## <a name="see-also"></a>另请参阅
 [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md) [DBCC FREEPROCCACHE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md)



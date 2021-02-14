---
description: sys.dm_database_replica_states（Azure SQL 数据库）
title: Azure SQL Database (sys.dm_database_replica_states) |Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_database_replica_states_TSQL
- sys.dm_database_replica_states
- dm_database_replica_states
- dm_database_replica_states_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_database_replica_states dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ea657e595c955589d645db5ec4fccb17a0549a62
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336995"
---
# <a name="sysdm_database_replica_states-azure-sql-database"></a>sys.dm_database_replica_states（Azure SQL 数据库）
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  为数据库返回一行，同时公开本地副本的状态。  
  
> [!IMPORTANT]
> 根据操作和更高级别的状态，数据库状态信息可能不可用或过期。 此外，这些值仅具有本地相关性。 
   
|列名称|数据类型|说明（针对主副本）|  
|-----------------|---------------|----------------------------------------|  
|database_id|**int**|数据库的标识符。|  
|**group_id**|**uniqueidentifier**|数据库所属的可用性组的标识符。|  
|**replica_id**|**uniqueidentifier**|可用性组内可用性副本的标识符。|  
|**group_database_id**|**uniqueidentifier**|可用性组内数据库的标识符。 在此数据库联接到的每个副本上，该标识符都是相同的。|  
|**is_local**|**bit**|可用性数据库是否是本地的，可以是下列值之一：<br /><br /> 0 = 数据库对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例而言不是本地的。<br /><br /> 1 = 数据库对于服务器实例而言是本地的。|  
|**is_primary_replica**|**bit**|如果副本是主副本，则返回 1; 如果副本是数据库所属的可用性组中的辅助副本，则返回0。 这不是指分布式可用性组或活动异地复制关系中的主数据库或辅助数据库。<br /><br />适用于：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。|  
|**synchronization_state**|**tinyint**|数据移动状态，为以下值之一。<br /><br /> 0 = 不同步。 对于某一主数据库，指示该数据库未做好将其事务日志与相应的辅助数据库同步的准备。 对于辅助数据库，指示数据库由于连接问题而未开始日志同步，正被挂起，或者在启动或角色切换过程中正在转换状态。<br /><br /> 1 = 正在同步。 对于主数据库，指示此数据库已做好接受来自辅助数据库的扫描请求的准备。 对于辅助数据库，指示对于该数据库正在发生活动数据移动。<br /><br /> 2 = 已同步。 主数据库显示 SYNCHRONIZED 来代替 SYNCHRONIZING。 同步提交辅助数据库在以下情况下将显示已同步：本地缓存指示数据库副本可供故障转移并且数据库正在同步。<br /><br /> 3 = 正在还原。 指示撤消进程中辅助数据库主动从主数据库获取页时的阶段。<br />**警告：** 如果辅助副本上的数据库处于 "正在还原" 状态，则强制故障转移到辅助副本将使数据库处于不能作为主数据库启动的状态。 该数据库将需要作为辅助数据库重新连接，或者您需要应用来自日志备份的新日志记录。<br /><br /> 4 = 正在初始化。 指示在正在辅助副本上传送和强制写入辅助数据库跟上撤消 LSN 所需的事务日志时的撤消阶段。<br />**警告：** 当辅助副本上的数据库处于正在初始化状态时，强制故障转移到辅助副本将使数据库处于不能作为主数据库启动的状态。 该数据库将需要作为辅助数据库重新连接，或者您需要应用来自日志备份的新日志记录。|  
|**synchronization_state_desc**|**nvarchar(60)**|数据移动状态的说明，可以是下列值之一：<br /><br /> NOT SYNCHRONIZING<br /><br /> SYNCHRONIZING<br /><br /> SYNCHRONIZED<br /><br /> REVERTING<br /><br /> INITIALIZING|  
|**is_commit_participant**|**bit**|0 = 就此数据库而言，事务提交未同步。<br /><br /> 1 = 就此数据库而言，事务提交同步。<br /><br /> 对于异步提交可用性副本上的数据库，该值始终为 0。<br /><br /> 对于同步提交可用性副本上的数据库，该值仅在主数据库上是准确的。|  
|**synchronization_health**|**tinyint**|反映联接到可用性副本上的可用性组的数据库的同步状态与可用性副本的可用性模式的交集 (同步提交或异步提交模式) （以下值之一）。<br /><br /> 0 = 不正常。 数据库的 **synchronization_state** 是 0 (不同步) 。<br /><br /> 1 = 部分正常。 如果 **synchronization_state** 为 1 (同步) ，同步提交可用性副本上的数据库将被视为部分正常。<br /><br /> 2 = 正常。 如果 **synchronization_state** 为 2 (同步) ，则将同步提交可用性副本上的数据库视为正常，如果 **synchronization_state** 为 1 (同步) ，则将异步提交可用性副本上的数据库视为正常。|  
|**synchronization_health_desc**|**nvarchar(60)**|可用性数据库的 **synchronization_health** 的说明。<br /><br /> NOT_HEALTHY<br /><br /> PARTIALLY_HEALTHY<br /><br /> HEALTHY|  
|**database_state**|**tinyint**|0 = 联机<br /><br /> 1 = 正在还原<br /><br /> 2 = 正在恢复<br /><br /> 3 = 恢复挂起<br /><br /> 4 = 可疑<br /><br /> 5 = 紧急<br /><br /> 6 = 脱机<br /><br /> **注意：** 与 sys.databases 中的 **state** 列相同。|  
|**database_state_desc**|**nvarchar(60)**|可用性副本的 **database_state** 的说明。<br /><br /> ONLINE<br /><br /> RESTORING<br /><br /> RECOVERING<br /><br /> RECOVERY_PENDING<br /><br /> SUSPECT<br /><br /> EMERGENCY<br /><br /> OFFLINE<br /><br /> **注意：** 与 sys.databases 中 **state_desc** 列相同。|  
|**is_suspended**|**bit**|数据库状态，可以是下列值之一：<br /><br /> 0 = 已恢复<br /><br /> 1 = 已挂起|  
|**suspend_reason**|**tinyint**|如果数据库处于已挂起状态，则为已挂起状态的原因，可以是下列值之一：<br /><br /> 0 = 用户操作<br /><br /> 1 = 挂起来自伙伴<br /><br /> 2 = 重做<br /><br /> 3 = 捕获<br /><br /> 4 = 应用<br /><br /> 5 = 重新启动<br /><br /> 6 = 撤消<br /><br /> 7 = 重新验证<br /><br /> 8 = 计算辅助副本同步点时出错|  
|**suspend_reason_desc**|**nvarchar(60)**|数据库挂起状态的原因的说明，可以是下列值之一：<br /><br /> SUSPEND_FROM_USER = 用户手动挂起的收据移动<br /><br /> SUSPEND_FROM_PARTNER = 在强制故障转移后挂起数据库副本<br /><br /> SUSPEND_FROM_REDO = 在重做阶段中出错<br /><br /> SUSPEND_FROM_APPLY = 在将日志写入文件时出错（请参阅错误日志）<br /><br /> SUSPEND_FROM_CAPTURE = 在捕获主副本上的日志时出错<br /><br /> SUSPEND_FROM_RESTART = 在重新启动数据库前挂起数据库副本（请参阅错误日志）<br /><br /> SUSPEND_FROM_UNDO = 在撤消阶段中出错（请参阅错误日志）<br /><br /> SUSPEND_FROM_REVALIDATION = 在重新连接时检测到了日志更改不匹配（请参阅错误日志）<br /><br /> SUSPEND_FROM_XRF_UPDATE = 找不到公共日志点（请参阅错误日志）|  
|**recovery_lsn**|**numeric(25,0)**|在主副本上，在恢复或故障转移之后、在主数据库写入任何新日志记录之前事务日志的结尾。 对于给定的辅助数据库，如果该值小于当前硬化的 LSN (last_hardened_lsn)，则 recovery_lsn 是此辅助数据库需要重新同步的值（即要恢复到和重新初始化的值）。 如果该值大于或等于当前硬化 LSN，重新同步将没有必要且不会发生。<br /><br /> **recovery_lsn** 反映了用零填充的日志块 ID。 它不是实际的日志序列号 (LSN)。|  
|**truncation_lsn**|**numeric(25,0)**|在主副本上，对于主数据库，反映了所有相应辅助数据库中的最小日志截断 LSN。 如果阻止本地日志截断（例如由备份操作阻止），则该 LSN 可能高于本地截断 LSN。<br /><br /> 对于给定的辅助数据库，反映了该数据库的截断点。<br /><br /> **truncation_lsn** 反映了用零填充的日志块 ID。 它不是实际的日志序列号。|  
|**last_sent_lsn**|**numeric(25,0)**|指示一个点（在该点前的所有日志块都已由主数据库发送）的日志块标识符。 该标识符是将发送的下一日志块的 ID，而非最近发送的日志块的 ID。<br /><br /> **last_sent_lsn** 反映了用零填充的日志块 ID，它不是实际的日志序列号。|  
|**last_sent_time**|**datetime**|发送最后一个日志块的时间。|  
|**last_received_lsn**|**numeric(25,0)**|标识一个点的日志块 ID，在该点之前，所有日志块都已由承载此辅助数据库的辅助副本接收。<br /><br /> **last_received_lsn** 反映了用零填充的日志块 ID。 它不是实际的日志序列号。|  
|**last_received_time**|**datetime**|在辅助副本上读取最后接收的消息中的日志块 ID 的时间。|  
|**last_hardened_lsn**|**numeric(25,0)**|包含辅助数据库上最后强制写入的 LSN 的日志记录的日志块开头。<br /><br /> 在异步提交主数据库上或其当前策略为“延迟”的同步提交数据库上，该值为 NULL。 对于其他同步提交主数据库， **last_hardened_lsn** 指示在所有辅助数据库中强制执行的 lsn 的最小值。<br /><br /> **注意： last_hardened_lsn** 反映了用零填充的日志块 ID。 它不是实际的日志序列号。|  
|**last_hardened_time**|**datetime**|在辅助数据库上，最后强化的 LSN 的日志块标识符的时间 (**last_hardened_lsn**) 。 在主数据库上，反映了与最小强制写入的 LSN 相对应的时间。|  
|**last_redone_lsn**|**numeric(25,0)**|在辅助数据库上重做的上一个日志记录的实际日志序列号。 **last_redone_lsn** 始终小于 **last_hardened_lsn**。|  
|**last_redone_time**|**datetime**|在辅助数据库上重做最后一个日志记录的时间。|  
|**log_send_queue_size**|**bigint**|主数据库中尚未发送到辅助数据库的日志记录量 (KB)。|  
|**log_send_rate**|**bigint**|主副本实例在上一个活动期间（kb (KB) 。）发送数据的平均速率（kb）|  
|**redo_queue_size**|**bigint**|辅助副本的日志文件中尚未重做的日志记录量 (KB)。|  
|**redo_rate**|**bigint**|在给定的辅助数据库上重做日志记录的平均速率（kb (KB) 。|  
|**filestream_send_rate**|**bigint**|FILESTREAM 文件传送到辅助副本的速率（KB/秒）。|  
|**end_of_log_lsn**|**numeric(25,0)**|日志 LSN 的本地结尾。 与主数据库和辅助数据库上日志缓存中的最后一个日志记录相对应的实际 LSN。 在主副本上，辅助行反映了辅助副本已发送到主副本的最新进度消息中日志 LSN 的结尾。<br /><br /> **end_of_log_lsn** 反映了用零填充的日志块 ID。 它不是实际的日志序列号。|  
|**last_commit_lsn**|**numeric(25,0)**|与事务日志中的最后提交的记录相对应的实际日志序列号。<br /><br /> 主数据库上，这对应于上次处理的提交记录。 辅助数据库的行显示辅助副本已发送到主副本的日志序列号。<br /><br /> 在辅助副本上，这是已重做的最后一个提交记录。|  
|**last_commit_time**|**datetime**|与最后一个提交记录对应的时间。<br /><br /> 在辅助数据库上，此时间与主数据库上的时间相同。<br /><br /> 在主副本上，每个辅助数据库行都显示承载该辅助数据库的辅助副本报告回主副本的时间。 主数据库行与给定的辅助数据库行之间的时间差值约为恢复点目标 (RPO) ，前提是该重做过程被发现并且辅助副本已将进度报告回主副本。|  
|**low_water_mark_for_ghosts**|**bigint**|针对数据库的单调递增的数字，指示主数据库上虚影清除使用的低水印。 如果这个数字没有随着时间的推移而增加，则意味着虚影清除可能未发生。 为了确定要清除的虚影行，主副本会在所有可用性副本（包括主副本）上将该列的最小值用于此数据库。|  
|**secondary_lag_seconds**|**bigint**|在同步期间，辅助副本在主副本后的秒数。<br /><br />适用于：[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 及更高版本。|  
|**quorum_commit_lsn**|**numeric(25,0)**|标识为仅供参考。 不支持。 不保证以后的兼容性。|
|**quorum_commit_time**|**datetime**|标识为仅供参考。 不支持。 不保证以后的兼容性。|


## <a name="permissions"></a>权限

需要对数据库拥有 VIEW DATABASE STATE 权限。


## <a name="see-also"></a>另请参阅

- [AlwaysOn 可用性组 (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)
- [监视可用性组 (Transact-SQL)](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)

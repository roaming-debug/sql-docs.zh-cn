---
description: sys.availability_groups (Transact-SQL)
title: sys.availability_groups (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.availability_groups_TSQL
- availability_groups_TSQL
- sys.availability_groups
- availability_groups
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_groups catalog view
ms.assetid: da7fa55f-c008-45d9-bcfc-3513b02d9e71
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c912d3cb46d4545bf0501281cec23c8f9ce4fb97
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101807"
---
# <a name="sysavailability_groups-transact-sql"></a>sys.availability_groups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为 SQL Server 本地实例为其托管可用性副本的每个可用性组都返回一行。 每一行都包含可用性组元数据的缓存的副本。  
   
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|可用性组的唯一标识符 (GUID)。|  
|name|**sysname**|可用性组的名称。 这是在 Windows Server 故障转移群集 (WSFC) 内必须唯一的用户指定的名称。|  
|**resource_id**|**nvarchar(40)**|WSFC 群集资源的资源 ID。|  
|**resource_group_id**|**nvarchar(40)**|可用性组的 WSFC 群集资源组的资源组 ID。|  
|**failure_condition_level**|**int**|用户定义的失败条件级别，在此级别下必须触发自动故障转移，其中一个整数值是此表下表中显示的一个整数值。<br /><br /> 失败条件级别的范围 (1-5) 是从最少限制的级别 1 到最多限制的级别 5。 给定的条件级别包含所有限制较少的级别。 因此，最严格的条件级别 5 包含四个限制较少的级别 (1-4)，级别 4 包含级别 1-3，依此类推。<br /><br /> 若要更改此值，请使用[ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md)语句的 FAILURE_CONDITION_LEVEL 选项 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。|  
|**health_check_timeout**|**int**|在假定服务器实例速度较慢或未响应之前， [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) 系统存储过程返回服务器运行状况信息的等待时间 (以毫秒为单位) 。 默认值为 30000 毫秒（30 秒）。<br /><br /> 若要更改此值，请使用[ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md)语句的 HEALTH_CHECK_TIMEOUT 选项 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。|  
|**automated_backup_preference**|**tinyint**|用于对此可用性组中的可用性数据库执行备份的首选位置。 下面是可能的值及其说明。<br /><br /> <br /><br /> 0：主要。 备份应该始终在主副本上发生。<br /><br /> 1：仅辅助副本。 首选是对辅助副本执行备份。<br /><br /> 2：首选辅助副本。 首选是对辅助副本执行备份，但如果没有可用于备份操作的辅助副本，对主副本执行备份是可接受的。 这是默认行为。<br /><br /> 3：任何副本。 没有是对主副本执行备份还是对辅助副本执行备份的优先选择。<br /><br /> <br /><br /> 有关详细信息，请参阅[活动次要副本：次要副本备份（Always On 可用性组）](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)。|  
|**automated_backup_preference_desc**|**nvarchar(60)**|**Automated_backup_preference** 的说明，请执行以下操作之一：<br /><br /> PRIMARY<br /><br /> SECONDARY_ONLY<br /><br /> SECONDARY<br /><br /> NONE|  
|**version**|**smallint**|存储在 Windows 故障转移群集中的可用性组元数据的版本。 添加新功能时，此版本号会递增。|  
|**basic_features**|**bit**|指定这是否为基本可用性组。 有关详细信息，请参阅[基本可用性组（Always On 可用性组）](../../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md)。|  
|**dtc_support**|**bit**|指定是否为此可用性组启用了 DTC 支持。 "**创建可用性组**" 的 **DTC_SUPPORT** 选项控制此设置。|  
|**db_failover**|**bit**|指定可用性组是否支持数据库运行状况条件的故障转移。 "**创建可用性组**" 的 **DB_FAILOVER** 选项控制此设置。|  
|**is_distributed**|**bit**|指定这是否为分布式可用性组。 有关详细信息，请参阅[分布式可用性组&#40;Always On 可用性组&#41;](../../database-engine/availability-groups/windows/distributed-availability-groups.md)。|
|**cluster_type**|**tinyint**|0： Windows Server 故障转移群集 <br/><br/>1：外部群集 (例如，Linux Pacemaker) <br/><br/>2：无|
|**cluster_type_desc**|**nvarchar(60)**|群集类型的文本说明|
|**required_synchronized_secondaries_to_commit**|**int**| 必须处于已同步状态才能完成提交的辅助副本数|
|**sequence_number**|**bigint**|标识可用性组配置序列。 每次可用性组主要副本更新组的配置时，都会增量增加。|
|**is_contained**|**bit**|1：为高可用性配置的大数据群集主实例。 <br/><br/> 0：所有其他。|
  
## <a name="failure-condition-level--values"></a>故障条件级别值  
 下表描述了 **failure_condition_level** 列的可能的失败条件级别。  
  
|值|失败条件|  
|-----------|-----------------------|  
|1|指定在发生以下任何情况时应启动自动故障转移：<br /><br /> <br /><br /> - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务已关闭。<br /><br /> -用于连接到 WSFC 故障转移群集的可用性组的租约过期，因为没有从服务器实例接收到 ACK。 有关详细信息，请参阅[工作原理：SQL Server Always On 租约超时](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268)。|  
|2|指定在发生以下任何情况时应启动自动故障转移：<br /><br /> <br /><br /> -的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 未连接到群集，并且超出了可用性组的用户指定的 **health_check_timeout** 阈值。<br /><br /> -可用性副本处于失败状态。|  
|3|指定在发生了严重的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内部错误（例如孤立的自旋锁、严重的写访问冲突或过多的转储）时应启动自动故障转移。<br /><br /> 这是默认值。|  
|4|指定在发生了中等程度的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内部错误（例如在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内部资源池中出现持久的内存不足情况）时应启动自动故障转移。|  
|5|指定在出现任何符合的失败条件时应启动自动故障转移，这些失败条件包括：<br /><br /> <br /><br /> -缺少 SQL 引擎工作线程。<br /><br /> -检测到无法解决的死锁。|  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求具有服务器实例的 VIEW ANY DEFINITION 权限。  
  
## <a name="see-also"></a>另请参阅  
 [sys.availability_replicas (Transact-SQL)](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [AlwaysOn 可用性组 (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [&#40;Transact-sql 监视可用性组&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [监视可用性组 (Transact-SQL)](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  

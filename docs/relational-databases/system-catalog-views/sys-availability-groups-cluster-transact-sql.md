---
description: sys.availability_groups_cluster (Transact-SQL)
title: sys.availability_groups_cluster (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.availability_groups_cluster
- availability_groups_cluster
- availability_groups_cluster_TSQL
- sys.availability_groups_cluster_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_groups_cluster catalog view
- Availability Groups [SQL Server], WSFC clusters
ms.assetid: d0f4683f-cdf0-4227-8b68-720ffe58f158
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e6318cc58ce51cafd380dd1f5eb8a71aa0e0b479
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101811"
---
# <a name="sysavailability_groups_cluster-transact-sql"></a>sys.availability_groups_cluster (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为 Windows Server 故障转移群集 (WSFC) 中的每个 AlwaysOn 可用性组都返回一行。 每一行都包含 WSFC 群集的可用性组元数据。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|可用性组的唯一标识符 (GUID)。|  
|name|**sysname**|可用性组的名称。 这是在 Windows Server 故障转移群集 (WSFC) 内必须唯一的用户指定的名称。|  
|**resource_id**|**nvarchar(40)**|WSFC 群集资源的资源 ID。|  
|**resource_group_id**|**nvarchar(40)**|可用性组的 WSFC 群集资源组的资源组 ID。|  
|**failure_condition_level**|**int**|必须按其触发自动故障转移的用户定义的失败条件级别，可为以下整数值之一：<br /><br /> 1：指定在出现以下任何情况时应启动自动故障转移： <br />- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务已关闭。<br />-用于连接到 WSFC 故障转移群集的可用性组的租约过期，因为没有从服务器实例接收到 ACK。 有关详细信息，请参阅[工作原理：SQL Server Always On 租约超时](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268)。<br /><br /> 2：指定在出现以下任何情况时应启动自动故障转移：  <br />-的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 未连接到群集，并且超出了可用性组的用户指定的 **health_check_timeout** 阈值。 <br />-可用性副本处于失败状态。 <br />3：指定应在发生关键内部错误时启动自动故障转移， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 如孤立旋转锁、严重的写访问冲突或过多的转储。 这是默认值。 <br />4：指定应在中等内部错误上启动自动故障转移 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内部资源池中的持久内存不足的情况。<br />5：指定应在任何合格的故障条件下启动自动故障转移，包括：<br />-缺少 SQL 引擎工作线程。 <br />-检测到无法解决的死锁。<br /><br /> 失败条件级别的范围 (1-5) 是从最少限制的级别 1 到最多限制的级别 5。 给定的条件级别包含所有限制较少的级别。 因此，最严格的条件级别 5 包含四个限制较少的级别 (1-4)，级别 4 包含级别 1-3，依此类推。<br /><br /> 若要更改此值，请使用[ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md)语句的 FAILURE_CONDITION_LEVEL 选项 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。|  
|**health_check_timeout**|**int**|在假定服务器实例速度较慢或未响应之前， [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) 系统存储过程返回服务器运行状况信息的等待时间 (以毫秒为单位) 。 默认值为 30000 毫秒（30 秒）。<br /><br /> 若要更改此值，请使用[ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md)语句的 HEALTH_CHECK_TIMEOUT 选项 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。|  
|**automated_backup_preference**|**tinyint**|用于对此可用性组中的可用性数据库执行备份的首选位置。 以下值之一：<br /><br /> 0：主要。 备份应该始终在主副本上发生。<br />1：仅辅助副本。 首选是对辅助副本执行备份。<br />2：首选辅助副本。 首选是对辅助副本执行备份，但如果没有可用于备份操作的辅助副本，对主副本执行备份是可接受的。 这是默认行为。<br />3：任何副本。 没有是对主副本执行备份还是对辅助副本执行备份的优先选择。<br /><br /> 有关详细信息，请参阅[活动次要副本：次要副本备份（Always On 可用性组）](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)。|  
|**automated_backup_preference_desc**|**nvarchar(60)**|**Automated_backup_preference** 的说明，请执行以下操作之一：<br /><br /> PRIMARY<br /><br /> SECONDARY_ONLY<br /><br /> SECONDARY<br /><br /> NONE|  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求具有服务器实例的 VIEW ANY DEFINITION 权限。  
  
## <a name="see-also"></a>另请参阅  
 [sys.availability_replicas (Transact-SQL)](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [AlwaysOn 可用性组 (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [&#40;Transact-sql 监视可用性组&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [监视可用性组 (Transact-SQL)](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  

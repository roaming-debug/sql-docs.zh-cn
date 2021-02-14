---
description: sys.dm_hadr_availability_replica_states (Transact-SQL)
title: sys.dm_hadr_availability_replica_states (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 10/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_hadr_availability_replica_states
- sys.dm_hadr_availability_replica_states_TSQL
- sys.dm_hadr_availability_replica_states
- dm_hadr_availability_replica_states_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_hadr_availability_replica_states dynamic management view
ms.assetid: d2e678bb-51e8-4a61-b223-5c0b8d08b8b1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2c725e1758b1ff0197d6b4498cef6f5f54809d89
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354142"
---
# <a name="sysdm_hadr_availability_replica_states-transact-sql"></a>sys.dm_hadr_availability_replica_states (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为每个本地副本都返回一行，并为与本地副本位于同一 AlwaysOn 可用性组的每个远程副本都返回一行。 每一行都包含给定副本的状态信息。  
  
> [!IMPORTANT]  
>  若要获取有关给定可用性组中的每个副本的信息，请在承载主副本的服务器实例上查询 **sys.dm_hadr_availability_replica_states** 。 在对正在承载某一可用性组的辅助副本的服务器实例进行查询时，此动态管理视图仅返回该可用性组的本地信息。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**replica_id**|**uniqueidentifier**|副本的唯一标识符。|  
|**group_id**|**uniqueidentifier**|可用性组的唯一标识符。|  
|**is_local**|**bit**|副本是否为本地副本，请执行以下操作之一：<br /><br /> 0 = 指示某一可用性组中其主副本由本地服务器实例承载的远程辅助副本。 此值仅在主副本位置上出现。<br /><br /> 1 = 表示本地副本。 在辅助副本上，这是副本所属的可用性组的唯一可用值。|  
|**role**|**tinyint**|[!INCLUDE[ssHADR](../../includes/sshadr-md.md)]本地副本或连接的远程副本的当前角色，如下所示：<br /><br /> 0 = 正在解析<br /><br /> 1 = 主<br /><br /> 2 = 辅助<br /><br /> 有关 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 角色的详细信息，请参阅 [AlwaysOn 可用性组概述 (SQL Server)](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)。|  
|**role_desc**|**nvarchar(60)**|**角色** 说明，其中之一：<br /><br /> RESOLVING<br /><br /> PRIMARY<br /><br /> SECONDARY|  
|**operational_state**|**tinyint**|副本的当前操作状态，如下所示：<br /><br /> 0 = 挂起故障转移<br /><br /> 1 = 挂起<br /><br /> 2 = 联机<br /><br /> 3 = 脱机<br /><br /> 4 = 失败<br /><br /> 5 = 失败，无仲裁<br /><br /> NULL = 副本不在本地。<br /><br /> 有关详细信息，请参阅本主题后面的 [角色和操作状态](#RolesAndOperationalStates)。|  
|**操作 \_ 状态 \_ desc**|**nvarchar(60)**|**操作 \_ 状态** 的说明，如下所示：<br /><br /> PENDING_FAILOVER<br /><br /> PENDING<br /><br /> ONLINE<br /><br /> OFFLINE<br /><br /> FAILED<br /><br /> FAILED_NO_QUORUM<br /><br /> Null|  
|**恢复 \_ 运行状况**|**tinyint**|[Sys.dm_hadr_database_replica_states](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)动态管理视图的 "**数据库 \_ 状态**" 列的汇总。 下面是可能的值及其说明。<br /><br /> 0：正在进行。  至少一个已联接的数据库具有联机数据库状态之外的数据库状态 (**数据库 \_ 状态** 不是 0) 。<br /><br /> 1：联机。 所有联接的数据库的数据库状态都为 "联机" (**database_state**) 为0。<br /><br /> NULL： **is_local** = 0|  
|**recovery_health_desc**|**nvarchar(60)**|**Recovery_health** 的说明，请执行以下操作之一：<br /><br /> ONLINE_IN_PROGRESS<br /><br /> ONLINE<br /><br /> Null|  
|**同步 \_ 运行状况**|**tinyint**|反映了数据库同步状态的汇总 () 所有联接的可用性数据库 **synchronization_state** ， (也称为 *副本*) 和副本 (同步提交模式或异步提交模式) 的可用性模式。 汇总将反映副本上的数据库的运行状况不正常的最小状态。 下面是可能的值及其说明。<br /><br /> 0：不正常。   至少有一个联接的数据库处于 NOT SYNCHRONIZING 状态。<br /><br /> 1：部分运行正常。 一些副本未处于目标同步状态：同步提交副本应已同步，异步提交副本应正在同步。<br /><br /> 2：正常。 所有副本均处于目标同步状态：同步提交副本已同步，异步提交副本正在同步。|  
|**synchronization_health_desc**|**nvarchar(60)**|**Synchronization_health** 的说明，请执行以下操作之一：<br /><br /> NOT_HEALTHY<br /><br /> PARTIALLY_HEALTHY<br /><br /> HEALTHY|  
|**connected_state**|**tinyint**|辅助副本当前是否连接到主副本。 可能的值如下所示。<br /><br /> 0：已断开连接。 可用性副本到断开连接状态的响应取决于其角色：在主副本上，如果辅助副本断开连接，则其辅助数据库将在主副本上标记为不同步，这将等待辅助副本重新连接;在辅助副本上，检测到它已断开连接后，辅助副本会尝试重新连接到主副本。<br /><br /> 1：已连接。<br /><br /> 每个主副本都会跟踪同一可用性组中每个辅助副本的连接状态。 辅助副本仅跟踪主副本的连接状态。|  
|**connected_state_desc**|**nvarchar(60)**|**Connection_state** 的说明，请执行以下操作之一：<br /><br /> DISCONNECTED<br /><br /> CONNECTED|  
|**last_connect_error_number**|**int**|上一个连接错误的编号。|  
|**last_connect_error_description**|**nvarchar(1024)**|**Last_connect_error_number** 消息的文本。|  
|**last_connect_error_timestamp**|**datetime**|指示何时出现 **last_connect_error_number** 错误的日期和时间戳。|  
  
##  <a name="roles-and-operational-states"></a><a name="RolesAndOperationalStates"></a> 角色和操作状态  
 角色、 **角色**、反映给定的可用性副本的状态和操作状态 **operational_state**，说明副本是否准备好处理对可用性副本的所有数据库的客户端请求。 下面汇总了每个角色可以执行的操作状态：正在解析、主和辅助。  
  
 正在 **解析：** 如果可用性副本处于解析角色中，则可能的操作状态如下表中所示。  
  
|操作状态|描述|  
|-----------------------|-----------------|  
|PENDING_FAILOVER|正在为可用性组处理故障转移命令。|  
|OFFLINE|可用性副本的所有配置数据都已在 WSFC 群集上更新，并且也在本地元数据中更新，但可用性组目前缺少主副本。|  
|FAILED|在试图从 WSFC 群集中检索信息时发生了读取失败。|  
|FAILED_NO_QUORUM|本地 WSFC 节点没有仲裁。 这是一种推断状态。|  
  
 **主要：** 当某个可用性副本正在执行主角色时，该副本当前是主副本。 可能的操作状态如下表中所示。  
  
|操作状态|描述|  
|-----------------------|-----------------|  
|PENDING|这是一个临时状态，但是，如果工作线程无法处理请求，主副本可能会处于此状态。|  
|ONLINE|可用性组资源已处于联机状态，并且所有数据库工作线程均已选取。|  
|FAILED|可用性副本无法从 WSFC 群集读取和/或写入。|  
  
 **辅助：** 当某个可用性副本正在执行辅助角色时，该副本当前为辅助副本。 可能的操作状态如下表中所示。  
  
|操作状态|描述|  
|-----------------------|-----------------|  
|ONLINE|本地辅助副本连接到主副本。|  
|FAILED|本地辅助副本无法从 WSFC 群集读取和/或写入。|  
|Null|在主副本上，当行与某一辅助副本相关时，将返回该值。|  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [监视可用性组 (Transact-SQL)](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  

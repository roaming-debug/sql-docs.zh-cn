---
title: 配置可用性组扩展事件
description: SQL Server 定义了特定于 AlwaysOn 可用性组的扩展事件。 可在会话中监视这些扩展事件，以便在对可用性组进行故障排除时帮助诊断根本原因。
ms.custom: ag-guide, seodec18
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 5950f98a-3950-473d-95fd-cde3557b8fc2
author: rothja
ms.author: jroth
ms.openlocfilehash: 7ba062db14f9cb3d2ba262350c1ff5b93a079884
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091813"
---
# <a name="configure-extended-events-for-always-on-availability-groups"></a>配置 AlwaysOn 可用性组扩展事件
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  SQL Server 定义了特定于 AlwaysOn 可用性组的扩展事件。 可在会话中监视这些扩展事件，以便在对可用性组进行故障排除时帮助诊断根本原因。 可以使用以下查询查看可用性组扩展事件：  
  
```sql  
SELECT * FROM sys.dm_xe_objects WHERE name LIKE '%hadr%'  
```  
   
##  <a name="alwayson_health-session"></a><a name="BKMK_alwayson_health"></a>Alwayson_health 会话  
 创建可用性组时会自动创建 Alwayson_health 扩展事件会话，并捕获可用性组相关事件的子集。 此会话已预配置为一种有用且便利的工具，有助于在对可用性组进行故障排除时快速入门。 创建可用性组向导会自动在向导中已配置的每个参与可用性副本上启动会话。  
  
> [!IMPORTANT]  
>  如果未使用“新建可用性组向导”创建可用性组，则 alwayson_health 会话可能不会自动启动  。 如果会话未启动，则在发生意外问题时，它无法捕获事件数据。 应手动启动会话，并通过配置会话属性将会话配置为自动启动。  
  
 查看 alwayson_health 会话的定义：  
  
1.  在“对象资源管理器”中，依次展开“管理”、“扩展事件”和“会话”     。  
  
2.  右键单击“Alwayson_health”，依次指向“编写会话脚本为”、“CREATE To”，然后单击“新查询编辑器窗口”     。  

有关 alwayson_health 涵盖的部分事件的信息，请参阅[扩展事件引用](always-on-extended-events.md#BKMK_Reference)。  


##  <a name="extended-events-for-debugging"></a><a name="BKMK_Debugging"></a>用于调试的扩展事件  
 除了 Alwayson_health 会话所涵盖的扩展事件外，SQL Server 还为可用性组定义了大量调试事件。 要在会话中利用这些额外的扩展事件，请执行以下过程：  
  
1.  在“对象资源管理器”中，依次展开“管理”、“扩展事件”和“会话”     。  
  
2.  右键单击“会话”  ，然后选择“新建会话”  。 或者，右键单击“Alwayson_health”并选择“属性”   。  
  
3.  在“选择页”窗格中，单击“事件”   。  
  
4.  在事件库中的“类别”列中，选择“alwayson”并清除所有其他类别   。  
  
5.  在“频道”列中，选择“调试”   。 尚未选择的所有可用性组相关事件现在都显示在事件库中。  
  
6.  突出显示事件库中的一个事件，然后单击“>”按钮选择将其用于会话  。  
  
7.  完成会话后，单击“确定”将其关闭  。 确保会话已启动，以便捕获所选事件。  
  
##  <a name="always-on-availability-groups-extended-events-reference"></a><a name="BKMK_Reference"></a>Always On 可用性组扩展事件引用  
 本节介绍了一些用于监视可用性组的扩展事件。  
  
 [availability_replica_state_change](#BKMK_availability_replica_state_change)  
  
 [availability_group_lease_expired](#BKMK_availability_group_lease_expired)  
  
 [availability_replica_automatic_failover_validation](#BKMK_availability_replica_automatic_failover_validation)  
  
 [error_reported（多个错误号）：传输或连接问题](#BKMK_error_reported)  
  
 [data_movement_suspend_resume](#BKMK_data_movement_suspend_resume)  
  
 [alwayson_ddl_executed](#BKMK_alwayson_ddl_executed)  
  
 [availability_replica_manager_state](#BKMK_availability_replica_manager_state)  
  
 [error_reported (1480)：数据库副本角色更改](#BKMK_error_reported_1480)  
  
###  <a name="availability_replica_state_change"></a><a name="BKMK_availability_replica_state_change"></a>availability_replica_state_change  
 更改可用性副本的状态时发生。 创建可用性组或联接可用性副本可触发此事件。 它对诊断自动故障转移失败非常有用。 还可用于跟踪故障转移步骤。  
  
#### <a name="event-information"></a>事件信息  
  
|列|说明|  
|------------|-----------------|  
|名称|availability_replica_state_change|  
|类别|always on|  
|Channel|可运行|  
  
#### <a name="event-fields"></a>事件字段  
  
|名称|Type_name|说明|  
|----------|----------------|-----------------|  
|availability_group_id|guid|可用性组的 ID。|  
|availability_group_name|unicode_string|可用性组的名称。|  
|availability_replica_id|guid|可用性副本的 ID。|  
|previous_state|availability_replica_state|更改前副本的角色。<br /><br /> **可能的值包括：**<br /><br /> Primary_Normal<br /><br /> Secondary_Normal<br /><br /> Resolving_Pending_Failover<br /><br /> Resolving_Normal<br /><br /> Primary_Pending<br /><br /> Not_Available|  
|current_state|availability_replica_state|更改后副本的角色。<br /><br /> **可能的值包括：**<br /><br /> Primary_Normal<br /><br /> Secondary_Normal<br /><br /> Resolving_Pending_Failover<br /><br /> Resolving_Normal<br /><br /> Primary_Pending<br /><br /> Not_Available|  
  
#### <a name="alwayson_health-session-definition"></a>alwayson_health 会话定义  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT availability_replica_state_change  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="availability_group_lease_expired"></a><a name="BKMK_availability_group_lease_expired"></a>availability_group_lease_expired  
 群集和可用性组发生连接问题且租约已过期时发生。 此事件表示可用性组和基础 WSFC 群集之间的连接已断开。 如果主要副本上发生连接问题，该事件可能导致自动故障转移，或导致可用性组脱机。  
  
#### <a name="event-information"></a>事件信息  
  
|列|说明|  
|------------|-----------------|  
|名称|availability_group_lease_expired|  
|类别|always on|  
|Channel|可运行|  
  
#### <a name="event-fields"></a>事件字段  
  
|名称|Type_name|说明|  
|----------|----------------|-----------------|  
|availability_group_id|guid|可用性组的 ID。|  
|availability_group_name|unicode_string|可用性组的名称。|  
  
#### <a name="alwayson_health-session-definition"></a>alwayson_health 会话定义  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT availability_group_lease_expired  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="availability_replica_automatic_failover_validation"></a><a name="BKMK_availability_replica_automatic_failover_validation"></a>availability_replica_automatic_failover_validation  
 在自动故障转移验证可用性副本作为主要副本的准备情况时发生，它会显示目标可用性副本是否已准备好成为新的主要副本。 例如，如果并非所有数据库都已同步或未联接时，则故障转移验证返回“false”。 此事件旨在提供故障转移期间的故障点。 数据库管理员会关注此信息，尤其是对于自动故障转移，因为自动故障转移是无人参与的操作。 数据库管理员可查看事件，了解自动故障转移为何失败。  
  
#### <a name="event-information"></a>事件信息  
  
|名称|说明|  
|----------|-----------------|  
|availability_replica_automatic _failover_validation||  
|类别|always on|  
|Channel|分析|  
  
#### <a name="event-fields"></a>事件字段  
  
|名称|Type_name|说明|  
|----------|----------------|-----------------|  
|availability_group_id|guid|可用性组的 ID。|  
|availability_group_name|unicode_string|可用性组的名称。|  
|availability_replica_id|guid|可用性副本的 ID。|  
|forced_quorum|validation_result_type|如果值为 TRUE，自动故障转移会在此可用性副本上失效。<br /><br /> TRUE<br /><br /> FALSE|  
|joined_and_synchronized|validation_result_type|如果值为 FALSE，自动故障转移会在此可用性副本上失效。<br /><br /> TRUE<br /><br /> FALSE|  
|previous_primary_or_automatic_failover_target|validation_result_type|如果值为 FALSE，自动故障转移会在此可用性副本上失效。<br /><br /> TRUE<br /><br /> FALSE|  
  
#### <a name="alwayson_health-session-definition"></a>alwayson_health 会话定义  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT availability_replica_automatic_failover_validation (  
WHERE (  
[forced_quorum]=(TRUE) OR [joined_and_synchronized]=(FALSE) OR [previous_primary_or_automatic_failover_target]=(TRUE)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
  
```  
  
###  <a name="error_reported-multiple-error-numbers-for-transport-or-connection-issues"></a><a name="BKMK_error_reported"></a>error_reported（多个错误号）：传输或连接问题  
 每个已筛选事件表示在可用性组依赖的传输或数据库镜像终结点中发生了连接问题。  
  
|列|说明|  
|------------|-----------------|  
|名称|error_reported<br /><br /> 可筛选的错误号：35201、35202、35206、35204、35207、9642、9666、9691、9692、9693、28034、28036、28080、28091、33309|  
|类别|错误|  
|Channel|管理员|  
  
#### <a name="error-numbers-to-filter"></a>要筛选的错误号  
  
|错误号|说明|  
|------------------|-----------------|  
|35201|尝试建立与可用性副本“%ls”的连接时发生连接超时。|  
|35202|已成功建立可用性组“%ls”从 ID 为 [%ls] 的可用性副本“%ls”到 ID 为 [%ls] 的“%ls”的连接。  这只是一条信息性消息。 不需要任何用户操作。|  
|35206|之前建立的与可用性副本“%ls”的连接超时。|  
|35204|由于终结点关闭，实例“%ls”和“%ls”之间的连接已禁用。|  
|超时 + 已连接|  
|35207|由于错误 %d（严重性 %d，状态 %d），在可用性组 ID“%ls”上从副本 ID“%ls”到副本 ID“%ls”的连接尝试失败。  严重性 %d，状态 %d。 （这可能未正确使用 DBA。 在这种情况下，请稍后检查并删除）|  
|9642|Service Broker/数据库镜像传输连接终结点出错，错误: %i 状态: %i。 (近端点角色: %S_MSG 远端点地址: '%.*hs') 错误: %i 状态: %i. (近端点角色: %S_MSG 远端点地址: '%.\*hs')|  
|9666|%S_MSG 端点处于禁用或停止状态。|  
|9691|%S_MSG 端点已停止侦听连接。|  
|9692|%S_MSG 端点无法侦听端口 %d，因为其他进程正在使用此端口。|  
|9693|%S_MSG 端点无法侦听连接，因为存在以下错误:“%.*ls”。|  
|28034|连接握手失败。 登录名 '%.*ls' 没有该端点的 CONNECT 权限。 状态 %d。|  
|28036|连接握手失败。 找不到此端点使用的证书: %S_MSG。 请在 master 数据库中使用 DBCC CHECKDB 验证该端点的元数据完整性。 状态 %d。|  
|28080|连接握手失败。 未配置端点 %S_MSG。 状态 %d。|  
|28091|不支持未经身份验证启动 %S_MSG 的端点。|  
|33309|无法启动群集端点，因为尚未加载默认 %S_MSG 端点配置。|  
  
#### <a name="alwayson_health-session-definition"></a>alwayson_health 会话定义  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT sqlserver.error_reported(  
    WHERE   
(  
--Connectivity Error Messages  
[error_number]=(35201)   
OR [error_number]=(35202)   
OR [error_number]=(35204)   
OR [error_number]=(35206)   
OR [error_number]=(35207)   
OR [error_number]=(9642)   
--OR [error_number]=(9666)   
OR [error_number]=(9691)   
OR [error_number]=(9692)   
OR [error_number]=(9693)   
OR [error_number]=(28034)   
OR [error_number]=(28036)   
OR [error_number]=(28080)   
OR [error_number]=(28091)   
OR [error_number]=(33309)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="data_movement_suspend_resume"></a><a name="BKMK_data_movement_suspend_resume"></a>data_movement_suspend_resume  
 在暂停或恢复数据库副本的数据库移动时发生。  
  
#### <a name="event-information"></a>事件信息  
  
|列|说明|  
|------------|-----------------|  
|名称|data_movement_suspend_resume|  
|类别|始终启用|  
|Channel|可运行|  
  
#### <a name="event-fields"></a>事件字段  
  
|名称|Type_name|说明|  
|-|-|-|  
|availability_group_id|guid|可用性组的 ID。|  
|availability_group_name|unicode_string|可用性组的名称（如果可用）。|  
|availability_replica_id|guid|可用性副本的 ID。|  
|database_replica_id|guid|可用性数据库的 ID。|  
|database_replica_name|unicode_string|可用性数据库的名称。|  
|database_id|uint32|可用性数据库的 ID。|  
|suspend_status|suspend_status_type|挂起状态值。<br /><br /> SUSPEND_NULL<br /><br /> RESUMED<br /><br /> SUSPENDED<br /><br /> SUSPENDED_INVALID|  
|suspend_source|suspend_source_type|暂停或继续操作的源。|  
|suspend_reason|unicode_string|数据库副本管理器中捕获的暂停原因。|  
  
#### <a name="alwayson_health-session-definition"></a>alwayson_health 会话定义  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT data_movement_suspend_resume (  
WHERE (  
[suspend_status]=(SUSPENDED)or [suspend_status]=(SUSPENDED_INVALID) or   
[suspend_status]=(SUSPEND_NULL)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="alwayson_ddl_executed"></a><a name="BKMK_alwayson_ddl_executed"></a>alwayson_ddl_executed  
 执行可用性组数据定义语言 (DDL) 语句（包括 CREATE、ALTER 或 DROP）时发生。 该事件的主要用途是指示可用性副本上的用户操作问题，或指示操作性操作的起始点，其后跟随运行时问题，如手动故障转移、强制故障转移、已挂起的数据移动或已恢复的数据移动。  
  
#### <a name="event-information"></a>事件信息  
  
|列|说明|  
|------------|-----------------|  
|名称|alwayson_ddl_execution|  
|类别|always on|  
|Channel|分析|  
  
#### <a name="event-fields"></a>事件字段  
  
|名称|Type_name|说明|  
|----------|----------------|-----------------|  
|availability_group_id|Guid|可用性组的 ID。|  
|availability_group_name|unicode_string|可用性组的名称。|  
|ddl_action|alwayson_ddl_action|指示 DDL 操作的类型：CREATE、ALTER 或 DROP。|  
|ddl_phase|ddl_opcode|指示 DDL 操作的阶段：BEGIN、COMMIT 或 ROLLBACK。|  
|语句|unicode_string|已执行的语句的文本。|  
  
#### <a name="alwayson_health-session-definition"></a>alwayson_health 会话定义  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT alwayson_ddl_executed  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="availability_replica_manager_state"></a><a name="BKMK_availability_replica_manager_state"></a>availability_replica_manager_state  
 更改可用性副本管理器的状态时发生。 此事件指示可用性副本管理器的检测信号。 如果可用性副本管理器未处于正常状态，SQL Server 实例中的所有可用性副本都将关闭。  
  
#### <a name="event-information"></a>事件信息  
  
|列|说明|  
|------------|-----------------|  
|名称|availability_replica_manager_state_change|  
|类别|always on|  
|Channel|可运行|  
  
#### <a name="event-fields"></a>事件字段  
  
|名称|Type_name|说明|  
|----------|----------------|-----------------|  
|current_state|manager_state|可用性副本管理器的当前状态。<br /><br /> 联机<br /><br /> Offline<br /><br /> WaitingForClusterCommunication|  
  
#### <a name="alwayson_health-session-definition"></a>Alwayson_health 会话定义  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT availability_replica_manager_state (  
WHERE ([current_state]=(OFFLINE))  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="error_reported-1480-database-replica-role-change"></a><a name="BKMK_error_reported_1480"></a> error_reported (1480)：数据库副本角色更改  
 更改可用性副本角色后，此筛选的 error_reported 事件会异步发生。 它指示哪个可用性数据库无法在故障转移过程中更改其预期角色。  
  
#### <a name="event-information"></a>事件信息  
  
|列|说明|  
|------------|-----------------|  
|名称|error_reported<br /><br /> 错误号 1480：由于 REASON_MSG，REPLICATION_TYPE_MSG 数据库“DATABASE_NAME”正在将角色从“OLD_ROLE”更改为“NEW_ROLE”|  
|类别|错误|  
|Channel|管理员|  
  
#### <a name="alwayson_health-session-definition"></a>alwayson_health 会话定义  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT sqlserver.error_reported(  
    WHERE   
(  
--database replica role change message  
OR [error_number] = (1480)  
  
--database replica runtime error messages  
OR [error_number]=(823)   
OR [error_number]=(824)   
OR [error_number]=(829)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
## <a name="next-steps"></a>后续步骤  
 [查看事件会话数据](/previous-versions/sql/sql-server-2012/hh710068(v=sql.110))
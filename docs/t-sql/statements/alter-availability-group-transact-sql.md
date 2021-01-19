---
description: ALTER AVAILABILITY GROUP (Transact-SQL)
title: ALTER AVAILABILITY GROUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/02/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_AVAILABILITY_GROUP_TSQL
- ALTER_AVAILABILITY_TSQL
- ALTER AVAILABILITY GROUP
- ALTER AVAILABILITY
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- ALTER AVAILABILITY GROUP statement
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], Transact-SQL statements
ms.assetid: f039d0de-ade7-4aaf-8b7b-d207deb3371a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 23cd700fd8b25778118b2141bf0205dcbd87c92e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170459"
---
# <a name="alter-availability-group-transact-sql"></a>ALTER AVAILABILITY GROUP (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中现有的 Always On 可用性组。 只有当前主副本支持大多数 ALTER AVAILABILITY GROUP 参数。 但是，只有辅助副本支持 JOIN、FAILOVER 和 FORCE_FAILOVER_ALLOW_DATA_LOSS 参数。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
ALTER AVAILABILITY GROUP group_name   
  {  
     SET ( <set_option_spec> )   
   | ADD DATABASE database_name   
   | REMOVE DATABASE database_name  
   | ADD REPLICA ON <add_replica_spec>   
   | MODIFY REPLICA ON <modify_replica_spec>  
   | REMOVE REPLICA ON <server_instance>  
   | JOIN  
   | JOIN AVAILABILITY GROUP ON <add_availability_group_spec> [ ,...2 ]  
   | MODIFY AVAILABILITY GROUP ON <modify_availability_group_spec> [ ,...2 ]  
   | GRANT CREATE ANY DATABASE  
   | DENY CREATE ANY DATABASE  
   | FAILOVER  
   | FORCE_FAILOVER_ALLOW_DATA_LOSS   
   | ADD LISTENER 'dns_name' ( <add_listener_option> )  
   | MODIFY LISTENER 'dns_name' ( <modify_listener_option> )  
   | RESTART LISTENER 'dns_name'  
   | REMOVE LISTENER 'dns_name'  
   | OFFLINE  
  }  
[ ; ]  
  
<set_option_spec> ::=   
    AUTOMATED_BACKUP_PREFERENCE = { PRIMARY | SECONDARY_ONLY| SECONDARY | NONE }  
  | FAILURE_CONDITION_LEVEL  = { 1 | 2 | 3 | 4 | 5 }   
  | HEALTH_CHECK_TIMEOUT = milliseconds  
  | DB_FAILOVER  = { ON | OFF }   
  | DTC_SUPPORT  = { PER_DB | NONE }  
  | REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT = { integer }
  
<server_instance> ::=   
 { 'system_name[\instance_name]' | 'FCI_network_name[\instance_name]' }  
  
<add_replica_spec>::=  
  <server_instance> WITH  
    (  
       ENDPOINT_URL = 'TCP://system-address:port',  
       AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT | CONFIGURATION_ONLY },  
       FAILOVER_MODE = { AUTOMATIC | MANUAL }   
       [ , <add_replica_option> [ ,...n ] ]  
    )   
  
  <add_replica_option>::=  
       SEEDING_MODE = { AUTOMATIC | MANUAL }  
     | BACKUP_PRIORITY = n  
     | SECONDARY_ROLE ( {   
            [ ALLOW_CONNECTIONS = { NO | READ_ONLY | ALL } ]   
        [,] [ READ_ONLY_ROUTING_URL = 'TCP://system-address:port' ]  
     } )  
     | PRIMARY_ROLE ( {   
            [ ALLOW_CONNECTIONS = { READ_WRITE | ALL } ]   
        [,] [ READ_ONLY_ROUTING_LIST = { ( '<server_instance>' [ ,...n ] ) | NONE } ]  
        [,] [ READ_WRITE_ROUTING_URL = { ( '<server_instance>' ) ] 
     } )  
     | SESSION_TIMEOUT = integer
  
<modify_replica_spec>::=  
  <server_instance> WITH  
    (    
       ENDPOINT_URL = 'TCP://system-address:port'   
     | AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT }   
     | FAILOVER_MODE = { AUTOMATIC | MANUAL }   
     | SEEDING_MODE = { AUTOMATIC | MANUAL }   
     | BACKUP_PRIORITY = n  
     | SECONDARY_ROLE ( {   
          ALLOW_CONNECTIONS = { NO | READ_ONLY | ALL }   
        | READ_ONLY_ROUTING_URL = 'TCP://system-address:port'   
          } )  
     | PRIMARY_ROLE ( {   
          ALLOW_CONNECTIONS = { READ_WRITE | ALL }   
        | READ_ONLY_ROUTING_LIST = { ( '<server_instance>' [ ,...n ] ) | NONE }   
          } )  
     | SESSION_TIMEOUT = seconds  
    )   
  
<add_availability_group_spec>::=  
 <ag_name> WITH  
    (  
       LISTENER_URL = 'TCP://system-address:port',  
       AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT },  
       FAILOVER_MODE = MANUAL,  
       SEEDING_MODE = { AUTOMATIC | MANUAL }  
    )  
  
<modify_availability_group_spec>::=  
 <ag_name> WITH  
    (  
       LISTENER = 'TCP://system-address:port'  
       | AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT }  
       | SEEDING_MODE = { AUTOMATIC | MANUAL }  
    )  
  
<add_listener_option> ::=  
   {  
      WITH DHCP [ ON ( <network_subnet_option> ) ]  
    | WITH IP ( { ( <ip_address_option> ) } [ , ...n ] ) [ , PORT = listener_port ]  
   }  
  
  <network_subnet_option> ::=  
     'ipv4_address', 'ipv4_mask'    
  
  <ip_address_option> ::=  
     {   
        'four_part_ipv4_address', 'four_part_ipv4_mask'  
      | 'ipv6_address'  
     }  
  
<modify_listener_option>::=  
    {  
       ADD IP ( <ip_address_option> )   
     | PORT = listener_port  
    }  
  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 group_name  
 指定新可用性组的名称。 group_name 必须是一个有效的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识符，并且它在 WSFC 群集的所有可用性组中必须是唯一的。  
  
 AUTOMATED_BACKUP_PREFERENCE **=** { PRIMARY \| SECONDARY_ONLY\| SECONDARY \| NONE }  
 指定在选择执行备份的位置时有关备份作业应该如何评估主副本的首选项。 您可以编写给定备份作业的脚本，以便纳入自动备份首选项。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不会强制执行首选项，因此它对即席备份没有影响，了解这一点很重要。  
  
 仅在主要副本上受支持。  
  
 这些值如下所示：  
  
 PRIMARY  
 指定备份应该始终在主副本上发生。 如果您需要在对辅助副本运行备份时不支持的备份功能，例如创建差异备份，此选项将很有用。  
  
> [!IMPORTANT]  
>  如果你计划使用日志传送为可用性组准备任何辅助数据库，请将自动备份首选项设置为“主要”，直到准备好所有辅助数据库并将其联接到可用性组。  
  
 SECONDARY_ONLY  
 指定备份应该永远不会在主副本上执行。 如果主副本是唯一的联机副本，则备份应不会发生。  
  
 SECONDARY  
 指定备份应在辅助副本上发生，但在主副本是唯一联机的副本时除外。 在该情况下，备份应在主副本上发生。 此选项为默认行为。  
  
 无  
 指定您希望在选择要执行备份的副本时备份作业将忽略可用性副本的角色。 请注意，备份作业可能评估其他因素，例如每个可用性副本的备份优先级及其操作状态和已连接状态。  
  
> [!IMPORTANT]  
>  不会强制实施 AUTOMATED_BACKUP_PREFERENCE 设置。 对此首选项的解释依赖于您为给定可用性组中的数据库撰写作业脚本的逻辑（如果有）。 自动备份首选项设置对即席备份没有影响。 有关详细信息，请参阅[配置可用性副本备份 (SQL Server)](../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)。  
  
> [!NOTE]  
>  若要查看现有可用性组的自动备份首选项，请选择 [sys.availability_groups](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) 目录视图的 automated_backup_preference 或 automated_backup_preference_desc 列 。 此外，[sys.fn_hadr_backup_is_preferred_replica (Transact-SQL)](../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md) 可用于确定首选备份副本。  此函数始终对至少一个副本返回 1（即使 `AUTOMATED_BACKUP_PREFERENCE = NONE`）。  
  
 FAILURE_CONDITION_LEVEL = { 1 \| 2 \| 3 \| 4 \| 5 }   
 指定将为此可用性组触发自动故障转移的失败条件。 FAILURE_CONDITION_LEVEL 在组级别设置，但仅针对为同步-提交可用性模式 (AVAILABILITY_MODE = SYNCHRONOUS_COMMIT) 配置的可用性副本。 此外，只有在主要副本和次要副本均配置为自动故障转移模式 (FAILOVER_MODE = AUTOMATIC) 并且次要副本当前与主要副本同步的情况下，失败条件才可以触发自动故障转移。  
  
 仅在主要副本上受支持。  
  
 失败条件级别的范围 (1-5) 是从最少限制的级别 1 到最多限制的级别 5。 给定的条件级别包含所有限制较少的级别。 因此，最严格的条件级别 5 包含四个限制较少的级别 (1-4)，级别 4 包含级别 1-3，依此类推。 下表介绍了与各级别相对应的失败条件。  
  
|级别|失败条件|  
|-----------|-----------------------|  
|1|指定在发生以下任何情况时应启动自动故障转移：<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务停止。<br /><br /> 因为没有从服务器实例接收到 ACK，连接到 WSFC 群集的可用性组的租期到期。 有关详细信息，请参阅[工作原理：SQL Server Always On 租约超时](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268)。|  
|2|指定在发生以下任何情况时应启动自动故障转移：<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例未连接到群集，并且超出了可用性组的用户指定的 HEALTH_CHECK_TIMEOUT 阈值。<br /><br /> 可用性副本处于失败状态。|  
|3|指定在发生了严重的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内部错误（例如孤立的自旋锁、严重的写访问冲突或过多的转储）时应启动自动故障转移。<br /><br /> 此选项为默认行为。|  
|4|指定在发生了中等程度的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内部错误（例如在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内部资源池中出现持久的内存不足情况）时应启动自动故障转移。|  
|5|指定在出现任何符合的失败条件时应启动自动故障转移，这些失败条件包括：<br /><br /> SQL 引擎的工作线程耗尽。<br /><br /> 检测到无法解决的死锁。|  
  
> [!NOTE]  
>  缺少 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例对客户端请求的响应与可用性组无关。  
  
 FAILURE_CONDITION_LEVEL 和 HEALTH_CHECK_TIMEOUT 值为给定组定义“灵活的故障转移策略”。 此灵活的故障转移策略向您提供对必须导致自动故障转移的条件的精确控制。 有关详细信息，请参阅[针对可用性组的自动故障转移的灵活的故障转移策略 (SQL Server)](../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)。  
  
 HEALTH_CHECK_TIMEOUT = milliseconds  
 指定在 WSFC 群集假定服务器实例速度较慢或无响应前，等待 [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) 系统存储过程返回服务器运行状况信息的等待时间（毫秒）。 HEALTH_CHECK_TIMEOUT 在组级别设置，但仅针对为具有自动故障转移的同步-提交可用性模式 (AVAILABILITY_MODE = SYNCHRONOUS_COMMIT) 配置的可用性副本。  此外，只有在主要副本和次要副本均配置为自动故障转移模式 (FAILOVER_MODE = AUTOMATIC) 并且次要副本当前与主要副本同步的情况下，运行状况检查超时才可以触发自动故障转移。  
  
 默认的 HEALTH_CHECK_TIMEOUT 值为 30000 毫秒（30 秒）。 最小值为 15000 毫秒（15 秒），最大值为 4294967295 毫秒。  
  
 仅在主要副本上受支持。  
  
> [!IMPORTANT]  
>  **sp_server_diagnostics** 在数据库级别不执行运行状况检查。  
  
 DB_FAILOVER = { ON | OFF }  
 指定主要副本上的数据库脱机时要执行的响应。 设置为 ON 时，可用性组中数据库 ONLINE 以外的任何状态都会触发自动故障转移。 当此选项设置为 OFF 时，仅使用实例的运行状况来触发自动故障转移。  
 
 有关此设置的详细信息，请参阅[数据库级别运行状况检测选项](../../database-engine/availability-groups/windows/sql-server-always-on-database-health-detection-failover-option.md) 

DTC_SUPPORT  **=** { PER_DB | NONE }  
指定是否已对此可用性组启用分布式事务。 分布式事务仅适用于 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及以上版本中的可用性组数据库，而跨数据库事务仅适用于 [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] SP2 及以上版本。 `PER_DB` 将创建支持这些事务的可用性组，并且将自动把涉及可用性组中的数据库的跨数据库事务提升为分布式事务。 `NONE` 可阻止将跨数据库事务自动提升为分布式事务，并且不向 DTC 中的稳定 RMID 注册该数据库。 使用 `NONE` 设置时不会阻止分布式事务，但数据库故障转移和自动恢复在某些情况下可能失败。 有关详细信息，请参阅[用于 AlwaysOn 可用性组和数据库镜像的跨数据库事务和分布式事务 (SQL Server)](../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md)。 
 
> [!NOTE]
> [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] Service Pack 2 中引入了对更改可用性组的 DTC_SUPPORT 设置的支持。 此选项无法用于早期版本。 若要在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 的早期版本中更改此设置，必须对可用性组再次执行 DROP 和 CREATE 操作。
 
 REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT   
 在 SQL Server 2017 中引入。 用于设置需要在主要副本提交事务之前提交的最小数量的同步次要副本。 保证 SQL Server 事务等待至事务日志在最小数量的次要副本上更新为止。 默认值为 0，可提供与 SQL Server 2016 相同的行为。 最小值为 0。 最大值为副本数量减 1。 此选项与同步提交模式中的副本相关。 当副本处于同步提交模式时，对主要副本的写入操作会等待至将次要同步副本上的写入提交到副本数据库事务日志为止。 如果托管次要同步副本的 SQL Server 停止响应，则托管主要副本的 SQL Server 将此次要副本标记为 NOT SYNCHRONIZED（未同步）并继续执行操作。 当无响应的数据库恢复联机状态时，它将处于“未同步”状态，副本将标记为不正常，直到主要副本可再次对其执行同步。 此设置可确保主要副本会等待至最少数量的副本已提交每个事务。 如果最少数量的副本不可用，则主要副本上的提交会失败。 对于群集类型 `EXTERNAL`，可用性组添加到群集资源时，设置会更改。 请参阅[可用性组配置的高可用性和数据保护](../../linux/sql-server-linux-availability-group-ha.md)。
  
 ADD DATABASE database_name  
 指定要添加到可用性组的一个或多个用户数据库的列表。 这些数据库必须位于承载当前主副本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上。 您可以为一个可用性组指定多个数据库，但每个数据库只能属于一个可用性组。 有关可用性组可支持的数据库类型的详细信息，请参阅[针对 Always On 可用性组的先决条件、限制和建议 (SQL Server)](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)。 若要找出已属于某个可用性组的本地数据库，请参阅 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 replica_id 列。  
  
 仅在主要副本上受支持。  
  
> [!NOTE]  
>  在创建可用性组后，将需要连接到承载辅助副本的每个服务器实例，然后准备每个辅助数据库并将它们加入可用性组。 有关详细信息，请参阅本主题后面的 [启动 AlwaysOn 辅助数据库的数据移动 (SQL Server)](../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)。  
  
 REMOVE DATABASE database_name  
 从可用性组中删除指定的主数据库和相应的辅助数据库。 仅在主要副本上受支持。  
  
 有关在从可用性组中删除可用性数据库之后推荐执行的后续任务的信息，请参阅[从可用性组删除主要数据库 &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)。  
  
 ADD REPLICA ON  
 指定一到八个 SQL Server 实例以便在可用性组中承载辅助副本。  通过在每个副本的服务器实例地址后追加 WITH (…) 子句来指定每个副本。  
  
 仅在主要副本上受支持。  
  
 您需要将每个新的辅助副本联接到可用性组。 有关详细信息，请参阅本节后面对 JOIN 选项的说明。  
  
 \<server_instance>  
 指定承载副本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的地址。 地址格式依赖于该实例是默认实例还是命名实例以及它是独立实例还是故障转移群集实例 (FCI)。 语法如下所示：  
  
 { '*system_name*[\\*instance_name*]' | '*FCI_network_name*[\\*instance_name*]' }  
  
 此地址由以下部分组成：  
  
 *system_name*  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的目标实例所在的计算机系统的 NetBIOS 名称。 此计算机必须是一个 WSFC 节点。  
  
 *FCI_network_name*  
 用于访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集的网络名称。 如果服务器实例作为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移伙伴参与，则使用此名称。 对 FCI 服务器实例执行 SELECT [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) 会返回其完整的 'FCI_network_name[\\instance_name]' 字符串（即完整的副本名称） 。  
  
 *instance_name*  
 由 system_name 或 FCI_network_name 承载且已启用 Always On 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称 。 对于默认服务器实例， *instance_name* 是可选的。 此实例名不区分大小写。 在独立服务器实例上，此值名称与执行 SELECT [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) 所返回的值相同。  
  
 \  
 仅在指定 instance_name 时使用的分隔符，以便将其与 system_name 或 FCI_network_name 分隔  。  
  
 有关 WSFC 节点和服务器实例的先决条件的信息，请参阅[针对 Always On 可用性组的先决条件、限制和建议 (SQL Server)](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)。  
  
 ENDPOINT_URL ='TCP://system-address:port'  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例（该实例将承载要添加或修改的可用性副本）的[数据库镜像终结点](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)的 URL 路径。  
  
 ENDPOINT_URL 在 ADD REPLICA ON 子句中是必需的，在 MODIFY REPLICA ON 子句中是可选的。  有关详细信息，请参阅 [在添加或修改可用性副本时指定终结点 URL (SQL Server)](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)配置服务器实例时遇到的典型问题。  
  
 **'** TCP **://** _system-address_ **:** _port_ **'**  
 指定一个 URL，它用于指定端点 URL 或只读路由 URL。 URL 参数如下所示：  
  
 *system-address*  
 一个字符串，例如系统名称、完全限定的域名或 IP 地址，它们明确标识了目标计算机系统。  
  
 *port*  
 是与服务器实例的镜像端点关联的端口号（对于 ENDPOINT_URL 选项）或服务器实例的[!INCLUDE[ssDE](../../includes/ssde-md.md)]使用的端口号（对于 READ_ONLY_ROUTING_URL 选项）。  
  
 AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT | CONFIGURATION_ONLY }  
 指定在主副本可以在给定主数据库上提交事务前，是否必须等待辅助副本确认日志记录硬编码（写入）到磁盘。 针对同一主副本上不同数据库的事务可以单独提交。  
  
 SYNCHRONOUS_COMMIT  
 指定主副本已在此辅助副本上进行硬编码（同步提交模式）前，将等待提交事务。 您可以为最多三个副本（包括主副本）指定 SYNCHRONOUS_COMMIT。  
  
 ASYNCHRONOUS_COMMIT  
 指定主副本无需等待此辅助副本对日志进行硬编码（同步提交可用性模式）即可提交事务。 您可以为最多五个可用性副本（包括主副本）指定 ASYNCHRONOUS_COMMIT。  

 CONFIGURATION_ONLY 指定主要副本将可用性组配置元数据同步提交到此副本的主数据库。 副本将不包含用户数据。 此选项：

- 可以在任何版本的 SQL Server 上承载，包括 Express Edition。
- 要求 CONFIGURATION_ONLY 副本的数据镜像终结点为 `WITNESS` 类型。
- 无法更改。
- 当 `CLUSTER_TYPE = WSFC` 时无效。 

   有关详细信息，请参阅[仅配置副本](../../linux/sql-server-linux-availability-group-ha.md)。
    
 AVAILABILITY_MODE 在 ADD REPLICA ON 子句中是必需的，在 MODIFY REPLICA ON 子句中是可选的。 有关详细信息，请参阅 [可用性模式（AlwaysOn 可用性组）](../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)。  
  
 FAILOVER_MODE = { AUTOMATIC | MANUAL }  
 指定您要定义的可用性副本的故障转移模式。  
  
 AUTOMATIC  
 启用自动故障转移。 仅在指定 VAILABILITY_MODE = SYNCHRONOUS_COMMIT 的情况下才支持 AUTOMATIC。 可以为最多三个可用性副本（包括主要副本）指定 AUTOMATIC。  
  
> [!NOTE]  
>  在 SQL Server 2016 之前，限制为两个自动故障转移副本，包括主要副本
  
> [!NOTE]  
>  SQL Server 故障转移群集实例 (FCI) 不支持通过可用性组来自动进行故障转移，因此，只能为手动故障转移配置任何由 FCI 承载的可用性副本。  
  
 MANUAL  
 允许数据库管理员手动故障转移或强制手动故障转移（“强制故障转移”）。  
  
 FAILOVER_MODE 在 ADD REPLICA ON 子句中是必需的，在 MODIFY REPLICA ON 子句中是可选的。 存在在不同条件下支持的两种手动故障转移，没有数据丢失的手动故障转移和强制故障转移（可能存在数据丢失）。  有关详细信息，请参阅 [故障转移和故障转移模式（AlwaysOn 可用性组）](../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)概念。  
  
 SEEDING_MODE = { AUTOMATIC | MANUAL }  
 指定初始设定次要副本种子的方式。  
  
 AUTOMATIC  
 启用直接种子设定。 此方法将通过网络设定次要副本种子。 此方法不要求在副本上备份和还原主数据库的副本。  
  
> [!NOTE]  
>  为实现直接种子设定，必须通过使用 GRANT CREATE ANY DATABASE（授权创建任何数据库）选项调用 ALTER AVAILABILITY GROUP（改变可用性组）来允许在每个次要副本上创建数据库 。  
  
 MANUAL  
 指定手动种子设定（默认）。 此方法要求在主要副本上创建数据库的备份，并在次要副本上手动还原该备份。  
  
 BACKUP_PRIORITY =n  
 指定相对于同一可用性组中的其他副本，在此副本上执行备份的优先级。 该值是范围 0..100 中的整数。 这些值将具有以下含义：  
  
-   1..100 表示可被选择来执行备份的可用性副本。 1 表示最低优先级，100 表示最高优先级。 如果 BACKUP_PRIORITY = 1，则只有在没有更高的优先级可用性副本当前可用的情况下，才会选择可用性副本来执行备份。  
  
-   0 表示此可用性副本将永远不会被选择执行备份。 例如，这对于您永远不希望备份故障转移到的远程可用性副本十分有用。  
  
 有关详细信息，请参阅[活动次要副本：次要副本备份（Always On 可用性组）](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)。  
  
 SECONDARY_ROLE ( ... )  
 指定在此可用性副本当前拥有辅助角色（即它是辅助副本）时将要生效的角色特有设置。 在括号内指定一个或两个辅助角色选项。 如果指定两个选项，则使用以逗号分隔的列表。  
  
 辅助角色选项如下所示：  
  
 ALLOW_CONNECTIONS = { NO | READ_ONLY | ALL }  
 指定给定的可用性副本（正在执行辅助角色，也就是充当辅助副本）的数据库是否可以接受来自客户端的连接，可以是以下之一：  
  
 是  
 不允许与此副本的辅助数据库的用户连接。 它们不可用于读访问。 此选项为默认行为。  
  
 READ_ONLY  
 只允许连接应用程序意向属性设置为 ReadOnly 的次要副本中的数据库。 有关此属性的详细信息，请参阅 [Using Connection String Keywords with SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)。  
  
 ALL  
 允许针对辅助副本中的数据库的所有连接进行只读访问。  
  
 有关详细信息，请参阅[活动次要副本：可读次要副本（Always On 可用性组）](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)。  
  
 READ_ONLY_ROUTING_URL **='** TCP **://** _system-address_ **:** _port_ **'**  
 指定要用于此可用性副本的路由读意向连接请求的 URL。 这是 SQL Server 数据库引擎侦听的 URL。 通常，SQL Server 数据库引擎的默认实例侦听 TCP 端口 1433。  
  
 对于命名实例，可以通过查询 [sys.dm_tcp_listener_states](../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md) 动态管理视图的 port 和 type_desc 列来获取端口号 。 服务器实例使用 Transact-SQL 侦听器 (type_desc='TSQL')。  
  
 有关计算可用性副本的只读路由 URL 的详细信息，请参阅[计算 Always On 的 read_only_routing_url](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson)。  
  
> [!NOTE]  
>  对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的命名实例，应将 Transact-SQL 侦听器配置为使用特定端口。 有关详细信息，请参阅[将服务器配置为侦听特定 TCP 端口（SQL Sever 配置管理器）](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md)。  
  
 PRIMARY_ROLE ( ... )  
 指定在此可用性副本当前拥有主角色（即它是主副本）时将要生效的角色特有设置。 在括号内指定一个或两个主角色选项。 如果指定两个选项，则使用以逗号分隔的列表。  
  
 主角色选项如下所示：  
  
 ALLOW_CONNECTIONS = { READ_WRITE | ALL }  
 指定给定的可用性副本（正在执行主要角色，也就是充当主副本）的数据库可以接受的来自客户端的连接类型，可以是以下之一：  
  
 READ_WRITE  
 不允许 Application Intent 连接属性设置为 **ReadOnly** 的连接。  在 Application Intent 属性设置为 **ReadWrite** 或者未设置 Application Intent 连接属性时，将允许连接。 有关 Application Intent 连接属性的详细信息，请参阅 [Using Connection String Keywords with SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)。  
  
 ALL  
 主副本中的数据库允许所有连接。 此选项为默认行为。  
  
 READ_ONLY_ROUTING_LIST = { ('\<server_instance>' [ ,...n ] ) | NONE }  
 指定一个以逗号分隔的服务器实例列表，这些实例承载在以辅助角色运行时满足以下要求的此可用性组的可用性副本：  
  
-   被配置为允许所有连接或只读连接（参阅上文 SECONDARY_ROLE 选项的 ALLOW_CONNECTIONS 参数）。  
  
-   定义了只读路由 URL（参阅上文 SECONDARY_ROLE 选项的 READ_ONLY_ROUTING_URL 参数）。  
  
 READ_ONLY_ROUTING_LIST 的值如下：  
  
 \<server_instance>  
 指定承载可用性副本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的地址，该副本在以辅助角色运行时是可读辅助副本。  
  
 使用以逗号分隔的列表指定可能承载可读辅助副本的所有服务器实例。 只读路由将遵循在列表中指定服务器实例的顺序。 如果在副本的只读路由列表中包含副本的宿主服务器实例，通常将此服务器实例放在列表末尾比较好，这样在一个辅助副本可用时读意向连接将访问它。  
  
 从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 开始，可在可读次要副本间实现读意向请求的负载均衡。 可通过将副本放入只读路由列表中的一组嵌套括号中来指定。 有关详细信息和示例，请参阅[在只读副本间配置负载均衡](../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md#loadbalancing)。  
  
 无  
 指定此可用性副本为主副本时将不支持只读路由。 此选项为默认行为。 与 MODIFY REPLICA ON 一起使用时，此值将禁用现有列表（如果有）。  

 READ_WRITE_ROUTING_URL = { ('\<server_instance>') }  
 适用对象：SQL Server（从 SQL Server 2019 (15.x) 开始） 

 指定服务器实例，这些实例承载在以主角色运行时满足以下要求的此可用性组的可用性副本：
-   副本规范 PRIMARY_ROLE 包括 READ_WRITE_ROUTING_URL。
-   连接字符串为 ReadWrite，通过将 ApplicationIntent 定义为 ReadWrite 或不设置 ApplicationIntent 并使默认值 (ReadWrite) 生效来实现。

有关详细信息，请参阅[次要副本到主要副本读/写连接重定向（AlwaysOn 可用性组）](../../database-engine/availability-groups/windows/secondary-replica-connection-redirection-always-on-availability-groups.md)。

 SESSION_TIMEOUT =seconds  
 以秒为单位指定会话超时期限。 如果不指定此选项，则在默认情况下，超时期限为 10 秒。 最小值为 5 秒。  
  
> [!IMPORTANT]  
>  我们建议您将超时期限保持为 10 秒或更长。  
  
 有关会话超时期限的详细信息，请参阅 [Always On 可用性组概述 (SQL Server)](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)。  
  
 MODIFY REPLICA ON  
 修改可用性组的任何副本。 要修改的副本列表包含每个副本的服务器实例地址和 WITH (…) 子句。  
  
 仅在主要副本上受支持。  
  
 REMOVE REPLICA ON  
 从可用性组中删除指定的辅助副本。 不能从可用性组删除当前的主副本。 在删除时，副本停止接收数据。 其辅助数据库从可用性组中删除，并且进入 RESTORING 状态。  
  
 仅在主要副本上受支持。  
  
> [!NOTE]  
>  如果您在某一副本处于不可用或失败状态时删除该副本，则在其恢复联机状态时，将会发现不再属于该可用性组。  
  
 JOIN  
 导致本地服务器实例承载指定可用性组中的辅助副本。  
  
 仅在尚未加入可用性组的辅助副本上支持。  
  
 有关详细信息，请参阅 [将辅助副本联接到可用性组 (SQL Server)](../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)或 PowerShell 将辅助数据库联接到 Always On 可用性组。  
  
 FAILOVER  
启动可用性组的手动故障转移，并且没有对您连接到的辅助副本的数据丢失。 将承载主要副本的副本是故障转移目标。  故障转移目标将接管主要角色，恢复各数据库的副本并且使它们作为新的主数据库处于联机状态。 以前的主副本同时转换为辅助角色，并且其数据库将成为辅助数据库且立即挂起。 在发生一系列故障后，这些角色可能来回切换。  
  
 仅在当前与主副本同步的同步提交辅助副本上支持。 请注意，对于要同步的辅助副本，主副本也必须在同步提交模式下运行。  
  
> [!NOTE]  
>  故障转移命令将在故障转移目标接受它之后立即返回。 但是，在可用性组完成故障转移之后，数据库恢复操作将以异步方式执行。  
  
 有关执行计划的手动故障转移的限制、先决条件和建议的信息，请参阅[执行可用性组的计划的手动故障转移 &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)。  
  
 FORCE_FAILOVER_ALLOW_DATA_LOSS  
 > [!CAUTION]  
>  强制故障转移（这可能会涉及一些数据丢失）严格来说是一种灾难恢复方法。 因此，我们强烈建议您仅在以下情况下才强制故障转移：主副本不再运行、您愿意承担丢失数据的风险并且您必须立即将服务还原到可用性组。  
  
 仅在其角色处于 SECONDARY 或 RESOLVING 状态的副本上支持。 --对其输入故障转移命令的副本称为“故障转移目标”。  
  
 强制将可用性组故障转移到故障转移目标（可能会丢失数据）。 故障转移目标将接管主要角色，恢复各数据库的副本并且使它们作为新的主数据库处于联机状态。 在剩余的任何辅助副本上，在手动恢复前每个辅助数据库都处于挂起状态。 在以前的主副本可用前，它将切换到辅助角色，并且其数据库将成为挂起的辅助数据库。  
  
> [!NOTE]  
>  故障转移命令将在故障转移目标接受它之后立即返回。 但是，在可用性组完成故障转移之后，数据库恢复操作将以异步方式执行。  
  
 有关强制故障转移的限制、先决条件和建议的信息，以及强制故障转移对可用性组中以前的主数据库的影响，请参阅[执行可用性组的强制手动故障转移 &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)。  
  
 ADD LISTENER '_dns\_name_'( \<add_listener_option> )  
 为此可用性组定义新的可用性组侦听器。 仅在主要副本上受支持。  
  
> [!IMPORTANT]
>  创建第一个侦听器之前，强烈建议阅读[创建或配置可用性组侦听程序 (SQL Server)](../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)。  
> 
>  为给定可用性组创建侦听器后，我们强烈建议您执行以下操作：  
> 
>  -   请求您的网络管理员将该侦听器的 IP 地址保留为专用。  
> -   将该侦听器的 DNS 主机名提供给应用程序开发人员，以便在请求与此可用性组的客户端连接时用于连接字符串中。  
  
 dns_name  
 指定可用性组侦听器的 DNS 主机名。 在域和 NetBIOS 中，侦听器的 DNS 名称必须唯一。  
  
 dns_name 为字符串值。 该名称只能包含字母数字字符、破折号 (-) 和连字符 (_)，顺序不分先后。 DNS 主机名不区分大小写。 最大长度为 63 个字符。  
  
 我们建议您指定一个有意义的字符串。 例如，对于名为 `AG1`的可用性组，有意义的 DNS 主机名将是 `ag1-listener`。  
  
> [!IMPORTANT]  
>  NetBIOS 只识别 dns_name 中的前 15 个字符。 如果您的两个 WSFC 群集均由同一 Active Directory 控制，而您试图使用超过 15 个字符的名称（具有相同的 15 字符前缀）在这两个群集中创建可用性组侦听器，此时您将收到错误，报告无法使虚拟网络名称资源联机。 有关 DNS 名称的前缀命名规则的信息，请参阅 [分配域名](https://technet.microsoft.com/library/cc731265\(WS.10\).aspx)。  
  
 JOIN AVAILABILITY GROUP ON  
 联接到分布式可用性组。 创建分布式可用性组时，在群集上创建的可用性组为主要可用性组。 联接分布式可用性组的可用性组为次要可用性组。  
  
 \<ag_name>  
 指定构成一半分布式可用性组的可用性组名称。  
  
 LISTENER **='** TCP **://** _system-address_ **:** _port_ **'**  
 指定与可用性组关联的侦听器的 URL 路径。  
  
 必须有 LISTENER 子句。  
  
 **'** TCP **://** _system-address_ **:** _port_ **'**  
 指定与可用性组关联的侦听器的 URL。 URL 参数如下所示：  
  
 *system-address*  
 一个字符串，例如系统名称、完全限定的域名或 IP 地址，它们明确标识了侦听器。  
  
 *port*  
 是与可用性组的镜像终结点关联的端口号。 请注意，这不是侦听器的端口。  
  
 AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT }  
 指定在主要副本可以在给定主数据库上提交事务前，是否必须等待次要可用性组确认日志记录硬编码（写入）到磁盘。  
  
 SYNCHRONOUS_COMMIT  
 指定主要副本等到在次要可用性组上进行了硬编码后才提交事务。 可以为最多两个可用性组（包括主要可用性组）指定 SYNCHRONOUS_COMMIT。  
  
 ASYNCHRONOUS_COMMIT  
 指定主要副本无需等待该次要可用性组对日志进行硬编码即可提交事务。 可以为最多两个可用性组（包括主要可用性组）指定 ASYNCHRONOUS_COMMIT。  
  
 AVAILABILITY_MODE 子句是必需的。  
  
 FAILOVER_MODE = { MANUAL }  
 指定分布式可用性组的故障转移模式。  
  
 MANUAL  
 允许数据库管理员执行的手动故障转移或强制手动故障转移（通常称为“强制故障转移”）。  
  
 不支持自动故障转移到次要可用性组。  
  
 SEEDING_MODE= { AUTOMATIC | MANUAL }  
 指定初始设定次要可用性组种子的方式。  
  
 AUTOMATIC  
 启用自动种子设定。 此方法将通过网络设定次要可用性组种子。 此方法不要求在次要可用性组的副本上备份和还原主数据库的副本。  
  
 MANUAL  
 指定手动设定种子。 此方法要求在主要副本上创建数据库的备份，并在次要可用性组的副本上手动还原该备份。  
  
 MODIFY AVAILABILITY GROUP ON  
 修改分布式可用性组的任意可用性组设置。 要修改的可用性组列表包含可用性组名称和每个可用性组的 WITH (…) 子句。  
  
> [!IMPORTANT]  
>  必须同时对主要可用性组实例和次要可用性组实例重复此命令。  
  
 GRANT CREATE ANY DATABASE  
 允许可用性组代表支持直接设定种子的主要副本创建数据库 (SEEDING_MODE = AUTOMATIC)。 在次要副本联接可用性组后，应对支持直接设定种子的每个次要副本运行此参数。  需要 CREATE ANY DATABASE 权限。  
  
 DENY CREATE ANY DATABASE  
 删除可用性组代表主要副本创建数据库的功能。  
  
 \<add_listener_option>  
 ADD LISTENER 采用以下选项之一：  
  
 WITH DHCP [ ON { **('** _four\_part\_ipv4\_address_ **','** _four\_part\_ipv4\_mask_ **')** } ]  
 指定可用性组侦听器将使用动态主机配置协议 (DHCP)。  或者，使用 ON 子句标识将在其上创建此侦听器的网络。 DHCP 限制为单个子网，该子网用于在可用性组中承载可用性副本的每个服务器实例。  
  
> [!IMPORTANT]  
>  不建议在生产环境中使用 DHCP。 如果停止工作且 DHCP IP 租期已到，则需要额外的时间来注册与侦听器 DNS 名称相关联且影响客户端连接的新 DHCP 网络 IP 地址。 但是，DHCP 适合用于设置开发和测试环境以验证可用性组的基本功能并适合与应用程序集成。  
  
 例如：  
  
 `WITH DHCP ON ('10.120.19.0','255.255.254.0')`  
  
 WITH IP **(** { **(‘** _four\_part\_ipv4\_address_ **’,‘** _four\_part\_ipv4\_mask_ **’)**  |  **(‘** _ipv6\_address_ **’)** } [ **,** ..._n_ ] **)** [ **,** PORT **=** _listener\_port_ ]  
 指定可用性组侦听器将使用一个或多个静态 IP 地址，而不使用 DHCP。 若要跨多个子网创建一个可用性组，每个子网均需要一个侦听器配置中的静态 IP 地址。 对于某一给定子网，静态 IP 地址可以是 IPv4 地址或 IPv6 地址。 请与您的网络管理员联系以获取将承载新可用性组的可用性副本的每个子网的静态 IP 地址。  
  
 例如：  
  
 `WITH IP ( ('10.120.19.155','255.255.254.0') )`  
  
 *ipv4_address*  
 指定可用性组侦听器的由四部分组成的 IPv4 地址。 例如，`10.120.19.155`。  
  
 *ipv4_mask*  
 指定可用性组侦听器的由四部分组成的 IPv4 掩码。 例如，`255.255.254.0`。  
  
 ipv6_address  
 指定可用性组侦听器的 IPv6 地址。 例如，`2001::4898:23:1002:20f:1fff:feff:b3a3`。  
  
 PORT = listener_port  
 指定端口号 listener_port，以供由 WITH IP 子句指定的可用组侦听器使用。 PORT 是可选的。  
  
 支持默认端口号 1433。 但出于安全考虑，我们建议使用其他端口号。  
  
 例如： `WITH IP ( ('2001::4898:23:1002:20f:1fff:feff:b3a3') ) , PORT = 7777`  
  
 MODIFY LISTENER '_dns\_name_'( \<modify\_listener\_option\> )  
 修改此可用性组的现有可用性组侦听器。 仅在主要副本上受支持。  
  
 \<modify\_listener\_option\>  
 MODIFY LISTENER 采用以下选项之一：  
  
 ADD IP { **('** _four\_part\_ipv4\_address_ **','** _four\_part\_ipv4_mask_ **')** \| <b>('</b>dns\_name *ipv6\_address* __')__ }  
 将指定的 IP 地址添加到由 dns\_name 指定的可用性组侦听器。  
  
 PORT = listener_port  
 请参阅本节前面对此参数的说明。  
  
 RESTART LISTENER 'dns\_name'  
 重新启动与指定的 DNS 名称关联的侦听器。 仅在主要副本上受支持。  
  
 REMOVE LISTENER **'** _dns\_name_ **'**  
 删除与指定的 DNS 名称关联的侦听器。 仅在主要副本上受支持。  
  
 OFFLINE  
 使联机的可用性组脱机。 同步提交数据库没有数据丢失。  
  
 在某一可用性组脱机后，其数据库将不可用于客户端，并且您无法使该可用性组重新联机。 因此，在将可用性组资源迁移到新 WSFC 群集时，仅在 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 的跨群集迁移过程中使用 OFFLINE 选项。  
  
 有关详细信息，请参阅[使可用性组脱机 (SQL Server)](../../database-engine/availability-groups/windows/take-an-availability-group-offline-sql-server.md)。  
  
## <a name="prerequisites-and-restrictions"></a>先决条件和限制  
 有关可用性副本及其主机服务器实例与计算的先决条件和限制的信息，请参阅 [Always On 可用性组的先决条件、限制和建议 (SQL Server)](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)。  
  
 有关 AVAILABILITY GROUP Transact-SQL 语句的限制的信息，请参阅 [Always On 可用性组的 Transact-SQL 语句的概述 (SQL Server)](../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md)。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  还需要 ALTER ANY DATABASE 权限。   
  
## <a name="examples"></a>示例  
  
###  <a name="a-joining-a-secondary-replica-to-an-availability-group"></a><a name="Join_Secondary_Replica"></a> A. 将次要副本联接到可用性组  
 以下示例联接连接到 `AccountsAG` 可用性组的次要副本。  
  
```SQL  
ALTER AVAILABILITY GROUP AccountsAG JOIN;  
GO  
```  
  
###  <a name="b-forcing-failover-of-an-availability-group"></a><a name="Force_Failover"></a> B. 强制可用性组的故障转移  
 下面的示例强制 `AccountsAG` 可用性组故障转移到您所连接的辅助副本。  
  
```SQL
ALTER AVAILABILITY GROUP AccountsAG FORCE_FAILOVER_ALLOW_DATA_LOSS;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE AVAILABILITY GROUP (Transact-SQL)](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER DATABASE SET HADR (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)   
 [DROP AVAILABILITY GROUP (Transact-SQL)](../../t-sql/statements/drop-availability-group-transact-sql.md)   
 [sys.availability_replicas (Transact-SQL)](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [sys.availability_groups (Transact-SQL)](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   
 [Always On 可用性组配置故障排除 (SQL Server)](../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)   
 [AlwaysOn 可用性组概述 (SQL Server)](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [可用性组侦听程序、客户端连接和应用程序故障转移 (SQL Server)](../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)  
  

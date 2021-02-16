---
title: 可行性组的常见问题和解决方法
description: 解决在 SQL Server 中为 Always On 可用性组配置服务器实例时遇到的典型问题。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- troubleshooting [SQL Server], deploying
- Availability Groups [SQL Server], troubleshooting
- Availability Groups [SQL Server], configuring
ms.assetid: 8c222f98-7392-4faf-b7ad-5fb60ffa237e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7b7c6a5df1bcef7e8438c01910233c2c3a38d070
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337936"
---
# <a name="troubleshoot-always-on-availability-groups-configuration-sql-server"></a>AlwaysOn 可用性组配置疑难解答 (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  本主题提供的信息可帮助您解决在为 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]配置服务器实例时遇到的典型问题。 典型配置问题包括 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 被禁用、帐户配置不当、数据库镜像端点不存在、端点无法访问（SQL Server 错误 1418）、网络访问不存在，以及联接数据库命令失败（SQL Server 错误 35250）。  
  
> [!NOTE]  
>  确保您满足 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的先决条件。 有关详细信息，请参阅 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)配置服务器实例时遇到的典型问题。  
  
 **本主题内容：**  
  
|部分|说明|  
|-------------|-----------------|  
|[未启用 AlwaysOn 可用性组](#IsHadrEnabled)|如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例未启用 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]，该实例则不支持创建可用性组，也无法承载任何可用性副本。|  
|[帐户](#Accounts)|介绍了正确配置运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 所用的帐户的相关要求。|  
|[端点](#Endpoints)|介绍如何诊断与服务器实例的数据库镜像端点有关的问题。|  
|[系统名称](#SystemName)|概述了在端点 URL 中指定服务器实例的系统名称的备选方法。|  
|[网络访问](#NetworkAccess)|记录了承载可用性副本的每个服务器实例必须能够通过 TCP 访问其他各个服务器实例的端口的要求。|  
|[端点访问（SQL Server 错误 1418）](#Msg1418)|包含有关此 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 错误消息的信息。|  
|[联接数据库失败（SQL Server 错误 35250）](#JoinDbFails)|介绍由于与主副本的连接处于非活动状态而导致未能将辅助数据库联接到可用性组的可能原因和解决方法。|  
|[只读路由未正确工作](#ROR)||  
|[相关任务](#RelatedTasks)|包含 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 联机丛书中专门针对排除可用性组配置问题的面向任务的主题列表。|  
|[相关内容](#RelatedContent)|包含 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 联机丛书以外的相关资源的列表。|  
  
##  <a name="always-on-availability-groups-is-not-enabled"></a><a name="IsHadrEnabled"></a> 未启用 AlwaysOn 可用性组  
 必须在每个 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 实例上启用 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]功能。 有关详细信息，请参阅[启用和禁用 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)。  
  
##  <a name="accounts"></a><a name="Accounts"></a> 帐户  
 必须正确配置运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 所用的帐户。  
  
1.  帐户是否具有正确的权限？  
  
    1.  如果伙伴使用相同的域用户帐户运行，则正确的用户登录名将自动存在于全部两个 **master** 数据库中。 这样可简化数据库的安全配置并建议这样做。  
  
    2.  如果两个服务器实例使用不同的帐户运行，则必须在远程服务器实例上的 **master** 数据库中创建每个登录帐户，并且必须向该登录帐户授予 CONNECT 权限，以便连接到该服务器实例的数据库镜像端点。 有关详细信息，请参阅[设置数据库镜像或 AlwaysOn 可用性组的登录帐户 (SQL Server)](../../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md)。  
  
2.  如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 正在以内置帐户（例如 Local System、Local Service 或 Network Service）或非域帐户运行，则您必须使用证书来进行端点身份验证。 如果您的服务帐户使用的是同一个域中的域帐户，则您可以选择为所有副本位置上的每个服务帐户授予 CONNECT 访问权限，或者您可以使用证书。 有关详细信息，请参阅[使用数据库镜像终结点证书 (Transact-SQL)](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)。  
  
##  <a name="endpoints"></a><a name="Endpoints"></a> Endpoints  
 必须正确配置端点。  
  
1.  确保要托管可用性副本（每个副本位置）的各个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例都具有数据库镜像终结点。 若要确定给定服务器实例上是否存在数据库镜像终结点，请使用 [sys.database_mirroring_endpoints](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md) 目录视图。 有关详细信息，请参阅[创建 Windows 身份验证的数据库镜像终结点 (Transact-SQL)](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md) 或[允许数据库镜像终结点使用证书进行出站连接 (Transact-SQL)](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)。  
  
2.  检查端口号是否正确。  
  
     若要标识当前与服务器实例的数据库镜像端点关联的端口，请使用以下 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句：  
  
    ```  
    SELECT type_desc, port FROM sys.tcp_endpoints;  
    GO  
    ```  
  
3.  对于难以解释的 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 设置问题，建议您检查每个服务器实例以确定它是否正在侦听相应的端口。  
  
4.  确保已启动端点 (STATE = STARTED)。 对于各个服务器实例，使用以下 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句：  
  
    ```  
    SELECT state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
     有关 **state_desc** 列的详细信息，请参阅 [sys.database_mirroring_endpoints (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)。  
  
     若要启动端点，请使用以下 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句：  
  
    ```  
    ALTER ENDPOINT Endpoint_Mirroring   
    STATE = STARTED   
    AS TCP (LISTENER_PORT = <port_number>)  
    FOR database_mirroring (ROLE = ALL);  
    GO  
    ```  
  
     有关详细信息，请参阅 [ALTER ENDPOINT (Transact-SQL)](../../../t-sql/statements/alter-endpoint-transact-sql.md)。  
  
5.  确保其他服务器的登录帐户具有 CONNECT 权限。 若要确定哪个登录帐户拥有对端点的 CONNECT 权限，请对每个服务器实例使用以下 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句：  
  
    ```  
    SELECT 'Metadata Check';  
    SELECT EP.name, SP.STATE,   
       CONVERT(nvarchar(38), suser_name(SP.grantor_principal_id))   
          AS GRANTOR,   
       SP.TYPE AS PERMISSION,  
       CONVERT(nvarchar(46),suser_name(SP.grantee_principal_id))   
          AS GRANTEE   
       FROM sys.server_permissions SP , sys.endpoints EP  
       WHERE SP.major_id = EP.endpoint_id  
       ORDER BY Permission,grantor, grantee;   
    GO  
  
    ```  
  
##  <a name="system-name"></a><a name="SystemName"></a> System Name  
 对于端点 URL 中服务器实例的系统名称，可以使用明确标识系统的任何名称。 服务器地址可以是系统名称（如果各系统都在同一个域中）、完全限定域名或 IP 地址（最好是静态 IP 地址）。 保证使用完全限定域名的有效性。 有关详细信息，请参阅 [在添加或修改可用性副本时指定终结点 URL (SQL Server)](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)配置服务器实例时遇到的典型问题。  
  
##  <a name="network-access"></a><a name="NetworkAccess"></a> Network Access  
 要承载可用性副本的每个服务器实例必须能够通过 TCP 访问其他各个服务器实例的端口。 当服务器实例位于相互不信任的不同域（不可信的域）中时，这尤为重要。  
  
##  <a name="endpoint-access-sql-server-error-1418"></a><a name="Msg1418"></a> 端点访问（SQL Server 错误 1418）  
 此 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 消息指示无法到达端点 URL 中指定的服务器网络地址或该地址不存在，同时建议您确认网络地址名称并重新发出命令。  
  
##  <a name="join-database-fails-sql-server-error-35250"></a><a name="JoinDbFails"></a> 联接数据库失败（SQL Server 错误 35250）  
 此部分介绍由于与主副本的连接处于非活动状态而导致未能将辅助数据库联接到可用性组的可能原因和解决方法。  
  
 **解决方法：**  
  
1.  检查防火墙设置，确定是否允许在承载主副本的服务器实例与辅助副本之间进行端点端口通信（默认情况下为端口 5022）。  
  
2.  检查网络服务帐户是否拥有对端点的 CONNECT 权限。  
  
##  <a name="read-only-routing-is-not-working-correctly"></a><a name="ROR"></a> 只读路由未正确工作  
 验证以下配置值设置并且根据需要进行更正。  
  
|On...|操作|注释|链接|  
|---------|------------|--------------|----------|  
|当前主副本|确保可用性组侦听器处于联机状态。|**验证侦听器是否处于联机状态：**<br /><br /> `SELECT * FROM sys.dm_tcp_listener_states;`<br /><br /> **重新启动处于脱机状态的侦听器：**<br /><br /> `ALTER AVAILABILITY GROUP myAG RESTART LISTENER 'myAG_Listener';`|[sys.dm_tcp_listener_states (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|当前主副本|确保 READ_ONLY_ROUTING_LIST 仅包含承载可读辅助副本的服务器实例。|**标识可读次要副本：** sys.availability_replicas（**secondary_role_allow_connections_desc** 列）<br /><br /> **查看只读路由列表：** sys.availability_read_only_routing_lists<br /><br /> **更改只读路由列表：** ALTER AVAILABILITY GROUP|[sys.availability_replicas (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)<br /><br /> [sys.availability_read_only_routing_lists (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|read_only_routing_list 中的每个副本|请确保 Windows 防火墙未在阻止 READ_ONLY_ROUTING_URL 端口。|-|[为数据库引擎访问配置 Windows 防火墙](../../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)|  
|read_only_routing_list 中的每个副本|在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器中，请确认：<br /><br /> 已启用 SQL Server 远程连接。<br /><br /> 已启用 TCP/IP。<br /><br /> IP 地址已正确配置。|-|[查看或更改服务器属性 (SQL Server)](../../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md)<br /><br /> [配置服务器以侦听特定 TCP 端口（SQL Server 配置管理器）](../../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md)|  
|read_only_routing_list 中的每个副本|确保 READ_ONLY_ROUTING_URL (TCP <strong>://</strong>*system-address*<strong>:</strong>*port*) 包含正确的完全限定域名 (FQDN) 和端口号。|-|[计算 AlwaysOn 的 read_only_routing_url](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson)<br /><br /> [sys.availability_replicas (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)<br /><br /> [ALTER AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/alter-availability-group-transact-sql.md)|  
|客户端系统|确认客户端驱动程序支持只读路由。|-|[AlwaysOn 客户端连接 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)|  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [创建和配置可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)  
  
-   [为 Windows 身份验证创建数据库镜像终结点 (Transact-SQL)](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [在添加或修改可用性副本时指定终结点 URL (SQL Server)](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [为可用性组手动准备辅助数据库 (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [添加文件操作失败的故障排除（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
-   [管理可用性组中数据库的登录名和作业 (SQL Server)](../../../database-engine/availability-groups/windows/logins-and-jobs-for-availability-group-databases.md)  
  
-   [当数据库在其他服务器实例上可用时管理元数据 (SQL Server)](../../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   [查看故障转移群集的事件和日志](https://technet.microsoft.com/library/cc772342\(WS.10\).aspx)  
  
-   [Get-ClusterLog 故障转移群集 Cmdlet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10))  
  
-   [SQL Server Always On 团队博客：SQL Server Always On 团队官方博客](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>另请参阅  
 [针对数据库镜像和 AlwaysOn 可用性组的传输安全性 (SQL Server)](../../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [客户端网络配置](../../../database-engine/configure-windows/client-network-configuration.md)   
 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  

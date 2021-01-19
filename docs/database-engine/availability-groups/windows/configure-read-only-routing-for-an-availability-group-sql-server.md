---
title: 为可用性组配置只读路由
description: 使用 Transact-SQL (T-SQL) 或 PowerShell 将所有只读流量自动路由到 Always On 可用性组的使用只读路由的次要副本。
ms.custom: seodec18
ms.date: 02/25/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- read-only routing
- Availability Groups [SQL Server], readable secondary replicas
- Availability Groups [SQL Server], read-only routing
- readable secondary replicas
- Availability Groups [SQL Server], client connectivity
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 7bd89ddd-0403-4930-a5eb-3c78718533d4
author: cawrites
ms.author: chadam
ms.openlocfilehash: f7e96df4eba36bbcb3da18a1423b5162aef557a7
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170799"
---
# <a name="configure-read-only-routing-for-an-always-on-availability-group"></a>为 Always On 可用性组配置只读路由
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  若要配置 AlwaysOn 可用性组以便在 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]中支持只读路由，你可以使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 或 PowerShell。 *只读路由* 指的是 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将符合条件的只读连接请求路由到可用的 AlwaysOn [可读次要副本](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md) （即，配置为在辅助角色下运行时允许只读工作负荷的副本）的能力。 为支持只读路由，可用性组必须具备 [可用性组侦听器](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)。 只读客户端必须将其连接请求定向到此侦听器，并且客户端的连接字符串必须将应用程序意向指定为“只读”。 也就是说，它们必须是 *读意向连接请求*。  

只读路由在 [!INCLUDE[sssql15](../../../includes/sssql16-md.md)] 及更高版本中可用。

> [!NOTE]  
>  有关如何配置可读次要副本的信息，请参阅 [配置对可用性副本的只读访问 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)。  

  
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   可用性组必须拥有可用性组侦听器。 有关详细信息，请参阅 [创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)。  
  
-   必须在辅助角色中将一个或多个可用性副本配置为接受只读（即配置为 [可读次要副本](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)）。 有关详细信息，请参阅 [配置对可用性副本的只读访问 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)。  
  
-   您必须连接到承载当前主副本的服务器实例。  

-   如果使用 SQL 登录名，请确保帐户配置正确无误。 有关详细信息，请参阅[管理可用性组中数据库的登录名和作业 (SQL Server)](logins-and-jobs-for-availability-group-databases.md)。
  
##  <a name="what-replica-properties-do-you-need-to-configure-to-support-read-only-routing"></a><a name="RORReplicaProperties"></a> 为支持只读路由，您需要配置哪些副本属性？  
  
-   对于要支持只读路由的每个可读次要副本，你需要指定 *只读路由 URL*。 此 URL 仅在本地副本在辅助角色下运行时起作用。 必须根据需要在逐个副本的基础上指定只读路由 URL。 每个只读路由 URL 都用于将读意向请求路由到一个特定的可读辅助副本。 通常，向每个可读辅助副本分配一个只读路由 URL。  
  
     有关计算可用性副本的只读路由 URL 的信息，请参阅 [计算 AlwaysOn 的 read_only_routing_url](https://web.archive.org/web/20170512023255/https://blogs.msdn.microsoft.com/mattn/2012/04/25/calculating-read_only_routing_url-for-alwayson/)
  
-   对于要在其作为主要副本时支持只读路由的每个可用性副本，都需要指定一个 *只读路由列表*。 一个给定的只读路由列表仅在本地副本在主角色下运行时才起作用。 必须根据需要在逐个副本的基础上指定此列表。 通常，每个只读路由列表中将包含各只读路由 URL，并且在列表的末尾具有本地副本的 URL。  
  
    > [!NOTE]  
    >  读意向连接请求将被路由到当前主副本的只读路由列表上的第一个可用条目。 但支持在只读副本间的负载平衡。 有关详细信息，请参阅 [在只读副本间配置负载平衡](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md#loadbalancing)。  
  
> [!NOTE]  
>  有关可用性组侦听程序的信息，以及只读路由的详细信息，请参阅 [可用性组侦听程序、客户端连接和应用程序故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)。  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
  
|任务|权限|  
|----------|-----------------|  
|在创建可用性组时配置副本|需要 **sysadmin** 固定服务器角色的成员资格，以及 CREATE AVAILABILITY GROUP 服务器权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。|  
|修改可用性副本|对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。|  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
### <a name="configure-a-read-only-routing-list"></a>配置只读路由列表  
 使用以下步骤，通过使用 Transact-SQL 配置只读路由。 有关代码示例，请参阅本节后面的 [示例 (Transact-SQL)](#TsqlExample)。  
  
1.  连接到承载主副本的服务器实例。  
  
2.  若要指定的是新可用性组的副本，请使用 [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句。 如果正在添加或修改现有可用性组的副本，请使用 [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句。  
  
    -   若要配置辅助角色的只读路由，请在 ADD REPLICA 或 MODIFY REPLICA WITH 子句中指定 SECONDARY_ROLE 选项，如下所示：  
  
         SECONDARY_ROLE **(** READ_ONLY_ROUTING_URL **='** TCP **://** _system-address_ **:** _port_ **')**  
  
         只读路由 URL 的参数如下所示：  
  
         *system-address*  
         一个字符串，例如系统名称、完全限定的域名或 IP 地址，它们明确标识了目标计算机系统。  
  
         *port*  
         一个端口号，由 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的数据库引擎使用。  
  
         例如：   `SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER01.contoso.com:1433')`  
  
         如果副本已配置为允许只读连接，则在 MODIFY REPLICA 子句中，ALLOW_CONNECTIONS 是可选的。  
  
         有关详细信息，请参阅 [计算 AlwaysOn 的 read_only_routing_url](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson)。  
  
    -   若要配置主角色的只读路由，请在 ADD REPLICA 或 MODIFY REPLICA WITH 子句中指定 PRIMARY_ROLE 选项，如下所示：  
  
         PRIMARY_ROLE ( READ_ONLY_ROUTING_LIST =('server' [ ,...n ] ))         
  
         其中， *server* 标识一个托管可用性组中的只读次要副本的服务器实例。  
  
         例如：   `PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('Server1','Server2'))`  
  
        > [!NOTE]  
        >  您必须先设置只读路由 URL，然后才能配置只读路由列表。  
  
###  <a name="configure-load-balancing-across-read-only-replicas"></a><a name="loadbalancing"></a> 在只读副本间配置负载平衡  
 从 [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)]开始，可以在一组只读副本间配置负载平衡。 以前，只读路由始终都将流量定向到路由列表中第一个可用的副本。 若要利用此功能，请使用一个级别的嵌套括号将 **CREATE AVAILABILITY GROUP** 或 **ALTER AVAILABILITY GROUP** 命令中的 **READ_ONLY_ROUTING_LIST** 服务器实例括起来。  
  
 例如，以下路由列表在两个只读副本 `Server1` 和 `Server2`之间的读意向连接请求实现负载平衡。 括住这些服务器的嵌套圆括号可以标识已实现负载平衡的组。 如果该组中没有副本，则它将继续尝试按顺序连接到只读路由列表中的其他副本： `Server3` 和 `Server4`。  
  
```sql  
READ_ONLY_ROUTING_LIST = (('Server1','Server2'), 'Server3', 'Server4')  
```  
  
 请注意，路由列表中的每项本身也可以是一组负载平衡的只读副本。 下面的示例对这种情况进行了演示。  
  
```sql  
READ_ONLY_ROUTING_LIST = (('Server1','Server2'), ('Server3', 'Server4', 'Server5'), 'Server6')  
```  
  
 仅支持一个级别的嵌套圆括号。  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 以下示例将修改现有可用性组 `AG1` 的两个可用性副本以支持只读路由（如果其中一个副本拥有主角色）。 为了标识承载可用性副本的服务器实例，此示例指定了实例名称 `COMPUTER01` 和 `COMPUTER02`。  
  
```  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER01.contoso.com:1433'));  
  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER02.contoso.com:1433'));  
  
ALTER AVAILABILITY GROUP [AG1]   
MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER02','COMPUTER01')));  
  
ALTER AVAILABILITY GROUP [AG1]   
MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER01','COMPUTER02')));  
GO  
  
```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell  
  
### <a name="configure-a-read-only-routing-list"></a>配置只读路由列表  
 使用以下步骤，通过使用 PowerShell 配置只读路由。 有关代码示例，请参阅本节后面的 [示例 (PowerShell)](#PSExample)。  
  
1.  将默认值 (**cd**) 设置为托管主副本的服务器实例。  
  
2.  在将可用性副本添加到可用性组中时，使用 **New-SqlAvailabilityReplica** cmdlet。 在修改现有可用性副本时，使用 **Set-SqlAvailabilityReplica** cmdlet。 相关参数如下：  
  
    -   若要为辅助角色配置只读路由，请指定 **ReadonlyRoutingConnectionUrl"** _url_ **"** 参数。  
  
         其中， *url* 是当路由到副本时要用于建立只读连接的连接完全限定域名 (FQDN) 和端口。 例如：`-ReadonlyRoutingConnectionUrl "TCP://DBSERVER8.manufacturing.Adventure-Works.com:7024"`  
  
         有关详细信息，请参阅 [计算 AlwaysOn 的 read_only_routing_url](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson)。  
  
    -   若要为主要角色配置连接访问，请指定 **ReadonlyRoutingList"** _server_ **"** [ **,** ...*n* ]，其中， *server* 标识一个托管可用性组中的只读次要副本的服务器实例。 例如：`-ReadOnlyRoutingList "SecondaryServer","PrimaryServer"`  
  
        > [!NOTE]  
        >  您必须先设置副本的只读路由 URL，然后才能为其配置只读路由列表。  
  
    > [!NOTE]  
    >  若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
### <a name="set-up-and-use-the-sql-server-powershell-provider"></a>设置和使用 SQL Server PowerShell 提供程序  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
###  <a name="example-powershell"></a><a name="PSExample"></a> 示例 (PowerShell)  
 以下示例在可用性组中配置主副本和一个辅助副本以进行只读路由。 首先，该示例将向每个副本分配一个只读路由 URL。 然后，在主副本上设置只读路由列表。 连接字符串中的设置了“ReadOnly”属性的连接将被重定向到辅助副本。 如果此次要副本无法读取（由 **ConnectionModeInSecondaryRole** 设置确定），则连接将被定向回主要副本。  
  
```  
Set-Location SQLSERVER:\SQL\PrimaryServer\default\AvailabilityGroups\MyAg  
$primaryReplica = Get-Item "AvailabilityReplicas\PrimaryServer"  
$secondaryReplica = Get-Item "AvailabilityReplicas\SecondaryServer"  
  
Set-SqlAvailabilityReplica -ReadOnlyRoutingConnectionUrl "TCP://PrimaryServer.domain.com:1433" -InputObject $primaryReplica  
Set-SqlAvailabilityReplica -ReadOnlyRoutingConnectionUrl "TCP://SecondaryServer.domain.com:1433" -InputObject $secondaryReplica  
Set-SqlAvailabilityReplica -ReadOnlyRoutingList "SecondaryServer","PrimaryServer" -InputObject $primaryReplica  
```  
  
##  <a name="follow-up-after-configuring-read-only-routing"></a><a name="FollowUp"></a> 跟进：配置只读路由之后  
 在这两种角色中配置当前主副本和可读辅助副本以支持只读路由后，可读辅助副本可接收/读取来自通过可用性组侦听器连接的客户端的读意向连接。  
  
> [!TIP]  
>  使用 [bcp 实用工具](../../../tools/bcp-utility.md) 或 [sqlcmd 实用工具](../../../tools/sqlcmd-utility.md)时，你可以通过指定 **-K ReadOnly** 开关来对允许只读访问的任意次要副本指定只读访问。  
  
###  <a name="requirements-and-recommendations-for-client-connection-strings"></a><a name="ConnStringReqsRecs"></a> 针对客户端连接字符串的要求和建议  
 对于要使用只读路由的客户端应用程序，其连接字符串必须满足以下要求：  
  
-   使用 TCP 协议。  
  
-   将应用程序意向特性/属性设置为只读。  
  
-   引用配置为支持只读路由的可用性组的侦听器。  
  
-   引用该可用性组中的数据库。  
  
 此外，建议连接字符串启用多子网故障转移，这将支持每个子网上的每个副本的并行客户端线程。 这将最大程度地减小故障转移后的客户端重新连接时间。  
  
 连接字符串的语法取决于应用程序正在使用的 SQL Server 提供程序。 以下用于 SQL Server 的 .NET Framework 数据访问接口 4.0.2 的示例连接字符串说明了使用只读路由时所需的和建议的连接字符串的部分。  
  
```  
Server=tcp:MyAgListener,1433;Database=Db1;IntegratedSecurity=SSPI;ApplicationIntent=ReadOnly;MultiSubnetFailover=True  
```  
  
 有关只读应用程序意向和只读路由器详细信息，请参阅 [可用性组侦听程序、客户端连接和应用程序故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)。  
  
### <a name="if-read-only-routing-is-not-working-correctly"></a>如果只读路由未正确工作  
 有关解决只读路由配置问题的信息，请参阅 [只读路由未正确工作](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md#ROR)。  
  
##  <a name="next-steps"></a><a name="RelatedTasks"></a>后续步骤 
**查看只读路由配置**  
  
-   [sys.availability_read_only_routing_lists (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)  
  
-   [sys.availability_replicas (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)（**read_only_routing_url** 列）  
  
**配置客户端连接访问**  
  
-   [创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [配置对可用性副本的只读访问 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
**在应用程序中使用连接字符串**  
  
-   [对高可用性、灾难恢复的 SQL Server Native Client 支持](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)  
  
-   [将连接字符串关键字用于 SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)  
  
**博客：**  
  
-    [计算 AlwaysOn 的 read_only_routing_url](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson)  
  
-    [SQL Server Always On 团队博客：SQL Server Always On 团队官方博客](/archive/blogs/sqlalwayson/)  
  
-    [CSS SQL Server 工程师博客](/archive/blogs/psssql/)  
  
**白皮书：**  
  
-    [针对 SQL Server 2012 的 Microsoft 白皮书](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
-    [SQL Server 客户咨询团队白皮书](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  

**更多内容**

- [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   

- [活动次要副本：可读次要副本（Always On 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   

- [关于对可用性副本的客户端连接访问 (SQL Server)](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   
 
- [可用性组侦听程序、客户端连接和应用程序故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)
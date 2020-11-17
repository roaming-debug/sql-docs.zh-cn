---
title: 什么是可用性组侦听器？
description: 'Always On 可用性组侦听器的概述，以及它如何自动将流量定向到预期的服务器。 '
ms.custom: seodec18
ms.date: 02/27/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
- read-only routing
- read-intent connections [AlwaysOn Availability Groups]
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], read-only routing
- Availability Groups [SQL Server], client connectivity
ms.assetid: 76fb3eca-6b08-4610-8d79-64019dd56c44
author: cawrites
ms.author: chadam
ms.openlocfilehash: aa84b43cbcf636facdade77040eeadf7e8449c54
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584683"
---
# <a name="what-is-an-availability-group-listener"></a>什么是可用性组侦听器？  
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

可用性组侦听器是一个虚拟网络名称 (VNN)，客户端可连接到此名称以访问 Always On 可用性组的主要副本或次要副本中的数据库。 侦听器允许客户端连接到副本，而无需知道 SQL Server 的物理实例名称。 由于侦听器路由流量，因此在发生故障转移后不需要修改客户端连接字符串。 

可用性组侦听器由域名系统 (DNS) 侦听器名称、指定的侦听器端口以及一个或多个 IP 地址组成。 可用性组侦听器仅支持 TCP 协议。  在域和 NetBIOS 中，侦听器的 DNS 名称必须唯一。  创建侦听器时，该侦听器将成为群集中的资源，并具有关联的虚拟网络名称 (VNN)、虚拟 IP (VIP) 和可用性组依赖项。 客户端使用 DNS 将 VNN 解析为多个 IP 地址，然后尝试连接到每个地址，直到连接请求成功或超时。  
  
如果为一个或多个[可读次要副本](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)配置只读路由，则与侦听器的读意向客户端连接将自动重定向到可读的次要副本。 
  
本文概述了可用性组侦听器。 你还可以[配置侦听器](create-or-configure-an-availability-group-listener-sql-server.md)，然后了解如何[连接到侦听器](listeners-client-connectivity-application-failover.md)。
  
  
##  <a name="listener-parameters"></a><a name="AGlConfig"></a> 侦听器参数  

 可用性组侦听器使用以下内容：
  
 **唯一的 DNS 名称**  
 这也称为虚拟网络名称 (VNN)。 适用 DNS 主机名的 Active Directory 命名规则。 有关详细信息，请参阅知识库文章： [Active Directory 中计算机、域、站点和 OU 的命名约定](https://support.microsoft.com/kb/909264) 。  
  
**一个或多个虚拟 IP 地址 (VIP)**  
 为可用性组可以故障转移到的一个或多个子网配置 VIP。  
  
**IP 地址配置**  
 对于给定的可用性组侦听器，IP 地址可以使用动态主机配置协议 (DHCP)，或者一个或多个静态 IP 地址。 使用 DHCP 可能会导致故障转移期间的连接延迟，因此不建议在生产环境中使用。 跨多个子网扩展或使用混合网络配置的可用性组必须使用静态 IP 地址。 
 
  
##  <a name="listener-port"></a><a name="SelectListenerPort"></a> 侦听器端口 
 当配置可用性组侦听器时，您必须指定一个端口。  您可以将默认端口配置为 1433，以便允许使用客户端连接字符串以达到简化目的。 如果使用 1433，则无需在连接字符串中指定端口号。 此外，由于每个可用性组侦听器都将具有一个独立的虚拟网络名称，因此，在单个 WSFC 上配置的每个可用性组侦听器都可以配置为引用相同的默认端口 1433。  
  
 还可以指定一个非标准的侦听器端口，但是，这意味着您还需要在连接到可用性组侦听器时，在您的连接字符串中显式指定一个目标端口。  您还需要为非标准端口打开对防火墙的权限。  
  
 如果对可用性组侦听器 VNN 使用默认端口 1433，则仍需要确保群集节点上没有其他服务正在使用此端口；否则将导致端口冲突。  
  
 如果其中一个 SQL Server 实例已正在通过实例侦听器侦听 TCP 端口 1433，且在侦听端口 1433 的计算机上没有任何其他服务（包括其他 SQL Server 实例），则不会与可用性组侦听器的端口导致冲突。  这是因为，可用性组监听器在相同的服务过程中可以共享同一个 TCP 端口。  但是，不应将多个 SQL Server 实例（并行）配置为侦听同一个端口。  
  
  
##  <a name="behavior-of-client-connections-on-failover"></a><a name="CCBehaviorOnFailover"></a> 有关故障转移的客户端连接行为  

 当发生可用性组故障转移时，到可用性组的现有持久连接都将终止，并且客户端必须建立新连接才能继续使用相同的主数据库或只读辅助数据库。  当服务器端发生故障转移时，到可用性组的连接可能会失败，这就强制客户端应用程序重试连接，直至主副本完全回到联机状态。  
  
 如果在客户端应用程序的连接尝试期间但在连接超时期限之前可用性组回到联机状态，则在其内部的其中一次重试期间，客户端驱动程序可能会成功连接，并且在此情况下应用程序不会出现任何错误。  


## <a name="next-steps"></a>后续步骤

现在你已熟悉了可用性组侦听器的功能，请[创建侦听器](create-or-configure-an-availability-group-listener-sql-server.md)，然后配置应用程序为[连接到侦听器](listeners-client-connectivity-application-failover.md)。 还可以查看各种[可用性组监视策略](monitoring-of-availability-groups-sql-server.md)，以确保可用性组正常运行。 

有关可用性组的详细信息，请参阅 [AlwaysOn 可用性组 &#40;SQL Server&#41; 概述](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)。 
  

  
  

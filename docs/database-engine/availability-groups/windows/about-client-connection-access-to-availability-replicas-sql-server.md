---
title: 可用性组中连接到副本的客户端连接的类型
description: 了解客户端可以与 SQL Server 中 Always On 可用性组的主要或次要副本建立的连接的不同类型。
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], readable secondary replicas
- active secondary replicas [SQL Server], read-only access to
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], client connectivity
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 29027e46-43e4-4b45-b650-c4cdeacdf552
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6ed4b06649592e5cd904cfa75a5ec0fa4800eff4
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644318"
---
# <a name="types-of-client-connections-to-replicas-within-an-always-on-availability-group"></a>Always On 可用性组中连接到副本的客户端连接的类型
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  在 AlwaysOn 可用性组中，你可以配置一个或多个可用性副本，以便在辅助角色下运行时（即作为次要副本运行时）允许只读连接。 还可以将每个可用性副本配置为在主角色下运行时（即作为主副本运行时）允许或排除只读连接。  
  
 为了便于客户端访问给定可用性组的主数据库或辅助数据库，您应该定义可用性组侦听器。 默认情况下，可用性组侦听器将传入连接定向到主副本。 不过，您可以对可用性组进行配置以便支持只读路由，这使其可用性组侦听器能够将读意向应用程序的连接请求重定向到可读取的辅助副本。 有关详细信息，请参阅 [为可用性组配置只读路由 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)。  
  
 在故障转移期间，辅助副本转换为主角色，以前的主副本转换为辅助角色。 在故障转移过程中，所有到主副本或辅助副本的客户端连接都将终止。 故障转移之后，在将客户端重新连接到可用性组侦听器时，侦听器将客户端重新连接到新的主副本，只有读意向连接请求除外。 如果在承载新的主副本以及至少一个可读取辅助副本的客户端和服务器实例上配置了只读路由，则读意向连接请求将被重新路由到支持客户端要求的连接访问类型的辅助副本。 若要确保在故障转移之后获得良好的客户端体验，务必为每个可用性副本的辅助角色和主角色配置连接访问。  
  
> [!NOTE]  
>  有关处理客户端连接请求的可用性组侦听程序的信息，请参阅 [可用性组侦听程序、客户端连接和应用程序故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)。  
  
##  <a name="types-of-connection-access-supported-by-the-secondary-role"></a><a name="ConnectAccessForSecondary"></a> 辅助角色支持的连接访问类型  
 辅助角色对于客户端连接支持三种备选方式，如下所示：  
  
 无连接  
 不允许任何用户连接。 辅助数据库不可用于读访问。 这是辅助角色中的默认行为。  
  
 仅读意向连接  
 辅助数据库仅适用于其 **应用程序意向** 连接属性设置为“ReadOnly”  （读意向连接  ）。  
  
 有关此连接属性的信息，请参阅 [对高可用性、灾难恢复的 SQL Server Native Client 支持](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)。  
  
 允许任何只读连接  
 辅助数据库全部可用于读访问连接。 此选项允许较低版本的客户端进行连接。  
  
 有关详细信息，请参阅 [配置对可用性副本的只读访问 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)。  
  
##  <a name="types-of-connection-access-supported-by-the-primary-role"></a><a name="ConnectAccessForPrimary"></a> 主角色支持的连接访问类型  
 主角色对于客户端连接支持两种备选方式，如下所示：  
  
 允许所有连接  
 主数据库同时允许读写连接和只读连接。 这是主角色的默认行为。  
  
 仅允许读/写连接  
 当 **应用程序意向** 连接属性设置为“ReadWrite”  或未设置时，允许此连接。 不允许其 **Application Intent** 连接字符串关键字设置为 **ReadOnly** 的连接。 仅允许读写连接可帮助防止您的客户错误地将读意向工作负荷连接到主副本。  
  
 有关此连接属性的信息，请参阅 [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)。  
  
 有关详细信息，请参阅 [配置对可用性副本的只读访问 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)。  
  
##  <a name="how-the-connection-access-configuration-affects-client-connectivity"></a><a name="HowConnectionAccessAffectsConnectivity"></a> 连接访问配置如何影响客户端连接  
 副本的连接访问设置决定连接尝试是失败还是成功。 下表简要说明对于每个连接访问设置，给定连接尝试是失败还是成功。  
  
|副本角色|副本上支持的连接访问|连接意向|连接尝试结果|  
|------------------|--------------------------------------------|-----------------------|--------------------------------|  
|辅助副本|All|指定了读意向、读写意向或未指定连接意向|Success|  
|辅助副本|无（这是默认辅助行为。）|指定了读意向、读写意向或未指定连接意向|失败|  
|辅助副本|仅限读意向|读意向|Success|  
|辅助副本|仅限读意向|指定了读写意向或未指定连接意向|失败|  
|主|所有（这是默认主要行为。）|指定了只读意向、读写意向或未指定连接意向|Success|  
|主|读写|仅限读意向|失败|  
|主|读写|指定了读写意向或未指定连接意向|Success|  
  
 有关配置可用性组接受到其副本的客户端连接的信息，请参阅， [可用性组侦听程序、客户端连接和应用程序故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)。  
  
### <a name="example-connection-access-configuration"></a>连接访问配置示例  
 根据将不同可用性副本配置为进行连接访问的方式，在可用性组故障转移之后，对于客户端连接的支持可能会有所变化。 例如，请考虑要在远程异步提交辅助副本上对其执行报告的一个可用性组。 此可用性组中数据库的所有只读应用程序都会将其 **应用程序意向** 连接属性设置为“ReadOnly”  以使所有只读连接为读意向连接。  
  
 在本例中，可用性组在主计算中心具有两个同步提交副本，在附属站点拥有两个异步提交副本。 对于主角色，所有副本都配置为允许读写访问，这样，在所有情况下可防止与主副本进行读意向连接。 同步提交辅助角色使用默认连接访问配置（“无”），这样可以防止在辅助角色下进行的所有客户端连接。  与此相反，异步提交副本配置为允许在辅助角色下进行读意向连接。 下表概述了该示例配置：  
  
|副本|提交模式|初始角色|辅助角色的连接访问|主角色的连接访问|  
|-------------|-----------------|------------------|------------------------------------------|----------------------------------------|  
|Replica1|同步|主|无|读写|  
|Replica2|同步|辅助副本|无|读写|  
|Replica3|异步|辅助副本|仅读意向|读写|  
|Replica4|异步|辅助副本|仅限读意向|读写|  
  
 通常，在此示例方案中，仅在同步提交副本之间发生故障转移，但在故障转移之后，读意向应用程序立即可以重新连接到其中一个异步提交辅助副本。 然而，当主计算中心发生灾难时，两个同步提交副本都将丢失。 附属站点的数据库管理员通过强制手动故障转移到异步提交辅助副本来进行响应。 其余辅助副本上的辅助数据库已由于强制故障转移而被挂起，而使它们不可用于只读工作负载。 新的主副本（配置为读写连接）可防止读意向工作负荷与读写工作负荷发生争用。 这意味着，除非数据库管理员对于其他异步提交辅助副本恢复辅助数据库，否则读意向客户端无法连接到任何可用性副本。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [配置对可用性副本的只读访问 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [为可用性组配置只读路由 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [监视可用性组 (Transact-SQL)](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
-   [查看可用性副本属性 (SQL Server)](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
-   [使用“新建可用性组”对话框 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   [用于高可用性和灾难恢复的 Microsoft SQL Server AlwaysOn 解决方案指南](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
-   [SQL Server AlwaysOn 团队博客：SQL Server AlwaysOn 团队官方博客](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [可用性组侦听程序、客户端连接和应用程序故障转移 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [统计信息](../../../relational-databases/statistics/statistics.md)  
  

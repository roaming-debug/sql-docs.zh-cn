---
title: 可用性副本之间连接失败的原因
description: 本主题介绍参与 AlwaysOn 可用性组的副本之间的连接失败的不同可能原因。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- troubleshooting [SQL Server], HADR
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], troubleshooting
ms.assetid: cd613898-82d9-482f-a255-0230a6c7d6fe
author: cawrites
ms.author: chadam
ms.openlocfilehash: bd19b92c3ddd9cf6dad2cb917bbb598f4cff0438
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584086"
---
# <a name="determine-possible-reason-for-connectivity-failures-between-availability-replicas"></a>“确定可用性副本之间的连接失败的可能原因
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
物理故障、操作系统故障或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障都可能导致两个可用性副本之间的会话失败。 可用性副本不会定期检查 Sqlservr.exe 所依赖的组件来验证这些组件是在正常运行还是已出现故障。 但对于某些类型的故障，受影响的组件将向 Sqlservr.exe 报告错误。 由另一个组件报告的错误称为“硬错误  ”。 为了检测可能忽略的其他故障，[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]实施了自己的会话超时机制。 以秒为单位指定会话超时期限。 此超时期限是一个服务器实例在考虑断开另一实例的连接之前，等待接收来自该实例的 PING 消息的最长时间。 两个可用性副本之间发生会话超时时，可用性副本将假定已发生故障并声明一个“软错误  ”。  
  
> [!IMPORTANT]  
>  无法检测到主数据库之外的数据库中的故障。 此外，也不太可能检测到数据磁盘故障，除非数据库因为数据磁盘故障而重新启动。  
  
 错误检测的速度以及由此决定的对故障的反应时间取决于是硬错误还是软错误。 系统可以立即报告某些硬错误，例如网络故障。 但在某些情况下，特定于组件的超时期限可能会延迟报告某些硬错误。 对于软错误，会话超时期限的长度决定了错误检测的速度。 默认情况下，此期限为 10 秒钟。 这是建议的最小值。  
  
## <a name="failures-due-to-hard-errors"></a>硬错误导致的故障  
 可能的硬错误原因包括（但不限于）下列几种情况：  
  
-   连接或网线断开  
  
-   网卡出现故障  
  
-   路由器更改  
  
-   防火墙更改  
  
-   端点重新配置  
  
-   事务日志驻留的驱动器丢失  
  
-   操作系统或进程故障  
  
 例如，如果主数据库中的日志驱动器停止响应或失败，操作系统会通知 Sqlservr.exe 出现严重错误。  
  
 某些组件（如网络组件和某些 IO 子系统）使用它们自己的超时设置来确定故障。 这些超时设置独立于 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]，后者不了解它们，并且完全不能识别其行为。 在这些情况下，超时延迟会延长发生故障与可用性副本收到所引发硬错误之间的时间。  
  
> [!NOTE]  
>  仅在出现软错误时，才对可用性副本执行有效的错误检查。 有关详细信息，请参阅本主题后面的“软错误导致的故障”。  
  
 若要了解网络出现的错误情况，请咨询网络工程师，询问当 TCP 连接发生下列事件时，哪些错误消息会发送到端口：  
  
-   DNS 未运行。  
  
-   网线被拔掉。  
  
-   [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows 防火墙阻止了特定端口。  
  
-   监视端口的应用程序出现故障。  
  
-   重命名基于 Windows 的服务器。  
  
-   重新启动基于 Windows 的服务器。  
  
> [!NOTE]  
>  [!INCLUDE[ssHADRc](../../../includes/sshadrc-md.md)]无法避免与客户端访问服务器相关的问题。 例如，假设由公用网络适配器处理与主副本的客户端连接，而由专用网络接口卡处理承载可用性组的副本的服务器实例之间的所有通信流量。 此时，公用网络适配器的故障将阻止客户端访问数据库。  
  
## <a name="failures-due-to-soft-errors"></a>软错误导致的故障  
 可能导致会话超时的情况包括（但不限于）下列各项：  
  
-   诸如 TCP 链接超时、数据包被删除或损坏或数据包顺序错误等网络错误。  
  
-   不响应的操作系统、服务器或数据库。  
  
-   Windows 服务器超时。  
  
-   计算资源不足，例如 CPU 或磁盘超负荷运转，事务日志填满，或系统用完内存或线程。 在这些情况下，需要增加超时期限、降低工作负荷或更换硬件以处理相应的工作负荷。  
  
### <a name="the-session-timeout-mechanism"></a>会话超时机制  
 由于软错误不能由服务器实例直接检测到，因此，软错误可能导致一个可用性副本无限期等待会话中另一个可用性副本的响应。 为了防止发生这种情况， [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 实施了会话超时机制，此机制基于以下条件：所连接的可用性副本会在每个打开的连接上按固定间隔发送 ping。 在超时期限内收到 ping 指示连接仍是开放的，且服务器实例正在通过此连接进行通信。 收到 ping 后，副本将重置此连接上的超时计数器。 有关可用性模式和会话超时的关系的信息，请参阅[可用性模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)。  
  
 主副本和辅助副本相互 ping 以指示它们仍处于活动状态，会话超时限制防止一个副本无限期等待接收另一个副本的 ping。 会话超时限制是用户可配置的副本属性，默认值为 10 秒。 在超时期限内收到 ping 指示连接仍是开放的，且服务器实例正在通过此连接进行通信。 收到 ping 后，可用性副本将重置此连接的超时计数器。  
  
 如果在会话超时期限内没有收到来自另一个副本的 ping，该连接将超时。连接将关闭，超时的副本进入 DISCONNECTED 状态。 即使为同步提交模式配置了断开连接的副本，事务也将不等待该副本重新连接和重新同步。  
  
## <a name="responding-to-an-error"></a>响应错误  
 无论出现何种错误类型，检测到错误的服务器都会根据实例的角色、会话可用性模式以及会话中任何其他连接的状态做出相应的响应。 有关丢失伙伴后会发生的情况的信息，请参阅[可用性模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)。  
  
## <a name="related-tasks"></a>Related Tasks  
 **更改超时值（仅限同步提交可用性模式）**  
  
-   [更改可用性副本的会话超时期限 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
 **查看当前超时值**  
  
-   查询 [sys.availability_replicas（Transact-SQL）](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)中的 **session_timeout**。  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  

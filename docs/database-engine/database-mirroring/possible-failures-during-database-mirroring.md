---
title: 数据库镜像期间可能出现的故障 | Microsoft Docs
description: 了解由物理问题、操作系统问题或 SQL Server 问题导致的数据库镜像会话中的故障，以及如何响应错误。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- time-out period [SQL Server database mirroring]
- soft errors [SQL Server]
- database mirroring [SQL Server], troubleshooting
- timeout errors [SQL Server]
- troubleshooting [SQL Server], database mirroring
- hard errors
- failed database mirroring sessions [SQL Server]
ms.assetid: d7031f58-5f49-4e6d-9a62-9b420f2bb17e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 2cd8b70c401a0cfc3b12f76d289b395f3d9874ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343747"
---
# <a name="possible-failures-during-database-mirroring"></a>Possible Failures During Database Mirroring
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  物理故障、操作系统故障或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障都可能导致数据库镜像会话失败。 数据库镜像不会定期检查 Sqlservr.exe 所依赖的组件来验证组件是在正常运行还是已出现故障。 但对于某些类型的故障，受影响的组件将向 Sqlservr.exe 报告错误。 由另一个组件报告的错误称为“硬错误 ”。 为了检测可能未被注意的其他故障，数据库镜像采用了自己的超时机制。 发生镜像超时时，数据库镜像假定已发生故障并声明一个“软错误”。 然而，某些在 SQL Server 实例级别发生的故障不会导致镜像超时，并且检测不到。  
  
> [!IMPORTANT]  
>  在数据库镜像会话中无法检测到数据库（除镜像数据库之外）故障。 此外，也无法检测到数据磁盘故障，除非数据库因为数据磁盘故障而重新启动。  
  
 因此，错误检测的速度以及镜像会话对故障的反应时间取决于是硬错误还是软错误。 系统可以立即报告某些硬错误，例如网络故障。 但在某些情况下，特定于组件的超时期限可能会延迟报告某些硬错误。 对于软错误，镜像超时期限的长度决定了错误检测的速度。 默认情况下，此期限为 10 秒钟。 这是建议的最小值。  
  
## <a name="failures-due-to-hard-errors"></a>硬错误导致的故障  
 可能的硬错误原因包括（但不限于）下列几种情况：  
  
-   连接或网线断开  
  
-   网卡出现故障  
  
-   路由器更改  
  
-   防火墙更改  
  
-   端点重新配置  
  
-   事务日志驻留的驱动器丢失  
  
-   操作系统或进程故障  
  
 例如，如果主体数据库中的日志驱动器停止响应或失败，操作系统会通知 Sqlservr.exe 出现严重错误。  
  
 某些组件（如网络组件和某些 IO 子系统）使用它们自己的超时设置来确定故障。 这些超时设置独立于数据库镜像，数据库镜像不了解它们，并且完全不能识别其行为。 在这些情况下，超时延迟会延长发生故障与数据库镜像收到所引发硬错误之间的时间。  
  
> [!NOTE]  
>  出现软错误时，仅对数据库镜像执行活动的错误检查。 有关详细信息，请参阅本主题后面的“软错误导致的故障”。  
  
 若要了解网络出现的错误情况，请咨询网络工程师，询问当 TCP 连接发生下列事件时，哪些错误消息会发送到端口：  
  
-   DNS 未运行。  
  
-   网线被拔掉。  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 防火墙阻止了特定端口。  
  
-   监视端口的应用程序出现故障。  
  
-   重命名基于 Windows 的服务器。  
  
-   重新启动基于 Windows 的服务器。  
  
> [!NOTE]  
>  镜像无法避免与客户端访问服务器相关的问题。 例如，假设由公用网络适配器处理与主体服务器实例的客户端连接，而由专用网络接口卡处理服务器实例之间的所有镜像通信流量。 此时，尽管数据库可以继续进行镜像，但公用网络适配器的故障将防止客户端访问数据库。  
  
## <a name="failures-due-to-soft-errors"></a>软错误导致的故障  
 导致镜像超时的情况包括（但不限于）下列各项：  
  
-   诸如 TCP 链接超时、数据包被删除或损坏或数据包顺序错误等网络错误。  
  
-   不响应的操作系统、服务器或数据库。  
  
-   Windows 服务器超时。  
  
-   计算资源不足，例如 CPU 或磁盘超负荷运转，事务日志填满，或系统用完内存或线程。 在这些情况下，需要增加超时期限、降低工作负荷或更换硬件以处理相应的工作负荷。  
  
### <a name="the-mirroring-time-out-mechanism"></a>镜像超时机制  
 由于软错误不能由服务器实例直接检测到，因此，软错误可能导致服务器实例无限期等待。 为了防止发生这种情况，数据库镜像采用了它自己的超时机制，此机制基于镜像会话中的每个服务器实例会在每个开放连接上按固定间隔发送 ping。  
  
 为了使连接保持开放，服务器实例必须能够在超时期限内在该连接上接收到 ping，此期限为定义的镜像超时时间再加上再发送一个 ping 所需的时间。 在超时期限内收到 ping 指示连接仍是开放的，且服务器实例正在通过此连接进行通信。 接收到 ping 后，服务器实例将重置此连接上的超时计数器。  
  
 如果未在超时期限内从此连接上收到 ping，则服务器实例认为此连接已超时。服务器实例将关闭超时连接，然后根据会话的状态和运行模式处理超时事件。  
  
 即使其他服务器实际工作正常，超时也被认为是一个故障。 如果会话的超时值太短而不能使任一伙伴做出正常响应，则会产生虚假故障。 如果一个服务器实例成功地与另一个服务器实例实现通信，但后者的响应时间太短，以致于无法在超时期限过期之前接收到 ping，则会产生错误故障。  
  
 在高性能模式会话中，超时期限始终为 10 秒钟。 通常，该期限足以避免虚假故障。 在高安全性模式会话中，默认超时期限为 10 秒钟，但您可以更改该持续时间。 为了避免虚假故障，建议镜像超时期限始终为 10 秒钟或更长。  
  
 **更改超时值（仅限于高安全性模式）**  
  
-   使用 [ALTER DATABASE \<database> SET PARTNER TIMEOUT \<integer>](../../t-sql/statements/alter-database-transact-sql.md) 语句。  
  
 **查看当前超时值**  
  
-   在 **sys.database_mirroring** 中查询 [mirroring_connection_timeout](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md)。  
  
## <a name="responding-to-an-error"></a>响应错误  
 无论出现何种错误类型，检测到错误的服务器都会根据实例的角色、会话运行模式以及会话中任何其他连接的状态做出相应的响应。 有关丢失伙伴后会发生的情况的信息，请参阅 [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)。  
  
## <a name="see-also"></a>另请参阅  
 [估计在角色切换期间服务的中断（数据库镜像）](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md)   
 [数据库镜像运行模式](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)   
 [数据库镜像会话期间的角色切换 (SQL Server)](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)   
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  

---
title: 将读取缩放用于可用性组
description: 详细了解在使用 Always On 可用性组时如何实现读取扩展，以及如何将分布式可用性组用于地理读取扩展。
ms.custom: seodec18
ms.date: 10/24/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: ''
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0eab734a33e6487293999e219a43ab43ce1a7642
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344576"
---
# <a name="use-read-scale-with-always-on-availability-groups"></a>将读取缩放用于 Always On 可用性组
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

可用性组是一种全面的解决方案，赋予了 SQL Server 高可用性功能，并提供集成缩放解决方案。 在典型的数据库应用程序中，有多个客户端运行多种类型的工作负荷。 有时会由于资源限制出现瓶颈。 

在可用性组的上下文中，读取扩展将读取工作负荷卸载到次要副本。 可以释放资源并增加 OLTP 工作负荷的吞吐量。 还可以提高性能并缩放只读工作负荷。 利用 SQL Server 的最快复制技术，并创建一组复制数据库，分担对只读副本的报告和分析工作负荷。

利用可用性组，可将一个或多个次要副本配置为支持以只读方式访问辅助数据库。

运行分析或报告工作负荷的客户端应用程序可直接连接到辅助数据库。 还可以设置只读路由列表，并连接到主数据库。 主要数据库随后将连接请求循环转发到路由列表中的每个次要副本。

## <a name="read-scale-availability-groups-without-cluster"></a>不需要群集的读取缩放可用性组

在 [!INCLUDE[sssql15-md](../../../includes/sssql16-md.md)] 及更早版本中，所有可用性组都需要群集。 群集用于提供业务连续性，实现高可用性和灾难恢复 (HADR)。 此外，配置次要副本以执行读取操作。 如果目标不是高可用性，配置和运行群集消耗了相当大的运营开销。 SQL Server 2017 引入了不需要群集的读取缩放可用性组。 

如果业务要求是转换主要副本上运行的任务关键型工作负荷的资源，用户现在可以使用只读路由或直接连接到可读次要副本。 而无需依赖于与任何群集技术的集成。 Windows 和 Linux 平台上的 SQL Server 2017 支持这些新功能。

>[!IMPORTANT]
>这不是高可用性安装程序。 不需要监视基础结构、协调故障检测和进行自动故障转移。 如果没有群集，SQL Server 无法提供自动化高可用性解决方案可提供的低恢复时间目标 (RTO)。 如果需要高可用性功能，请使用群集管理器（Windows 上的 Windows Server 故障转移群集或 Linux 上的 Pacemaker）。
>
>读取缩放可用性组可提供灾难恢复功能。 当只读副本处于同步提交模式时，可提供恢复点目标 (RPO) 0。 要对读取缩放可用性组进行故障转移，请参阅[对读取缩放可用性组上的主要副本进行故障转移](perform-a-planned-manual-failover-of-an-availability-group-sql-server.md#ReadScaleOutOnly)。

## <a name="use-distributed-availability-groups-for-geographic-read-scale"></a>使用分布式可用性组进行地理读取缩放

不同地理位置的解决方案可以使用分布式可用性组实现读取缩放解决方案。 这可以用于减轻主要副本、可读次要副本以及靠近读取工作负荷源的站点的读取工作负荷。 分布式可用性组降低了主要副本上的资源利用率。 还可以通过降低网络延迟和利用专用资源来改善读取吞吐量。

单个分布式可用性组最多可具有 17 个可读次要副本。 增加的缩放功能表现为：菊花链使可用性组的数量成倍增加，进一步增加了可读副本的数量。 还可以在同一可用性组中部署两个分布式可用性组，以在地理位置不同的环境中实现低延迟读取。




## <a name="next-steps"></a>后续步骤

[在 Linux 上配置读取缩放可用性组](../../../linux/sql-server-linux-availability-group-configure-rs.md)

[在 Windows 上配置读取缩放可用性组](../../../database-engine/availability-groups/windows/configure-read-scale-availability-groups.md)

## <a name="see-also"></a>另请参阅

 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)

---
title: “常规”页面（可用性副本属性）
description: SQL Server Management Studio 中“可用性副本属性”页的“常规”页上的各种属性说明。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: end-user-help
f1_keywords:
- sql13.swb.availabilityreplicaproperties.general.f1
ms.assetid: 8318fefb-e045-4fab-8507-e1951fc7cec6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1851f55f215e78bc0243ee650c1f4e878b5b8fc6
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584554"
---
# <a name="availability-replica-properties-general-page-for-always-on-availability-groups"></a>Always On 可用性组的可用性副本属性（“常规”页）
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  使用此对话框可以查看可用性副本的属性。  
  
## <a name="task-list"></a>任务列表  
 **查看可用性副本属性**  
  
-   [查看可用性副本属性 (SQL Server)](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
-   [使用 AlwaysOn 面板 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
## <a name="ui-element-list"></a>UI 元素列表  
 **可用性组名称**  
 可用性组的名称。 这是在 Windows Server 故障转移群集 (WSFC) 内必须唯一的用户指定的名称。  
  
 **服务器实例**  
 承载此副本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的服务器名称；对于非默认实例，则为其实例名称。  
  
 **角色**  
 **主要节点**  
 当前主副本。  
  
 **辅助节点**  
 当前辅助副本。  
  
 **正在解析**  
 当前该副本角色处于正在解析为主角色或辅助角色的进程中。  
  
 **可用性模式**  
 副本的可用性模式，可为下列值之一：  
  
 **异步提交**  
 主副本可以不必等待辅助副本将日志写入磁盘，即可提交事务。  
  
 **同步提交**  
 主副本等待提交给定的事务，直到辅助副本将事务写入磁盘。  
  
 有关详细信息，请参阅 [可用性模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)。  
  
 **Failover mode**  
 副本的故障转移模式，可为下列值之一：  
  
 **自动**  
 自动故障转移。 副本为自动故障转移的目标。 仅当可用性模式设置为同步提交时，才选择此选项。  
  
 **手动**  
 手动故障转移。 副本仅能由数据库管理员手动进行故障转移。  
  
 **主角色中的连接**  
 当副本拥有主角色时支持的客户端连接类型。  
  
 **允许所有连接**  
 主副本中的数据库允许所有连接。 这是默认设置。  
  
 **允许读/写连接**  
 不允许 Application Intent 连接属性设置为 **ReadOnly** 的连接。 在 Application Intent 属性设置为 **ReadWrite** 或者未设置 Application Intent 连接属性时，将允许连接。  
  
 **可读取辅助角色**  
 正在履行辅助角色的可用性副本（也就是辅助副本）是否可以接受来自客户端的连接，可为下列值之一：  
  
 **是**  
 不允许与此副本的辅助数据库的直接连接。 它们不可用于读访问。 这是默认设置。  
  
 **仅限读意向**  
 仅允许与此副本的辅助数据库的直接只读连接。 辅助数据库全都可用于读访问。  
  
 **是**  
 允许与此副本的辅助数据库的所有连接，但仅限读访问。 辅助数据库全都可用于读访问。  
  
 有关详细信息，请参阅[活动次要副本：可读次要副本（Always On 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)。  
  
 **会话超时(秒)**  
 超时期限（秒）。 超时期限是指副本接收来自其他副本的消息而等待的最长时间，超过此时间，将主副本和辅助副本之间的连接视为已失败。 会话超时检测辅助副本是否与主副本相连接。 在检测到与辅助副本的连接失败时，主副本将辅助副本视为 NOT_SYNCHRONIZED。 在检测到与辅助副本的连接失败时，辅助副本只会尝试重新连接。  
  
> [!NOTE]  
>  会话超时不会导致自动故障转移。  
  
 **端点 URL**  
 用户指定的数据库镜像端点的字符串表示形式，该数据库镜像端点由用于数据同步的主副本和辅助副本之间的连接使用。 有关这些端点 URL 语法的信息，请参阅[在添加或修改可用性副本时指定端点 URL (SQL Server)](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)。  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  

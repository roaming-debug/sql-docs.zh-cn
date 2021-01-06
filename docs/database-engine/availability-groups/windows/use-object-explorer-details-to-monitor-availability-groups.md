---
title: 使用“对象资源管理器详细信息”来监视可用性组 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 来监视和管理现有的 Always On 可用性组、可用性副本和可用性数据库。
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.OEdetails.f1
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], databases
- Availability Groups [SQL Server]
ms.assetid: 84affc47-40e0-43d9-855e-468967068c35
author: cawrites
ms.author: chadam
ms.openlocfilehash: e6b0c8547718c1a7580108427314a4a58f4ba0b5
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641803"
---
# <a name="use-object-explorer-details-to-monitor-availability-groups"></a>使用“对象资源管理器详细信息”来监视可用性组
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何通过使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 的“对象资源管理器详细信息”窗格来监视和管理现有的 Always On 可用性组、可用性副本和可用性数据库。  
  
> [!NOTE]  
>  有关使用“对象资源管理器详细信息”窗格的信息，请参阅 [对象资源管理器详细信息窗格](../../../ssms/object/object-explorer-details-pane.md)。  
  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
 您必须连接到承载主副本或辅助副本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例（服务器实例）。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **监视可用性组、可用性副本和可用性数据库**  
  
1.  在“视图”菜单上单击 **“对象资源管理器详细信息”** ，或按 **F7** 键。  
  
2.  在对象资源管理器中，连接到要在其上监视可用性组的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例，然后单击服务器名称以展开服务器树。  
  
3.  依次展开“Always On 高可用性”节点和“可用性组”节点。  
  
4.  **“对象资源管理器详细信息”** 窗格显示所连接的服务器实例为其承载副本的所有可用性组。 对于每个可用性组，“服务器实例(主)”列都显示当前承载主要副本的服务器实例的名称。  若要显示有关给定的可用性组的详细信息，请在对象资源管理器中选择它。  
  
5.  **“对象资源管理器详细信息”** 窗格随后显示此可用性组的 **“可用性副本”** 和 **“可用性数据库”** 节点：  
  
    -   当您展开对象资源管理器中的 **“可用性组”** 节点并选择 **“可用性副本”** 节点后， **“对象资源管理器详细信息”** 窗格将显示有关该组中每个可用性副本的信息。 有关详细信息，请参阅本主题后面的 [可用性副本详细信息](#AvReplicaDetails)。  
  
         若要对多个可用性副本执行操作，请选择这些副本，然后右键单击它们以打开一个列出可用命令的上下文菜单。  
  
    -   当您展开对象资源管理器中的 **“可用性组”** 节点并选择 **“可用性数据库”** 节点后， **“对象资源管理器详细信息”** 窗格将显示有关该组中每个可用性数据库的信息。 有关详细信息，请参阅本主题后面的 [可用性数据库详细信息](#AvDbDetails)。  
  
         若要对多个可用性数据库执行操作，请选择这些数据库，然后右键单击它们以打开一个列出可用命令的上下文菜单。  
  
##  <a name="availability-groups-details"></a><a name="AvGroupsDetails"></a> 可用性组详细信息  
 **“可用性组”** 详细信息屏幕显示以下列：  
  
 **名称**  
 列出所选可用性组的“可用性副本”、“可用性数据库”和“可用性组”侦听器等文件夹。    
  
##  <a name="availability-replica-details"></a><a name="AvReplicaDetails"></a> 可用性副本详细信息  
 **“可用性副本”** 详细信息屏幕显示以下列：  
  
 **服务器实例**  
 显示承载可用性副本的服务器实例的名称，以及指示该服务器实例与本地服务器实例的当前连接状态的图标。  
  
 **角色**  
 指示可用性副本的当前角色，即“主”或“辅助”。 有关 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 角色的详细信息，请参阅 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)。  
  
 **辅助角色中的连接模式**  
 指示给定的可用性副本（正在执行辅助角色，也就是充当辅助副本）的数据库是否可以接受来自客户端的连接。  
  
 可能的值如下：  
  
|值|说明|  
|-----------|-----------------|  
|**禁止连接**|当此可用性副本充当辅助副本时，不允许直接连接到可用性数据库。 辅助数据库不可用于读访问。|  
|**只允许读意向连接**|当此副本充当辅助副本时，仅允许直接只读连接。 副本中的所有数据库都可用于读访问。|  
|**允许所有连接**|当此副本充当辅助副本时，允许与这些数据库建立的所有连接进行只读访问。|  
  
 **连接状态**  
 指示辅助副本当前是否连接到主副本。 可能的值如下：  
  
|值|说明|  
|-----------|-----------------|  
|**已断开连接**|对于远程可用性副本，指示它与本地可用性副本已断开连接。 本地副本对于“已断开连接”状态的响应取决于它的角色，如下所示：<br /><br /> 在主副本上，如果辅助副本已断开连接，辅助数据库将在主副本上标记为 **“未同步”** ，主副本等待辅助副本重新连接。<br /><br /> 在辅助副本上，一旦检测到其未连接，辅助副本会尝试重新连接主副本。|  
|**已连接**|远程可用性副本当前连接到本地副本。|  
|**NULL**|如果本地副本是一个辅助副本，则此值对于其他辅助副本均为 NULL。|  
  
 **同步状态**  
 指示辅助副本当前是否与主副本同步。 可能的值如下：  
  
|值|说明|  
|-----------|-----------------|  
|**“未同步”**|该数据库未同步或尚未联接到可用性组。|  
|**已同步**|该数据库与当前主副本（如果有）或上一个主副本上的主数据库同步。<br /><br /> 注意：在性能模式中，数据库从不处于“已同步”状态。|  
|**NULL**|未知状态。 当本地服务器实例无法与 WSFC 故障转移群集通信（即本地节点不是 WSFC 仲裁的一部分）时，出现此值。|  
  
> [!NOTE]  
>  有关可用性副本的性能计数器的信息，请参阅 [SQL Server，可用性副本](../../../relational-databases/performance-monitor/sql-server-availability-replica.md)。  
  
##  <a name="availability-database-details"></a><a name="AvDbDetails"></a> 可用性数据库详细信息  
 **“可用性数据库”** 详细信息屏幕显示给定可用性组中的可用性数据库的以下属性：  
  
 **名称**  
 可用性数据库的名称。  
  
 **同步状态**  
 指示可用性数据库当前是否与主副本同步。  
  
 可能的同步状态如下所示：  
  
|值|说明|  
|-----------|-----------------|  
|正在同步|辅助数据库已收到主数据库尚未写入磁盘（硬编码）的事务日志记录。<br /><br /> 注意：在异步提交模式中，同步状态始终是“正在同步”。|  
|||  
  
 **已挂起**  
 指示可用性数据库当前是否联机。 可能的值如下：  
  
|值|说明|  
|-----------|-----------------|  
|**已挂起**|此状态指示该数据库在本地挂起，需要手动恢复。<br /><br /> 在主副本上，该值对于辅助数据库是不可靠的。 若要确认辅助数据库是否挂起，请在承载该数据库的辅助副本上进行查询。|  
|**未联接**|指示辅助数据库要么未联接到可用性组，要么已从该组中删除。|  
|**联机**|指示数据库在本地可用性副本上未挂起，并且该数据库处于连接状态。|  
|**未连接**|指示辅助副本当前未能连接到主副本。|  
  
> [!NOTE]  
>  有关可用性数据库的性能计数器的信息，请参阅 [SQL Server，数据库复制](../../../relational-databases/performance-monitor/sql-server-database-replica.md)。  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_os_performance_counters (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)   
 [使用 AlwaysOn 仪表板 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)   
 [查看可用性组属性 (SQL Server)](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)   
 [查看可用性副本属性 (SQL Server)](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
  

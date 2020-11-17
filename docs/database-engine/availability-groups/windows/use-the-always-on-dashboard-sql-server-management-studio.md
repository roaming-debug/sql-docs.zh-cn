---
title: 在 SSMS 中使用可用性组仪表板
description: 介绍如何使用 Always On 可用性组仪表板监视 SQL Server Management Studio (SSMS) 中可用性组的运行状况。
ms.custom: seo-lt-2019
ms.date: 08/09/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
- Availability Groups [SQL Server], dashboard
ms.assetid: c9ba2589-139e-42bc-99e1-94546717c64d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 80b7f3e83af181676f09a6dc38b3652ae6142c89
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94583652"
---
# <a name="use-the-always-on-availability-group-dashboard-sql-server-management-studio"></a>使用 Always On 可用性组仪表板 (SQL Server Management Studio)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  数据库管理员使用 Always On 可用性组仪表板大致了解 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 中的可用性组及其可用性副本和数据库的运行状况。 可用性组仪表板的一些典型用法如下：  
  
-   选择要手动故障转移的副本。    
-   如果强制故障转移，则估计数据丢失。  
-   评估数据同步性能。   
-   评估同步提交辅助副本对性能的影响 
  -  仪表板提供关键的可用性组状态和性能指示器，支持使用以下几类信息轻松做出关于高可用性的可行性决策。  
-   副本汇总状态    
-   同步模式和状态   
-   估计数据丢失    
-   估计的恢复时间（重做追赶时间）    
-   数据库副本详细信息    
-   同步模式和状态    
-   还原日志所需的时间  
  
  
## <a name="prerequisites"></a>必备条件  
 您必须连接到承载可用性组的主副本或辅助副本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例（服务器实例）。  
  
 
### <a name="permissions"></a>权限  
 需要 CONNECT、VIEW SERVER STATE 和 VIEW ANY DEFINITION 权限。  
  
##  <a name="to-start-the-always-on-dashboard"></a>启动 Always On 仪表板  
  
1.  在对象资源管理器中，连接到要运行 AlwaysOn 面板的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。  
  
2.  展开“AlwaysOn 高可用性”  节点，右键单击“可用性组”  节点，然后单击“显示面板”  。  
  
##  <a name="change-always-on-dashboard-options"></a>更改 Always On 仪表板选项  
 可以使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 的“选项”  对话框配置 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] AlwaysOn 面板行为，使其自动刷新和启用自动定义的 AlwaysOn 策略。  
  
1.  从 **“工具”** 菜单中，单击 **“选项”** 。  
  
2.  若要自动刷新面板，在 **“选项”** 对话框中，选择 **“启用自动刷新”** ，输入以秒计的刷新间隔，然后输入要重试连接的次数。  
  
3.  若要启用用户定义的策略，请选择“启用用户定义的 Always On 策略”  。  
  
##  <a name="availability-group-summary"></a>可用性组摘要  
 可用性组屏幕为所连接服务器实例承载其副本的每个可用性组都显示一行摘要。 此窗格显示以下列。  
  
 **可用性组名称**  
 所连接服务器实例承载其副本的可用性组的名称。  
  
 **主实例**  
 正在承载可用性组的主副本的服务器实例的名称。  
  
 **故障转移模式**  
 显示为副本配置的故障转移模式。 可能的故障转移模式值包括：  
  
-   **自动**。 指示一个或多个副本处于自动故障转移模式。  
  
-   **手动**。 指示没有副本处于自动故障转移模式。  
  
 **问题**  
 单击“问题”  链接可打开针对某一问题的故障排除文档。 有关所有 AlwaysOn 策略问题的列表，请参阅[针对 Always On 可用性组运行问题的 AlwaysOn 策略 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)。  
  
> [!TIP]  
>  单击列标题可按可用性组的名称、主实例、故障转移模式或问题对可用性组信息进行排序。  
  
##  <a name="availability-group-details"></a><a name="AvGroupDetails"></a> 可用性组详细信息  
 将为您可以从摘要屏幕中选择的可用性组显示以下详细信息：  
  
 **可用性组状态**  
 显示可用性组的运行状况。  
  
 **Primary instance**  
 正在承载可用性组的主副本的服务器实例的名称。  
  
 **Failover mode**  
 显示为副本配置的故障转移模式。 可能的故障转移模式值包括：  
  
-   **自动**。 指示一个或多个副本处于自动故障转移模式。  
  
-   **手动**。 指示没有副本处于自动故障转移模式。  
  
 **群集状态**  
 群集的名称和状态，所连接的服务器实例和可用性组都是该群集的成员节点。  
  
##  <a name="availability-replica-details"></a><a name="AvReplicaDetails"></a> 可用性副本详细信息  

连接到主要副本时，“可用性副本详细信息”会显示可用性组中所有副本中的信息  。 连接到次要副本时，显示内容仅显示已连接副本中的信息。  

**“可用性副本”** 窗格显示以下列：  
  
 **名称**  
 承载可用性副本的服务器实例的名称。 默认情况下显示此列。  
  
 **角色**  
 指示可用性副本的当前角色，即“主”  或“辅助”  。 有关 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 角色的详细信息，请参阅 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)。 默认情况下显示此列。  
  
 **故障转移模式**  
 显示为副本配置的故障转移模式。 可能的故障转移模式值包括：  
  
-   **自动**。 指示一个或多个副本处于自动故障转移模式。  
  
-   **手动**。 指示没有副本处于自动故障转移模式。  
  
 **同步状态**  
 指示辅助副本当前是否与主副本同步。 默认情况下显示此列。 可能的值包括：  
  
-   **未同步**。 副本中的一个或多个数据库未同步或尚未联接到可用性组。  
  
-   **正在同步**。 正在同步副本中的一个或多个数据库。  
  
-   **已同步**。 辅助副本中的所有数据库均与当前主副本（如果有）或上一个主副本上的相应主数据库同步。  
  
    > [!NOTE]  
    >  在性能模式中，数据库从不处于“已同步”状态。  
  
-   **NULL**。 未知状态。 当本地服务器实例无法与 WSFC 故障转移群集通信（即本地节点不是 WSFC 仲裁的一部分）时，出现此值。  
  
 **问题**  
 列出问题名称。 默认情况下显示此值。 有关所有 AlwaysOn 策略问题的列表，请参阅[针对 Always On 可用性组运行问题的 AlwaysOn 策略 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)。  
  
 **可用性模式**  
 指示用户为每个可用性副本分别设置的副本属性。 默认情况下隐藏此值。 可能的值包括：  
  
-   **异步**。 辅助副本从不与主副本同步。  
  
-   **Synchronous**。 在追赶主数据库以保持与其同步时，辅助数据库将进入此状态，而且只要该数据库一直在同步数据，就会保持追赶状态。  
  
 **主连接模式**  
 指示用于连接主副本的模式。  默认情况下隐藏此值。  
  
 **辅助连接模式**  
 指示用于连接辅助副本的模式。  默认情况下隐藏此值。  
  
 **连接状态**  
 指示辅助副本当前是否连接到主副本。 默认情况下隐藏此列。 可能的值包括：  
  
-   **已断开连接**。 对于远程可用性副本，指示它与本地可用性副本已断开连接。 本地副本对于“已断开连接”状态的响应取决于它的角色，如下所示：  
  
    -   在主副本上，如果辅助副本已断开连接，辅助数据库将在主副本上标记为 **“未同步”** ，主副本等待辅助副本重新连接。  
  
    -   在辅助副本上，一旦检测到其未连接，辅助副本会尝试重新连接主副本。  
  
-   **Connected**。 远程可用性副本当前连接到本地副本。  
  
 **操作状态**  
 指示辅助副本的当前操作状态。 默认情况下隐藏此值。 可能的值包括：  
  
 **0**。挂起故障转移    
 **1**。挂起    
 **2**。联机    
 **3**。Offline   
 **4**。失败    
 **5**。失败，无仲裁  
  
 **NULL**。 副本不在本地  
  
 **上一个连接错误编号**  
 上一个连接错误的编号。  默认情况下隐藏此值。  
  
 **上一个连接错误说明**  
 上一个连接错误的说明。  默认情况下隐藏此值。  
  
 **上一个连接错误时间戳**  
 上一个连接错误的时间戳。 默认情况下隐藏此值。  
  
> [!NOTE]  
>  有关可用性副本的性能计数器的信息，请参阅 [SQL Server，可用性副本](../../../relational-databases/performance-monitor/sql-server-availability-replica.md)。  
  
##  <a name="group-by-availability-group-information"></a>按可用性组信息分组  
 若要对信息进行分组，请单击 **“分组依据”** ，并选择下列选项之一：  
  
-   **可用性副本**    
-   **可用性数据库** 
-   **Synchronization state**  
-   **故障转移就绪**   
-   **问题**  
  
 显示分组依据信息的窗格显示以下列：  
  
 **名称**  
 可用性数据库的名称。 默认情况下显示此值。  
  
 **副本**  
 承载可用性副本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称。 默认情况下显示此值。  
  
 **同步状态**  
 指示可用性数据库当前是否与主副本同步。 默认情况下显示此值。 可能的同步状态如下：  
  
-   **未同步**：  
-   
    -   对于主角色，指示该数据库未做好将其事务日志与相应的辅助数据库同步的准备。   
    -   对于辅助数据库，指示数据库由于连接问题而未开始日志同步，正被挂起，或者在启动或角色切换过程中正在转换状态。  
  
-   **正在同步**：
- 在主副本上：   
    - 在主数据库上，指示此数据库已做好接受来自辅助数据库的扫描请求的准备。  
    - 在辅助副本上，指示存在为该辅助数据库进行的处于活动状态的数据移动。 
  
  
-   **已同步**： 
  
    - 对于主数据库，指示至少同步了一个辅助数据库。
    - 对于辅助数据库，指示该数据库与相应的主数据库保持同步。  
  
-   **正在恢复**。  
  
     指示撤消进程中辅助数据库主动从主数据库获取页时的阶段。  
  
    > [!CAUTION]  
    >  当数据库处于 REVERTING 状态时，强制故障转移到辅助副本可能将使数据库处于不能启动的状态。  
  
-   **正在初始化**。  
  
     指示在正在辅助副本上传送和强制写入辅助数据库跟上撤消 LSN 所需的事务日志时的撤消阶段。  
  
    > [!CAUTION]  
    >  当数据库处于 INITIALIZING 状态时，强制故障转移到辅助副本将使数据库始终处于不能启动的状态。  
  
 **“故障转移就绪”**  
 指示哪些可用性副本可以进行故障转移（有/无数据丢失）。 默认情况下显示此列。 可能的值包括：  
  
-   **数据丢失**   
-   **无数据丢失**  
  
 **问题**  
 列出问题名称。 默认情况下显示此列。 可能的值包括：  
  
-   **警告**。 单击此选项可显示阈值和警告问题。   
-   **严重**。 单击此选项可显示严重问题。  
  
 有关所有 AlwaysOn 策略问题的列表，请参阅[针对 Always On 可用性组运行问题的 AlwaysOn 策略 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)。  
  
 **已挂起**  
 指示数据库“已挂起”  还是“已恢复”  。 默认情况下隐藏此值。  
  
 **挂起原因**  
 指示已挂起状态的原因。 默认情况下隐藏此值。  
  
 **估计数据丢失（秒）**  
 指示主副本和辅助副本中最后一个事务日志记录的时间差异。 如果主副本失败，则丢失该时间窗口内的所有事务日志记录。 默认情况下隐藏此值。  
  
 **估计的恢复时间（秒）**  
 指示重做追赶时间所需的时间（秒）。 追赶时间  是次要副本要与主要副本保持同步所需的时间。 默认情况下隐藏此值。  
  
 **同步性能（秒）**  
 指示主副本与辅助副本之间的同步所需的时间（秒）。 默认情况下隐藏此值。  
  
 **日志发送队列大小 (KB)**  
 指示主数据库的日志文件中尚未发送到次要副本的日志记录数。 默认情况下隐藏此值。  
  
 **日志发送速率（KB/秒）**  
 指示日志记录发送到次要副本的速率（KB/秒）。 默认情况下隐藏此值。  
  
 **重做队列大小 (KB)**  
 指示次要副本的日志文件中尚未重做的日志记录数。 默认情况下隐藏此值。  
  
 **重做速率（KB/秒）**  
 指示日志记录重做的速率（KB/秒）。 默认情况下隐藏此值。  
  
 **FileStream 发送速率（KB/秒）**  
 指示将事务发送到副本的 FileStream 速率（KB/秒）。 默认情况下隐藏此值。  
  
 **日志 LSN 的结尾**  
 指示与主副本和辅助副本上日志缓存中的最后一个日志记录相对应的实际日志序列号 (LSN)。 默认情况下隐藏此值。  
  
 **恢复 LSN**  
 指示在主副本上，在主副本上恢复或故障转移之后、在副本写入任何新日志记录之前事务日志的结尾。 默认情况下隐藏此值。  
  
 **截断 LSN**  
 指示主副本的最小日志截断值。 默认情况下隐藏此值。  
  
 **上次提交 LSN**  
 指示与事务日志中的最后提交的记录相对应的实际 LSN。 默认情况下隐藏此值。  
  
 **上次提交时间**  
 指示与最后一个提交记录对应的时间。 默认情况下隐藏此值。  
  
 **上次发送的 LSN**  
 指示一个点，在该点之前，所有日志块都已由主副本发送。 默认情况下隐藏此值。  
  
 **上次发送时间**  
 指示发送最后一个日志块的时间。 默认情况下隐藏此值。  
  
 **上次接收的 LSN**  
 指示一个点，在该点之前，所有日志块都已由承载此辅助数据库的辅助副本接收。 默认情况下隐藏此值。  
  
 **上次接收时间**  
 指示在辅助副本上，最后收到的消息中日志块标识符的读取时间。 默认情况下隐藏此值。  
  
 **上次强化的 LSN**  
 指示一个点，在该点之前，所有日志记录都已刷新到辅助副本上的磁盘。 默认情况下隐藏此值。  
  
 **上次强化时间**  
 指示在辅助副本上，上次强制写入的 LSN 的日志块标识符的接收时间。 默认情况下隐藏此值。  
  
 **上次重做的 LSN**  
 指示在辅助数据库上上次重做的日志记录的实际 LSN。 默认情况下隐藏此值。  
  
 **上次重做时间**  
 指示在辅助数据库上重做最后一个日志记录的时间。 默认情况下隐藏此值。  
 

   > [!NOTE]  
   >  大多数数据基于 sys.dm_hadr_database_replica_states，因此可能存在某些限制。 有关详细信息，请参阅 [sys.dm_hadr_database_replica_states (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)。


## <a name="always-on-availability-group-latency-reports"></a>Always On 可用性组延迟报表
可用性组延迟报表是内置于可用性组仪表板中的报表工具，可在 [SQL Server Management Studio 17.4](../../../ssms/download-sql-server-management-studio-ssms.md) 版本中使用。 此功能提供了易于理解的报表，其中详细说明了在日志传输过程的各个阶段所花费的时间。 这提供了一种在同步过程中缩小潜在延迟原因搜索范围的方法。 

SQL 代理运行数据收集，且必须在主要副本和至少一个次要副本上启用。 右键单击 SQL Server Management Studio“对象资源管理器”中的“可用性组”>“报表”>“标准报表”来查看报表  。  

有关详细信息，请参阅 [Always On 可用性组延迟报表](/archive/blogs/sql_server_team/new-in-ssms-always-on-availability-group-latency-reports)。

## <a name="related-tasks"></a>Related Tasks  
  
-   [使用 AlwaysOn 策略查看可用性组的运行状况 (SQL Server)](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_os_performance_counters (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)   
 [监视可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  

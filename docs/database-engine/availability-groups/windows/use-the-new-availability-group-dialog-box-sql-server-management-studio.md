---
title: SSMS 中的“新建可用性组”对话框
description: 了解如何使用 SQL Server Management Studio 的“新建可用性组”对话框来配置 Always On 可用性组。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], creating
ms.assetid: 1b0a6421-fbd4-4bb4-87ca-657f4782c433
author: cawrites
ms.author: chadam
ms.openlocfilehash: abab816f9248d1981d62bfeb313130a8b71c4256
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641751"
---
# <a name="use-the-new-availability-group-dialog-box-sql-server-management-studio"></a>使用“新建可用性组”对话框 (SQL Server Management Studio)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题包含有关如何使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 的“新建可用性组”对话框在为 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 启用的 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 实例上创建 Always On 可用性组的信息。 “可用性组”  定义一组用户数据库，这些用户数据库将以支持故障转移的单个单元和一组故障转移伙伴（称作“可用性副本” ）的形式进行故障转移。  
  
> [!NOTE]  
>  有关可用性组的简介，请参阅 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)中通过 PowerShell 创建和配置 AlwaysOn 可用性组。  
   
> [!NOTE]  
>  有关创建可用性组的替代方法的信息，请参阅本主题后面的 [相关任务](#RelatedTasks)。  
  
  
##  <a name="prerequisites"></a><a name="PrerequisitesRestrictions"></a>先决条件  
  
-   创建可用性组之前，请先验证承载可用性副本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例位于同一 WSFC 故障转移群集内的不同 Windows Server 故障转移群集 (WSFC) 节点上。 此外，还要验证是否为 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 启用了每个服务器实例以及是否满足其他 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 先决条件。 有关详细信息，我们强烈建议你参阅 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)。  
  
-   在创建可用性组之前，请确保将承载可用性副本的每个服务器实例拥有一个功能正常的数据库镜像端点。 有关详细信息，请参阅 [数据库镜像终结点 (SQL Server)](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)。  
  
-   为了使用 **“新建可用性组”** 对话框，您需要知道将承载可用性副本的服务器实例的名称。 此外，需要知道要添加到新可用性组的任何数据库的名称，并确保这些数据库满足 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)。 如果输入无效值，新的可用性组将无法工作。  
  
## <a name="limitations"></a><a name="Limitations"></a> 限制  
 **“新建可用性组”** 对话框不：  
  
-   创建可用性组侦听器。    
-   将辅助副本联接到可用性组。    
-   执行初始数据同步。  
  
 有关这些配置任务的信息，请参阅本话题后面部分的[跟进：在创建可用性组之后](#FollowUp)。  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要 **sysadmin** 固定服务器角色的成员资格，以及 CREATE AVAILABILITY GROUP 服务器权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  
  
##  <a name="using-the-new-availability-group-dialog-box-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用“新建可用性组”对话框 (SQL Server Management Studio)  
 **创建可用性组**  
  
1.  在对象资源管理器中，连接到承载主副本的服务器实例，然后单击该服务器名称。  
  
2.  展开“Always On 高可用性”节点。  
  
3.  右键单击“可用性组”节点，然后选择“新建可用性组”。  
  
4.  此命令将打开 **“新建可用性组”** 对话框。  
  
5.  使用 **“常规”** 页上的 **“可用性组名称”** 字段，输入新的可用性组的名称。 此名称必须是有效的 SQL Server 标识符，该标识符在 WSFC 群集的所有可用性组中保持唯一。 可用性组名称的最大长度为 128 个字符。  
  
6.  在 **“可用性数据库”** 网格中，单击 **“添加”** ，然后输入希望属于此可用性组的本地数据库的名称。 对要添加的每个数据库重复此操作。 单击 **“确定”** 后，该对话框将验证指定的数据库是否满足属于可用性组的先决条件。 有关先决条件的详细信息，请参阅 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)。  
  
7.  在 **“可用性数据库”** 网格中，单击 **“添加”** ，然后输入要承载辅助副本的服务器实例名称。 该对话框将不尝试连接到这些实例。 如果指定不正确的服务器名称，将添加辅助副本，但是您将无法连接到该副本。  
  
    > [!TIP]  
    >  如果添加了副本但是无法连接到主机服务器实例，可以删除该副本并添加新副本。 有关详细信息，请参阅[将辅助副本从可用性组删除 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md) 和[将辅助副本添加到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)。  
  
8.  在对话框的 **“选择页”** 窗格中，单击 **“备份首选项”** 。 然后，在 **“备份首选项”** 页上，指定应基于副本角色执行备份的位置并将备份优先级分配给将承载此可用性组的可用性副本的每个服务器实例。 有关详细信息，请参阅[可用性组属性面板：新建可用性组（“备份首选项”页）](../../../database-engine/availability-groups/windows/availability-group-properties-new-availability-group-backup-preferences-page.md)。  
  
9. 若要创建可用性组，请单击 **“确定”** 。 这将导致对话框验证指定的数据库是否满足先决条件的要求。  
  
     要退出对话框而不创建可用性组，请单击 **“取消”** 。  
  
##  <a name="follow-up-after-using-the-new-availability-group-dialog-box-to-create-an-availability-group"></a><a name="FollowUp"></a> 跟进：在使用“新建可用性组”对话框创建可用性组之后  
  
-   您将需要依次连接到承载可用性组的辅助副本的每个服务器实例并完成以下步骤：  
  
    1.  将辅助副本联接到该可用性组。 有关详细信息，请参阅 [将辅助副本联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)或 PowerShell 将辅助数据库联接到 Always On 可用性组。  
  
    2.  还原每个主数据库及其事务日志的当前副本（使用 RESTORE WITH NORECOVERY）。 有关详细信息，请参阅 [为可用性组手动准备辅助数据库 (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)或 PowerShell 将辅助数据库联接到 Always On 可用性组。  
  
    3.  立即将每个新准备的辅助数据库加入可用性组。 有关详细信息，请参阅 [将辅助数据库联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)。  
  
-   我们建议您为新的可用性组创建可用性组侦听器。 这要求您连接到承载当前主副本的服务器实例。 有关详细信息，请参阅 [创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **配置可用性组和副本属性**  
  
-   [更改可用性副本的可用性模式 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [更改可用性副本的故障转移模式 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [配置灵活的故障转移策略以控制自动故障转移的条件（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
-   [在添加或修改可用性副本时指定终结点 URL (SQL Server)](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [配置可用性副本备份 (SQL Server)](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
-   [配置对可用性副本的只读访问 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [为可用性组配置只读路由 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [更改可用性副本的会话超时期限 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
 **完成可用性组配置**  
  
-   [将辅助副本联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [为可用性组手动准备辅助数据库 (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [将辅助数据库联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
 **用于创建可用性组的其他方法**  
  
-   [使用可用性组向导 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [创建可用性组 (Transact-SQL)](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)  
  
-   [创建可用性组 (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)  
  
 **启用 AlwaysOn 可用性组**  
  
-   [启用和禁用 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)  
  
 **配置数据库镜像端点**  
  
-   [为 AlwaysOn 可用性组创建数据库镜像终结点 (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [为 Windows 身份验证创建数据库镜像终结点 (Transact-SQL)](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [使用数据库镜像终结点证书 (Transact-SQL)](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   [在添加或修改可用性副本时指定终结点 URL (SQL Server)](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
 **解决 AlwaysOn 可用性组配置问题**  
  
-   [AlwaysOn 可用性组配置故障排除 (SQL Server)](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
-   [添加文件操作失败的故障排除（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   [用于高可用性和灾难恢复的 Microsoft SQL Server AlwaysOn 解决方案指南](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [数据库镜像终结点 (SQL Server)](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [可用性组侦听程序、客户端连接和应用程序故障转移 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  

---
title: 查看可用性组侦听程序属性
description: 介绍如何使用 SQL Server Management Studio、Transact-SQL 或 PowerShell 在 SQL Server 中查看 Always On 可用性组侦听程序的属性。
ms.custom: seo-lt-2019
ms.date: 07/11/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygrouplistenerproperties.general.f1
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
ms.assetid: aca0d016-3228-40b8-bdc3-285ed6d9b280
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1f4f4439f8fc931e024ab53cabd92ab5d3d174c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349240"
---
# <a name="view-availability-group-listener-properties-sql-server"></a>查看可用性组侦听器属性 (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 在 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 中查看 AlwaysOn 可用性组侦听程序的属性。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **查看侦听器属性**  
  
1.  在对象资源管理器中，连接到服务器实例（其上承载要查看其侦听器的可用性组的任何可用性副本）。 单击服务器名称以展开服务器树。  
  
2.  依次展开“Always On 高可用性”节点和“可用性组”节点。  
  
3.  展开可用性组节点，然后展开 **“可用性组侦听器”** 节点。  
  
4.  右键单击要查看的侦听器，然后选择“属性”命令。  
  
5.  这将打开 **“可用性组侦听器属性”** 对话框。 有关详细信息，请参阅本主题后面的 [可用性组侦听程序属性（对话框）](#AgListenerPropertiesDialog)。  
  
###  <a name="availability-group-listener-properties-dialog-box"></a><a name="AgListenerPropertiesDialog"></a> 可用性组侦听程序属性（对话框）  
 **侦听器 DNS 名称**  
 可用性组侦听器的网络名称。  
  
 端口  
 该侦听器使用的 TCP 端口。  
  
> [!NOTE]  
>  如果您连接到主副本，则可以使用此字段来修改侦听器的端口号。 这要求针对可用性组的 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  
  
 **网络模式**  
 指示该侦听器使用的 TCP 协议，选择如下一种：  
  
 **DHCP**  
 该侦听器使用运行动态主机配置协议 (DHCP) 的服务器分配的动态 IP 地址。  
  
 **静态 IP**  
 侦听器使用一个或多个静态 IP 地址。 若要访问不同的子网，可用性组侦听器必须使用静态 IP 地址。  
  
 网格显示侦听器侦听的各个子网以及与该子网关联的 IP 地址。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **查看侦听器属性**  
  
 若要监视可用性组侦听器，请使用以下视图：  
  
 [sys.availability_group_listener_ip_addresses](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  
 针对可用性组侦听器，为当前联机的每个符合标准的虚拟 IP 地址返回一行。  
  
 **列名：** listener_id、ip_address、ip_subnet_mask、is_dhcp、network_subnet_ip、network_subnet_prefix_length、network_subnet_ipv4_mask、state、state_desc  
  
 [sys.availability_group_listeners](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)  
 对于给定的可用性组，返回零行（指示没有与该可用性组关联的网络名称），或为 WSFC 群集中的每个可用性组侦听器配置返回一行。  
  
 **列名：** group_id、listener_id、dns_name、port、is_conformant、ip_configuration_string_from_cluster  
  
 [sys.dm_tcp_listener_states](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)  
 返回包含各个 TCP 侦听器的动态信息的行。  
  
 **列名：** listener_id、ip_address、is_ipv4、port、type、type_desc、state、state_desc、start_time  
  
> [!NOTE]  
>  有关使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 监视你的 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 环境的详细信息，请参阅 [监视可用性组 (Transact-SQL)](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [删除可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/remove-an-availability-group-listener-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [可用性组侦听程序、客户端连接和应用程序故障转移 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [监视可用性组 (Transact-SQL)](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  

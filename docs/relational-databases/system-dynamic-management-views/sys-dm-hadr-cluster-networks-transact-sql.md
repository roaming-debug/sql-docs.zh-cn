---
description: sys.dm_hadr_cluster_networks (Transact-SQL)
title: sys.dm_hadr_cluster_networks (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_hadr_cluster_networks
- sys.dm_hadr_cluster_networks_TSQL
- sys.dm_hadr_cluster_networks
- dm_hadr_cluster_networks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_cluster_networks dynamic management view
ms.assetid: ece32b15-d63f-4f93-92b7-e2930333e97a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d97862778240fac789f95c00a474df12770f8ef3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101589"
---
# <a name="sysdm_hadr_cluster_networks-transact-sql"></a>sys.dm_hadr_cluster_networks (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为每个参与可用性组子网配置的 WSFC 群集成员都返回一行。 您可以使用此动态管理视图来验证为每个可用性副本配置的网络虚拟 IP。  
  
 主键： **member_name**  +  **network_subnet_IP**  +  **network_subnet_prefix_length**  
  
 > [!TIP]
 > 从开始 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ，此动态管理视图除了支持 Always On 可用性组外，还支持 Always On 故障转移群集实例。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**member_name**|**nvarchar(128)**|WSFC 群集中某节点的计算机名称。|  
|**network_subnet_ip**|**nvarchar (48)**|该计算机所属子网的网络 IP 地址。 该地址可以是 IPv4 或 IPv6 地址。|  
|**network_subnet_ipv4_mask**|**nvarchar (45)**|指定 IP 地址所属子网的网络子网掩码。 **network_subnet_ipv4_mask** 在 [CREATE Availability GROUP](../../t-sql/statements/create-availability-group-transact-sql.md)或 [ALTER AVAILABILITY group](../../t-sql/statements/alter-availability-group-transact-sql.md)语句的 WITH DHCP 子句中指定 DHCP <network_subnet_option> 选项 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。<br /><br /> NULL = IPv6 子网。|  
||||  
|**network_subnet_prefix_length**|**int**|指定该计算机所属子网的网络 IP 前缀长度。|  
|**is_public**|**bit**|该网络在 WSFC 群集中是专用还是公共网络，可为下列值之一：<br /><br /> 0 = 专用<br /><br /> 1 = 公共|  
|**is_ipv4**|**bit**|子网的类型，可为下列值之一：<br /><br /> 1 = IPv4<br /><br /> 0 = IPv6|  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [故障转移群集和 AlwaysOn 可用性组 (SQL Server)](../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)   
 [&#40;Transact-sql 监视可用性组&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [sys.dm_os_cluster_nodes &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  

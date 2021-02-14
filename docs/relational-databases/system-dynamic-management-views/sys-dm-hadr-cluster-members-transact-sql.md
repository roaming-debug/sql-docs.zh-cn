---
description: sys.dm_hadr_cluster_members (Transact-SQL)
title: sys.dm_hadr_cluster_members (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 01/31/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_hadr_cluster_members_TSQL
- sys.dm_hadr_cluster_members
- dm_hadr_cluster_members_TSQL
- dm_hadr_cluster_members
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_cluster_members catalog view
ms.assetid: feb20b3a-8835-41d3-9a1c-91d3117bc170
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: eb47ace512b9db4983a9b61ccbdb6db490545937
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354177"
---
# <a name="sysdm_hadr_cluster_members-transact-sql"></a>sys.dm_hadr_cluster_members (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  如果承载启用了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 本地实例的 WSFC 节点具有 SQL 仲裁，则为构成仲裁的每一个成员及各个成员的状态都返回一行。 这包括群集中的所有节点 (通过 **Clusterenum** 函数 CLUSTER_ENUM_NODE 类型返回，) 和磁盘或文件共享见证（如果有）。 为给定成员返回的行包含有关该成员状态的信息。 例如，对于具有 "多数节点仲裁" 节点处于关闭状态的五个节点群集，当从为驻留在具有仲裁的节点上的启用的服务器实例中查询 **sys.dm_hadr_cluster_members** 时 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] ， **sys.dm_hadr_cluster_members** 会将向下节点的状态反映为 "NODE_DOWN"。  
  
 如果 WSFC 节点没有仲裁，则不会返回任何行。  
  
 使用此动态管理视图可以解答下列问题：  
  
-   哪些节点当前正在 WSFC 群集上运行？  
  
-   WSFC 群集可以容忍多少次失败，之后才会在多数节点情况下失去仲裁？  

 > [!TIP]
 > 从开始 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ，此动态管理视图除了支持 Always On 可用性组外，还支持 Always On 故障转移群集实例。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**member_name**|**nvarchar(128)**|成员名称，可以是一个计算机名称、驱动器号或文件共享路径。|  
|**member_type**|**tinyint**|成员的类型，可为下列值之一：<br /><br /> 0 = WSFC 节点<br /><br /> 1 = 磁盘见证服务器<br /><br /> 2 = 文件共享见证服务器<br /><br /> 3 = 云见证|  
|**member_type_desc**|**nvarchar(50)**|**Member_type** 的说明，请执行以下操作之一：<br /><br /> CLUSTER_NODE<br /><br /> DISK_WITNESS<br /><br /> FILE_SHARE_WITNESS<br /><br /> CLOUD_WITNESS|  
|**member_state**|**tinyint**|成员状态，可为下列值之一：<br /><br /> 0 = 脱机<br /><br /> 1 = 联机|  
|**member_state_desc**|**nvarchar(60)**|**Member_state** 的说明，请执行以下操作之一：<br /><br /> UP<br /><br /> DOWN|  
|**number_of_quorum_votes**|**tinyint**|此仲裁成员拥有的仲裁票数。 对于“无大多数: 仅限磁盘”仲裁，此值默认为 0。 对于其他仲裁类型，此值默认为 1。|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="examples"></a>示例  
  
## <a name="see-also"></a>另请参阅  
 [Always On 可用性组动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [AlwaysOn 可用性组目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [&#40;Transact-sql 监视可用性组&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [AlwaysOn 可用性组 &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  

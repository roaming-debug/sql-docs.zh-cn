---
description: sys.dm_hadr_cluster (Transact-SQL)
title: sys.dm_hadr_cluster (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 01/31/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_hadr_cluster
- dm_hadr_cluster_HADR
- sys.dm_hadr_cluster_TSQL
- dm_hadr_cluster
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_hadr_cluster catalog view
- Availability Groups [SQL Server], WSFC clusters
ms.assetid: 13ce70e4-9d43-4a80-a826-099e6213bf85
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0648f9c281c59789f3ae9d4f97cc3d277303abd1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171802"
---
# <a name="sysdm_hadr_cluster-transact-sql"></a>sys.dm_hadr_cluster (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  如果 Windows Server 故障转移群集 (为启用了具有 WSFC 仲裁的实例而承载的实例的 WSFC) 节点 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] ，则 **sys.dm_hadr_cluster** 会返回一个显示群集名称和有关仲裁的信息的行。 如果 WSFC 节点没有仲裁，则不返回任何行。  
 > [!TIP]
 > 从开始 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ，此动态管理视图除了支持 Always On 可用性组外，还支持 Always On 故障转移群集实例。

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**cluster_name**|**nvarchar(128)**|承载启用了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 实例的 WSFC 群集的名称。|  
|**quorum_type**|**tinyint**|此 WSFC 群集使用的仲裁的类型，可为下列值之一：<br /><br /> 0 = 节点的大多数。 此仲裁配置可以承受半数（舍入）减 1 个节点故障。 例如，对于七个节点的群集，此仲裁配置可以承受三个节点故障。<br /><br /> 1 = 节点和磁盘的大多数。 如果磁盘见证服务器保持联机，此仲裁配置可以承受半数（舍入）节点故障。 例如，一个磁盘见证服务器保持联机的六节点群集可以承受三个节点故障。 如果磁盘见证服务器脱机或失败，此仲裁配置可以承受半数（舍入）减 1 个节点故障。 例如，一个磁盘见证服务器已失败的六节点群集可以承受两个 (3-1=2) 节点故障。<br /><br /> 2 = 节点和文件共享的大多数。 此仲裁配置的工作方式与“节点和磁盘的大多数”的工作方式类似，但使用文件共享见证服务器而不是磁盘见证服务器。<br /><br /> 3 = 无大多数: 仅限磁盘。 如果仲裁磁盘联机，此仲裁配置可以承受除一个节点之外的所有节点故障。<br /><br /> 4 = 未知仲裁。 群集的未知仲裁。<br /><br /> 5 = 云见证。 群集利用 Microsoft Azure 进行仲裁仲裁。 如果云见证可用，则群集可能会占用一半节点的故障， (向上舍入) 。|  
|**quorum_type_desc**|**varchar(50)**|**Quorum_type** 的说明，请执行以下操作之一：<br /><br /> NODE_MAJORITY<br /><br /> NODE_AND_DISK_MAJORITY<br /><br /> NODE_AND_FILE_SHARE_MAJORITY<br /><br /> NO_MAJORITY:_DISK_ONLY <br /><br /> UNKNOWN_QUORUM <br /><br /> CLOUD_WITNESS|  
|**quorum_state**|**tinyint**|WSFC 仲裁的状态，可为下列值之一：<br /><br /> 0 = 未知仲裁状态<br /><br /> 1 = 标准仲裁<br /><br /> 2 = 强制仲裁|  
|**quorum_state_desc**|**varchar(50)**|**Quorum_state** 的说明，请执行以下操作之一：<br /><br /> UNKNOWN_QUORUM_STATE<br /><br /> NORMAL_QUORUM<br /><br /> FORCED_QUORUM|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [Always On 可用性组动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [AlwaysOn 可用性组目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [&#40;Transact-sql 监视可用性组&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [sys.dm_hadr_cluster_members (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)  
  
  

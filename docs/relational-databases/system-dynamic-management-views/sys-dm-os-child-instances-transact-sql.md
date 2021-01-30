---
description: sys.dm_os_child_instances (Transact-SQL)
title: sys.dm_os_child_instances (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_child_instances
- sys.dm_os_child_instances_TSQL
- dm_os_child_instances
- dm_os_child_instances_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- server state information [SQL Server]
- sys.dm_os_child_instances dynamic management view
- monitoring server health
ms.assetid: 1bef3074-0ccc-48fa-8f3d-14f3d99df86b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 67e13b08aa1feddf49d91933942f93140c43dc44
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184937"
---
# <a name="sysdm_os_child_instances-transact-sql"></a>sys.dm_os_child_instances (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为从父服务器实例创建的每个用户实例返回一行。  
  
> **重要说明！** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 从 **sys.dm_os_child_instances** 返回的信息可用于确定 (heart_beat 的每个用户实例的状态) 并获取可用于使用或 SQLCmd 创建到用户实例的连接的管道名称 (instance_pipe_name) [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 。 只有在外部进程（例如客户端应用程序）启动了用户实例之后，您才能连接到该用户实例。 SQL 管理工具无法启动用户实例。  
  
> **注意：** 用户实例仅是的一项功能 [!INCLUDE[ssExpressEd11](../../includes/ssexpressed11-md.md)] 。  
> 
> **注意** 若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_os_child_instances**。  
  
|列|数据类型|说明|  
|------------|---------------|-----------------|  
|**owning_principal_name**|**nvarchar(256)**|为其创建该用户实例的用户的名称。|  
|owning_principal_sid|nvarchar(256)|拥有该用户实例的主体的 SID（安全标识符）。 它与 Windows SID 相匹配。|  
|owning_principal_sid_binary|varbinary(85)|拥有用户实例的用户的二进制版 SID。|  
|**instance_name**|**nvarchar(128)**|该用户实例的名称。|  
|**instance_pipe_name**|**nvarchar(260)**|创建用户实例时，便会创建与应用程序连接的命名管道。 可以在连接字符串中使用该名称以连接到该用户实例。|  
|**os_process_id**|**Int**|该用户实例的 Windows 进程的进程号。|  
|**os_process_creation_date**|**日期时间**|上次启动该用户实例进程的日期和时间。|  
|**heart_beat**|**nvarchar (5)**|该用户实例的当前状态，可以是 ALIVE 或 DEAD。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="remarks"></a>备注  
 有关动态管理视图的详细信息，请参阅联机丛书中的 [动态管理视图和函数 &#40;transact-sql&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="see-also"></a>另请参阅  
 [User Instances for Non-Administrators（非管理员的用户实例）](/previous-versions/sql/)  
  

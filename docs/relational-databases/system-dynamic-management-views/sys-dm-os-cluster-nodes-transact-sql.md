---
description: sys.dm_os_cluster_nodes (Transact-SQL)
title: sys.dm_os_cluster_nodes (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_cluster_nodes_TSQL
- dm_os_cluster_nodes_TSQL
- dm_os_cluster_nodes
- sys.dm_os_cluster_nodes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_cluster_nodes dynamic management view
ms.assetid: 92fa804e-2d08-42c6-a36f-9791544b1d42
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7db044bba8fc69287653fe5806a02d287593176c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184959"
---
# <a name="sysdm_os_cluster_nodes-transact-sql"></a>sys.dm_os_cluster_nodes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为故障转移群集实例配置中的每个节点返回一行。 如果当前实例是故障转移群集实例，则它将返回此故障转移群集实例 (原为 "虚拟服务器" ) 的节点的列表。 如果当前服务器实例不是故障转移群集实例，则返回空行集。  
  
> **注意：** 若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_os_cluster_nodes**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**NodeName**|**sysname**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例（虚拟服务器）配置中的节点名称。|  
|状态|**int**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]故障转移群集实例中节点的状态：0、1、2、3、-1。 有关详细信息，请参阅 [GetClusterNodeState 函数](/windows/win32/api/clusapi/nf-clusapi-getclusternodestate)。|  
|status_description|**nvarchar (20)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集节点的状态的描述。<br /><br /> 0 = 正常运行<br /><br /> 1 = 停止<br /><br /> 2 = 已暂停<br /><br /> 3 = 正在联接<br /><br /> -1 = 未知|  
|is_current_owner|bit|1 表示此节点是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集资源的当前所有者。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="remarks"></a>备注  
 启用故障转移群集时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例可在指定为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例（虚拟服务器）配置一部分的故障转移群集的任何节点上运行。  
  
> **注意：** 此视图将替换 fn_virtualservernodes 函数，该函数将在未来版本中弃用。  
  
## <a name="permissions"></a>权限  
 需要对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例具有 VIEW SERVER STATE 权限。  
  
## <a name="examples"></a>示例  
 下面的示例使用 sys. dm_os_cluster_nodes 返回群集服务器实例上的节点。  
  
```  
SELECT NodeName, status, status_description, is_current_owner   
FROM sys.dm_os_cluster_nodes;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|NodeName|状态|status_description|is_current_owner|  
|--------------|------------|-------------------------|------------------------|  
|node1|0|已启动|1|  
|node2|0|已启动|0|  
|Node3|1|闭|0|  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_os_cluster_properties &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-properties-transact-sql.md)   
 [sys.dm_io_cluster_shared_drives &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-io-cluster-shared-drives-transact-sql.md)   
 [sys.fn_virtualservernodes &#40;Transact-sql&#41;](../../relational-databases/system-functions/sys-fn-virtualservernodes-transact-sql.md)   
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  

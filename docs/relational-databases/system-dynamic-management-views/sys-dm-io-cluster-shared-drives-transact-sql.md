---
description: sys.dm_io_cluster_shared_drives (Transact-SQL)
title: sys.dm_io_cluster_shared_drives (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_io_cluster_shared_drives_TSQL
- sys.dm_io_cluster_shared_drives
- dm_io_cluster_shared_drives_TSQL
- dm_io_cluster_shared_drives
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_io_cluster_shared_drives dynamic management view
ms.assetid: c8fcced8-c780-49dc-99bd-6beb3ca532c4
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ed6f7581fb605009fbbdc25c116e3317a5236ec7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97444258"
---
# <a name="sysdm_io_cluster_shared_drives-transact-sql"></a>sys.dm_io_cluster_shared_drives (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  如果当前服务器实例为群集服务器，则此视图返回每个共享驱动器的名称。 如果当前服务器实例不是群集实例，则返回空行集。  
  
> [!NOTE]  
>  若要从调用此 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_io_cluster_shared_drives**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**DriveName**|**nchar(2)**|驱动器的名称（驱动器号），表示共享群集磁盘阵列中的单个磁盘。 此列不可为空值。|  
|pdw_node_id|**int**|**适用** 于： ssPDW<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="remarks"></a>备注  
 如果已启用群集，则故障转移群集实例要求数据和日志文件位于共享磁盘上，以便在实例故障转移到另一节点后，可以访问这些数据和日志文件。 该视图中的每一行代表一个由此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 群集实例使用的共享磁盘。 只有该视图中所列磁盘可以用于存储该 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的数据或日志文件。 该视图中所列磁盘属于与实例关联的群集资源组。  
  
> [!NOTE]  
>  此视图将在未来版本中弃用。 建议改用 [&#40;transact-sql&#41;sys.dm_io_cluster_valid_path_names ](../../relational-databases/system-dynamic-management-views/sys-dm-io-cluster-valid-path-names-transact-sql.md) 。  
  
## <a name="permissions"></a>权限  
 用户必须对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例拥有 VIEW SERVER STATE 权限。  
  
## <a name="examples"></a>示例  
 以下示例使用 sys.dm_io_cluster_shared_drives，在群集服务器实例上确定共享驱动器：  
  
```  
SELECT * FROM sys.dm_io_cluster_shared_drives;  
```  
  
 结果集如下：  
  
 DriveName  
  
 --------\-  
  
 m  
  
 n  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_io_cluster_valid_path_names &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-io-cluster-valid-path-names-transact-sql.md)   
 [sys.dm_os_cluster_nodes &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md)   
 [sys.fn_servershareddrives &#40;Transact-sql&#41;](../../relational-databases/system-functions/sys-fn-servershareddrives-transact-sql.md)   
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

---
description: sys.availability_databases_cluster (Transact-SQL)
title: sys.availability_databases_cluster (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.availability_databases_cluster_TSQL
- sys.availability_databases_cluster
- availability_databases_cluster_TSQL
- availability_databases_cluster
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.availability_databases_cluster catalog view
- Availability Groups [SQL Server], databases
ms.assetid: 8d9c57e5-7f39-4315-b466-92748231140a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: caf27210827b7c3ab981efe65c320bc5ddf6dab5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338955"
---
# <a name="sysavailability_databases_cluster-transact-sql"></a>sys.availability_databases_cluster (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 承载 Windows Server 故障转移群集 (WSFC) 群集中任意 Always On 可用性组的可用性副本的实例上的每个可用性数据库，都包含一行，不管本地副本数据库是否已联接到该可用性组。  
  
> [!NOTE]  
>  将数据库添加到可用性组后，主数据库自动联接到该组。 必须在每个辅助副本上准备辅助数据库，之后才能将其联接到可用性组。   
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|数据库参与其中的可用性组（如果有）的唯一标识符。<br /><br /> NULL = 数据库不是可用性组中的可用性副本的一部分。|  
|**group_database_id**|**uniqueidentifier**|数据库在其参与的可用性组（如果有）中的唯一标识符。 对于主副本上的此数据库以及已将数据库联接到可用性组的每个辅助副本， **group_database_id** 都是相同的。<br /><br /> NULL = 数据库不是任何可用性组中的可用性副本的一部分。|  
|**database_name**|**sysname**|已添加到可用性组的数据库的名称。|  
  
## <a name="permissions"></a>权限  
 如果 **sys.availability_databases_cluster** 的调用方不是数据库的所有者，则查看对应行所需的最小权限为 ALTER any DATABASE 或 VIEW any database 服务器级权限，或 **master** 数据库中的 CREATE database 权限。  
  
## <a name="see-also"></a>另请参阅  
 [sys.availability_groups (Transact-SQL)](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.dm_hadr_database_replica_states &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)   
 [sys.dm_hadr_database_replica_cluster_states &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)   
 [AlwaysOn 可用性组概述 (SQL Server)](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  

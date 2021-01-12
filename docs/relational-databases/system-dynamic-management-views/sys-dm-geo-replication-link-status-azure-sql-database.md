---
description: sys.dm_geo_replication_link_status（Azure SQL 数据库）
title: sys.dm_geo_replication_link_status
titleSuffix: Azure SQL Database
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- dm_geo_replication_link_status
- dm_geo_replication_link_status_TSQL
- sys.dm_geo_replication_link_status
- sys.dm_geo_replication_link_status_TSQL
helpviewer_keywords:
- dm_geo_replication_link_status dynamic management view
- sys.dm_geo_replication_link_status dynamic management view
ms.assetid: d763d679-470a-4c21-86ab-dfe98d37e9fd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 2134a7890d23a7a080a576f8337e750bee2563eb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092789"
---
# <a name="sysdm_geo_replication_link_status-azure-sql-database"></a>sys.dm_geo_replication_link_status（Azure SQL 数据库）

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  在异地复制合作关系中，主数据库和辅助数据库之间的每个复制链接都包含一行。 这包括主数据库和辅助数据库。 如果给定主数据库有多个连续复制链接，此表将对每种关系包含一行。 将在所有数据库（包括逻辑 master）中创建该视图。 但是，在主数据库中查询此视图将返回空集合。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|link_guid|**uniqueidentifier**|复制链接的唯一 ID。|  
|partner_server|**sysname**|包含链接数据库的 SQL 数据库服务器的名称。|  
|partner_database|**sysname**|链接 SQL Database 服务器上链接数据库的名称。|  
|last_replication|**datetimeoffset**|根据主数据库时钟，辅助数据库的上次事务确认的时间戳。 此值仅在主数据库上可用。|  
|replication_lag_sec|**int**|基于主数据库时钟，在主数据库上的 last_replication 值与该事务的提交时间戳之间的时间差（以秒为单位）。  此值仅在主数据库上可用。|  
|replication_state|**tinyint**|此数据库的异地复制状态，如下所示：<br /><br /> 1 = 种子设定。 异地复制目标正在进行种子设定，但两个数据库尚未同步。 在完成种子设定之前，你无法连接到辅助数据库。 从主数据库中删除辅助数据库将取消播种操作。<br /><br /> 2 = 追赶。 辅助数据库处于事务一致的状态，并与主数据库保持同步。<br /><br /> 4 = 已挂起。 这不是活动的连续复制关系。 此状态通常指示可用的互连带宽不足，无法满足主数据库上事务活动的水平。 但是，连续复制关系仍保持不变。|  
|replication_state_desc|**nvarchar(256)**|PENDING<br /><br /> SEEDING<br /><br /> CATCH_UP|  
|角色 (role)|**tinyint**|异地复制角色，如下所示：<br /><br /> 0 = 主要。 Database_id 指的是异地复制合作关系中的主数据库。<br /><br /> 1 = 辅助。  Database_id 指的是异地复制合作关系中的主数据库。|  
|role_desc|**nvarchar(256)**|PRIMARY<br /><br /> SECONDARY|  
|secondary_allow_connections|**tinyint**|辅助类型，为以下类型之一：<br /><br /> 0 = 不允许直接连接到辅助数据库，并且数据库不可用于读访问。<br /><br /> 2 = 允许对辅助复制中的数据库进行所有连接; i 用于只读访问。|  
|secondary_allow_connections_desc|**nvarchar(256)**|否<br /><br /> 全部|  
|last_commit|**datetimeoffset**|上次提交给数据库的事务的时间。 如果在主数据库上检索，它表示主数据库上的上次提交时间。 如果在辅助数据库上检索，它将指示辅助数据库上的上次提交时间。 如果在复制链接的主节点关闭时在辅助数据库上检索，它将指示辅助数据库在哪个位置上捕获。|
  
> [!NOTE]  
>  如果通过删除辅助数据库 (部分 4.2) 来终止复制关系，则 **sys.dm_geo_replication_link_status** 视图中该数据库的行将会消失。  
  
## <a name="permissions"></a>权限  
 具有 view_database_state 权限的任何帐户都可以查询 **sys.dm_geo_replication_link_status**。  
  
## <a name="example"></a>示例  
 显示复制延迟和辅助数据库的上次复制时间。  
  
```  
SELECT   
     link_guid  
   , partner_server  
   , last_replication  
   , replication_lag_sec   
FROM sys.dm_geo_replication_link_status;  
```  
  
## <a name="see-also"></a>另请参阅  
 [更改 Azure SQL 数据库 &#40;的数据库&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Azure SQL Database &#40;sys.geo_replication_links&#41;](../../relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database.md)   
 [Azure SQL Database &#40;sys.dm_operation_status&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md)   
 [sp_wait_for_database_copy_sync](../system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync.md)

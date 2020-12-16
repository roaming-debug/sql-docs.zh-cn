---
description: sys.geo_replication_links（Azure SQL 数据库）
title: Azure SQL Database (sys.geo_replication_links) |Microsoft Docs
ms.custom: ''
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- dm_geo_replication_links_TSQL
- dm_geo_replication_links
- sys.dm_geo_replication_links
- sys.dm_geo_replication_links_TSQL
helpviewer_keywords:
- sys.dm_geo_replication_links dynamic management view
- dm_geo_replication_links dynamic management view
ms.assetid: 58911798-1d60-4f28-87ab-2def2bfc3de7
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-current
ms.openlocfilehash: c69b375ae55f8d7a4d76c4c619f9c9afe1d50def
ms.sourcegitcommit: 86534989f7827f1c36ed1333ad9c4557dfd77f3d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97515373"
---
# <a name="sysgeo_replication_links-azure-sql-database"></a>sys.geo_replication_links（Azure SQL 数据库）

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  在异地复制合作关系中，主数据库和辅助数据库之间的每个复制链接都包含一行。 此视图驻留在逻辑 master 数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|**int**|Sys.databases 视图中当前数据库的 ID。|  
|start_date|**datetimeoffset**|启动数据库复制时区域 SQL 数据库数据中心的 UTC 时间。|  
|modify_date|**datetimeoffset**|数据库异地复制完成后，区域 SQL 数据库数据中心的 UTC 时间。 此时，新数据库与主数据库同步。|  
|link_guid|**uniqueidentifier**|异地复制链接的唯一 ID。|  
|partner_server|**sysname**|包含异地复制数据库的 SQL 数据库服务器的名称。|  
|partner_database|**sysname**|链接的 SQL 数据库服务器上异地复制的数据库的名称。|  
|replication_state|**tinyint**|此数据库的异地复制状态，如下所示：<br /><br /> 0 = 挂起。 已计划创建活动辅助数据库，但尚未完成必要的准备步骤。<br /><br /> 1 = 种子设定。 异地复制目标正在进行种子设定，但两个数据库尚未同步。 在完成种子设定之前，你无法连接到辅助数据库。 从主数据库中删除辅助数据库将取消播种操作。<br /><br /> 2 = 追赶。 辅助数据库处于事务一致的状态，并与主数据库保持同步。|  
|replication_state_desc|**nvarchar(256)**|PENDING<br /><br /> SEEDING<br /><br /> CATCH_UP|  
|角色 (role)|**tinyint**|异地复制角色，如下所示：<br /><br /> 0 = 主要。 Database_id 指的是异地复制合作关系中的主数据库。<br /><br /> 1 = 辅助。  Database_id 指的是异地复制合作关系中的主数据库。|  
|role_desc|**nvarchar(256)**|PRIMARY<br /><br /> SECONDARY|  
|secondary_allow_connections|**tinyint**|辅助类型，为以下类型之一：<br /><br /> 0 = 否。 在故障转移之前，辅助数据库不可访问。<br /><br /> 1 = 只读。 只有 ApplicationIntent = ReadOnly 的客户端连接才能访问辅助数据库。<br /><br /> 2 = 全部。 任何客户端连接都可以访问辅助数据库。|  
|secondary_allow_connections _desc|**nvarchar(256)**|否<br /><br /> 全部<br /><br /> 只读|  
  
## <a name="permissions"></a>权限

此视图仅在 **master** 数据库中适用于服务器级主体登录名。  
  
## <a name="example"></a>示例

显示具有异地复制链接的所有数据库。  

```sql
SELECT
     database_id  
   , start_date  
   , partner_server  
   , partner_database  
   , replication_state  
   , role_desc  
   , secondary_allow_connections_desc
FROM sys.geo_replication_links;  
```

## <a name="see-also"></a>另请参阅

 [ALTER DATABASE（Azure SQL 数据库）](../../t-sql/statements/alter-database-transact-sql.md)   
 [Azure SQL Database &#40;sys.dm_geo_replication_link_status&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md)   
 [Azure SQL Database &#40;sys.dm_operation_status&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md)

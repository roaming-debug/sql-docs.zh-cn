---
description: sys.dm_database_copies（Azure SQL 数据库）
title: Azure SQL Database (sys.dm_database_copies) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- dm_database_copies_TSQL
- sys.dm_database_copies
- dm_database_copies
- sys.dm_database_copies_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_database_copies
- sys.dm_database_copies
ms.assetid: d03d4657-86d1-4496-97e6-cc3bc292e0b1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: a9a6e570b6c535620152f49947b4d4e289a6ff2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195094"
---
# <a name="sysdm_database_copies-azure-sql-database"></a>sys.dm_database_copies（Azure SQL 数据库）
[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

  返回有关数据库副本的信息。  
  
若要返回有关异地复制链接的信息，请使用 SQL 数据库 V12) 中 (可用 [sys.geo_replication_links](../../relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database.md) 或 [sys.dm_geo_replication_link_status](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md) 视图。
  
  
|列名|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|**int**|`sys.databases` 视图中当前数据库的 ID。|  
|start_date|**datetimeoffset**|当启动数据库复制时，区域 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 数据中心的 UTC 时间。|  
|modify_date|**datetimeoffset**|当完成数据库复制时，区域 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 数据中心的 UTC 时间。 截至此时，新数据库与主数据库在事务上一致。 完成信息每1分钟更新一次。<br /><br />反映 percent_complete 字段的上一次更新的 UTC 时间。|  
|**percent_complete**|**real**|已复制的字节的百分比。 值介于 0 到 100 之间。 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 可以自动从某些错误（如故障转移）中恢复，并重新启动数据库复制。 在这种情况下，percent_complete 将从 0 重新开始。|  
|**error_code**|**int**|当大于 0 时，代码指示复制时发生的错误。 如果不发生错误，则值等于 0。|  
|**error_desc**|**nvarchar (4096)**|复制时发生的错误的说明。|  
|**error_severity**|**int**|如果数据库复制失败，则返回 16。|  
|**error_state**|**int**|如果复制恢复，则返回 1。|  
|**copy_guid**|**uniqueidentifier**|复制操作的唯一 ID。|  
|**partner_server**|**sysname**|创建副本的 SQL 数据库服务器的名称。|  
|**partner_database**|**sysname**|伙伴服务器上的数据库副本的名称。|  
|**replication_state**|**tinyint**|此数据库的连续复制复制状态。 值为：<br /><br /> 0 = 挂起。 已计划创建数据库副本，但必要的准备步骤尚未完成或者被种子设定配额临时阻止。<br /><br /> 1 = 种子设定。 要播种的副本数据库尚未与源数据库完全同步。 在此状态下，您无法连接到副本。 若要取消正在进行的种子设定操作，必须删除复制数据库。|  
|**replication_state_desc**|**nvarchar(256)**|replication_state 的说明，它是以下某项：<br /><br /> PENDING<br /><br /> SEEDING<br />|  
|**maximum_lag**|**int**|保留的字段。|  
|**is_continuous_copy**|**bit**|0 = 返回0|  
|**is_target_role**|**bit**|0 = 源数据库<br /><br /> 1 = 复制数据库|  
|**is_interlink_connected**|bit|保留的字段。|  
|**is_offline_secondary**|bit|保留的字段。|  
  
## <a name="permissions"></a>权限  
 此视图仅在 **master** 数据库中适用于服务器级主体登录名。  
  
## <a name="remarks"></a>备注  
 您可以使用源服务器或目标服务器的 **master** 数据库中的 " **sys.dm_database_copies** " 视图 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 。 当数据库复制成功完成并且新的数据库处于联机状态时，将自动删除 **sys.dm_database_copies** 视图中的行。  
  
  

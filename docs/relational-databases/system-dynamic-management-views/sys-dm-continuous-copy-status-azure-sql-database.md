---
description: sys.dm_continuous_copy_status（Azure SQL 数据库）
title: sys.dm_continuous_copy_status
titleSuffix: Azure SQL Database
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_continuous_copy_status_TSQL
- dm_continuous_copy_status_TSQL
- dm_continuous_copy_status
- sys.dm_continuous_copy_status
dev_langs:
- TSQL
helpviewer_keywords:
- dm_continuous_copy_status
- sys.dm_continuous_copy_status
ms.assetid: 411b2e71-4421-4ef5-900d-5af068750899
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 94370b562160315e53feb46a83cbb68a848e064f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099955"
---
# <a name="sysdm_continuous_copy_status-azure-sql-database"></a>sys.dm_continuous_copy_status（Azure SQL 数据库）
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  为当前参与异地复制连续复制关系 (V11) 的每个用户数据库返回一行。 如果对给定主数据库发起多个连续复制关系，则每个活动辅助数据库在此表中都占有一行。  
  
如果你使用的是 SQL 数据库 V12，则应使用 [sys.dm_geo_replication_link_status](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md) (，因为 *sys.dm_continuous_copy_status* 仅适用于 V11) 。

  
|列名|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**copy_guid**|**uniqueidentifier**|副本数据库的唯一 ID。|  
|**partner_server**|**sysname**|链接 SQL Database 服务器的名称。|  
|**partner_database**|**sysname**|链接 SQL Database 服务器上链接数据库的名称。|  
|**last_replication**|**datetimeoffset**|上次应用的复制事务的时间戳。|  
|**replication_lag_sec**|**int**|当前时间与上次成功提交到主数据库上但活动辅助数据库尚未确认的事务的时间戳之间的时间差（以秒计）。|  
|**replication_state**|**tinyint**|此数据库的连续复制复制状态。 下面是可能的值及其说明。<br /><br /> 1：种子设定。 复制目标正在设定种子，处于事务不一致的状态。 直到设定种子完毕后，才能连接到活动辅助数据库。 <br />2：追赶。 活动辅助数据库当前正在与主数据库保持同步，处于事务一致的状态。<br />3：重新设定种子。 因无法恢复的复制故障，正在自动为活动辅助数据库重新设定种子。<br />4：挂起。 这不是活动的连续复制关系。 此状态通常指示可用的互连带宽不足，无法满足主数据库上事务活动的水平。 但是，连续复制关系仍保持不变。|  
|**replication_state_desc**|**nvarchar(256)**|replication_state 的说明，它是以下某项：<br /><br /> SEEDING<br /><br /> CATCH_UP<br /><br /> RE_SEEDING<br /><br /> SUSPENDED|  
|**is_rpo_limit_reached**|**bit**|这始终设置为 0|  
|**is_target_role**|**bit**|0 = 复制关系源<br /><br /> 1 = 复制关系目标|  
|**is_interlink_connected**|**bit**|1 = 互连已连接。<br /><br /> 0 = 互连已断开连接。|  
  
## <a name="permissions"></a>权限  
 若要检索数据，需要 **db_owner** 数据库角色的成员身份。 Dbo 用户、 **dbmanager** 数据库角色的成员以及 sa 登录名也可以查询此视图。  
  
## <a name="remarks"></a>备注  
 **Sys.dm_continuous_copy_status** 视图在 **资源** 数据库中创建，并在所有数据库（包括逻辑 master）中可见。 但是，在主数据库中查询此视图将返回空集合。  
  
 如果在数据库中终止了连续复制关系，则 **sys.dm_continuous_copy_status** 视图中该数据库的行将会消失。  
  
 与 " **sys.dm_database_copies** " 视图一样， **sys.dm_continuous_copy_status** 反映连续复制关系的状态，其中该数据库是主数据库或活动辅助数据库。 与 **sys.dm_database_copies** 不同， **sys.dm_continuous_copy_status** 包含多个列，这些列提供有关操作和性能的详细信息。 这些列包括 **last_replication** 和 **replication_lag_sec**。  
  
## <a name="see-also"></a>另请参阅  
 [Azure SQL Database &#40;sys.dm_database_copies&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md)   
 [Active Geo-Replication 存储过程 &#40;Transact-sql&#41;](../system-stored-procedures/system-stored-procedures-transact-sql.md)  
  

---
description: sys.dm_db_missing_index_groups (Transact-SQL)
title: sys.dm_db_missing_index_groups (Transact-SQL)
ms.custom: ''
ms.date: 03/12/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_missing_index_groups
- sys.dm_db_missing_index_groups_TSQL
- dm_db_missing_index_groups
- dm_db_missing_index_groups_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_groups dynamic management view
- missing indexes feature [SQL Server], sys.dm_db_missing_index_groups dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 692b51f1085becac61f54a3b638a3365e7fa06cd
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551524"
---
# <a name="sysdm_db_missing_index_groups-transact-sql"></a>sys.dm_db_missing_index_groups (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  此 DMV 返回特定索引组中缺少的索引的相关信息（空间索引除外）。 
  
 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，动态管理视图不能公开将影响数据库包含的信息，也不能公开有关用户可以访问的其他数据库的信息。 为了避免公开此信息，每个包含不属于所连接的租户的数据的行都将被筛选掉。  
   
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**index_group_handle**|**int**|标识缺失索引组。|  
|**index_handle**|**int**|标识属于由 **index_group_handle** 指定的组的缺失索引。<br /><br /> 一个索引组仅包含一个索引。|  
  
## <a name="remarks"></a>备注  
 查询优化 `sys.dm_db_missing_index_groups` 器优化查询时，返回的信息会更新，并且不会保留。 仅在重新启动数据库引擎之前，缺少索引信息。 如果数据库管理员要在服务器回收后保留缺失索引信息，则应定期制作缺失索引信息的备份副本。 使用 `sqlserver_start_time` [sys.dm_os_sys_info](sys-dm-os-sys-info-transact-sql.md) 中的列查找上次数据库引擎启动时间。   
  
 输出结果集中的任一列都不是一个键，但是它们结合在一起将形成一个索引键。  

  >[!NOTE]
  >此 DMV 的结果集限制为600行。 每一行都包含一个缺失索引。 如果缺少超过600个索引，则应该解决现有的缺失索引，以便可以查看更新的索引。
  
## <a name="permissions"></a>权限  
 若要查询此动态管理视图，必须授予用户 VIEW SERVER STATE 权限或隐含 VIEW SERVER STATE 权限的任何权限。  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_db_missing_index_columns &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)  
 [sys.dm_db_missing_index_group_stats_query &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-query-transact-sql.md)    
 [sys.dm_os_sys_info &#40;Transact-sql&#41;](sys-dm-os-sys-info-transact-sql.md)
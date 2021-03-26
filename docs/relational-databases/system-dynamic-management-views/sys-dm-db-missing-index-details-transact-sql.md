---
description: sys.dm_db_missing_index_details (Transact-SQL)
title: sys.dm_db_missing_index_details (Transact-SQL)
ms.custom: ''
ms.date: 03/12/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_missing_index_details
- dm_db_missing_index_details
- sys.dm_db_missing_index_details_TSQL
- dm_db_missing_index_details_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- missing indexes feature [SQL Server], sys.dm_db_missing_index_details dynamic management view
- sys.dm_db_missing_index_details dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 994fdc45a49486f1f57214be7646aa89ffb7d7f9
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551611"
---
# <a name="sysdm_db_missing_index_details-transact-sql"></a>sys.dm_db_missing_index_details (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回有关缺失索引的详细信息，不包括空间索引。  
  
 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，动态管理视图不能公开将影响数据库包含的信息，也不能公开有关用户可以访问的其他数据库的信息。 为了避免公开此信息，每个包含不属于所连接的租户的数据的行都将被筛选掉。  

  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**index_handle**|**int**|标识特定的缺失索引。 该标识符在服务器中是唯一的。 `index_handle` 此表的键。|  
|database_id|**smallint**|标识带有缺失索引的表所驻留的数据库。|  
|object_id|**int**|标识索引缺失的表。|  
|**equality_columns**|**nvarchar(4000)**|构成相等谓词的列的逗号分隔列表，谓词的形式如下：<br /><br /> *表列*  =*constant_value*|  
|**inequality_columns**|**nvarchar(4000)**|构成不等谓词的列的逗号分隔列表，例如以下形式的谓词：<br /><br /> *表列*  > *constant_value*<br /><br /> “=”之外的任何比较运算符都表示不相等。|  
|**included_columns**|**nvarchar(4000)**|用于查询的涵盖列的逗号分隔列表。 有关覆盖列或包含列的详细信息，请参阅 [创建包含列的索引](../../relational-databases/indexes/create-indexes-with-included-columns.md)。<br /><br /> 对于内存优化索引 (哈希和内存优化的非聚集) ，请忽略 `included_columns` 。 每个内存优化索引中均包含表的所有列。|  
|**语句**|**nvarchar(4000)**|索引缺失的表的名称。|  
  
## <a name="remarks"></a>备注  
 查询优化 `sys.dm_db_missing_index_details` 器优化查询时，返回的信息会更新，并且不会保留。 仅在重新启动数据库引擎之前，缺少索引信息。 如果数据库管理员要在服务器回收后保留缺失索引信息，则应定期制作缺失索引信息的备份副本。 使用 `sqlserver_start_time` [sys.dm_os_sys_info](sys-dm-os-sys-info-transact-sql.md) 中的列查找上次数据库引擎启动时间。   

  
 若要确定特定缺失索引所属的缺少索引组，可以 `sys.dm_db_missing_index_groups` 通过使用方法的列，查询动态管理视图 `sys.dm_db_missing_index_details` `index_handle` 。  

  >[!NOTE]
  >此 DMV 的结果集限制为600行。 每一行都包含一个缺失索引。 如果缺少超过600个索引，则应该解决现有的缺失索引，以便可以查看更新的索引。 
  
## <a name="using-missing-index-information-in-create-index-statements"></a>在 CREATE INDEX 语句中使用缺失索引信息  
 若要将返回的信息转换为 `sys.dm_db_missing_index_details` 内存优化索引和基于磁盘的索引的 CREATE INDEX 语句，应将相等列放在不等列之前，并将它们作为索引的键。 应该使用 INCLUDE 子句将包含列添加到 CREATE INDEX 语句。 若要确定相等列的有效顺序，请基于其选择性排序：首先列出选择性最强的列（列列表中的最左侧）。  
  
 有关内存优化索引的详细信息，请参阅 [Memory-Optimized 表的索引](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md)。  
  
## <a name="transaction-consistency"></a>事务一致性  
 如果事务创建或删除了一个表，则包含有关已删除对象的缺失索引信息的行将从此动态管理对象中删除，以保持事务一致性。  
  
## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标上，对于弹性池中的数据库， [服务器管理员](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) 帐户或 [Azure Active Directory 管理员](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) 帐户是必需的。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   

## <a name="see-also"></a>另请参阅  
 [sys.dm_db_missing_index_columns &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)  
 [sys.dm_db_missing_index_group_stats_query &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-query-transact-sql.md)     
 [sys.dm_os_sys_info &#40;Transact-sql&#41;](sys-dm-os-sys-info-transact-sql.md)

---
description: sys.dm_db_missing_index_columns (Transact-SQL)
title: sys.dm_db_missing_index_columns (Transact-SQL)
ms.custom: ''
ms.date: 03/12/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_missing_index_columns_TSQL
- sys.dm_db_missing_index_columns_TSQL
- sys.dm_db_missing_index_columns
- dm_db_missing_index_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_columns dynamic management function
- missing indexes feature [SQL Server], sys.dm_db_missing_index_columns dynamic management function
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f893a961bb2ecc0f3c1d0238017bf7592699301e
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551508"
---
# <a name="sysdm_db_missing_index_columns-transact-sql"></a>sys.dm_db_missing_index_columns (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回与缺少索引（不包括空间索引）的数据库表列有关的信息。 `sys.dm_db_missing_index_columns` 是一个动态管理函数。  

## <a name="syntax"></a>语法  
  
```syntaxsql  
sys.dm_db_missing_index_columns(index_handle)  
```  
  
## <a name="arguments"></a>参数  
 *index_handle*  
 唯一地标识缺失索引的整数。 它可以从下列动态管理对象中获得：  
  
 [sys.dm_db_missing_index_details &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)  
  
 [sys.dm_db_missing_index_groups &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|column_id|**int**|列的 ID。|  
|column_name|**sysname**|表列的名称。|  
|**column_usage**|**varchar (20)**|查询使用列的方式。 可能的值及其说明如下：<br /><br /> 相等性：列分配给表达相等性的谓词，格式为： <br />                        *表列*  = *constant_value*<br /><br /> 不等：列分配给表达不相等的谓词，例如，形式 *为：*  >  *constant_value* 的谓词。 “=”之外的任何比较运算符都表示不相等。<br /><br /> INCLUDE：列不用于计算谓词，但用于其他原因，例如，用于覆盖查询。|  
  
## <a name="remarks"></a>备注  
 查询优化 `sys.dm_db_missing_index_columns` 器优化查询时，返回的信息会更新，并且不会保留。 仅在重新启动数据库引擎之前，缺少索引信息。 如果数据库管理员要在服务器回收后保留缺失索引信息，则应定期制作缺失索引信息的备份副本。 使用 `sqlserver_start_time` [sys.dm_os_sys_info](sys-dm-os-sys-info-transact-sql.md) 中的列查找上次数据库引擎启动时间。   

  
## <a name="transaction-consistency"></a>事务一致性  
 如果事务创建或删除了一个表，则包含有关已删除对象的缺失索引信息的行将从此动态管理对象中删除，以保持事务一致性。  
  
## <a name="permissions"></a>权限  
 必须授予用户 VIEW SERVER STATE 权限或任何隐含 VIEW SERVER STATE 权限的权限，以便查询此动态管理函数。  
  
## <a name="examples"></a>示例  
 以下示例对 `Address` 表运行查询，然后使用 `sys.dm_db_missing_index_columns` 动态管理视图运行查询以返回缺失索引的表列。  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT City, StateProvinceID, PostalCode  
FROM Person.Address  
WHERE StateProvinceID = 9;  
GO  
SELECT mig.*, statement AS table_name,  
    column_id, column_name, column_usage  
FROM sys.dm_db_missing_index_details AS mid  
CROSS APPLY sys.dm_db_missing_index_columns (mid.index_handle)  
INNER JOIN sys.dm_db_missing_index_groups AS mig ON mig.index_handle = mid.index_handle  
ORDER BY mig.index_group_handle, mig.index_handle, column_id;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_db_missing_index_details &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)  
 [sys.dm_db_missing_index_group_stats_query &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-query-transact-sql.md)     
 [sys.dm_os_sys_info &#40;Transact-sql&#41;](sys-dm-os-sys-info-transact-sql.md)  

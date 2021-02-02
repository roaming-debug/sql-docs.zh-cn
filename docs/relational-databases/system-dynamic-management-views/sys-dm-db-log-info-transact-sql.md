---
description: 'sys.dm_db_log_info (Transact-sql) '
title: sys.dm_db_log_info (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_db_log_info
- sys.dm_db_log_info_TSQL
- dm_db_log_info
- dm_db_log_info_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_log_info dynamic management view
ms.assetid: f6b40060-c17d-472f-b0a3-3b350275d487
author: savjani
ms.author: pariks
manager: ajayj
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 171cada53538f8e5b615e6096a169b0976b57532
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236051"
---
# <a name="sysdm_db_log_info-transact-sql"></a>sys.dm_db_log_info (Transact-sql) 
[!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)]

返回 (事务日志) 信息的 [虚拟日志文件 ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) 。 请注意，所有事务日志文件都在表输出中合并。 输出中的每一行都表示事务日志中的一个 VLF，并在日志中提供与该 VLF 相关的信息。

## <a name="syntax"></a>语法  
  
```  
sys.dm_db_log_info ( database_id )  
``` 

## <a name="arguments"></a>参数  
 *database_id* |NULL |缺省值  
 数据库的 ID。 *database_id* 是 **int**。有效的输入包括数据库的 ID 号、NULL 或默认值。 默认值为 NULL。 NULL 和默认值在当前数据库的上下文中是等效值。
 
 指定 NULL 可返回当前数据库的 VLF 信息。

 可以指定内置函数 [DB_ID](../../t-sql/functions/db-id-transact-sql.md)。 如果在 `DB_ID` 不指定数据库名称的情况下使用，则当前数据库的兼容级别必须为90或更高。  

## <a name="table-returned"></a>返回的表  

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|**int**|数据库 ID。|
|file_id|**smallint**|事务日志的文件 id。|  
|vlf_begin_offset|**bigint** |[虚拟日志文件的偏移位置 (VLF) ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch)从事务日志文件的开头开始。|
|vlf_size_mb |**float** |[虚拟日志文件 (VLF) ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) 大小（以 MB 为单位），舍入到2个小数位。|     
|vlf_sequence_number|**bigint** |[虚拟日志文件 (VLF ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) 创建的顺序中的) 序列号。 用于唯一标识日志文件中的 Vlf。|
|vlf_active|**bit** |指示 [虚拟日志文件 () ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) 是否正在使用中。 <br />0-VLF 未被使用。<br />1-VLF 处于活动状态。|
|vlf_status|**int** |[虚拟日志文件的状态 (VLF) ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch)。 可能的值包括 <br />0-VLF 处于非活动状态 <br />1-VLF 已初始化但未使用 <br /> 2-VLF 处于活动状态。|
|vlf_parity|**tinyint** |[虚拟日志文件 (VLF) ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch)的奇偶校验。用于在 VLF 中确定日志的结尾。|
|vlf_first_lsn|**nvarchar (48)** |虚拟日志文件中第一条日志记录[ (LSN) 的日志序列号](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) [ (VLF) ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch)。|
|vlf_create_lsn|**nvarchar (48)** |创建[虚拟日志文件 (VLF) ](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch)的日志记录[ (LSN) 的日志序列号](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)。|
|vlf_encryptor_thumbprint|**varbinary(20)**| **适用于：** [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] <br><br> 如果使用 [透明数据加密](../../relational-databases/security/encryption/transparent-data-encryption.md)对 VLF 进行了加密，则显示 VLF 的加密程序的指纹，否则为 NULL。 |

## <a name="remarks"></a>备注
`sys.dm_db_log_info`动态管理函数将替换 `DBCC LOGINFO` 语句。    
 
## <a name="permissions"></a>权限  
需要 `VIEW DATABASE STATE` 数据库中的权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-determing-databases-in-a-sql-server-instance-with-high-number-of-vlfs"></a>A. 确定具有大量 Vlf 的 SQL Server 实例中的数据库
下面的查询确定日志文件中的 Vlf 超过100的数据库，这可能会影响数据库的启动、还原和恢复时间。

```sql
SELECT [name], COUNT(l.database_id) AS 'vlf_count' 
FROM sys.databases s
CROSS APPLY sys.dm_db_log_info(s.database_id) l
GROUP BY [name]
HAVING COUNT(l.database_id) > 100
```

### <a name="b-determing-the-position-of-the-last-vlf-in-transaction-log-before-shrinking-the-log-file"></a>B. `VLF`收缩日志文件之前，确定事务日志中最后一个的位置

以下查询可用于在事务日志上运行 shrinkfile 之前确定最后一个活动 VLF 的位置，以确定事务日志是否可以收缩。

```sql
USE AdventureWorks2016
GO

;WITH cte_vlf AS (
SELECT ROW_NUMBER() OVER(ORDER BY vlf_begin_offset) AS vlfid, DB_NAME(database_id) AS [Database Name], vlf_sequence_number, vlf_active, vlf_begin_offset, vlf_size_mb
    FROM sys.dm_db_log_info(DEFAULT)),
cte_vlf_cnt AS (SELECT [Database Name], COUNT(vlf_sequence_number) AS vlf_count,
    (SELECT COUNT(vlf_sequence_number) FROM cte_vlf WHERE vlf_active = 0) AS vlf_count_inactive,
    (SELECT COUNT(vlf_sequence_number) FROM cte_vlf WHERE vlf_active = 1) AS vlf_count_active,
    (SELECT MIN(vlfid) FROM cte_vlf WHERE vlf_active = 1) AS ordinal_min_vlf_active,
    (SELECT MIN(vlf_sequence_number) FROM cte_vlf WHERE vlf_active = 1) AS min_vlf_active,
    (SELECT MAX(vlfid) FROM cte_vlf WHERE vlf_active = 1) AS ordinal_max_vlf_active,
    (SELECT MAX(vlf_sequence_number) FROM cte_vlf WHERE vlf_active = 1) AS max_vlf_active
    FROM cte_vlf
    GROUP BY [Database Name])
SELECT [Database Name], vlf_count, min_vlf_active, ordinal_min_vlf_active, max_vlf_active, ordinal_max_vlf_active,
((ordinal_min_vlf_active-1)*100.00/vlf_count) AS free_log_pct_before_active_log,
((ordinal_max_vlf_active-(ordinal_min_vlf_active-1))*100.00/vlf_count) AS active_log_pct,
((vlf_count-ordinal_max_vlf_active)*100.00/vlf_count) AS free_log_pct_after_active_log
FROM cte_vlf_cnt
GO
```

## <a name="see-also"></a>另请参阅  
[动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[与数据库相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)   
[sys.dm_db_log_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-stats-transact-sql.md)


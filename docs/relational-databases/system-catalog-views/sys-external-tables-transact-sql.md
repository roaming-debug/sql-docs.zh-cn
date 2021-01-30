---
description: 'sys.external_tables (Transact-sql) '
title: sys.external_tables (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: fac4720c-b679-4ab2-864b-ff7810a9b559
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: caa4ea9fa1ae72760652912b3001f40e7c9e0c09
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193967"
---
# <a name="sysexternal_tables-transact-sql"></a>sys.external_tables (Transact-sql) 
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  当前数据库中的每个外部表在表中各占一行。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|\<inherited columns>||有关此视图所继承的列的列表，请参阅 [sys.databases &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)。||  
|max_column_id_used|**int**|此表曾经使用的最大列 ID。||  
|uses_ansi_nulls|**bit**|在创建表时，将 SET ANSI_NULLS 数据库选项设置为 ON。||  
|data_source_id|**int**|外部数据源的对象 ID。||  
|file_format_id|**int**|对于 HADOOP 外部数据源的外部表，这是外部文件格式的对象 ID。||  
|location|**nvarchar(4000)**|对于 HADOOP 外部数据源的外部表，这是 HDFS 中外部数据的路径。||  
|reject_type|**tinyint**|对于 HADOOP 外部数据源的外部表，这是在查询外部数据时对已拒绝的行进行计数的方式。|VALUE-已拒绝的行数。<br /><br /> 百分比-已拒绝的行的百分比。|  
|reject_value|**float**|对于 HADOOP 外部数据源的外部表：<br /><br /> 对于 *reject_type =* value，这是在查询失败之前允许的行拒绝次数。<br /><br /> 对于 *reject_type* = 百分比，这是在查询失败之前允许的行拒绝百分比。||  
|reject_sample_value|**int**|对于 *reject_type* = 百分比，这是在计算拒绝行的百分比之前，要加载的行数（成功或失败）。|如果 reject_type = VALUE，则为 NULL。|  
|distribution_type|**int**|对于 SHARD_MAP_MANAGER 外部数据源的外部表，这是基础表中各行的数据分布。|0-分片<br /><br /> 1-已复制<br /><br /> 2轮循机制|  
|distribution_desc|**nvarchar(120)**|对于 SHARD_MAP_MANAGER 外部数据源的外部表，这是以字符串形式显示的分布类型。||  
|sharding_column_id|**int**|对于外部表 SHARD_MAP_MANAGER 外部数据源和分片分布，这是包含分片键值的列的列 ID。||  
|remote_schema_name|**sysname**|对于 SHARD_MAP_MANAGER 外部数据源的外部表，这是一个架构，其中基表位于远程数据库 (（如果不同于在其中定义了外部表的架构）) 。||  
|remote_object_name|**sysname**|对于 SHARD_MAP_MANAGER 外部数据源的外部表，此名称是远程数据库中的基表的名称 (如果不同于外部表) 的名称。||  
  
## <a name="permissions"></a>权限  
 目录视图中仅显示用户拥有的安全对象的元数据，或用户对其拥有某些权限的安全对象的元数据。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [sys.external_file_formats &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)   
 [sys.external_data_sources &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md)   
 [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)  
  
  

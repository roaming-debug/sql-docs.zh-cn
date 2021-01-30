---
description: sys.dm_db_resource_stats（Azure SQL 数据库）
title: Azure SQL Database (sys.dm_db_resource_stats) |Microsoft Docs
ms.custom: ''
ms.date: 02/27/2020
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.dm_db_resource_stats
- sys.dm_db_resource_stats_TSQL
- dm_db_resource_stats
- dm_db_resource_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_resource_stats
- dm_db_resource_stats
ms.assetid: 6e76b39f-236e-4bbf-b0b5-38be190d81e8
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current
ms.openlocfilehash: c7cfb333e3cb2d67e61b2f8ae8cb12d0748b05d7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204816"
---
# <a name="sysdm_db_resource_stats-azure-sql-database"></a>sys.dm_db_resource_stats（Azure SQL 数据库）
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  返回 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 数据库的 CPU、I/O 和内存消耗量。 即使数据库中没有活动，也会每隔 15 秒返回一行数据。 历史数据的保留时间大约为1小时。  
  
|列|数据类型|说明|  
|-------------|---------------|-----------------|  
|end_time|**datetime**|UTC 时间用于指示当前报告间隔的结束时间。|  
|avg_cpu_percent|**decimal (5，2)**|平均计算使用率（以服务层限制的百分比表示）。|  
|avg_data_io_percent|**decimal (5，2)**|平均数据 i/o 利用率（以服务层限制的百分比表示）。 对于超大规模数据库，请参阅 [资源利用率统计信息中的数据 IO](/azure/sql-database/sql-database-hyperscale-performance-diagnostics#data-io-in-resource-utilization-statistics)。|  
|avg_log_write_percent|**decimal (5，2)**|平均事务日志写入 (以) 服务层限制的百分比表示。|  
|avg_memory_usage_percent|**decimal (5，2)**|平均内存使用率（以服务层限制的百分比表示）。<br /><br /> 这包括用于缓冲池页和存储 In-Memory OLTP 对象的内存。|  
|xtp_storage_percent|**decimal (5，2)**|In-Memory OLTP 的存储利用率，以服务层限制的百分比表示) 报表间隔结束时 (。 这包括用于存储以下 In-Memory OLTP 对象的内存：内存优化表、索引和表变量。 它还包括用于处理 ALTER TABLE 操作的内存。<br /><br /> 如果数据库中未使用 In-Memory OLTP，则返回0。|  
|max_worker_percent|**decimal (5，2)**| (请求的最大并发工作线程数) 以数据库的服务层限制的百分比表示。|  
|max_session_percent|**decimal (5，2)**|以数据库服务层限制的百分比表示的最大并发会话数。|  
|dtu_limit|**int**|此数据库在此时间间隔内的当前最大数据库 DTU 设置。 对于使用基于 vCore 的模型的数据库，此列为 NULL。|
|cpu_limit|**decimal (5，2)**|此数据库在此时间间隔内的 Vcore 的数目。 对于使用基于 DTU 的模型的数据库，此列为 NULL。|
|avg_instance_cpu_percent|**decimal (5，2)**|承载数据库的 SQL Server 实例的平均 CPU 使用率（以操作系统度量）。 包括用户和内部工作负荷的 CPU 利用率。|
|avg_instance_memory_percent|**decimal (5，2)**|承载数据库的 SQL Server 实例的平均内存使用率（以操作系统度量）。 包括用户和内部工作负荷的内存使用率。|
|avg_login_rate_percent|**decimal (5，2)**|标识为仅供参考。 不支持。 不保证以后的兼容性。|
|replica_role|**int**|表示当前副本的角色，其中0为主，1作为辅助副本，2作为转发器 (异地辅助主) 。 与 ReadOnly 意向连接到所有可读辅助副本时，你会看到 "1"。 如果在未指定 ReadOnly 意向的情况下连接到异地辅助数据库，则应看到 "2" (连接到转发器) 。|
|||
  
> [!TIP]  
> 有关这些限制和服务层的详细信息，请参阅主题 [服务层](/azure/azure-sql/database/purchasing-models)、 [手动优化 Azure SQL 数据库中的查询性能](/azure/azure-sql/database/performance-guidance)以及 [SQL 数据库资源限制和资源调控](/azure/sql-database/sql-database-resource-limits-database-server)。
  
## <a name="permissions"></a>权限
 此视图需要拥有 VIEW DATABASE STATE 权限。  
  
## <a name="remarks"></a>备注
 **Sys.dm_db_resource_stats** 返回的数据表示为运行的服务层/性能级别所允许的最大限制的百分比。
 
 如果数据库在过去60分钟内已故障转移到另一台服务器，则该视图将仅返回该故障转移后的时间数据。  
  
 若要在保持期较长的情况下更细化地查看此数据，请在 **master** 数据库中使用 **sys.resource_stats** 目录视图。 此视图每 5 分钟捕获一次数据，并将历史数据保留 14 天。  有关详细信息，请参阅 [AZURE SQL 数据库&#41;sys.resource_stats &#40;](../../relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database.md)。  
  
 如果数据库是弹性池的成员，则显示为百分比值的资源统计信息将表示为在弹性池配置中设置的数据库的最大限制百分比。  
  
## <a name="example"></a>示例  
  
以下示例将返回当前连接的数据库按最新时间排序的资源利用率数据。  
  
```  
SELECT * FROM sys.dm_db_resource_stats ORDER BY end_time DESC;  
  
```  
  
 以下示例将根据过去几个小时内用户数据库的性能级别所允许的最大 DTU 限制百分比来标识平均 DTU 使用率。 请考虑提高性能级别，因为这些百分比将始终接近于 100%。  
  
```  
SELECT end_time,   
  (SELECT Max(v)    
   FROM (VALUES (avg_cpu_percent), (avg_data_io_percent), (avg_log_write_percent)) AS    
   value(v)) AS [avg_DTU_percent]   
FROM sys.dm_db_resource_stats;  
  
```  
  
 以下示例将返回最近一小时的 CPU 使用率、数据和日志 I/O 以及内存使用率的平均值和最大值。  
  
```  
SELECT    
    AVG(avg_cpu_percent) AS 'Average CPU Utilization In Percent',   
    MAX(avg_cpu_percent) AS 'Maximum CPU Utilization In Percent',   
    AVG(avg_data_io_percent) AS 'Average Data IO In Percent',   
    MAX(avg_data_io_percent) AS 'Maximum Data IO In Percent',   
    AVG(avg_log_write_percent) AS 'Average Log Write I/O Throughput Utilization In Percent',   
    MAX(avg_log_write_percent) AS 'Maximum Log Write I/O Throughput Utilization In Percent',   
    AVG(avg_memory_usage_percent) AS 'Average Memory Usage In Percent',   
    MAX(avg_memory_usage_percent) AS 'Maximum Memory Usage In Percent'   
FROM sys.dm_db_resource_stats;  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.resource_stats &#40;AZURE SQL 数据库&#41;](../../relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database.md) [服务层](/azure/azure-sql/database/purchasing-models)
---
description: sys.resource_stats (Azure SQL Database)
title: Azure SQL Database (sys.resource_stats) |Microsoft Docs
ms.custom: ''
ms.date: 09/13/2018
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- resource_stats
- sys.resource_stats
- sys.resource_stats_TSQL
- resource_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_stats
- resource_stats
ms.assetid: 02379a1b-3622-4578-8c59-a1b8f1a17914
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current
ms.openlocfilehash: 14785888953906f42b2196dc2cb06ca35cfc3a8b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210030"
---
# <a name="sysresource_stats-azure-sql-database"></a>sys.resource_stats (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  返回 Azure SQL 数据库的 CPU 使用率和存储数据。 在五分钟间隔内收集并聚合数据。 对于每个用户数据库，资源消耗发生变化的每五分钟报告时段都有一行。 返回的数据包括 CPU 使用率、存储大小更改和数据库 SKU 修改。 不带更改的空闲数据库可能每五分钟间隔没有行。 历史数据保留大约 14 天。  
  
 根据与数据库关联的 Azure SQL 数据库服务器的版本， **sys.resource_stats** 视图具有不同的定义。 在升级到新的服务器版本时，请考虑这些不同之处和应用程序所需的任何修改。  
  
 下表介绍 v12 服务器中可用的列：  
  
|列|数据类型|说明|  
|----------------------------|---------------|-----------------|  
|start_time|**datetime**|指示5分钟报告间隔的开始时间的 UTC 时间。|  
|end_time|**datetime**|指示五分钟报告间隔结束时间的 UTC 时间。|  
|database_name|**nvarchar(128)**|用户数据库的名称。|  
|sku|**nvarchar(128)**|数据库的服务层。 下面是可能的值：<br /><br /> 基本<br /><br /> 标准<br /><br /> Premium<br /><br />常规用途<br /><br />业务关键|  
|storage_in_megabytes|**float**|时间段的最大存储大小（以 mb 为单位），包括数据库数据、索引、存储过程和元数据。|  
|avg_cpu_percent|**decimal (5，2)**|平均计算使用率（以服务层限制的百分比表示）。|  
|avg_data_io_percent|**decimal (5，2)**|平均 I/O 使用率（以基于服务层限制的百分比表示）。 对于超大规模数据库，请参阅 [资源利用率统计信息中的数据 IO](/azure/sql-database/sql-database-hyperscale-performance-diagnostics#data-io-in-resource-utilization-statistics)。|  
|avg_log_write_percent|**decimal (5，2)**|平均写入资源使用率（以服务层限制的百分比表示）。|  
|max_worker_percent|**decimal (5，2)**| (请求的最大并发工作线程数) 以数据库的服务层限制为百分比。<br /><br /> 最大值当前是根据并发辅助进程计数的15秒的时间间隔计算的五分钟间隔。|  
|max_session_percent|**decimal (5，2)**|基于数据库服务层的限制的最大并发会话数（以百分比表示）。<br /><br /> 最大值当前根据并发会话计数的15秒的时间间隔计算出五分钟间隔。|  
|dtu_limit|**int**|此数据库在此时间间隔内的当前最大数据库 DTU 设置。 |
|xtp_storage_percent|**decimal (5，2)**|In-Memory OLTP 的存储利用率，以服务层限制的百分比表示) 报表间隔结束时 (。 这包括用于存储以下 In-Memory OLTP 对象的内存：内存优化表、索引和表变量。 它还包括用于处理 ALTER TABLE 操作的内存。<br /><br /> 如果数据库中未使用 In-Memory OLTP，则返回0。|
|avg_login_rate_percent|**decimal (5，2)**|标识为仅供参考。 不支持。 不保证以后的兼容性。|
|avg_instance_cpu_percent|**decimal (5，2)**|SQL 数据库进程的平均 CPU 使用率（以百分比表示）。|
|avg_instance_memory_percent|**decimal (5，2)**|作为 SQL 数据库进程的百分比的平均数据库内存使用率。|
|cpu_limit|**decimal (5，2)**|此数据库在此时间间隔内的 Vcore 的数目。 对于使用基于 DTU 的模型的数据库，此列为 NULL。|
|allocated_storage_in_megabytes|**float**|用于存储数据库数据的格式化文件空间量（以 MB 为单位）。 格式化文件空间也称为 "分配的数据空间"。  有关详细信息，请参阅： [SQL Database 中的文件空间管理](/azure/sql-database/sql-database-file-space-management)|
  
> [!TIP]  
>  有关这些限制和服务层的详细信息，请参阅主题 [服务层](/azure/azure-sql/database/purchasing-models)。  
    
## <a name="permissions"></a>权限  
 此视图可用于具有连接到虚拟 **master** 数据库的权限的所有用户角色。  
  
## <a name="remarks"></a>备注  
 **Sys.resource_stats** 返回的数据表示为运行的服务层/性能级别所允许的最大限制的百分比。  
  
 如果数据库是弹性池的成员，则显示为百分比值的资源统计信息将表示为在弹性池配置中设置的数据库的最大限制百分比。  
  
 若要更详细地了解此数据，请使用用户数据库中 **sys.dm_db_resource_stats** 动态管理视图。 此视图每 15 秒钟捕获一次数据，并将历史数据保留 1 个小时。  有关详细信息，请参阅 [AZURE SQL 数据库&#41;sys.dm_db_resource_stats &#40;](../../relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database.md)。  

## <a name="examples"></a>示例  
 以下示例返回与上周相比平均参与至少 80% 的计算使用率的所有数据库。  
  
```sql  
DECLARE @s datetime;  
DECLARE @e datetime;  
SET @s= DateAdd(d,-7,GetUTCDate());  
SET @e= GETUTCDATE();  
SELECT database_name, AVG(avg_cpu_percent) AS Average_Compute_Utilization   
FROM sys.resource_stats   
WHERE start_time BETWEEN @s AND @e  
GROUP BY database_name  
HAVING AVG(avg_cpu_percent) >= 80  
```  
    
## <a name="see-also"></a>另请参阅  
 [服务层](/azure/azure-sql/database/purchasing-models)   
 [服务层功能和限制](/azure/azure-sql/database/performance-guidance)  
  

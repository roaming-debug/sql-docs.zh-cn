---
description: sys.database_connection_stats (Azure SQL Database)
title: sys.database_connection_stats
titleSuffix: Azure SQL Database
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.database_connection_stats
- database_connection_stats
- database_connection_stats_TSQL
- sys.database_connection_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_connection_stats
- database_connection_stats
ms.assetid: 5c8cece0-63b0-4dee-8db7-6b43d94027ec
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current
ms.openlocfilehash: 9aaca2dee46593d637b11cde9b4e9e2a55664f6b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350893"
---
# <a name="sysdatabase_connection_stats-azure-sql-database"></a>sys.database_connection_stats (Azure SQL Database)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  包含数据库连接 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 事件的统计信息，提供数据库连接成功和失败的概述。 有关连接事件的详细信息，请参阅 [AZURE SQL 数据库&#41;sys.event_log &#40;](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md)中的事件类型。  
  
|统计信息|类型|描述|  
|---------------|----------|-----------------|  
|**database_name**|**sysname**|数据库的名称。|  
|**start_time**|**datetime2**|聚合间隔开始的 UTC 日期和时间。 时间始终为 5 分钟的倍数。 例如：<br /><br /> '2011-09-28 16:00:00'<br />'2011-09-28 16:05:00'<br />'2011-09-28 16:10:00'|  
|**end_time**|**datetime2**|聚合间隔结束的 UTC 日期和时间。 **End_time** 始终比同一行中对应的 **start_time** 正好晚5分钟。|  
|**success_count**|**int**|成功连接数。|  
|**total_failure_count**|**int**|失败连接的总数。 这是 **connection_failure_count**、 **terminated_connection_count** 和 **throttled_connection_count** 的总和，不包括死锁事件。|  
|**connection_failure_count**|**int**|登录失败数。|  
|**terminated_connection_count**|**int**|**_仅适用于 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] v11。_**<br /><br /> 终止连接数。|  
|**throttled_connection_count**|**int**|**_仅适用于 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] v11。_**<br /><br /> 中止的连接数。|  
  
## <a name="remarks"></a>备注  
  
### <a name="event-aggregation"></a>事件聚合

 在 5 分钟的间隔内收集和聚合此视图的事件信息。 计数列表示特定连接事件在给定的时间间隔内针对特定数据库发生的次数。  
  
 例如，如果用户在 2/5/2012 (UTC) 的 11:00 到 11:05 之间七次均无法连接到数据库 Database1，则此信息将出现在此视图的单一行内：  
  
|**database_name**|**start_time**|**end_time**|**success_count**|**total_failure_count**|**connection_failure_count**|**terminated_connection_count**|**throttled_connection_count**|  
|------------------------|---------------------|-------------------|------------------------|-------------------------------|------------------------------------|---------------------------------------|--------------------------------------|  
|`Database1`|`2012-02-05 11:00:00`|`2012-02-05 11:05:00`|`0`|`7`|`7`|`0`|`0`|  
  
### <a name="interval-start_time-and-end_time"></a>间隔 start_time 和 end_time

 事件发生的时间或 _之后_**start_time** 和该间隔 _之前_**end_time** *时，* 聚合间隔中包含一个事件。 例如，恰好在 `2012-10-30 19:25:00.0000000` 发生的事件将只包含在如下所示的第二个间隔内：  
  
```  
  
start_time                    end_time  
2012-10-30 19:20:00.0000000   2012-10-30 19:25:00.0000000  
2012-10-30 19:25:00.0000000   2012-10-30 19:30:00.0000000  
```  
  
### <a name="data-updates"></a>数据更新

 此视图中的数据会随时间推移而累积。 通常，数据将在聚合间隔开始后的一小时内累积，但可能需要多达 24 小时才能使所有数据都出现在此视图中。 在此期间，可能会定期更新单一行中的信息。  
  
### <a name="data-retention"></a>数据保留

 此视图中的数据最长保留30天，或者可能不太多，具体取决于数据库的数目和每个数据库生成的唯一事件数。 要将此信息保留更长期间，请将数据复制到单独的数据库。 在对视图进行初始复制后，视图中的行可能会随数据的累积而进行更新。 为了使数据副本保持最新状态，请定期对表中的行进行扫描，以查看现有行的事件计数的增加并确定新行（您可以通过使用开始时间和结束时间来确定唯一的行），然后使用这些更改更新您的数据副本。  
  
### <a name="errors-not-included"></a>未包括的错误

 此视图可能并未包含所有连接和错误信息：  
  
- 此视图不包含 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 可能会发生的所有数据库错误，只包括 [Sys.event_log &#40;Azure SQL 数据库&#41;](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md)的事件类型中指定的所有错误。  
  
- 如果数据中心内存在计算机故障 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ，则事件表中可能缺少少量数据。  
  
- 如果通过 DoSGuard 拦截了 IP 地址，则无法收集来自该 IP 地址的连接尝试事件，这些事件不会出现在此视图中。  
  
## <a name="permissions"></a>权限

 有权访问 **master** 数据库的用户对此视图具有只读访问权限。  
  
## <a name="example"></a>示例

 下面的示例演示了一个 **sys.database_connection_stats** 查询，该查询返回在 9/28/2011 (UTC) 上的中午9/25/2011 和中午之间发生的数据库连接摘要。 默认情况下，查询结果按) **start_time** (升序排序。  
  
```sql
SELECT *  
FROM sys.database_connection_stats
WHERE start_time>='2011-09-25 12:00:00' and end_time<='2011-09-28 12:00:00';  
```  

## <a name="see-also"></a>另请参阅

 [排查 Azure SQL 数据库的连接问题](/azure/sql-database/sql-database-troubleshoot-common-connection-issues)  
  
  

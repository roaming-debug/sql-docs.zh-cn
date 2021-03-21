---
description: sys.dm_exec_external_work (Transact-SQL)
title: sys.dm_exec_external_work (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/10/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- DM_EXEC_EXTERNAL_WORK
- DM_EXEC_EXTERNAL_WORK_TSQL
- SYS.DM_EXEC_EXTERNAL_WORK_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_external_work management view
- dm_exec_external_work management view
- PolyBase,views
- PolyBase
ms.assetid: 7597d97b-1fde-4135-ac35-4af12968f300
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6b68c597892bad6965b86f62fda3834bb9c069e1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752207"
---
# <a name="sysdm_exec_external_work-transact-sql"></a>sys.dm_exec_external_work (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

返回有关每个计算节点上每个工作负荷的工作负荷的信息。  
  
查询 `sys.dm_exec_external_work` 用于标识与外部数据源进行通信的工作（例如，Hadoop 或 MongoDB) ） (。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|`nvarchar(32)`|关联的 PolyBase 查询的唯一标识符。|请参阅 [sys.dm_exec_requests &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)中的 *request_ID* 。|  
|step_index|`int`|此工作线程正在执行的请求。|请参阅 [sys.dm_exec_requests &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)中的 *step_index* 。|  
|dms_step_index|`int`|此工作线程正在执行的 DMS 计划的步骤。|请参阅 [&#40;transact-sql&#41;sys.dm_exec_dms_workers ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-dms-workers-transact-sql.md)。|  
|compute_node_id|`int`|正在运行辅助角色的节点。|请参阅 [&#40;transact-sql&#41;sys.dm_exec_compute_nodes ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md)。|  
|类型|`nvarchar(60)`|外部工作的类型。|Hadoop 和 Azure 存储的 "文件拆分" () <br/><br/>其他外部数据源的 "ODBC 数据拆分" ()  |  
|work_id|`int`|实际拆分的 ID。|大于或等于0。|  
|input_name|`nvarchar(4000)`|要读取的输入的名称|使用 Hadoop 或 Azure 存储时，使用路径)  (文件名。 对于其他外部数据源，它是外部数据源位置和外部表位置的连接： `scheme://DataSourceHostname[:port]/[DatabaseName.][SchemaName.]TableName`|  
|read_location|`bigint`|读取位置的偏移量。| `0` 文件中的字节数减1。<br/><br/>`NULL` 适用于非 Hadoop 或非 Azure 存储。 |  
|read_command|`nvarchar(4000)`|发送到外部数据源的查询。 已在 [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)] 中引入。|表示查询的文本。 对于 Hadoop 和 Azure 存储空间，返回 `NULL` 。|
|bytes_processed|`bigint`|此辅助进程分配用于处理数据的总字节数。 此值不一定表示查询返回的总数据 |大于或等于0。|  
|length|`bigint`|Split 的长度或 Hadoop 的 HDFS 块的长度|用户可定义的。 默认值为 Ed-64m|  
|status|`nvarchar(32)`|辅助进程的状态|挂起，处理，已完成，失败，已中止|  
|start_time|`datetime`|工作开始||  
|end_time|`datetime`|工作结束||  
|total_elapsed_time|`int`|总时间（毫秒）||
|compute_pool_id|`int`|运行辅助进程的池的唯一标识符。 仅适用于 SQL Server 大数据群集。 请参阅 [ (transact-sql) sys.dm_exec_compute_pools ](sys-dm-exec-compute-pools.md)。|`0` [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] 在 Windows 和 Linux 上返回。|

## <a name="see-also"></a>另请参阅  
 [通过动态管理视图进行 PolyBase 故障排除](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与数据库相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  

---
description: SQL 和并行数据仓库动态管理视图
title: SQL 和并行数据仓库动态管理视图
ms.custom: seo-dt-2019
ms.date: 11/05/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 26a0163fc039e240405e4dafa75ba247fa85dfe8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174571"
---
# <a name="sql-and-parallel-data-warehouse-dynamic-management-views"></a>SQL 和并行数据仓库动态管理视图
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

本主题列出了 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 动态管理视图 (dmv) 。  
  
 All [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] dmv 都从 **sys.dm_pdw** 开始。  
  
## <a name="sssdw-and-sspdw-dynamic-management-views"></a>[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 动态管理视图  
 以下动态管理视图适用于 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ：  
  
 [sys.dm_pdw_dms_cores &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-cores-transact-sql.md)  
  
 [sys.dm_pdw_dms_external_work &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-external-work-transact-sql.md)  
  
 [sys.dm_pdw_dms_workers &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql.md)  
  
 [sys.dm_pdw_errors &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-errors-transact-sql.md)  
  
 [sys.dm_pdw_exec_connections &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-connections-transact-sql.md)  
  
 [sys.dm_pdw_exec_requests &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md)  
  
 [sys.dm_pdw_exec_sessions &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md)  
  
 [sys.dm_pdw_hadoop_operations &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-hadoop-operations-transact-sql.md)  
  
 [sys.dm_pdw_lock_waits &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-lock-waits-transact-sql.md)  
  
 [sys.dm_pdw_nodes &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md)  
  
 [sys.dm_pdw_nodes_database_encryption_keys &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql.md)  
  
 [sys.dm_pdw_os_threads &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-os-threads-transact-sql.md)  
  
 [sys.dm_pdw_request_steps &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md)  
  
 [sys.dm_pdw_resource_waits &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-resource-waits-transact-sql.md)  
  
 [sys.dm_pdw_sql_requests &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql.md)  
  
 [sys.dm_pdw_sys_info &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-sys-info-transact-sql.md)  
  
 [sys.dm_pdw_wait_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-wait-stats-transact-sql.md)  
  
 [sys.dm_pdw_waits &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql.md)

## <a name="sssdw-dynamic-management-views"></a>[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 动态管理视图 
 以下动态管理视图仅适用于 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] ：
 
[sys.dm_pdw_nodes_exec_query_plan &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-exec-query-plan-transact-sql.md)  

[sys.dm_pdw_nodes_exec_query_profiles &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-exec-query-profiles-transact-sql.md)  

[sys.dm_pdw_nodes_exec_query_statistics_xml &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-exec-query-statistics-xml-transact-sql.md)  

[sys.dm_pdw_nodes_exec_sql-text &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-exec-sql-text-transact-sql.md)  

[sys.dm_pdw_nodes_exec_text_query_plan &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-exec-text-query-plan-transact-sql.md)

 [sys.dm_workload_management_workload_groups_stats &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md) (预览) 

## <a name="sspdw-dynamic-management-views"></a>[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 动态管理视图  
 以下动态管理视图仅适用于 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ：  
  
 [sys.dm_pdw_component_health_active_alerts &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-component-health-active-alerts-transact-sql.md)  
  
 [sys.dm_pdw_component_health_alerts &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-component-health-alerts-transact-sql.md)  
  
 [sys.dm_pdw_component_health_status &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-component-health-status-transact-sql.md)  
  
 [sys.dm_pdw_diag_processing_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-diag-processing-stats-transact-sql.md)  
  
 [sys.dm_pdw_network_credentials &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-network-credentials-transact-sql.md)  
  
 [sys.dm_pdw_node_status &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-node-status-transact-sql.md)  
  
 [sys.dm_pdw_os_event_logs &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-os-event-logs-transact-sql.md)  
  
 [sys.dm_pdw_os_performance_counters &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-os-performance-counters-transact-sql.md)  
  
 [sys.dm_pdw_query_stats_xe &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-query-stats-xe-transact-sql.md)  
  
 [sys.dm_pdw_query_stats_xe_file &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-query-stats-xe-file-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  

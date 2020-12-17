---
title: 监视加载
description: 使用分析平台系统 (AP) 管理控制台或并行数据仓库 (PDW) 系统视图来监视活动和最近负载。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: b67460528da7cac2e7d3d2d10dfbb4719b08d77f
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97638066"
---
# <a name="monitor-loads-into-parallel-data-warehouse"></a>监视负载到并行数据仓库
使用分析平台系统 (AP) 管理控制台或并行数据仓库 (PDW) [系统视图](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views)来监视活动和最近的[dwloader](dwloader.md)加载。 
  
> [!TIP]  
> 某些负载使用 INSERT 语句或商业智能工具启动，这些工具使用 SQL 语句来执行负载。 

<!-- MISSING LINKS
To monitor this type of load, see [Monitoring Active Queries](monitor-active-queries.md).  
-->
  
## <a name="prerequisites"></a>先决条件  
无论使用哪种方法来监视负载，登录名都必须有权访问基础数据源。 

<!-- MISSING LINKS
For the permissions to grant, see "Use All of the Admin Console" in [Grant Permissions to Use the Admin Console](grant-permissions-admin-console.md). 

--> 
  
## <a name="monitoring-loads"></a>监视负载  
以下部分介绍如何监视负载。  
  
### <a name="to-monitor-loads-by-using-the-admin-console"></a>使用管理控制台监视负载  
  
1.  登录到管理控制台。 <!-- MISSING LINKS See [Monitor the Appliance by Using the Admin Console;](monitor-admin-console.md) for instructions. --> 
  
2.  在顶部菜单中，单击 " **加载**"。 你将看到一个可排序的表，其中显示了所有最近和活动的负载以及其他信息，例如负载是已完成还是仍处于活动状态。 单击列标题可对行进行排序。  
  
3.  若要查看特定负载的其他详细信息，请单击左栏中的负载 **ID** 。 在详细视图中，可以查看每个负载步骤的进度。  
  
请参阅以下系统视图，了解有关管理控制台中显示的负载的元数据的信息：  
  
-   [sys.dm_pdw_exec_requests](../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md)  
  
-   [sys.pdw_loader_run_stages](../relational-databases/system-catalog-views/sys-pdw-loader-run-stages-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)  
  
-   [sys.pdw_loader_backup_runs](../relational-databases/system-catalog-views/sys-pdw-loader-backup-runs-transact-sql.md)  
  
-   [sys.pdw_loader_backup_run_details](../relational-databases/system-catalog-views/sys-pdw-loader-backup-run-details-transact-sql.md)  
  
### <a name="to-monitor-loads-by-using-system-views"></a>使用系统视图监视负载  
若要使用 SQL Server PDW 视图监视活动和最近的负载，请按照以下步骤进行操作。 对于使用的每个系统视图，请参阅该视图的文档，以获取有关视图返回的列和可能值的信息。  
  
1.  `request_id`通过在此视图的列中找到加载程序命令行来查找[sys.dm_pdw_exec_requests](../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md)视图中的负载 `command` 。  
  
    例如，以下命令将返回命令文本和当前状态以及 `request_id` 。  
  
    ```sql  
    SELECT request_id, status, command FROM sys.dm_pdw_exec_requests;  
    ```  
  
2.  使用 `request_id` [sys.pdw_loader_run_stages](../relational-databases/system-catalog-views/sys-pdw-loader-run-stages-transact-sql.md) 和 [sys.pdw_loader_backup_run_details](../relational-databases/system-catalog-views/sys-pdw-loader-backup-run-details-transact-sql.md) 视图来检索负载的其他信息。 例如，下面的查询将返回有关 `run_id` 加载的开始时间、结束时间和持续时间以及所有错误的信息，以及有关处理的行数的信息：  
  
    ```sql  
    SELECT lbr.run_id,   
    er.submit_time, er.end_time, er.total_elapsed_time, er.error_id, lbr.rows_processed, lbr.rows_rejected, lbr.rows_inserted   
    FROM sys.dm_pdw_exec_requests er   
    LEFT OUTER JOIN   
    sys.pdw_loader_backup_runs lbr   
    ON (er.request_id=lbr.requst_id)   
    WHERE er.request_id='12738';  
    ```  
  
<!-- MISSING LINKS

## See Also  
[Common metadata query examples](metadata-query-examples.md)
-->  

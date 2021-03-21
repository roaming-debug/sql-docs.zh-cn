---
description: 'sys.workload_management_workload_groups (Transact-sql) '
title: sys.workload_management_workload_groups (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/05/2019
ms.prod: sql
ms.technology: system-objects
ms.prod_service: synapse-analytics
ms.reviewer: jrasnick
ms.topic: reference
dev_langs:
- TSQL
author: ronortloff
ms.author: rortloff
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 0a8f145be38259f5f3a600cb7a270ccf193b4672
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747747"
---
# <a name="sysworkload_management_workload_groups-transact-sql"></a>sys.workload_management_workload_groups (Transact-sql) 

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

 返回工作负荷组的详细信息。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|
|group_id|**int**|工作负荷组的唯一 ID。 不可为 null。||
|name|**sysname**|工作负荷组的名称。 对于实例必须是唯一的。  不可为 null。||
|importance|**nvarchar(128)**|是此工作负荷组中的请求与共享资源的工作负荷组之间的相对重要性。 不可为 null。|low、below_normal、normal (默认) 、above_normal、高||
|min_percentage_resource|**tinyint**|工作负荷组中的请求的保证资源量。 资源不与其他工作负荷组共享。 不可为 null。||
|cap_percentage_resource|**tinyint**|工作负荷组中请求的资源百分比分配的硬上限。 限制分配给指定级别的最大资源数。 允许的值范围为 1 到 100。||
|request_min_resource_grant_percent|**decimal (5，2)**|指定分配给请求的最小资源量。 值的允许范围是从0.75 到100。||
|request_max_resource_grant_percent |**decimal (5，2)**|指定分配给请求的最大资源量。||
|query_execution_timeout_sec|**int**|取消查询之前允许的执行时间量（以秒为单位）。  查询在达到执行的返回阶段后，便无法取消。  query_execution_timeout_sec 不包括排队所用的时间。|
|query_wait_timeout_sec|**int**|INTERNAL||
|create_time|**datetime**|工作负荷组的创建时间。 不可为 null。||
modify_time|**datetime**|上次修改工作负荷组的时间。 不可为 null。||
|&nbsp;||||
  
## <a name="permissions"></a>权限

需要 VIEW SERVER STATE 权限。

## <a name="next-steps"></a>后续步骤

 有关 Azure Synapse Analytics 和并行数据仓库的所有目录视图的列表，请参阅 [Azure Synapse analytics 和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)。 若要创建工作负荷组，请参阅 [创建工作负荷组](../../t-sql/statements/create-workload-group-transact-sql.md)。 有关工作负荷分类的详细信息，请参阅 [工作负荷隔离](/azure/sql-data-warehouse/sql-data-warehouse-workload-isolation)

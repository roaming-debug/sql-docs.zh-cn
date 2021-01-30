---
description: 'sys.workload_management_workload_classifiers (Transact-sql) '
title: sys.workload_management_workload_classifiers (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/05/2019
ms.prod: sql
ms.technology: system-objects
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.topic: reference
dev_langs:
- TSQL
author: ronortloff
ms.author: rortloff
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 0b354af4a220adcaed93663fc899f80306e0a85a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190847"
---
# <a name="sysworkload_management_workload_classifiers-transact-sql"></a>sys.workload_management_workload_classifiers (Transact-sql) 

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

 返回工作负荷分类器的详细信息。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|
|classifier_id|**int**|分类器的唯一 ID。 不可为 Null。||
group_name|**sysname**|分类器分配到的工作负荷组的名称。 不可为 null。 可加入到 sys.workload_management_workload_groups ||
name|**sysname**|分类器的名称。 对于实例必须是唯一的。 不可为 null。||
|importance|**sysname**|是此工作负荷组中的请求与共享资源的工作负荷组之间的相对重要性。  分类器中指定的重要性覆盖工作负荷组重要性设置。 可以为 Null。  如果为 null，则使用工作负荷组重要性设置。|low、below_normal、normal (默认) 、above_normal、高 |
|create_time|**datetime**|分类器的创建时间。 不可为 null。||
modify_time|**datetime**|上次修改分类器的时间。 不可为 null。||
is_enabled|**bit**|INTERNAL||
|&nbsp;||||
  
## <a name="permissions"></a>权限

需要 VIEW SERVER STATE 权限。

## <a name="next-steps"></a>后续步骤

 有关 Azure Synapse Analytics 和并行数据仓库的所有目录视图的列表，请参阅 [Azure Synapse analytics 和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)。 若要创建工作负荷分类器，请参阅 [创建工作负荷分类器](../../t-sql/statements/create-workload-classifier-transact-sql.md)。 有关工作负荷分类的详细信息，请参阅 [工作负荷分类](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification)

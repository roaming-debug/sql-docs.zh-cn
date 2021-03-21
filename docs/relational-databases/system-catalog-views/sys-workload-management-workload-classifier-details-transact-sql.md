---
description: 'sys.workload_management_workload_classifier_details (Transact-sql) '
title: sys.workload_management_workload_classifier_details (Transact-sql) |Microsoft Docs
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
ms.openlocfilehash: b8b2a43c84f2aaa294fd68931472f0cf1f7d5339
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747797"
---
# <a name="sysworkload_management_workload_classifier_details-transact-sql"></a>sys.workload_management_workload_classifier_details (Transact-sql) 

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

  返回每个分类器的详细信息。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|
|classifier_id|**int**|分类器的 ID。  不可为 null。|
|classifier_type|**sysname**|要 [sys.workload_management_workload_classifiers](sys-workload-management-workload-classifiers-transact-sql.md)的可加入。|`membername`</br>`wlm_label`</br>`wlm_context`</br>`start_time`</br>`end_time`|
|classifier_value|**sysname**|分类器的值。 不可为 null。||

## <a name="permissions"></a>权限

需要 VIEW SERVER STATE 权限。

## <a name="next-steps"></a>后续步骤
  
有关 Azure Synapse Analytics 和并行数据仓库的所有目录视图的列表，请参阅 [Azure Synapse analytics 和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)。 若要创建工作负荷分类器，请参阅 [创建工作负荷分类器](../../t-sql/statements/create-workload-classifier-transact-sql.md)。 有关工作负荷分类的详细信息，请参阅 Azure Synapse 分析 [工作负荷分类](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification) 和 [工作负荷重要性](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification)

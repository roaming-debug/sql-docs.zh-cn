---
title: sys.dm_pdw_nodes_exec_text_query_plan (Transact-sql) |Microsoft Docs
description: 动态管理视图，以文本格式返回针对 TSQL 批处理或批处理中的特定语句的显示计划。
ms.custom: ''
ms.date: 10/14/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: ''
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 5ecfdb5747eee9fc12934f3514b3df4b74cd8d46
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644037"
---
# <a name="sysdm_pdw_nodes_exec_text_query_plan--transact-sql"></a>sys.dm_pdw_nodes_exec_text_query_plan (Transact-sql) 
[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

为 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理或批处理中的特定语句返回文本格式的显示计划。

## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**pdw_node_ID**|**int**|与节点关联的唯一数字 ID。|
|**dbid**|**smallint**|在编译对应于此计划的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句时有效的上下文数据库的 ID。 对于临时和预定义 SQL 语句，指编译这些语句时所在的数据库的 ID。<br /><br /> 此列可为空值。|  
|**objectid**|**int**|此查询计划的对象（如存储过程或用户定义函数）的 ID。 对于即席批处理和已准备好的批处理，此列为 **null**。<br /><br /> 此列可为空值。|  
|**数字**|**smallint**|为存储过程编号的整数。 对于即席批处理和已准备好的批处理，此列为 **null**。<br /><br /> 此列可为空值。| 
|**过**|**bit**|指示对应的存储过程是否已加密。<br /><br /> 0 = 未加密<br /><br /> 1 = 已加密<br /><br /> 此列不可为空值。|  
|**query_plan**|**nvarchar(max)**|包含与 *plan_handle* 一起指定的查询执行计划的编译时显示计划表示形式。 显示计划采用文本格式。 为包含即席 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句、存储过程调用以及用户定义函数调用等内容的每个批查询生成一个计划。<br /><br /> 此列可为空值。|  

## <a name="remarks"></a>备注  
[Sys.dm_exec_text_query_plan](./sys-dm-exec-text-query-plan-transact-sql.md)适用的相同备注。  

## <a name="permissions"></a>权限  
 要求对服务器具有 **sysadmin** 服务器角色或 `VIEW SERVER STATE` 权限。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 Azure Synapse 分析和并行数据仓库动态管理视图 ](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  

  ## <a name="next-steps"></a>后续步骤
 有关更多开发技巧，请参阅 [Azure Synapse Analytics 开发概述](/azure/sql-data-warehouse/sql-data-warehouse-overview-develop)。
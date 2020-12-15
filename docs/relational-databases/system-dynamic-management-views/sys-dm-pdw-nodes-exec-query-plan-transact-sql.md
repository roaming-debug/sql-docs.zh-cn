---
title: sys.dm_pdw_nodes_exec_query_plan (Transact-sql) |Microsoft Docs
description: 动态管理视图，以 XML 格式返回计划句柄指定的批处理的显示计划。 计划句柄指定的计划可以处于缓存或正在执行状态。
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
ms.openlocfilehash: 9cc46d90e27f70870e320c7815752351397b9c97
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472748"
---
# <a name="syspdw_nodes_dm_exec_query_plan-transact-sql"></a>sys.pdw_nodes_dm_exec_query_plan (Transact-sql) 
[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

以 XML 格式返回计划句柄指定的批查询的显示计划。 计划句柄指定的计划可以处于缓存或正在执行状态。  

> [!note] 
> 在 Synapse SQL 中，在查询中添加空格将构成查询更改，这会导致重新计算查询哈希，而不重复使用以前缓存的执行计划。


## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|pdw_node_id|**int**|与节点关联的唯一数字 ID。| 
|**dbid**|**smallint**|在编译对应于此计划的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句时有效的上下文数据库的 ID。 对于计划外和预定义 SQL 语句，是编译语句的数据库的 ID。<br /><br /> 此列可为空值。|  
|**objectid**|**int**|此查询计划的对象（如存储过程或用户定义函数）的 ID。 对于即席批处理和已准备好的批处理，此列为 **null**。<br /><br /> 此列可为空值。|  
|**数字**|**smallint**|为存储过程编号的整数。 对于即席批处理和已准备好的批处理，此列为 **null**。<br /><br /> 此列可为空值。| 
|**过**|**bit**|指示对应的存储过程是否已加密。<br /><br /> 0 = 未加密<br /><br /> 1 = 已加密<br /><br /> 此列不可为空值。|  
|**query_plan**|**xml**|包含与 *plan_handle* 一起指定的查询执行计划的编译时显示计划表示形式。 显示计划的格式为 XML。 为包含即席 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句、存储过程调用以及用户定义函数调用等内容的每个批查询生成一个计划。<br /><br /> 此列可为空值。|  
  
## <a name="remarks"></a>备注  
[Sys.dm_exec_query_plan](./sys-dm-exec-query-plan-transact-sql.md?view=sql-server-ver15)适用的相同备注。  
  
## <a name="permissions"></a>权限  
 要求对服务器具有 **sysadmin** 服务器角色或 `VIEW SERVER STATE` 权限。  
  
## <a name="see-also"></a>请参阅  
 [&#40;Transact-sql&#41;的 Azure Synapse 分析和并行数据仓库动态管理视图 ](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  

 ## <a name="next-steps"></a>后续步骤
 有关更多开发技巧，请参阅 [Azure Synapse Analytics 开发概述](/azure/sql-data-warehouse/sql-data-warehouse-overview-develop)。

---
title: sys.dm_pdw_nodes_exec_query_statistics_xml (Transact-sql) |Microsoft Docs
description: 动态管理视图，返回正在进行的请求的查询执行计划。 使用此 DMV 检索带有暂时性统计信息的显示计划 XML。
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
ms.openlocfilehash: 65791195a30dcd23b184dbceeafdd786b64f857e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644079"
---
# <a name="dm_pdw_nodes_exec_query_statistics_xml-transact-sql"></a>dm_pdw_nodes_exec_query_statistics_xml (Transact-sql) 
[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

返回正在进行的请求的查询执行计划。 使用此 DMV 检索带有暂时性统计信息的显示计划 XML。

## <a name="table-returned"></a>返回的表

|列名|数据类型|说明|  
|-----------------|---------------|-----------------|
|node_id|**int**|与节点关联的唯一数字 ID。|
|session_id|**smallint**|会话的 ID。 不可为 Null。|
|request_id|**int**|请求的 ID。 不可为 Null。|
|sql_handle|**varbinary(64)**|是唯一标识查询所属的批处理或存储过程的标记。 可以为 NULL。|
|plan_handle|**varbinary(64)**|是一个标记，用于为当前正在执行的批处理唯一标识查询执行计划。 可以为 NULL。|
|query_plan|**xml**|包含与 *plan_handle* 包含部分统计信息的查询执行计划的运行时显示计划表示形式。 显示计划的格式为 XML。 为包含即席 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句、存储过程调用以及用户定义函数调用等内容的每个批查询生成一个计划。 可以为 NULL。|

## <a name="remarks"></a>备注
[Sys.dm_exec_query_statistics_xml](./sys-dm-exec-query-statistics-xml-transact-sql.md)适用的相同备注。   

## <a name="permissions"></a>权限  
 要求具有对服务器的 `VIEW SERVER STATE` 权限。  

## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 Azure Synapse 分析和并行数据仓库动态管理视图 ](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  

 ## <a name="next-steps"></a>后续步骤
 有关更多开发技巧，请参阅 [Azure Synapse Analytics 开发概述](/azure/sql-data-warehouse/sql-data-warehouse-overview-develop)。
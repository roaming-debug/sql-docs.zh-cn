---
title: sys.dm_pdw_nodes_exec_sql_text (Transact-sql) |Microsoft Docs
description: 动态管理视图，它返回由指定 sql_handle 标识的 SQL 批处理的文本。
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
ms.openlocfilehash: b4e4c686411d40a2c161c670821e6566460db4a4
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644059"
---
# <a name="syspdw_nodes_dm_exec_sql_text-transact-sql"></a>sys.pdw_nodes_dm_exec_sql_text (Transact-sql) 
[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

返回由指定 *sql_handle* 标识的 SQL 批处理的文本。 该表值函数将替换系统函数 **fn_get_sql**。  
   
## <a name="table-returned"></a>返回的表  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|pdw_node_id|**int**|与节点关联的唯一数字 ID。|
|**dbid**|**smallint**|数据库的 ID。<br /><br /> 对于计划外和预定义 SQL 语句，是编译语句的数据库的 ID。|  
|**objectid**|**int**|对象的 ID。<br /><br /> 对于临时和预定义 SQL 语句为 NULL。|  
|**数字**|**smallint**|对于带编号的存储过程，此列返回存储过程的编号。 有关详细信息，请参阅 [&#40;transact-sql&#41;sys.numbered_procedures ](../../relational-databases/system-catalog-views/sys-numbered-procedures-transact-sql.md)。<br /><br /> 对于临时和预定义 SQL 语句为 NULL。|  
|**过**|**bit**|1： SQL 文本已加密。<br /><br /> 0：不加密 SQL 文本。|  
|**text**|**nvarchar(max)**|SQL 查询的文本。<br /><br /> 对于已加密对象为 NULL。|  

## <a name="remarks"></a>备注  
[Sys.dm_exec_sql_text](./sys-dm-exec-sql-text-transact-sql.md)适用的相同备注。  
  
## <a name="permissions"></a>权限  
 要求对服务器具有 **sysadmin** 服务器角色或 `VIEW SERVER STATE` 权限。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 Azure Synapse 分析和并行数据仓库动态管理视图 ](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  

  ## <a name="next-steps"></a>后续步骤
 有关更多开发技巧，请参阅 [Azure Synapse Analytics 开发概述](/azure/sql-data-warehouse/sql-data-warehouse-overview-develop)。
---
description: 'sys.dm_pdw_exec_sessions (Transact-sql) '
title: sys.dm_pdw_exec_sessions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/22/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 31c262b3-7e4d-44c4-af71-aaef0fd1a980
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 829eb5d53fc4e60c7c975a75a2e3159885b85171
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99142452"
---
# <a name="sysdm_pdw_exec_sessions-transact-sql"></a>sys.dm_pdw_exec_sessions (Transact-sql) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  保存有关设备上当前或最近打开的所有会话的信息。 每个会话在表中各占一行。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|session_id|**nvarchar(32)**|如果会话已终止并且查询是在终止时执行) ，则当前查询或最后一个查询的 id 将 (。 此视图的键。|在系统中的所有会话中是唯一的。|  
|status|**nvarchar (10)**|对于当前会话，确定会话当前处于活动状态还是处于空闲状态。 对于过去的会话，如果已将会话强行关闭) ，则会话状态可能显示已关闭或已终止 (。|"活动"、"已关闭"、"空闲"、"已终止"|  
|request_id|**nvarchar(32)**|当前查询或上次查询运行的 id。|系统中所有请求都是唯一的。 如果未运行任何，则为 Null。|  
|security_id|**varbinary(85)**|运行会话的主体的安全 ID。||  
|login_name|**nvarchar(128)**|运行会话的主体的登录名。|符合用户命名约定的任何字符串。|  
|login_time|**datetime**|用户登录的日期和时间，创建此会话的日期和时间。|当前时间之前有效的 **日期** 时间。|  
|query_count|**int**|捕获自创建后运行的查询/requeststhis 会话数。|大于或等于0。|  
|is_transactional|**bit**|捕获会话当前是否在事务中。|对于自动提交，为 0; 对于事务，则为1。|  
|client_id|**nvarchar(255)**|捕获会话的客户端信息。|任何有效的字符串。|  
|app_name|**nvarchar(255)**|捕获应用程序名称信息，可以选择在连接过程中进行设置。|任何有效的字符串。|  
|sql_spid|**int**|SPID 的 id 号。 使用 `session_id` 此会话。 使用 `sql_spid` 列来联接 **sys.dm_pdw_nodes_exec_sessions**。<br /><br /> 警告此列包含已关闭的 spid。 **\* \* \* \***||  
  
 有关此视图保留的最大行的信息，请参阅 [容量限制](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) 主题中的元数据部分。  
  
## <a name="permissions"></a>权限  
 需要 `VIEW SERVER STATE` 权限。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 Azure Synapse 分析和并行数据仓库动态管理视图 ](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  

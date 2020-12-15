---
description: 'sys.dm_pdw_exec_connections (Transact-sql) '
title: sys.dm_pdw_exec_connections (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 2625466b-d0ef-4c71-bedc-6d13491a8351
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 0bd3d4c55a61f7e4911d4b9f1e962a11960976f5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482604"
---
# <a name="sysdm_pdw_exec_connections-transact-sql"></a>sys.dm_pdw_exec_connections (Transact-sql) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  返回有关与此 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 实例建立的连接的信息以及每个连接的详细信息。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|session_id|**int**|标识与此连接关联的会话。 使用 `SESSION_ID()` 可返回 `session_id` 当前连接的。|  
|connect_time|**datetime**|连接建立时的时间戳。 不可为 null。|  
|encrypt_option|**nvarchar(40)**|指示 (连接是否已加密) 或 FALSE (连接未 enctypred) 。|  
|auth_scheme|**nvarchar(40)**|指定此连接使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Windows 身份验证方案。 不可为 null。|  
|client_id|**varchar(48)**|连接到此服务器的客户端的 IP 地址。 可以为 Null。|  
|sql_spid|**int**|连接的服务器进程 ID。 使用 `@@SPID` 可返回 `sql_spid` 当前连接的。对于大多数用作其他用途，请改用 `session_id` 。|  
  
## <a name="permissions"></a>权限  
 要求对服务器具有 **VIEW SERVER STATE** 权限。  
  
## <a name="relationship-cardinalities"></a>关系基数  
  
| From | 功能 | 关系 |
| ---- | -- | ------------ |
|dm_pdw_exec_sessions dm_pdw_exec_sessions.session_id|dm_pdw_exec_connections dm_pdw_exec_connections.session_id|一对一|  
|dm_pdw_exec_requests dm_pdw_exec_requests.connection_id|dm_pdw_exec_connections dm_pdw_exec_connections.connection_id|多对一|  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 收集查询自有连接有关信息的典型查询。  
  
```  
SELECT  
    c.session_id, c.encrypt_option,  
    c.auth_scheme, s.client_id, s.login_name,   
    s.status, s.query_count  
FROM sys.dm_pdw_exec_connections AS c  
JOIN sys.dm_pdw_exec_sessions AS s  
    ON c.session_id = s.session_id  
WHERE c.session_id = SESSION_ID();  
```  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 Azure Synapse 分析和并行数据仓库动态管理视图 ](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  


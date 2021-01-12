---
description: sys.dm_exec_connections (Transact-SQL)
title: sys.dm_exec_connections (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_connections_TSQL
- sys.dm_exec_connections_TSQL
- sys.dm_exec_connections
- dm_exec_connections
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_connections dynamic management view
ms.assetid: 6bd46fe1-417d-452d-a9e6-5375ee8690d8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 89d36c36c0d1e32bd8da68db6847dde61dc6a7a7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101611"
---
# <a name="sysdm_exec_connections-transact-sql"></a>sys.dm_exec_connections (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回有关与此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例建立的连接的信息以及每个连接的详细信息。 返回 SQL Server 的服务器范围连接信息。 返回 SQL 数据库的当前数据库连接信息。  
  
> [!NOTE]
> 若要从或调用它 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用 [sys.dm_pdw_exec_connections &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-connections-transact-sql.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|session_id|**int**|标识与此连接关联的会话。 可以为 Null。|  
|most_recent_session_id|**int**|表示与此连接关联的最近请求的会话 ID。 其他会话可以重复使用 (SOAP 连接。 ) 可以为 null。|  
|connect_time|**datetime**|连接建立时的时间戳。 不可为 null。|  
|net_transport|**nvarchar(40)**|当连接具有多个活动的结果集 (MARS) 启用时，始终返回 **Session** 。<br /><br /> **注意：** 描述此连接使用的物理传输协议。 不可为 null。|  
|protocol_type|**nvarchar(40)**|指定负载的协议类型。 此参数当前可区分 TDS (TSQL) 和 SOAP。 可以为 Null。|  
|protocol_version|**int**|与此连接关联的数据访问协议的版本。 可以为 Null。|  
|endpoint_id|**int**|说明其连接类型的标识符。 此 endpoint_id 可用于查询 sys.endpoints 视图。 可以为 Null。|  
|encrypt_option|**nvarchar(40)**|说明是否为此连接启用了加密的布尔值。 不可为 null。|  
|auth_scheme|**nvarchar(40)**|指定此连接使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Windows 身份验证方案。 不可为 null。|  
|node_affinity|**smallint**|标识与此连接关联的内存节点。 不可为 null。|  
|num_reads|**int**|此连接上发生的字节读取次数。 可以为 Null。|  
|num_writes|**int**|此连接上发生的字节写入数。 可以为 Null。|  
|last_read|**datetime**|此连接中上一次发生读操作的时间戳。 可以为 Null。|  
|last_write|**datetime**|此连接中上一次发生写操作的时间戳。 不可为 Null。|  
|net_packet_size|**int**|用于信息和数据传输的网络包的大小。 可以为 Null。|  
|client_net_address|**varchar(48)**|与此服务器连接的客户端的主机地址。 可以为 Null。<br /><br /> 在版本早于 V12 的 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，此列始终返回 NULL。|  
|client_tcp_port|**int**|与此连接关联的客户端计算机上的端口号。 可以为 Null。<br /><br /> 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，此列始终返回 NULL。|  
|local_net_address|**varchar(48)**|表示此连接的目标服务器的 IP 地址。 只对使用 TCP 传输提供程序的连接可用。 可以为 Null。<br /><br /> 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，此列始终返回 NULL。|  
|local_tcp_port|**int**|如果此连接使用 TCP 传输，则表示此连接的目标服务器的 TCP 端口。 可以为 Null。<br /><br /> 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，此列始终返回 NULL。|  
|connection_id|**uniqueidentifier**|对每个连接进行唯一标识。 不可为 null。|  
|parent_connection_id|**uniqueidentifier**|标识 MARS 会话正在使用的主要连接。 可以为 Null。|  
|most_recent_sql_handle|**varbinary(64)**|此连接上执行的上一个请求的 SQL 句柄。 most_recent_sql_handle 列始终与 most_recent_session_id 列同步。 可以为 Null。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   

## <a name="physical-joins"></a>物理联接  
 ![sys.dm_exec_connections 的联接](../../relational-databases/system-dynamic-management-views/media/join-dm-exec-connections-1.gif "sys.dm_exec_connections 的联接")  
  
## <a name="relationship-cardinalities"></a>关系基数  
  
| 第一个元素 | 第二个元素 | Relationship |
| --------------| -------------- | ------------ |  
|dm_exec_sessions.session_id|dm_exec_connections.session_id|一对一|  
|dm_exec_requests.connection_id|dm_exec_connections.connection_id|多对一|  
|dm_broker_connections.connection_id|dm_exec_connections.connection_id|一对一|  
  
## <a name="examples"></a>示例  
 收集查询自有连接有关信息的典型查询。  
  
```sql  
SELECT   
    c.session_id, c.net_transport, c.encrypt_option,   
    c.auth_scheme, s.host_name, s.program_name,   
    s.client_interface_name, s.login_name, s.nt_domain,   
    s.nt_user_name, s.original_login_name, c.connect_time,   
    s.login_time   
FROM sys.dm_exec_connections AS c  
JOIN sys.dm_exec_sessions AS s  
    ON c.session_id = s.session_id  
WHERE c.session_id = @@SPID;  
```  
  
## <a name="see-also"></a>另请参阅  

 [与执行相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  



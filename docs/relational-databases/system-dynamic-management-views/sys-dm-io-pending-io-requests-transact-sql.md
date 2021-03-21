---
description: sys.dm_io_pending_io_requests (Transact-SQL)
title: sys.dm_io_pending_io_requests (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_io_pending_io_requests
- dm_io_pending_io_requests
- dm_io_pending_io_requests_TSQL
- sys.dm_io_pending_io_requests_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_io_pending_io_requests dynamic management view
ms.assetid: d1fb46dd-5c74-4c04-9ecf-8934b1bedb5b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: df9ba0491e64fb0dc60fbf8d8d7977b4f904b0ab
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750297"
---
# <a name="sysdm_io_pending_io_requests-transact-sql"></a>sys.dm_io_pending_io_requests (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中每个挂起的 I/O 请求返回一行。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_io_pending_io_requests**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**io_completion_request_address**|**varbinary(8)**|IO 请求的内存地址。 不可为 null。|  
|**io_type**|**nvarchar(60)**|挂起的 IO 请求的类型。 不可为 null。|  
|**io_pending_ms_ticks**|**bigint**|仅限内部使用。 不可为 null。| 
|**io_pending**|**int**|指示 IO 请求被挂起还是已由 Windows 完成。 即使在 Windows 已完成 I/O 请求但 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 尚未执行上下文切换（在其中处理 I/O 请求并将其从此列表中删除）时，I/O 请求仍可处于挂起状态。 不可为 null。|  
|**io_completion_routine_address**|**varbinary(8)**|I/O 请求完成时调用的内部函数。 可以为 Null。|  
|**io_user_data_address**|**varbinary(8)**|仅限内部使用。 可以为 Null。|  
|**scheduler_address**|**varbinary(8)**|发出此 I/O 请求的计划程序。 I/O 请求将显示于计划程序的挂起 I/O 列表中。 有关详细信息，请参阅 [&#40;transact-sql&#41;sys.dm_os_schedulers ](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md)。 不可为 null。|  
|**io_handle**|**varbinary(8)**|I/O 请求中所使用文件的文件句柄。 可以为 Null。|  
|**io_offset**|**bigint**|IO 请求的偏移量。 不可为 null。|  
|**io_handle_path**|**nvarchar(256)**| I/o 请求中使用的文件的路径。 可以为 Null。|
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限  

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标上，对于弹性池中的数据库， [服务器管理员](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) 帐户或 [Azure Active Directory 管理员](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) 帐户是必需的。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与 SQL&#41;相关的动态管理视图和函数 &#40;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)  
  

---
description: sys.dm_server_services (Transact-SQL)
title: sys.dm_server_services (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 01/07/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_server_services
- sys.dm_server_services
- sys.dm_server_services_TSQL
- dm_server_services_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_server_services dynamic management view
ms.assetid: 3f0defd0-478d-4e7f-96be-8795c9de4e3f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 65ba3ca60ceb6d1c4ba327ee191084fc63aca8d6
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170649"
---
# <a name="sysdm_server_services-transact-sql"></a>sys.dm_server_services (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回当前实例中的 SQL Server、全文 SQL Server Launchpad 服务 (SQL Server 2017 +) 和 SQL Server 代理服务的相关信息 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 使用此动态管理视图可以报告有关这些服务的状态信息。  
  
 
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|servicename|**nvarchar(256)**|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]、全文或 SQL Server 代理服务的名称。 不能为 null。|  
|startup_type|**int**|指示服务的启动模式。 下面是可能的值及其相应的说明。<br /><br /> 0：其他<br />1：其他<br />2：自动<br />3：手动<br />4：已禁用<br /><br /> 可以为 Null。|  
|startup_type_desc|**nvarchar(256)**|描述服务的启动模式。 下面是可能的值及其相应的说明。<br /><br /> 其他：其他 (启动启动) <br />其他：其他 (系统启动) <br />自动：自动启动<br />手动：请求开始<br />禁用：已禁用<br /><br /> 不能为 null。|  
|状态|**int**|指示服务的当前状态。 下面是可能的值及其相应的说明。<br /><br /> 1：已停止<br />2：其他 (开始挂起) <br />3：其他 (停止挂起) <br />4：正在运行<br />5：其他 (继续挂起) <br />6：其他 (暂停挂起) <br />7：已暂停<br /><br /> 可以为 Null。|  
|status_desc|**nvarchar(256)**|描述服务的当前状态。 下面是可能的值及其相应的说明。<br /><br /> 已停止：服务已停止。<br />其他 (启动操作挂起) ：服务正在启动。<br />其他 (停止操作挂起) ：服务正在停止。<br />正在运行：服务正在运行。<br />其他 (继续操作挂起) ：服务处于挂起状态。<br />其他 (暂停挂起的) ：服务正在暂停。<br />已暂停：服务已暂停。<br /><br /> 不能为 null。|  
|process_id|**int**|服务的进程 ID。 不能为 null。|  
|last_startup_time|**datetimeoffset(7)**|上次启动服务的日期和时间。 可以为 Null。|  
|service_account|**nvarchar(256)**|授权来控制服务的帐户。 此帐户可以启动或停止服务，或修改服务属性。 不能为 null。|  
|filename|**nvarchar(256)**|服务可执行文件的路径和文件名。 不能为 null。|  
|is_clustered|**nvarchar (1)**|指示服务是否作为群集服务器的资源安装。 不能为 null。|  
|cluster_nodename|**nvarchar(256)**|安装此服务的群集节点的名称。 可以为 Null。|
|instant_file_initialization_enabled|**nvarchar (1)**|指定是否为服务启用即时文件初始化 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 。<br /><br />Y = 为服务启用即时文件初始化。<br /><br />N = 为服务禁用即时文件初始化。<br /><br /> 可以为 Null。<br /><br /> **注意：** 不适用于其他服务，例如 SQL Server 代理。<br /><br /> **适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从 SP4 和 [!INCLUDE[sssql11](../../includes/sssql11-md.md)] [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 及更高) 版本开始 (。|  

## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求具有对服务器的 `VIEW SERVER STATE` 权限。  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_server_registry &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-registry-transact-sql.md)  
  

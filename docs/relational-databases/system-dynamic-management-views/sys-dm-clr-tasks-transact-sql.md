---
description: sys.dm_clr_tasks (Transact-SQL)
title: sys.dm_clr_tasks (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_clr_tasks
- sys.dm_clr_tasks_TSQL
- dm_clr_tasks
- dm_clr_tasks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_tasks dynamic management view
ms.assetid: 462b9061-09fa-4858-9707-03d6cc19c769
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c16eea33c1c0211b37823ecff1e2ae1131d2a335
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837604"
---
# <a name="sysdm_clr_tasks-transact-sql"></a>sys.dm_clr_tasks (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  对于当前正在运行的所有公共语言运行时 (CLR) 任务，相应地返回一行。 如果 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批中包含对 CLR 例程的引用，那么这个批将创建单独的任务，以执行该批中的所有托管代码。 如果批中的多个语句需要执行托管代码，那么这些批将使用相同的 CLR 任务。 CLR 任务负责维护执行托管代码所涉及的对象和状态，以及在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例和公共语言运行时之间的转换。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**task_address**|**varbinary(8)**|CLR 任务的地址。|  
|**sos_task_address**|**varbinary(8)**|基础 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批任务的地址。|  
|**appdomain_address**|**varbinary(8)**|此任务在其中运行的应用程序域的地址。|  
|State |**nvarchar(128)**|任务的当前状态。|  
|**abort_state**|**nvarchar(128)**|中止任务时当前所处的状态（如果取消任务）。中止任务时涉及多个状态。|  
|type|**nvarchar(128)**|任务类型。|  
|**affinity_count**|**int**|任务的关联。|  
|**forced_yield_count**|**int**|强制产生任务的次数。|  
  
## <a name="permissions"></a>权限  

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标上，对于弹性池中的数据库， [服务器管理员](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) 帐户或 [Azure Active Directory 管理员](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) 帐户是必需的。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与公共语言运行时相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  

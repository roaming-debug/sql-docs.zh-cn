---
description: sys.dm_external_script_requests
title: sys.dm_external_script_requests |Microsoft Docs
ms.custom: ''
ms.date: 06/26/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: language-reference
f1_keywords:
- sys.dm_external_script_requests
- sys.dm_external_script_requests_TSQL
- dm_external_script_requests
- dm_external_script_requests_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_external_script_requests dynamic management view
ms.assetid: e7e7c50f-b8b2-403c-b8c8-1955da5636c3
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 972e9527db2eaa546210ccc54528d0ce60197b4c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482678"
---
# <a name="sysdm_external_script_requests"></a>sys.dm_external_script_requests
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

为运行外部脚本的每个活动工作线程帐户都返回一行。
  
> [!NOTE]
> 仅当你已安装并启用支持外部脚本执行的功能时，此动态管理视图 (DMV) 才可用。 有关详细信息，请参阅 [SQL Server 2017 和更高版本中的机器学习服务 (r、Python) ](../../machine-learning/sql-server-machine-learning-services.md)、 [SQL Server 2016 中的 r SERVICES](../../machine-learning/r/sql-server-r-services.md)和 [Azure SQL 机器学习服务中的托管实例](/azure/azure-sql/managed-instance/machine-learning-services-overview)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|external_script_request_id|**唯一标识符**|发送外部脚本请求的进程的 ID。 这对应于接收到的 SQL 实例的进程 ID。|  
|语言|**nvarchar**|表示支持的脚本语言的关键字。 |  
|degree_of_parallelism|**int**|数字，指示已创建的并行进程数。 此值可能与请求的并行进程数不同。|  
|external_user_name|**nvarchar**|在其下执行脚本的 Windows 工作线程帐户。|  
  
## <a name="permissions"></a>权限

 需要 `VIEW SERVER STATE` 对服务器的权限。  
  
> [!NOTE]
> 运行外部脚本的用户必须具有其他权限 `EXECUTE ANY EXTERNAL SCRIPT` ，但是，管理员可以使用此 DMV 而无需此权限。 
  
## <a name="remarks"></a>备注  

此视图可以使用脚本语言标识符进行筛选。

此视图还返回在其下运行脚本的工作线程帐户。 有关外部脚本使用的辅助角色帐户的信息，请参阅 [SQL Server 机器学习服务中的扩展性框架的 "安全概述](../../machine-learning/concepts/security.md#sqlrusergroup)" 中处理 (SQLRUserGroup) 部分。

在 **external_script_request_id** 字段中返回的 GUID 还表示用于存储临时文件的受保护目录的文件名。 每个辅助角色帐户（如 MSSQLSERVER01）代表一个 SQL 登录名或 Windows 用户，并可用于运行多个脚本请求。 默认情况下，这些临时文件会在请求脚本完成之后进行清理。

此 DMV 仅监视活动进程，不能报告已完成的脚本。 如果需要跟踪脚本的持续时间，我们建议将计时信息添加到脚本并在脚本执行过程中进行捕获。

## <a name="examples"></a>示例  
  
### <a name="viewing-the-currently-active-scripts-for-a-particular-process"></a>查看特定进程的当前活动脚本

 下面的示例显示在当前实例上运行的外部脚本执行数。  
  
```sql
SELECT external_script_request_id
  , [language]
  , degree_of_parallelism
  , external_user_name
FROM sys.dm_external_script_requests;
```  

结果  

external_script_request_id  |语言  |degree_of_parallelism  |external_user_name  
---------|---------|---------|---------
183EE6FC-7399-4318-AA2E-7A6C68E435A8     |     R    |      1   |  MSSQLSERVER01

## <a name="see-also"></a>请参阅

+ [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
+ [与执行相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
+ [sys.dm_external_script_execution_stats](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md)
+ [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)  

---
description: sys.dm_external_script_execution_stats
title: sys.dm_external_script_execution_stats |Microsoft Docs
ms.custom: ''
ms.date: 06/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: language-reference
f1_keywords:
- sys.dm_external_script_execution_stats
- sys.dm_external_script_execution_stats_TSQL
- dm_external_script_execution_stats
- dm_external_script_execution_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_external_script_execution_stats dynamic management view
ms.assetid: 2e99f026-ceb2-42a2-a549-c71d31ed0cf4
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 32a35224dd40cfc33bdc3c8bb0c93fd5a783966a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482688"
---
# <a name="sysdm_external_script_execution_stats"></a>sys.dm_external_script_execution_stats
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

为每种类型的外部脚本请求返回一行。 外部脚本请求由受支持的外部脚本语言分组。 为每个已注册的外部脚本函数生成一行。 除非是由父进程（如 `rxExec`）发送，否则不会记录任意外部脚本函数。
  
> [!NOTE]  
> 仅当你已安装并启用支持外部脚本执行的功能时，此动态管理视图 (DMV) 才可用。 有关详细信息，请参阅 [SQL Server 2016 中的 R Services](../../machine-learning/r/sql-server-r-services.md)、 [机器学习服务 (R、Python) 在 SQL Server 2017 和更高版本中](../../machine-learning/sql-server-machine-learning-services.md) 以及 [Azure 托管实例机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)。
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|语言|**nvarchar**|已注册的外部脚本语言的名称。 每个外部脚本必须在脚本请求中指定语言以启动关联的启动器。 |  
|counter_name|**nvarchar**|已注册的外部脚本函数的名称。 不可为 null。|  
|counter_value|**integer**|已在服务器上调用已注册外部脚本函数的实例的总数。 此值是累计的（从在实例上安装该功能的时间开始），且不能重置。|  

## <a name="permissions"></a>权限

 需要针对服务器的 VIEW SERVER STATE 权限。  
  
> [!NOTE]  
> 运行外部脚本的用户必须具有额外权限 EXECUTE ANY EXTERNAL SCRIPT，但是，此 DMV 可由没有此权限的管理员使用。
  
## <a name="remarks"></a>备注

  此 DMV 为内部遥测提供，以监视 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]中提供的新外部脚本执行功能的总体使用情况。 启动 LaunchPad 时会启动遥测服务，每次调用已注册的外部脚本函数时会递增基于磁盘的计数器。

通常情况下，只要生成性能计数器的进程处于活动状态，它们便有效。 因此，对 DMV 的查询不能显示已停止运行的服务的详细数据。 例如，如果启动程序执行外部脚本并很快完成它们，则常规 DMV 可能不会显示任何数据。

因此，即使实例已关闭，此 DMV 所跟踪的计数器仍会保持运行，并且通过使用写入到磁盘保留 sys.dm_external_script_requests 的状态。

### <a name="counter-values"></a>计数器值

在 SQL Server 2016 中，唯一受支持的外部语言是 R，外部脚本请求由进行处理 [!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)] 。 在 SQL Server 2017 及更高版本以及 Azure SQL 托管实例上，R 和 Python 都是支持的外部语言，且外部脚本请求由处理 [!INCLUDE[rsql_productname_md](../../includes/rsql-productnamenew-md.md)] 。

对于 R，此 DMV 跟踪对实例发出的 R 调用数。 例如，如果调用且并行运行了 `rxLinMod` ，该计数器会增大 1。

对于 R 语言， *counter_name* 字段中显示的计数器值表示已注册的 ScaleR 函数的名称。 *counter_value* 字段中的值表示特定 ScaleR 函数实例的累计数。 

对于 Python，此 DMV 跟踪对实例执行的 Python 调用的数目。

在实例上安装并启用该功能时，计数开始；计数会一直累积，直到删除维护状态的文件或管理员将该文件覆盖。 因此，通常不可能重置 *counter_value* 中的值。 如要希望根据会话、日历时间和其他间隔来监视使用情况，我们建议将计数捕获到表中。

### <a name="registration-of-external-script-functions-in-r"></a>在 R 中注册外部脚本函数

R 支持任意脚本，而 R 社区提供了数千个包，每个包都具有自己的函数和方法。 但是，此 DMV 仅监视 SQL Server 2016 R 服务安装的 ScaleR 函数。

安装该功能时将执行这些函数的注册，而且不能添加或删除已注册的函数。

## <a name="examples"></a>示例  
  
### <a name="viewing-the-number-of-r-scripts-run-on-the-server"></a>查看服务器上运行的 R 脚本的数量

 下面的示例显示 R 语言的外部脚本执行的累计数。  
  
```sql
SELECT counter_name, counter_value
FROM sys.dm_external_script_execution_stats
WHERE language = 'R';
```  

### <a name="viewing-the-number-of-python-scripts-run-on-the-server"></a>查看服务器上运行的 Python 脚本的数量

下面的示例显示 Python 语言的外部脚本执行的累计数量。  
  
```sql
SELECT counter_name, counter_value
FROM sys.dm_external_script_execution_stats
WHERE language = 'Python';
```  

## <a name="see-also"></a>请参阅

+ [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
+ [与执行相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)
+ [sys.dm_external_script_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md)  
+ [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)  

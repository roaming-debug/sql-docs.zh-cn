---
description: catalog.execution_component_phases
title: catalog.execution_component_phases | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 07a9a163-4787-40f7-b371-ac5c6cb4b095
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1d3d9c223a4d415b9bfd28ed4c202570141c1f1f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347373"
---
# <a name="catalogexecution_component_phases"></a>catalog.execution_component_phases 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  显示数据流组件在每个执行阶段中所花的时间。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|phase_stats_id|**bigint**|阶段的唯一标识符 (ID)。|  
|execution_id|**bigint**|执行实例的唯一 ID。|  
|package_name|**nvarchar(260)**|在执行过程中启动的第一个包的名称。|  
|task_name|**nvarchar(4000)**|数据流任务的名称。|  
|subcomponent_name|**nvarchar(4000)**|数据流组件的名称。|  
|phase|**nvarchar(128)**|执行阶段的名称。|  
|start_time|**datetimeoffset(7)**|阶段开始的时间。|  
|end_time|**datetimeoffset(7)**|阶段结束的时间。|  
|execution_path|**nvarchar(max)**|数据流任务的执行路径。|  
  
## <a name="remarks"></a>备注  
 此视图显示数据流组件的每个执行阶段（如 Validate、Pre-Execute、Post-Execute、PrimeOutput 和 ProcessInput）对应的行。 每行显示特定执行阶段的开始时间和结束时间。  
  
## <a name="example"></a>示例  
 下面的示例使用 catalog.execution_component_phases 视图查看特定包在所有阶段花在执行上的总时间 (**active_time**) 以及包的总运行时间 (**total_time**)。  
  
> [!WARNING]  
>  如果包执行的日志记录级别设置为“性能”或“详细”，则 catalog.execution_component_phases 视图将提供此信息。 有关详细信息，请参阅 [在 SSIS 服务器上启用包执行的日志记录](../../integration-services/performance/integration-services-ssis-logging.md#server_logging)。  
  
```sql
use SSISDB  
select package_name, task_name, subcomponent_name, execution_path,  
    SUM(DATEDIFF(ms,start_time,end_time)) as active_time,  
    DATEDIFF(ms,min(start_time), max(end_time)) as total_time  
from catalog.execution_component_phases  
where execution_id = 1841  
group by package_name, task_name, subcomponent_name, execution_path  
order by package_name, task_name, subcomponent_name, execution_path  
```  
  
## <a name="permissions"></a>权限  
 此视图需要下列权限之一：  
  
-   针对执行实例的 READ 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
> [!NOTE]  
>  当您具有在服务器上执行操作的权限时，您还具有查看有关此操作的信息的权限。 将实施行级安全性；只显示您有权查看的行。  
  
  

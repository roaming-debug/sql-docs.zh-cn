---
description: sys.dm_exec_cached_plan_dependent_objects (Transact-SQL)
title: sys.dm_exec_cached_plan_dependent_objects (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_cached_plan_dependent_objects
- dm_exec_cached_plan_dependent_objects_TSQL
- sys.dm_exec_cached_plan_dependent_objects_TSQL
- dm_exec_cached_plan_dependent_objects
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_cached_plan_dependent_objects dynamic management function
ms.assetid: 9b6cf5f7-b267-44fb-aac8-f49c9aa10cc1
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 29b533890f22891d8bd7e4cfe7a0323507874753
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464608"
---
# <a name="sysdm_exec_cached_plan_dependent_objects-transact-sql"></a>sys.dm_exec_cached_plan_dependent_objects (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  针对每个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 执行计划、公共语言运行时 (CLR) 执行计划和与计划关联的游标返回一行。  
  
## <a name="syntax"></a>语法  
  
```  
sys.dm_exec_cached_plan_dependent_objects(plan_handle)  
```  
  
## <a name="arguments"></a>自变量  
*plan_handle*  
是一个标记，用于唯一标识已执行并且其计划驻留在计划缓存中的批处理的查询执行计划。 *plan_handle* 为 **varbinary (64)**。   

可以从以下动态管理对象中获取 *plan_handle* ：  
  
-   [sys.dm_exec_cached_plans (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
-   [sys.dm_exec_query_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  

-   [sys.dm_exec_procedure_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  

-   [sys.dm_exec_trigger_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**usecounts**|**int**|执行上下文或游标的已用次数。<br /><br /> 此列不可为空值。|  
|**memory_object_address**|**varbinary(8)**|执行上下文或游标的内存地址。<br /><br /> 此列不可为空值。|  
|**cacheobjtype**|**nvarchar(50)**|计划缓存对象类型。 此列不可为空值。 可能的值为<br /><br /> 可执行计划<br /><br /> CLR 编写函数<br /><br /> CLR 编写过程<br /><br /> 游标|  
  
## <a name="permissions"></a>权限  
 要求具有对服务器的 `VIEW SERVER STATE` 权限。  
  
## <a name="physical-joins"></a>物理联接  
 ![关系图](../../relational-databases/system-dynamic-management-views/media/dm-dependent-objects.gif "关系图")  
  
## <a name="relationship-cardinalities"></a>关系基数  
  
|From|功能|On|关系|  
|----------|--------|--------|------------------|  
|**dm_exec_cached_plan_dependent_objects**|**dm_os_memory_objects**|**memory_object_address**|一对一|  
  
## <a name="see-also"></a>另请参阅  
 [与执行相关的动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.syscacheobjects &#40;Transact-sql&#41;](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md)  
  
  

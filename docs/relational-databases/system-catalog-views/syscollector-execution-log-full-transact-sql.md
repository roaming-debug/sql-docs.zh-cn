---
description: syscollector_execution_log_full (Transact-SQL)
title: syscollector_execution_log_full (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syscollector_execution_log_full
- syscollector_execution_log_full_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- data collector view
- syscollector_execution_log_full view
ms.assetid: 6c8db22d-2e4c-4b7c-ac5a-8762ef1b175b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bb65b192b63ef75247deb716115bb0b4cba13ab2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199751"
---
# <a name="syscollector_execution_log_full-transact-sql"></a>syscollector_execution_log_full (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  提供有关在执行日志已满时收集组或包的信息。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|log_id|**bigint**|标识收集组的每次执行。 用于将此视图与其他详细日志联接起来。 可以为 Null。|  
|parent_log_id|**bigint**|标识父包或父收集组。 不可为 null。 各个 ID 以父子关系链接在一起，这样，您便可以确定哪个包是由哪个收集组启动的。 此视图将日志项按其父子链接进行分组并缩进包的名称，从而使调用链清晰可见。|  
|name|**nvarchar(4000)**|此日志项所表示的收集组或包的名称。 可以为 Null。|  
|status|**smallint**|指示收集组或包的当前状态。 可以为 Null。<br /><br /> 值为：<br /><br /> 0 = 正在运行<br /><br /> 1 = 完成<br /><br /> 2 = 失败|  
|runtime_execution_mode|**smallint**|指示收集组活动是收集数据还是上载数据。 可以为 Null。|  
|start_time|**datetime**|收集组或包的开始时间。 可以为 Null。|  
|last_iteration_time|**datetime**|对于连续运行的包而言，是包上次捕获快照的时间。 可以为 Null。|  
|finish_time|**datetime**|已完成的包和收集组完成运行的时间。 可以为 Null。|  
|duration|**int**|包或收集组已经运行的时间（以秒为单位）。 可以为 Null。|  
|failure_message|**nvarchar(2048)**|收集组或包失败时该组件的最新错误消息。 可以为 Null。 若要获取更详细的错误信息，请使用 [fn_syscollector_get_execution_details &#40;transact-sql&#41;](../../relational-databases/system-functions/fn-syscollector-get-execution-details-transact-sql.md) 函数。|  
|operator|**nvarchar(128)**|标识启动了收集组或包的用户。 可以为 Null。|  
|package_execution_id|**uniqueidentifier**|提供指向 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 日志表的链接。 可以为 Null。|  
|collection_set_id|**int**|提供指向 msdb 中数据收集配置表的链接。 可以为 Null。|  
  
## <a name="permissions"></a>权限  
 需要 **dc_operator** 的 SELECT。  
  
## <a name="see-also"></a>另请参阅  
 [数据收集器存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [数据收集器视图 (Transact-SQL)](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)   
 [“数据收集”](../../relational-databases/data-collection/data-collection.md)  
  
  

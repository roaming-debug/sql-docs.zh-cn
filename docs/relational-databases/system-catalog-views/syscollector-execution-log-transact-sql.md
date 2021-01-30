---
description: syscollector_execution_log (Transact-SQL)
title: syscollector_execution_log (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syscollector_execution_log_TSQL
- syscollector_execution_log
dev_langs:
- TSQL
helpviewer_keywords:
- data collector view
- syscollector_execution_log view
ms.assetid: 11554d64-0426-42ce-b7ce-5591f67864d2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1920cc669307230bd29acabd04e9ade88147261b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199741"
---
# <a name="syscollector_execution_log-transact-sql"></a>syscollector_execution_log (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  提供收集组或包的执行日志中的信息。   
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|log_id|**bigint**|标识收集组的每次执行。 用于将此视图与其他详细日志联接起来。 不可为 null。|  
|parent_log_id|**bigint**|标识父包或父收集组。 不可为 null。 各个 ID 以父子关系链接在一起，这样，您便可以确定哪个包是由哪个收集组启动的。 此视图将日志项按其父子链接进行分组并缩进包的名称，因而调用链清晰可见。|  
|collection_set_id|**int**|标识此日志项表示的收集组或包。 不可为 null。|  
|collection_item_id|**int**|标识收集项。 可以为 Null。|  
|start_time|**datetime**|收集组或包的开始时间。 不可为 null。|  
|last_iteration_time|**datetime**|对于连续运行的包而言，是包上次捕获快照的时间。 可以为 Null。|  
|finish_time|**datetime**|已完成的包和收集组完成运行的时间。 可以为 Null。|  
|runtime_execution_mode|**smallint**|指示收集组活动是收集数据还是上载数据。 可以为 Null。<br /><br /> 值为：<br /><br /> 0 = 收集<br /><br /> 1 = 上载|  
|status|**smallint**|指示收集组或包的当前状态。 不可为 null。<br /><br /> 值为：<br /><br /> 0 = 正在运行<br /><br /> 1 = 完成<br /><br /> 2 = 失败|  
|operator|**nvarchar(128)**|标识启动了收集组或包的用户。 不可为 null。|  
|package_id|**uniqueidentifier**|标识生成此日志的收集组或包。 可以为 Null。|  
|package_name|**nvarchar(4000)**|生成此日志的包的名称。 可以为 Null。|  
|package_execution_id|**uniqueidentifier**|提供指向 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 日志表的链接。 可以为 Null。|  
|failure_message|**nvarchar(2048)**|收集组或包失败时该组件的最新错误消息。 可以为 Null。 若要获取更详细的错误信息，请使用 [fn_syscollector_get_execution_details &#40;transact-sql&#41;](../../relational-databases/system-functions/fn-syscollector-get-execution-details-transact-sql.md) 函数。|  
  
## <a name="permissions"></a>权限  
 需要拥有 dc_operator 的 SELECT 权限。  
  
## <a name="see-also"></a>另请参阅  
 [数据收集器存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [数据收集器视图 (Transact-SQL)](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)   
 [“数据收集”](../../relational-databases/data-collection/data-collection.md)  
  
  

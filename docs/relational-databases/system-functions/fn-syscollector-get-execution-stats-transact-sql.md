---
description: fn_syscollector_get_execution_stats (Transact-SQL)
title: fn_syscollector_get_execution_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_syscollector_get_execution_stats
- fn_syscollector_get_execution_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_syscollector_get_execution_stats function
ms.assetid: 793ad72c-a992-4a8d-8584-bcb6b3b476f1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 47a90958d7de27e087df3768949a5defa148c8f4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097481"
---
# <a name="fn_syscollector_get_execution_stats-transact-sql"></a>fn_syscollector_get_execution_stats (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关收集组或包的详细统计信息，包括包数据流任务记录的错误行数。 数据流任务是处理数据的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 组件。 该数据采用关系格式，因此它具有由行组成的输入和输出数据集。  
  
 统计信息是对 syscollector_execution_stats 视图中的条目进行计算而得出的。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
fn_syscollector_get_execution_stats ( log_id )  
```  
  
## <a name="arguments"></a>参数  
 *log_id*  
 执行日志的本地唯一标识符。 *log_id* 是 **int**。  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|avg_row_count_in|**int**|进入包的数据流任务的平均行数。<br /><br /> 注意：数据流任务是 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 处理数据的组件。 该数据采用关系格式，因此它具有由行组成的输入数据集。 这是进入该任务的行数。 数据在转换后成为由行组成的结果集输出。 数据流任务将转换数据并输出由行组成的结果集。 该输出是退出任务的行数。|  
|min_row_count_in|**int**|进入包的数据流任务的最少行数。|  
|max_row_count_in|**int**|进入包的数据流任务的最大行数。|  
|avg_row_count_out|**int**|退出包的数据流任务的平均行数。|  
|min_row_count_out|**int**|退出包的数据流任务的最小行数。|  
|max_row_count_out|**int**|退出包的数据流任务的最大行数。|  
|avg_duration|**int**|在包的数据流组件中消耗的平均时间，以毫秒为单位。|  
|min_duration|**int**|在包的数据流组件中消耗的最短时间，以毫秒为单位。|  
|max_duration|**int**|在包的数据流组件中消耗的最长时间，以毫秒为单位。|  
  
## <a name="permissions"></a>权限  
 需要 **dc_operator** 的 SELECT。  
  
## <a name="see-also"></a>另请参阅  
 [syscollector_execution_stats &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/syscollector-execution-stats-transact-sql.md)   
 [“数据收集”](../../relational-databases/data-collection/data-collection.md)  
  
  

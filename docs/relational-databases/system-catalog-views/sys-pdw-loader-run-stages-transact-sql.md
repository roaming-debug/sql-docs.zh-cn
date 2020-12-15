---
description: 'sys.pdw_loader_run_stages (Transact-sql) '
title: sys.pdw_loader_run_stages (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 255681e9-323c-42c0-a63c-1f05536efdd5
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: ee8eb27964a7fc26d5af6c8f7a13293720ca7821
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475148"
---
# <a name="syspdw_loader_run_stages-transact-sql"></a>sys.pdw_loader_run_stages (Transact-sql) 
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  包含有关中正在进行的和已完成的加载操作的信息 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 。 信息在两次系统重启之间仍会保留。  
  
| 列名 | 数据类型 | 说明 | 范围 |
| ----------- | --------- | ----------- | ----- |
|run_id|**int**|运行时加载程序的唯一标识符。||  
|阶段 (stage)|**nvarchar(30)**|运行的当前阶段。|"CREATE_STAGING"、"DMS_LOAD"、"LOAD_INSERT"、"LOAD_CLEANUP"|  
|request_id|**nvarchar(32)**|运行此阶段的请求的 ID。||  
|status|**nvarchar (16)**|此阶段的状态。||  
|start_time|**datetime**|阶段开始的时间。||  
|end_time|**datetime**|阶段结束的时间（如果有）。|如果未开始或正在进行，则为 NULL。|  
|total_elapsed_time|**int**|此阶段所用的总时间 () 运行。|如果 total_elapsed_time 超过24.8 天（以毫秒为单位） (整数的最大值) ，则会导致具体化失败，因为溢出。<br /><br /> 最大值（以毫秒为单位）等效于24.8 天。|  
  
## <a name="see-also"></a>另请参阅  
 [Azure Synapse Analytics 和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  

---
description: 'sys.pdw_diag_events (Transact-sql) '
title: sys.pdw_diag_events (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 59bb3e9c-2829-49a0-b382-652ed1f54f88
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 68cdc1013969dde85bbdc0bded31610a1673b91d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461608"
---
# <a name="syspdw_diag_events-transact-sql"></a>sys.pdw_diag_events (Transact-sql) 
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  保存有关可以包含在系统上的诊断会话中的事件的信息。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|**name**|**nvarchar(255)**|特定诊断事件的名称。||  
|**source**|**nvarchar(255)**|事件源 (引擎、常规、dms 等 ) ||  
|**is_enabled**|**bit**|事件是否正在发布。||  
  
## <a name="see-also"></a>另请参阅  
 [Azure Synapse Analytics 和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  

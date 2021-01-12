---
description: sys.trace_columns (Transact-SQL)
title: sys.trace_columns (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.trace_columns
- trace_columns
- trace_columns_TSQL
- sys.trace_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.trace_columns catalog view
ms.assetid: 5c48eb09-9e9b-45dd-b151-ca39b026ece5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 740f553e4809a5eb971bb7bfe7e97b5b628dfebd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094380"
---
# <a name="systrace_columns-transact-sql"></a>sys.trace_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **Sys.trace_columns** 目录视图包含所有跟踪事件列的列表。 在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的给定版本中，这些列不变。  
  
 有关支持的跟踪事件的完整列表，请参阅 [SQL Server 事件类引用](../../relational-databases/event-classes/sql-server-event-class-reference.md)。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用扩展事件目录视图。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**trace_column_id**|**smallint**|该列的唯一 ID。|  
|name|**nvarchar(128)**|该列的唯一名称。 此参数未本地化。|  
|type_name|**nvarchar(128)**|该列的数据类型名称。|  
|**max_size**|**int**|该列数据的最大大小，以字节表示。|  
|**is_filterable**|**bit**|指示是否可在筛选器说明中使用该列。<br /><br /> 0 = false<br /><br /> 1 = true|  
|**is_repeatable**|**bit**|指示是否可以在“重复列”数据中引用该列。<br /><br /> 0 = false<br /><br /> 1 = true|  
|**is_repeated_base**|**bit**|指示在引用重复数据时是否将该列作为唯一键使用。<br /><br /> 0 = false<br /><br /> 1 = true|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [sys.databases &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-traces-transact-sql.md)   
 [sys.trace_categories &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-trace-categories-transact-sql.md)   
 [sys.trace_events &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-trace-events-transact-sql.md)   
 [sys.trace_event_bindings &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-trace-event-bindings-transact-sql.md)   
 [sys.trace_subclass_values &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-trace-subclass-values-transact-sql.md)  
  
  

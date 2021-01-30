---
description: cdc.ddl_history (Transact-SQL)
title: cdc.ddl_history (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- cdc.ddl_history_TSQL
- cdc.ddl_history
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.ddl_history
ms.assetid: cb97ea71-da2f-441a-bbd2-db1f5f48ab49
author: cawrites
ms.author: chadam
ms.openlocfilehash: 579e6752527118b579b9bb1dc0975f5a0e9b3495
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183322"
---
# <a name="cdcddl_history-transact-sql"></a>cdc.ddl_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为对启用了变更数据捕获的表所做的每一项数据定义语言 (DDL) 更改返回一行。 可以使用此表来确定源表发生 DDL 更改的时间以及更改的内容。 此表中不包含未发生 DDL 更改的源表的任何条目。  
  
 我们建议您不要直接查询系统表， 请改为执行 [sys.sp_cdc_get_ddl_history](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-ddl-history-transact-sql.md) 存储过程。  
   
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**source_object_id**|**int**|应用 DDL 更改的源表的 ID。|  
|object_id|**int**|与源表的捕获实例相关联的更改表的 ID。|  
|**required_column_update**|**bit**|指示在源表中修改了捕获列的数据类型。 此修改改变了更改表中的列。|  
|**ddl_command**|**nvarchar(max)**|应用于源表的 DDL 语句。|  
|**ddl_lsn**|**binary(10)**|与 DDL 修改的提交相关联的日志序列号 (LSN)。|  
|**ddl_time**|**datetime**|对源表所做的 DDL 更改的日期和时间。|  
  
## <a name="see-also"></a>另请参阅  
 [sys.sp_cdc_help_change_data_capture &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)  
  
  

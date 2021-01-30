---
description: cdc.change_tables (Transact-SQL)
title: cdc.change_tables (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- cdc.change_tables
- cdc.change_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.change_tables
ms.assetid: 3525a5f5-8d8b-46a8-b334-4b7cd9fb7c21
author: cawrites
ms.author: chadam
ms.openlocfilehash: 653e1b9b7fa37e510b4ce2c50d7bc9e2e0fff78a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183355"
---
# <a name="cdcchange_tables-transact-sql"></a>cdc.change_tables (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为数据库中的每个更改表返回一行。 对源表启用变更数据捕获时，将创建一个更改表。 我们建议您不要直接查询系统表， 请改为执行 [sys.sp_cdc_help_change_data_capture](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md) 存储过程。  

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|更改表的 ID。 在数据库中是唯一的。|  
|**version**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 对于 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]，此列始终返回 0。|  
|**source_object_id**|**int**|为变更数据捕获启用的源表的 ID。|  
|**capture_instance**|**sysname**|用于命名特定于实例的跟踪对象的捕获实例的名称。 默认情况下，该名称从源架构名称加上源表名称派生，格式 *schemaname_sourcename*。|  
|**start_lsn**|**binary(10)**|日志序列号 (LSN)，表示查询更改表中的更改数据时的低端点。<br /><br /> NULL = 尚未建立低端点。|  
|**end_lsn**|**binary(10)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 对于 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]，此列始终返回 NULL。|  
|**supports_net_changes**|**bit**|对更改表启用了查询净更改支持。|  
|**has_drop_pending**|**bit**|捕获进程收到关于源表已被删除的通知。|  
|role_name|**sysname**|用于访问更改数据的数据库角色的名称。<br /><br /> NULL = 未使用角色。|  
|index_name|**sysname**|用于唯一标识源表中的行的索引名称。 **index_name** 为源表的主键索引的名称，或者在对源表启用了变更数据捕获时指定的唯一索引的名称。<br /><br /> NULL = 在变更数据捕获启用时，源表无主键，且未指定唯一索引。<br /><br /> 注意：如果对具有主键的表启用了变更数据捕获，则不管是否启用了净更改，"变更数据捕获" 功能都将使用索引。 启用变更数据捕获之后，将不允许对主键进行修改。 如果该表没有主键，则仍可以启用变更数据捕获，但是只能将净更改设置为 False。 启用变更数据捕获之后，即可以创建主键。 由于变更数据捕获功能不使用主键，因此还可以修改主键。|  
|filegroup_name|**sysname**|更改表所驻留的文件组的名称。<br /><br /> NULL = 更改表在数据库的默认文件组中。|  
|create_date|**datetime**|启用源表的日期。|  
|**partition_switch**|**bit**|指示是否可以对启用了变更数据捕获的表执行 **ALTER TABLE** 的 **SWITCH PARTITION** 命令。 0 指示分区切换被阻止。 未分区表始终返回 1。|  
  
## <a name="see-also"></a>另请参阅  
 [sys.sp_cdc_help_change_data_capture &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)  
  
  

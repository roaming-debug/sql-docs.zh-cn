---
description: syscollector_collection_sets (Transact-SQL)
title: syscollector_collection_sets (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syscollector_collection_sets_TSQL
- syscollector_collection_sets
dev_langs:
- TSQL
helpviewer_keywords:
- data collector view
- syscollector_collection_sets view
ms.assetid: db0def92-f25b-45da-9709-eab972b33800
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ffa91193b3c09f64d608c255bb8960b7dfb85823
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199777"
---
# <a name="syscollector_collection_sets-transact-sql"></a>syscollector_collection_sets (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  提供有关收集组的信息，包括计划、收集模式及其状态。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|collection_set_id|**int**|收集组的本地标识符。 不可为 null。|  
|collection_set_uid|**uniqueidentifier**|收集组的全局唯一标识符。 不可为 null。|  
|name|**nvarchar(4000)**|收集组的名称。 可以为 Null。|  
|目标|**nvarchar(max)**|标识收集组的目标。 可以为 Null。|  
|is_system|**bit**|打开 (1) 或关闭 (0) 时可指示收集组是随数据收集器一起提供的还是由 dc_admin 在后来添加的。 这可以是内部开发的或由第三方开发的自定义收集组。 不可为 null。|  
|is_running|**bit**|指示收集组是否正在运行。 不可为 null。|  
|collection_mode|**smallint**|指定收集组的收集模式。 不可为 null。<br /><br /> 收集模式可为以下模式之一：<br /><br /> 0 - 缓存模式。 数据收集和上载分别位于各自的计划中。<br /><br /> 1 - 非缓存模式。 数据收集和上载处于同一个计划中。|  
|proxy_id|**int**|标识用于运行收集组作业步骤的代理。 可以为 Null。|  
|schedule_uid|**uniqueidentifier**|提供指向收集组计划的指针。 可以为 Null。|  
|collection_job_id|**uniqueidentifier**|标识收集作业。 可以为 Null。|  
|upload_job_id|**uniqueidentifier**|标识收集上载作业。 可以为 Null。|  
|logging_level|**smallint**|指定日志记录级别（0、1 或 2）。 不可为 null。|  
|days_until_expiration|**smallint**|收集的数据保存在管理数据仓库中的天数。 不可为 null。|  
|description|**nvarchar(4000)**|描述收集组。 可以为 Null。|  
|dump_on_any_error|**bit**|打开 (1) 或关闭 (0) ，以指示是否 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 在出现任何错误时创建转储文件。 不可为 null。|  
|dump_on_codes|**nvarchar(max)**|包含用于触发转储文件的 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 错误代码的列表。 可以为 Null。|  
  
## <a name="permissions"></a>权限  
 要求拥有 dc_operator 和 dc_proxy 的 SELECT 权限。  
  
## <a name="remarks"></a>备注  
 数据收集器 API 只允许您更改或删除您所创建的收集组。 不能修改或删除随系统提供的收集组。 不过，您仍可以启用或禁用系统收集组，还可以更改其配置。  
  
## <a name="see-also"></a>另请参阅  
 [数据收集器存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [数据收集器视图 (Transact-SQL)](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)   
 [“数据收集”](../../relational-databases/data-collection/data-collection.md)  
  
  

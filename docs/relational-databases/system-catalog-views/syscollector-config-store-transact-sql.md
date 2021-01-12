---
description: syscollector_config_store (Transact-SQL)
title: syscollector_config_store (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- syscollector_config_store_TSQL
- syscollector_config_store
dev_langs:
- TSQL
helpviewer_keywords:
- data collector view
- syscollector_config_store view
ms.assetid: f15f6b05-6808-4b76-b6a8-48dec844cf63
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c5914dfa56d4ee6dd360c391c64e636638bb4342
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094281"
---
# <a name="syscollector_config_store-transact-sql"></a>syscollector_config_store (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回应用于整个数据收集器（而非单个收集组实例）的属性。 此视图中的每一行均描述一个特定数据收集器属性，例如，管理数据仓库的名称和管理数据仓库所在的实例的名称。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|parameter_name|**nvarchar(128)**|属性的名称。 不可为 null。|  
|parameter_value|**sql_variant**|属性的实际值。 可以为 Null。|  
  
## <a name="permissions"></a>权限  
 要求视图的 SELECT 权限或者 dc_operator、dc_proxy 或 dc_admin 固定数据库角色的成员身份。  
  
## <a name="remarks"></a>备注  
 可用属性的列表是固定的，这些属性的值只能使用适当的存储过程来更改。 下表描述了通过此视图公开的属性。  
  
|属性名称|说明|  
|-------------------|-----------------|  
|CacheDirectory|文件系统中目录的名称，收集器类型包在该目录中存储临时信息。<br /><br /> NULL = 使用默认的临时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目录。|  
|CacheWindow|指示缓存目录保留上载失败数据的数据保留策略。<br /><br /> -1 = 保留所有上载失败的数据。<br /><br /> 0 = 不保留任何上载失败的数据。<br /><br /> *n* = 保留 *n* 个以前的上载失败的数据，其中 *n* >= 1。<br /><br /> 可使用 sp_syscollector_set_cache_window 存储过程更改此值。|  
|CollectorEnabled|指示数据收集器的状态。<br /><br /> 0 = 已禁用<br /><br /> 1 = 已启用<br /><br /> 可使用 sp_syscollector_enable_collector 或 sp_syscollector_disable_collector 存储过程更改此值。|  
|MDWDatabase|管理数据仓库的名称。 可使用 sp_syscollector_set_warehouse_database_name 存储过程更改此值。|  
|MDWInstance|管理数据仓库的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。 可使用 sp_syscollector_set_warehouse_instance_name 存储过程更改此值。|  
  
## <a name="examples"></a>示例  
 下面的示例查询 syscollector_config_store 视图。  
  
```sql  
SELECT parameter_name, parameter_value  
FROM msdb.dbo.syscollector_config_store;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据收集器存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [数据收集器视图 (Transact-SQL)](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)   
 [数据收集](../../relational-databases/data-collection/data-collection.md)   
 [sp_syscollector_enable_collector (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-syscollector-enable-collector-transact-sql.md)   
 [sp_syscollector_disable_collector (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-syscollector-disable-collector-transact-sql.md)   
 [sp_syscollector_set_warehouse_database_name &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-set-warehouse-database-name-transact-sql.md)   
 [sp_syscollector_set_warehouse_instance_name &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-set-warehouse-instance-name-transact-sql.md)   
 [sp_syscollector_set_cache_window (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-syscollector-set-cache-window-transact-sql.md)  
  
  

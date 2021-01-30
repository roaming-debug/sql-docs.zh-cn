---
description: sys.dm_os_buffer_pool_extension_configuration (Transact-SQL)
title: sys.dm_os_buffer_pool_extension_configuration (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 09/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_buffer_pool_extension_configuration
- sys.dm_os_buffer_pool_extension_configuration_TSQL
- dm_os_buffer_pool_extension_configuration_TSQL
- sys.dm_os_buffer_pool_extension_configuration
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_buffer_pool_extension_configuration dynamic management view
ms.assetid: d52cc481-4d29-4f33-b63d-231ec35d092f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 60e65c3dc79cccb0c3b01d7369e68cd6ff8efa53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184951"
---
# <a name="sysdm_os_buffer_pool_extension_configuration-transact-sql"></a>sys.dm_os_buffer_pool_extension_configuration (Transact-SQL)

[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中有关缓冲池扩展的配置信息。 对每个缓冲池扩展文件返回一行。  
  

  
| 列名称 | 数据类型 | 说明 |
| :---------- | :-------- | :---------- |
|path|**nvarchar** (256) |缓冲池扩展缓存的路径和文件名。 可以为 NULL。|  
|file_id|**int**|缓冲池扩展文件的 ID。 不可为 null。|  
|state|**int**|缓冲池扩展功能的状态。 不可为 null。<br /><br /> 0 - 已禁用缓冲池扩展<br /><br /> 1 - 正在禁用缓冲池扩展<br /><br /> 2-保留以供将来使用<br /><br /> 3 - 正在启用缓冲池扩展<br /><br /> 4 - 保留以供将来使用<br /><br /> 5 - 已启用缓冲池扩展|  
|state_description|**nvarchar** (60) |说明缓冲池扩展功能的状态。 可以为 Null。<br /><br /> 0 = BUFFER POOL EXTENSION DISABLED<br /><br /> 5 = 已启用缓冲池扩展|
|current_size_in_kb|**bigint**|缓冲池扩展文件的当前大小。 不可为 null。|
| &nbsp; | &nbsp; | &nbsp; |

## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-configuration-buffer-pool-extension-information"></a>A. 返回配置缓冲池扩展信息  
 下面的示例从 sys.dm_os_buffer_pool_extension_configruation DMV 返回所有列。  
  
```sql  
SELECT path, file_id, state, state_description, current_size_in_kb  
FROM sys.dm_os_buffer_pool_extension_configuration;  
```  
  
### <a name="b-returning-the-number-of-cached-pages-in-the-buffer-pool-extension-file"></a>B. 返回缓冲池扩展文件中的缓存页数。  
 下面的示例返回每个缓冲池扩展文件中的缓存页数。  
  
```sql  
SELECT COUNT(*) AS cached_pages_count  
FROM sys.dm_os_buffer_descriptors  
WHERE is_in_bpool_extension <> 0  
;  
```  
  
## <a name="see-also"></a>另请参阅  
 [缓冲池扩展](../../database-engine/configure-windows/buffer-pool-extension.md)   
 [sys.dm_os_buffer_descriptors (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-descriptors-transact-sql.md)  
  
  

---
description: sys.dm_os_loaded_modules (Transact-SQL)
title: sys.dm_os_loaded_modules (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_loaded_modules
- dm_os_loaded_modules
- sys.dm_os_loaded_modules_TSQL
- dm_os_loaded_modules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_loaded_modules dynamic management view
ms.assetid: 56c7743a-b568-4943-bd3b-73c57d9d641c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 77f4e2e69e73be88bd4cb70dcad7d7069eb8539d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094022"
---
# <a name="sysdm_os_loaded_modules-transact-sql"></a>sys.dm_os_loaded_modules (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  针对每个加载到服务器地址空间的模块返回一行。  
  
> [!NOTE]  
>  若要从调用此 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_os_loaded_modules**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**base_address**|**varbinary(8)**|进程中的模块的地址。|  
|**file_version**|**varchar (23)**|文件的版本。 按如下格式显示：<br /><br /> x.x:x.x|  
|**product_version**|**varchar (23)**|产品的版本。 按如下格式显示：<br /><br /> x.x:x.x|  
|**debug.exe**|**bit**|1 = 模块是调试版本的已加载模块。|  
|**patched**|**bit**|1 = 模块已得到修补。|  
|**早期**|**bit**|1 = 模块是预发行版本的已加载模块。|  
|**private_build**|**bit**|1 = 模块是专用版本的已加载模块。|  
|**special_build**|**bit**|1 = 模块是特殊版本的已加载模块。|  
|**language**|**int**|模块的版本信息语言。|  
|**上市公司**|**nvarchar(256)**|创建模块的公司的名称。|  
|description|**nvarchar(256)**|模块的说明。|  
|name|**nvarchar(255)**|模块的名称。 包括模块的完整路径。|  
|pdw_node_id|**int**|适用于：[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [&#40;Transact-sql 的与操作系统相关的动态管理视图 SQL Server&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  

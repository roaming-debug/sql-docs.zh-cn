---
description: sys.dm_io_backup_tapes (Transact-SQL)
title: sys.dm_io_backup_tapes (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_io_backup_tapes
- dm_io_backup_tapes_TSQL
- sys.dm_io_backup_tapes_TSQL
- dm_io_backup_tapes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_io_backup_tapes dynamic management view
ms.assetid: 2e27489e-cf69-4a89-9036-77723ac3de66
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bc4eb0de019ee40145c15c344850744d9fef3b15
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101577"
---
# <a name="sysdm_io_backup_tapes-transact-sql"></a>sys.dm_io_backup_tapes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回磁带设备的列表和用于备份的装入请求的状态。   
 
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**physical_device_name**|**nvarchar (520)**|可以执行备份的实际物理设备的名称。 不可为 null。|  
|**logical_device_name**|**nvarchar(256)**|用户指定的驱动器名称 (**sys.backup_devices**) 。 如果用户指定名称不可用，则为 NULL。 可以为 Null。|  
|**status**|**int**|磁带的状态：<br /><br /> 1 = 打开，可以使用<br /><br /> 2 = 装入挂起<br /><br /> 3 = 在使用中<br /><br /> 4 = 正在加载<br /><br /> **注意：** 如果正在加载磁带 (**状态 = 4**) ，则尚未读取介质标签。 复制媒体标签值的列（如 **media_sequence_number**）显示预期值，这些值可能与磁带上的实际值不同。 读取标签后， **状态** 将更改为 **3** (使用中) ，介质标签列将反映已加载的实际磁带。<br /><br /> 不可为 null。|  
|**status_desc**|**nvarchar (520)**|磁带状态的说明：<br /><br /> AVAILABLE<br /><br /> MOUNT PENDING<br /><br /> IN USE<br /><br /> LOADING MEDIA<br /><br /> 不可为 null。|  
|**mount_request_time**|**datetime**|装入的请求时间。 如果没有挂起的装载 (**status！ = 2**) ，则为 NULL。 可以为 Null。|  
|**mount_expiration_time**|**datetime**|装入请求的过期时间（超时）。 如果没有挂起的装载 (**status！ = 2**) ，则为 NULL。 可以为 Null。|  
|**database_name**|**nvarchar(256)**|要备份到该设备上的数据库。 可以为 Null。|  
|spid|**int**|会话 ID。 用于标识磁带的用户。 可以为 Null。|  
|**command**|**int**|执行备份的命令。 可以为 Null。|  
|**command_desc**|**nvarchar(120)**|命令的说明。 可以为 Null。|  
|**media_family_id**|**int**|媒体簇 (1 的索引 .。。*n*) 中， *n* 是介质集中的介质簇数。 可以为 Null。|  
|**media_set_name**|**nvarchar(256)**|介质集（如果有）的名称，它是创建介质集时由 MEDIANAME 选项指定的。 可以为 Null。|  
|**media_set_guid**|**uniqueidentifier**|用来唯一标识介质集的标识符。 可以为 Null。|  
|**media_sequence_number**|**int**|介质簇中的卷的索引 (1 .。。*n*) 。 可以为 Null。|  
|**tape_operation**|**int**|正在执行的磁带操作：<br /><br /> 1 = 读取<br /><br /> 2 = 格式化<br /><br /> 3 = 初始化<br /><br /> 4 = 追加<br /><br /> 可以为 Null。|  
|**tape_operation_desc**|**nvarchar(120)**|将要执行的磁带操作：<br /><br /> READ<br /><br /> FORMAT<br /><br /> INIT<br /><br /> APPEND<br /><br /> 可以为 Null。|  
|**mount_request_type**|**int**|装入请求的类型：<br /><br /> 1 = 特定磁带。 需要由 **media_ \** _ 字段标识的磁带。<br /><br /> 2 = 下一个介质簇。 请求尚未还原的下一个介质簇。 用于从比介质簇更少的设备进行还原时。<br /><br /> 3 = 延续磁带。 介质簇正在扩展，并且请求延续磁带。<br /><br /> 可以为 Null。|  
|_ *mount_request_type_desc**|**nvarchar(120)**|装入请求的类型：<br /><br /> SPECIFIC TAPE<br /><br /> NEXT MEDIA FAMILY<br /><br /> CONTINUATION VOLUME<br /><br /> 可以为 Null。|  
  
## <a name="permissions"></a>权限  
 用户必须对服务器拥有 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与 i/o 相关的动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


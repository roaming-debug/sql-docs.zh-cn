---
description: sys.dm_os_windows_info (Transact-SQL)
title: sys.dm_os_windows_info (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_windows_info
- dm_os_windows_info_TSQL
- sys.dm_os_windows_info
- sys.dm_os_windows_info_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_windows_info dynamic management view
ms.assetid: adc81283-fdc2-46c0-bb48-abe82bbf2459
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 111e5b732dae02a5bc2bc8dcffebb3b563206f9c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094018"
---
# <a name="sysdm_os_windows_info-transact-sql"></a>sys.dm_os_windows_info (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回一个显示 Windows 操作系统版本信息的行。  
  
  仅适用于在 Windows 上运行的 SQL Server。 若要查看在非 Windows 主机（如 Linux）上运行 SQL Server 的类似信息，请使用 [sys.dm_os_host_info &#40;transact-sql&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-os-host-info-transact-sql.md)。 
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**windows_release**|**nvarchar(256)**|对于 Windows，返回发布号。 有关值和说明的列表，请参阅 [操作系统版本 (Windows) ](/windows/desktop/SysInfo/operating-system-version)。 不能为 NULL。|  
|**windows_service_pack_level**|**nvarchar(256)**| 对于 Windows，将返回 Service Pack 号。 不能为 NULL。 |  
|**windows_sku**|**int**|对于 Windows，返回 Windows 库存 (SKU) ID。 有关 SKU Id 和说明的列表，请参阅 [GetProductInfo 函数](/windows/win32/api/sysinfoapi/nf-sysinfoapi-getproductinfo)。 可以为 Null。 |  
|**os_language_version**|**int**| 对于 Windows，将返回 (LCID) 操作系统的 Windows 区域设置标识符。 有关 LCID 值和说明的列表，请参阅 [Microsoft 分配的区域设置 id](/openspecs/windows_protocols/ms-lcid/a9eac961-e77d-41a6-90a5-ce1a8b0cdb9c)。 不能为 NULL。|  
  
  
## <a name="permissions"></a>权限  
默认情况下，将 sys.dm_os_windows_info 的 SELECT 权限授予 public 角色。 如果已吊销，则需要对服务器具有 VIEW SERVER STATE 权限。  

## <a name="limitations-and-restrictions"></a>限制和局限
若要查看在非 Windows 主机（如 Linux）上运行的 SQL 的信息，请使用 [&#40;transact-sql&#41;sys.dm_os_host_info ](../../relational-databases/system-dynamic-management-views/sys-dm-os-host-info-transact-sql.md)。 
  
## <a name="examples"></a>示例  
 下面的示例返回 **sys.dm_os_windows_info** 视图中的所有列。  
  
```  
SELECT windows_release, windows_service_pack_level, windows_sku, os_language_version  
FROM sys.dm_os_windows_info;  
```  
  
 下面是一个结果集示例。  
  
 `windows_release  windows_service_pack_level   windows_sku   os_language_version`  
  
 `---------------  ---------------------------  ------------  -------------------`  
  
 `6.0              Service Pack 2                4            1033`  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_os_sys_info (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [sys.dm_os_host_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-host-info-transact-sql.md)  
  

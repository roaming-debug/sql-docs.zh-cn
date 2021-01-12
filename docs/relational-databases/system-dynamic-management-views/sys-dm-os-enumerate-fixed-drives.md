---
description: 'sys.dm_os_enumerate_fixed_drives (Transact-sql) '
title: sys.dm_os_enumerate_fixed_drives (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 09/18/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_enumerate_fixed_drives
- sys.dm_os_enumerate_fixed_drives_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_enumerate_fixed_drives dynamic management view
ms.assetid: 2e27489e-cf69-4a89-9036-77723ac3de66
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e909c2b38df99f0e2cd5aa4addc2e657f60b5cfa
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097564"
---
# <a name="sysdm_os_enumerate_fixed_drives-transact-sql"></a>sys.dm_os_enumerate_fixed_drives (Transact-sql) 

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

在 SQL Server 2019 中引入。

枚举装载到驱动器号的卷，例如 `C:\` 。

|列名称|数据类型|说明|
|-----------------|---------------|-----------------|  
|`fixed_drive_path`|`nvarchar(512)`|卷的路径，如 `C:\` 。|  
|`drive_type`|`int`|驱动器类型的代码。 请参阅[ `GetDriveTypeW` 函数](/windows/win32/api/fileapi/nf-fileapi-getdrivetypew)。|
|`drive_type_desc`|`nvarchar(512)`|驱动器类型的说明。 请参阅[ `GetDriveTypeW` 函数](/windows/win32/api/fileapi/nf-fileapi-getdrivetypew)。|
|`free_space_in_bytes`|`bigint`|磁盘可用空间（以字节为单位）。|

## <a name="permissions"></a>权限

用户必须具有 `VIEW SERVER STATE` 对服务器的权限。

## <a name="see-also"></a>另请参阅  

 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与 i/o 相关的动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)  

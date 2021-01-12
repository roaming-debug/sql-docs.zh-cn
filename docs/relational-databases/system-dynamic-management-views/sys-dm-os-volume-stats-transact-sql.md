---
description: sys.dm_os_volume_stats (Transact-SQL)
title: sys.dm_os_volume_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 09/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_volume_stats_TSQL
- dm_os_volume_stats
- sys.dm_os_volume_stats
- sys.dm_os_volume_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_volume_stats dynamic management function
ms.assetid: fa1c58ad-8487-42ad-956c-983f2229025f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 32deca4f06709806d90ee7d25e25105f191ad23e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098815"
---
# <a name="sysdm_os_volume_stats-transact-sql"></a>sys.dm_os_volume_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-2008R2SP1-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-2008R2sp1-xxxx-xxxx-xxx-md.md)]

  返回有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014 中存储指定数据库和文件的操作系统卷（目录）的信息。 通过使用此动态管理函数，可以检查物理磁盘驱动器的属性，或返回有关目录的可用空间的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sys.dm_os_volume_stats (database_id, file_id)  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> 参数  
 database_id  
 数据库 ID。 database_id 的数据类型为 int，无默认值。 不能为 NULL。  
  
 file_id  
 文件的 ID。 *file_id* 为 **int**，没有默认值。 不能为 NULL。  
  
## <a name="table-returned"></a>返回的表  
  
||||  
|-|-|-|  
|**列**|**数据类型**|**说明**|  
|database_id|**int**|数据库 ID。 不能为 null。|  
|file_id|**int**|文件的 ID。 不能为 null。|  
|**volume_mount_point**|**nvarchar(512)**|根卷上的装入点。 可返回空字符串。 在 Linux 操作系统上返回 null。|  
|**volume_id**|**nvarchar(512)**|操作系统卷 ID。 可返回空字符串。 在 Linux 操作系统上返回 null。|  
|**logical_volume_name**|**nvarchar(512)**|逻辑卷名称。 可返回空字符串。 在 Linux 操作系统上返回 null。|  
|**file_system_type**|**nvarchar(512)**|文件系统卷的类型（例如 NTFS、FAT、RAW）。 可返回空字符串。 在 Linux 操作系统上返回 null。|  
|**total_bytes**|**bigint**|卷的总大小（字节）。 不能为 null。|  
|**available_bytes**|**bigint**|卷上的可用空间。 不能为 null。|  
|**supports_compression**|**tinyint**|指示卷是否支持操作系统压缩。 在 Windows 上不能为 null，并且在 Linux 操作系统上返回 null。|  
|**supports_alternate_streams**|**tinyint**|指示卷是否支持备用流。 在 Windows 上不能为 null，并且在 Linux 操作系统上返回 null。|  
|**supports_sparse_files**|**tinyint**|指示卷是否支持稀疏文件。  在 Windows 上不能为 null，并且在 Linux 操作系统上返回 null。|  
|**is_read_only**|**tinyint**|指示卷当前是否标记为只读。 不能为 null。|  
|**is_compressed**|**tinyint**|指示此卷当前是否已压缩。 在 Windows 上不能为 null，并且在 Linux 操作系统上返回 null。|  
|**incurs_seek_penalty**|**tinyint**|指示支持此卷的存储的类型。 可能的值为：<br /><br />0：在此卷上无搜寻惩罚，通常是在存储设备为 PMM 或 SSD 时<br /><br />1：在此卷上搜寻惩罚，通常是在存储设备是 HDD 时<br /><br />2：当卷位于 UNC 路径或装载的共享上时，无法确定存储类型<br /><br />NULL：无法在 Linux 操作系统上确定存储类型<br /><br />**适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]从) 开始 ([!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]|  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 需要 `VIEW SERVER STATE` 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-return-total-space-and-available-space-for-all-database-files"></a>A. 返回所有数据库文件的总空间和可用空间  
 下面的示例返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中所有数据库文件的总空间和可用空间（字节）。  
  
```sql  
SELECT f.database_id, f.file_id, volume_mount_point, total_bytes, available_bytes  
FROM sys.master_files AS f  
CROSS APPLY sys.dm_os_volume_stats(f.database_id, f.file_id);  
```  
  
### <a name="b-return-total-space-and-available-space-for-the-current-database"></a>B. 返回当前数据库的总空间和可用空间  
 下面的示例返回当前数据库中数据库文件的总空间和可用空间（字节）。  
  
```sql  
SELECT database_id, f.file_id, volume_mount_point, total_bytes, available_bytes  
FROM sys.database_files AS f  
CROSS APPLY sys.dm_os_volume_stats(DB_ID(f.name), f.file_id);  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.master_files &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
  
  

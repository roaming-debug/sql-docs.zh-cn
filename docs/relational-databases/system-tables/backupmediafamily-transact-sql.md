---
title: backupmediafamily (Transact-sql) |Microsoft Docs
description: Backupmediafamily 的参考，其中每个介质簇对应一行。
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- backupmediafamily
- backupmediafamily_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backupmediafamily system table
- backup media [SQL Server], backupmediafamily system table
author: cawrites
ms.author: chadam
ms.openlocfilehash: df687040bf73fb1d78f15d7cca18cc0fc092ee1f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198673"
---
# <a name="backupmediafamily-transact-sql"></a>backupmediafamily (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

每个介质簇在表中占一行。 如果介质簇驻留在镜像介质集中，则对于介质集中的每个镜像，该介质簇都具有一个单独的行。 该表存储在 **msdb** 数据库中。  
    
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**media_set_id**|**int**|标识该簇所属介质集的唯一标识号。 引用 **backupmediaset (media_set_id)**|  
|**family_sequence_number**|**tinyint**|该介质簇在介质集中的位置。|  
|**media_family_id**|**uniqueidentifier**|标识介质簇的唯一标识号。 可以为 NULL。|  
|**media_count**|**int**|介质簇中的介质数。 可以为 NULL。|  
|**logical_device_name**|**nvarchar(128)**|**Sys.backup_devices** 中此备份设备的名称。 如果这是临时备份设备 (与 **sys.backup_devices**) 中存在的永久备份设备相对），则 **logical_device_name** 的值为 NULL。|  
|**physical_device_name**|**nvarchar(260)**|备份设备的物理名称。 可以为 NULL。 此字段在备份和还原过程之间共享。 它可能包含原始备份目标路径或原始还原源路径。 具体取决于数据库的服务器上的备份还是还原。 请注意，从同一备份文件连续还原不会更新路径，而不管它在还原时的位置。 因此， **physical_device_name** "字段不能用于查看使用的还原路径。|  
|**device_type**|**tinyint**|备份设备的类型：<br /><br /> 2 = 磁盘<br /><br /> 5 = 磁带<br /><br /> 7 = 虚拟设备<br /><br /> 9 = Azure 存储<br /><br /> 105 = 永久备份设备。<br /><br /> 可以为 NULL。<br /><br /> 可以在 **sys.backup_devices** 中找到所有永久性设备名称和设备号。|  
|**physical_block_size**|**int**|用于写入介质簇的物理块大小。 可以为 NULL。|  
|**mirror**|**tinyint**|镜像服务器号 (0-3)。|  
  
## <a name="remarks"></a>备注  
 通过 VERIFYONLY 从 *BACKUP_DEVICE* 还原将用介质集标头中的相应值填充 **backupmediaset** 表的列。  
  
 若要减少此表以及其他备份和历史记录表中的行数，请执行 [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) 存储过程。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;备份和还原表 ](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfile (Transact-SQL)](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupfilegroup (Transact-SQL)](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupmediaset (Transact-SQL)](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [backupset (Transact-SQL)](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [系统表 (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  

---
description: 'sys.fn_db_backup_file_snapshots (Transact-sql) '
title: sys.fn_db_backup_file_snapshots (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/03/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 45010ff2-219f-4086-9ea4-016a6c17cddd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7e0df203aaf46dfbf2ab89f68eff9e1efde8a57f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206070"
---
# <a name="sysfn_db_backup_file_snapshots-transact-sql"></a>sys.fn_db_backup_file_snapshots (Transact-sql) 
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  返回与数据库文件关联的 Azure 快照。 如果找不到指定的数据库，或者如果数据库文件未存储在 Microsoft Azure Blob 存储服务中，则不会返回任何行。 将此系统函数与 **sys.sp_delete_backup_file_snapshot** 系统存储过程结合使用，以标识和删除孤立的备份快照。 有关详细信息，请参阅 [Azure 中数据库文件的文件快照备份](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.fn_db_backup_file_snapshots   
   [ ( database_name ) ]  
```  
  
## <a name="arguments"></a>参数  
 *Database_name*  
 正在查询的数据库的名称。 如果为 NULL，则在当前数据库范围内执行此函数。  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|file_id|**int**|数据库的文件 ID。 不可为 null。|  
|snapshot_time|**nvarchar(260)**|REST API 返回快照时的时间戳。 如果不存在快照，则返回 NULL。|  
|snapshot_url|**nvarchar(360)**|文件快照的完整 URL。 如果不存在快照，则返回 NULL。|  
  
## <a name="permissions"></a>权限  
 需要对数据库拥有 VIEW DATABASE STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [sp_delete_backup_file_snapshot &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md)   
 [sp_delete_backup (Transact-SQL)](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md)  
  
  

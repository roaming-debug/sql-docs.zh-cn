---
description: backupfilegroup (Transact-SQL)
title: backupfilegroup (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- backupfilegroup_TSQL
- backupfilegroup
dev_langs:
- TSQL
helpviewer_keywords:
- filegroups [SQL Server], backupfilegroup system table
- backupfilegroup system table
ms.assetid: d26e8fbe-f5c5-4e10-b2bd-0d8e16ea21f9
author: cawrites
ms.author: chadam
ms.openlocfilehash: be220cdc204488f0c2f82244b543ae324218bc49
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198671"
---
# <a name="backupfilegroup-transact-sql"></a>backupfilegroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  备份时数据库中的每个文件组在表中占一行。 **backupfilegroup** 存储在 **msdb** 数据库中。  
  
> [!NOTE]  
>  **Backupfilegroup** 表显示了数据库的文件组配置，而不是备份集的配置文件组。 若要确定备份集中是否包含文件，请使用 [backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md)表的 **is_present** 列。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**backup_set_id**|**int**|包含该文件组的备份集。|  
|name|**sysname**|文件组的名称。|  
|**filegroup_id**|**int**|文件组的 ID；在数据库中唯一。 对应于 **sys.databases** 中的 **data_space_id** 。|  
|**filegroup_guid**|**uniqueidentifier**|文件组的全局唯一标识符。 可以为 NULL。|  
|type|**char(2)**|内容类型，可为下列类型之一：<br /><br /> FG =“行”文件组<br /><br /> SL = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日志文件组|  
|**type_desc**|**nvarchar(60)**|函数类型的说明，可为下列值之一：<br /><br /> ROWS_FILEGROUP<br /><br /> SQL_LOG_FILEGROUP |  
|**is_default**|**bit**|默认文件组，在 CREATE TABLE 或 CREATE INDEX 中未指定文件组时使用。|  
|**is_readonly**|**bit**|1 = 文件组为只读文件组。|  
|**log_filegroup_guid**|**uniqueidentifier**|可以为 NULL。|  
  
## <a name="remarks"></a>备注  
  
> [!IMPORTANT]  
>  相同的文件组名称可以出现在不同数据库中；但是，每个文件组都有自己的 GUID。 因此， **(backup_set_id，filegroup_guid)** 是一个唯一键，用于标识 **backupfilegroup** 中的文件组。  
  
 通过 VERIFYONLY 从 *BACKUP_DEVICE* 还原将用介质集标头中的相应值填充 **backupmediaset** 表的列。  
  
 若要减少此表以及其他备份和历史记录表中的行数，请执行 [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) 存储过程。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;备份和还原表 ](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfile (Transact-SQL)](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupmediafamily (Transact-SQL)](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [backupmediaset (Transact-SQL)](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [backupset (Transact-SQL)](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [系统表 (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  

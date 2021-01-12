---
description: sys.master_files (Transact-SQL)
title: sys.master_files (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/10/2016
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.master_files
- master_files_TSQL
- sys.master_files_TSQL
- master_files
dev_langs:
- TSQL
helpviewer_keywords:
- sys.master_files catalog view
ms.assetid: 803b22f2-0016-436b-a561-ce6f023d6b6a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 55b5465b2c43b02247a48478272393f941a4a51d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095519"
---
# <a name="sysmaster_files-transact-sql"></a>sys.master_files (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  master 数据库中的每个文件对应一行。 这是一个系统范围视图。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|**int**|应用此文件的数据库的 ID。 Masterdatabase_id 始终为1。|  
|file_id|**int**|数据库内文件的 ID。 主 file_id 始终为 1。|  
|file_guid|**uniqueidentifier**|文件的唯一标识符。<br /><br /> NULL = 数据库已从 (的早期版本升级 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 到 SQL Server 2005 及更低版本的) 。|  
|type|**tinyint**|文件类型：<br /><br /> 0 = 行。<br /><br /> 1 = 日志<br /><br /> 2 = FILESTREAM<br /><br /> 3 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 4 = 全文（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 之前的全文目录；升级到的或在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 或更高版本中创建的全文目录将报告文件类型 0。）|  
|type_desc|**nvarchar(60)**|文件类型的说明：<br /><br /> ROWS<br /><br /> LOG<br /><br /> FILESTREAM<br /><br /> FULLTEXT（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 之前的全文目录。）|  
|data_space_id|**int**|此文件所属数据空间的 ID。 数据空间是一个文件组。<br /><br /> 0 = 日志文件|  
|name|**sysname**|数据库中文件的逻辑名称。|  
|physical_name|**nvarchar(260)**|操作系统文件名。|  
|state|**tinyint**|文件状态：<br /><br /> 0 = ONLINE <br /><br /> 1 = RESTORING<br /><br /> 2 = RECOVERING<br /><br /> 3 = RECOVERY_PENDING<br /><br /> 4 = SUSPECT <br /><br /> 5 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 6 = OFFLINE <br /><br /> 7 = DEFUNCT|  
|state_desc|**nvarchar(60)**|文件状态的说明：<br /><br /> ONLINE<br /><br /> RESTORING<br /><br /> RECOVERING<br /><br /> RECOVERY_PENDING<br /><br /> SUSPECT<br /><br /> OFFLINE<br /><br /> DEFUNCT<br /><br /> 有关详细信息，请参阅[文件状态](../../relational-databases/databases/file-states.md)。|  
|大小|**int**|当前文件大小（以 8 KB 为单位的页数）。 对于数据库快照来说，size 表示该快照可以一直用于文件的最大空间。<br /><br /> 注意：对于 FILESTREAM 容器，此字段填充为零。 查询 *sys.database_files* 目录视图，了解 FILESTREAM 容器的实际大小。|  
|max_size|**int**|最大文件大小（以 8 KB 为单位的页数）：<br /><br /> 0 = 不允许增长。<br /><br /> -1 = 文件将一直增长到磁盘充满为止。<br /><br /> 268435456 = 日志文件将增长到最大大小 2 TB。<br /><br /> 注意：如果使用无限制的日志文件大小升级的数据库，日志文件的最大大小将报告为-1。|  
|growth|**int**|0 = 文件大小固定，不会增长。<br /><br /> >0 = 文件将自动增长。<br /><br /> 如果 is_percent_growth = 0，则以若干个 8 KB 页为增量递增，舍入为最小 64 KB。<br /><br /> 如果 is_percent_growth = 1，增量将用整数百分比表示。|  
|is_media_read_onlyF|**bit**|1 = 文件位于只读介质上。<br /><br /> 0 = 文件位于可读/写介质上。|  
|is_read_only|**bit**|1 = 文件标记为只读。<br /><br /> 0 = 文件标记为读/写。|  
|is_sparse|**bit**|1 = 文件是稀疏文件。<br /><br /> 0 = 文件不是稀疏文件。<br /><br /> 有关详细信息，请参阅[查看数据库快照的稀疏文件大小 (Transact-SQL)](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md)。|  
|is_percent_growth|**bit**|1 = 文件的增长以百分比表示。<br /><br /> 0 = 以页数为单位表示绝对增长大小。|  
|is_name_reserved|**bit**|1 = 可重用已删除的文件名。 必须进行日志备份后才能将该名称（name 或 physical_name）重新用于新建文件名。<br /><br /> 0 = 文件名不可重复使用。|  
|create_lsn|**numeric(25,0)**|文件创建时的日志序列号 (LSN)。|  
|drop_lsn|**numeric(25,0)**|文件删除时的 LSN。|  
|read_only_lsn|**numeric(25,0)**|包含该文件的文件组从可读/写更改为只读（最新更改）时的 LSN。|  
|read_write_lsn|**numeric(25,0)**|包含该文件的文件组从只读更改为可读/写（最新更改）时的 LSN。|  
|differential_base_lsn|**numeric(25,0)**|差异备份的基准。 在此 LSN 之后更改的数据区将包含在差异备份中。|  
|differential_base_guid|**uniqueidentifier**|差异备份所基于的基准备份的唯一标识符。|  
|differential_base_time|**datetime**|与 differential_base_lsn 相对应的时间。|  
|redo_start_lsn|**numeric(25,0)**|下一次前滚必须开始时的 LSN。<br /><br /> 除非 state = RESTORING 或 state = RECOVERY_PENDING，否则为 NULL。|  
|redo_start_fork_guid|**uniqueidentifier**|恢复分叉的唯一标识符。 还原的下一个日志备份的 first_fork_guid 必须与此值匹配。 这将展示容器的当前状态。|  
|redo_target_lsn|**numeric(25,0)**|对此文件的联机前滚可以停止时的 LSN。<br /><br /> 除非 state = RESTORING 或 state = RECOVERY_PENDING，否则为 NULL。|  
|redo_target_fork_guid|**uniqueidentifier**|可恢复容器的恢复分叉。 与 redo_target_lsn 成对使用。|  
|backup_lsn|**numeric(25,0)**|文件的最新数据或差异备份的 LSN。|  
|credential_id|**int**|`credential_id` `sys.credentials` 用于存储文件的。 例如，当在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Azure 虚拟机上运行并且数据库文件存储在 azure blob 存储中时，将使用存储位置的访问凭据配置凭据。|  
  
> [!NOTE]  
>  在删除或重新生成大型索引时，或者在删除或截断大型表时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将延迟实际页释放及其关联锁，直至事务提交完毕为止。 延迟的删除操作不会立即释放已分配的空间。 因此，sys.master_files 返回的值在删除或截断了大型对象后，可能无法立即反映出磁盘的实际可用空间。  
  
## <a name="permissions"></a>权限  
 查看相应行所必需的最低权限是 CREATE DATABASE、ALTER ANY DATABASE 或 VIEW ANY DEFINITION。  
  
## <a name="see-also"></a>另请参阅  
 [数据库和文件目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [文件状态](../../relational-databases/databases/file-states.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [数据库文件和文件组](../../relational-databases/databases/database-files-and-filegroups.md)  
  
  

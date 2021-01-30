---
description: sp_filestream_force_garbage_collection (Transact-SQL)
title: sp_filestream_force_garbage_collection (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_filestream_force_garbage_collection
- sp_filestream_force_garbage_collection_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- FILESTREAM [SQL Server]
- sp_filestream_force_garbage_collection
ms.assetid: 9d1efde6-8fa4-42ac-80e5-37456ffebd0b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0c234c5c03b00b25babbec0a4fbc197f235c966d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165244"
---
# <a name="sp_filestream_force_garbage_collection-transact-sql"></a>sp_filestream_force_garbage_collection (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  强制运行 FILESTREAM 垃圾回收器，从而删除任何不需要的 FILESTREAM 文件。  
  
 在垃圾回收器清除 FILESTREAM 容器中的所有删除文件后，才能删除该容器。 FILESTREAM 垃圾回收器是自动运行的。 但是，如果需要在垃圾回收器运行之前删除容器，则可以使用 sp_filestream_force_garbage_collection 手动运行垃圾回收器。  
  
  
## <a name="syntax"></a>语法  
  
```  
sp_filestream_force_garbage_collection
    [ @dbname = ]  'database_name'
    [ , [ @filename = ] 'logical_file_name' ]
```  
  
## <a name="arguments"></a>参数  
 `[ @dbname = ]  'database_name'`  
 指示要运行垃圾回收器的数据库的名称。  
  
> [!NOTE]  
>  的数据类型为 sysname`@dbname`。 如果未指定，则假定为当前数据库。  
  
 `[ @filename = ] 'logical_file_name'`  
 指定要运行垃圾回收器的 FILESTREAM 容器的逻辑名称。 `@filename` 是可选项。 如果未指定逻辑文件名，垃圾回收器将清除指定数据库中的所有 FILESTREAM 容器。  
  
## <a name="return-code-values"></a>返回代码值  
  
| 值 | 说明 |
| ----- | ----------- |   
|0|操作成功|  
|1|操作失败|  
  
## <a name="result-sets"></a>结果集  
  
|值|说明|  
|-----------|-----------------|  
|file_name|指示 FILESTREAM 容器名称|  
|*num_collected_items*|指示此容器中已回收（删除）的 FILESTREAM 项目（文件/目录）数。|  
|*num_marked_for_collection_items*|指示此容器中已标记为回收（删除）的 FILESTREAM 项目（文件/目录）数。 尚未删除这些项，但可以在垃圾回收阶段后删除这些项。|  
|*num_unprocessed_items*|指示此 FILESTREAM 容器中符合条件但未进行垃圾回收处理的 FILESTREAM 项目（文件或目录）数。 可能由于各种原因而未处理项目，其中包括：<br /><br /> 由于尚未执行日志备份或检查点操作，需要暂时锁定文件。<br /><br /> 文件处于 FULL 或 BULK_LOGGED 恢复模式。<br /><br /> 有长时间运行的活动事务。<br /><br /> 复制日志读取器作业尚未运行。 有关详细信息，请参阅 [SQL Server 2008 中的白皮书 FILESTREAM 存储](/previous-versions/sql/sql-server-2008/hh461480(v=msdn.10)) 。|  
|*last_collected_xact_seqno*|返回最后一个相应的日志序列号 (LSN)，已垃圾回收指定 FILESTREAM 容器中小于该编号的文件。|  
  
## <a name="remarks"></a>备注  
 在请求的数据库（和 FILESTREAM 容器）上显式运行 FILESTREAM 垃圾回收器任务，直至完成。 垃圾回收进程将删除不再需要的文件。 完成此操作所需的时间取决于该数据库或容器中的 FILESTREAM 数据大小，以及最近对 FILESTREAM 数据执行的 DML 活动的数量。 尽管可以在数据库联机时运行此操作，但这可能会在运行期间由于垃圾回收进程执行的各种 I/O 活动而影响数据库性能。  
  
> [!NOTE]  
>  建议仅在必要时运行此操作，并且在通常的工作时间以外运行。  
  
只能在单独容器或单独数据库上同时运行针对此存储过程的多个调用。  

由于有两个阶段的操作，存储过程应该运行两次，以实际删除底层的 Filestream 文件。  

垃圾回收 (GC) 依赖于日志截断。 因此，如果使用完整恢复模式最近在数据库上删除了文件，则只有在创建了这些事务日志部分的日志备份并将日志部分标记为非活动后，它们才是 GC。 在使用简单恢复模式的数据库上，在对数据库发出后发生日志截断 `CHECKPOINT` 。  


## <a name="permissions"></a>权限  
 需要 db_owner 数据库角色中的成员身份。  
  
## <a name="examples"></a>示例  
 以下示例为 `FSDB` 数据库中的 FILESTREAM 容器运行垃圾回收器。  
  
### <a name="a-specifying-no-container"></a>A. 不指定任何容器  
  
```sql  
USE FSDB;  
GO  
EXEC sp_filestream_force_garbage_collection @dbname = N'FSDB';  
```  
  
### <a name="b-specifying-a-container"></a>B. 指定一个容器  
  
```sql  
USE FSDB;  
GO  
EXEC sp_filestream_force_garbage_collection @dbname = N'FSDB',
    @filename = N'FSContainer';  
```  
  
## <a name="see-also"></a>另请参阅  
[文件流](../../relational-databases/blob/filestream-sql-server.md)
<br>[Filetable](../../relational-databases/blob/filetables-sql-server.md)
<br>[Filestream 和 FileTable 动态管理视图 (Transact-SQL)](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
<br>[Filestream 和 FileTable 目录视图 (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
<br>[sp_kill_filestream_non_transacted_handles (Transact-SQL)](filestream-and-filetable-sp-kill-filestream-non-transacted-handles.md)
  

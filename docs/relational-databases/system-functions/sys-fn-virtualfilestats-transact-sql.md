---
description: sys.fn_virtualfilestats (Transact-SQL)
title: sys.fn_virtualfilestats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/16/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_virtualfilestats_TSQL
- fn_virtualfilestats
dev_langs:
- TSQL
helpviewer_keywords:
- I/O [SQL Server], statistics
- fn_virtualfilestats function
- sys.fn_virtualfilestats function
- statistical information [SQL Server], I/O
ms.assetid: 96b28abb-b059-48db-be2b-d60fe127f6aa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4c49575ef666cf35c26dd4e924cbb75b17cb9c4e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170139"
---
# <a name="sysfn_virtualfilestats-transact-sql"></a>sys.fn_virtualfilestats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回数据库文件（包括日志文件）的 I/O 统计信息。 在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，也可以从 [sys.dm_io_virtual_file_stats](../../relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql.md) 动态管理视图中获取此信息。  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
fn_virtualfilestats ( { database_id | NULL } , { file_id | NULL } )  
```  
  
## <a name="arguments"></a>参数  
 *database_id* |无效  
 数据库的 ID。 database_id 的数据类型为 int，无默认值。 指定 NULL 可返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中所有数据库的信息。  
  
 *file_id* |无效  
 文件的 ID。 *file_id* 为 **int**，没有默认值。 指定 NULL 可为数据库中的所有文件返回信息。  
  
## <a name="table-returned"></a>返回的表  
  
|列名|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**DbId**|**smallint**|数据库 ID。|  
|**FileId**|**smallint**|文件 ID。|  
|**标志**|**bigint**|提取数据时的数据库时间戳。 在之前的版本中为 **int** [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] 。 |  
|**NumberReads**|**bigint**|对文件发出的读取次数。|  
|**BytesRead**|**bigint**|对文件发出的读取字节数。|  
|**IoStallReadMS**|**bigint**|用户等待文件的读取 I/O 完成所费的总时间（以毫秒为单位）。|  
|**NumberWrites**|**bigint**|对文件的写入次数。|  
|**BytesWritten**|**bigint**|对文件写入的字节数。|  
|**IoStallWriteMS**|**bigint**|用户等待文件的写入 I/O 完成所费的总时间（以毫秒为单位）。|  
|**IoStallMS**|**bigint**|**IoStallReadMS** 和 **IoStallWriteMS** 的总和。|  
|**FileHandle**|**bigint**|文件句柄的值。|  
|**BytesOnDisk**|**bigint**|磁盘上的物理文件大小（以字节为单位）。<br /><br /> 对于数据库文件，此值与 **sys.database_files** 中的 **大小** 相同，但是以字节而不是页表示。<br /><br /> 对于数据库快照备用文件，它是操作系统用于文件的空间。|  
  
## <a name="remarks"></a>备注  
 **fn_virtualfilestats** 是提供统计信息的系统表值函数，例如对文件执行的 i/o 总数。 可使用该函数来帮助跟踪用户读取文件或写入到文件必须等待的时间长度。 该函数还可帮助您识别出发生了大量 I/O 活动的文件。  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-displaying-statistical-information-for-a-database"></a>A. 显示数据库的统计信息  
 以下示例显示 ID 为 `1` 的数据库中的文件 ID 1 的统计信息。  
  
```sql  
SELECT *  
FROM fn_virtualfilestats(1, 1);  
GO  
```  
  
### <a name="b-displaying-statistical-information-for-a-named-database-and-file"></a>B. 显示命名数据库和文件的统计信息  
 以下示例显示 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 示例数据库中日志文件的统计信息。 系统函数 `DB_ID` 用于指定 *database_id* 参数。  
  
```sql  
SELECT *  
FROM fn_virtualfilestats(DB_ID(N'AdventureWorks2012'), 2);  
GO  
```  
  
### <a name="c-displaying-statistical-information-for-all-databases-and-files"></a>C. 显示所有数据库和文件的统计信息  
 以下示例显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例内所有数据库中的所有文件的统计信息。  
  
```sql  
SELECT *  
FROM fn_virtualfilestats(NULL,NULL);  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [DB_ID &#40;Transact-sql&#41;](../../t-sql/functions/db-id-transact-sql.md)   
 [FILE_IDEX (Transact-SQL)](../../t-sql/functions/file-idex-transact-sql.md)   
 [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  

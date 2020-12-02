---
description: DBCC SHRINKFILE (Transact-SQL)
title: DBCC SHRINKFILE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SHRINKFILE
- DBCC_SHRINKFILE_TSQL
- DBCC SHRINKFILE
- SHRINKFILE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- data shrinking [SQL Server]
- TRUNCATEONLY option
- shrinking files
- NOTRUNCATE option
- shrinking databases
- decreasing database size
- file shrinking [SQL Server]
- database shrinking [SQL Server]
- logs [SQL Server], shrinking
- reducing database size
- DBCC SHRINKFILE statement
ms.assetid: e02b2318-bee9-4d84-a61f-2fddcf268c9f
author: pmasl
ms.author: umajay
ms.openlocfilehash: 7d7d3c9e8fa3e67a4ee6ba5c2eb2590ee65c18b2
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96119560"
---
# <a name="dbcc-shrinkfile-transact-sql"></a>DBCC SHRINKFILE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

收缩当前数据库的指定数据或日志文件大小。 可以使用它将一个文件中的数据移到同一文件组中的其他文件，这会清空文件，从而允许删除数据库。 可以将文件收缩到小于创建大小，同时将最小文件大小重置为新值。
  
![文章链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DBCC SHRINKFILE   
(  
    { file_name | file_id }   
    { [ , EMPTYFILE ]   
    | [ [ , target_size ] [ , { NOTRUNCATE | TRUNCATEONLY } ] ]  
    }  
)  
[ WITH NO_INFOMSGS ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
file_name  
要收缩的文件的逻辑名称。
  
file_id  
要收缩的文件的标识 (ID) 号。 若要获取文件 ID，请使用 [FILE_IDEX](../../t-sql/functions/file-idex-transact-sql.md) 系统函数，或查询当前数据库中的 [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) 目录视图。
  
*target_size*  
整数，文件的新大小（以 MB 为单位）。 如果未指定，DBCC SHRINKFILE 缩小到文件创建大小。
  
> [!NOTE]  
>  可以使用 DBCC SHRINKFILE target_size 缩小空文件的默认大小。 例如，如果创建一个 5 MB 的文件，然后在文件仍然为空的时候将文件收缩为 3 MB，默认文件大小将设置为 3 MB。 这只适用于永远不会包含数据的空文件。  
  
FILESTREAM 文件组容器不支持此选项。  
如果 target_size 已指定，DBCC SHRINKFILE 会尝试将文件收缩到目标大小。 要释放的文件区域中的已用页移到文件保留区域中的可用空间。 例如，对于 10MB 数据文件，target_size 为 8 的 DBCC SHRINKFILE 操作会将文件最后 2MB 中的所有已用页移到文件前 8MB 中的任何未分配页区域中。 DBCC SHRINKFILE 不会收缩已超过所需存储数据大小的文件。 例如，如果使用 10 MB 数据文件中的 7 MB，则带有 target_size 为 6 的 DBCC SHRINKFILE 语句只能将该文件收缩到 7 MB，而不能收缩到 6 MB。
  
EMPTYFILE  
将指定文件中的所有数据迁移到同一文件组中的其他文件。 也就是说，EMPTYFILE 将指定文件中的数据迁移到同一文件组中的其他文件。 EMPTYFILE 确保不会将任何新数据添加到文件中（尽管此文件不是只读文件）。 可以使用 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) 语句删除文件。 如果你使用 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) 语句更改文件大小，只读标志会重置，并能添加数据。

对于 FILESTREAM 文件组容器，无法使用 ALTER DATABASE 删除文件，除非 FILESTREAM 垃圾回收器已运行，并删除了 EMPTYFILE 已复制到另一个容器的所有不必要文件组容器文件。 有关详细信息，请参阅 [sp_filestream_force_garbage_collection (Transact-SQL)](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md)
  
> [!NOTE]  
>  有关删除 FILESTREAM 容器的信息，请参阅 [ALTER DATABASE 文件和文件组选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md) 中的相应章节  
  
NOTRUNCATE  
无论是否指定 target_percent，将数据文件末尾中的已分配页移到文件开头的未分配页区域中。 操作系统不会回收文件末尾的可用空间，文件的物理大小也不会改变。 因此，如果指定 NOTRUNCATE，文件看起来就像没有收缩一样。
NOTRUNCATE 只适用于数据文件。 日志文件不受影响。   FILESTREAM 文件组容器不支持此选项。
  
TRUNCATEONLY  
将文件末尾的所有可用空间释放给操作系统，但不在文件内部移动任何页。 数据文件只收缩到最后分配的区。
如果使用 TRUNCATEONLY 指定，则会忽略 target_size。  
TRUNCATEONLY 选项不会移动日志中的信息，但会删除日志文件末尾的失效 VLF。 FILESTREAM 文件组容器不支持此选项。
  
WITH NO_INFOMSGS  
取消显示所有信息性消息。
  
## <a name="result-sets"></a>结果集  
下表描述了结果集列。
  
|列名称|说明|  
|---|---|
|**DbId**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]试图收缩的文件的数据库标识号。|  
|**FileId**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]试图收缩的文件的文件标识号。|  
|**CurrentSize**|文件当前占用的 8 KB 页数。|  
|**MinimumSize**|文件最低可以占用的 8 KB 页数。 此数字对应于文件的大小下限或最初创建大小。|  
|**UsedPages**|文件当前使用的 8 KB 页数。|  
|**EstimatedPages**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]估计文件能够收缩到的 8 KB 页数。|  
  
## <a name="remarks"></a>备注  
DBCC SHRINKFILE 适用于当前数据库的文件。 有关如何更改当前数据库的详细信息，请参阅 [USE (Transact-SQL)](../../t-sql/language-elements/use-transact-sql.md)。
  
可以随时停止执行 DBCC SHRINKFILE 操作，并保留任何已完成的工作。 如果你使用 EMPTYFILE 参数并取消操作，文件不会被标记，以防添加其他数据。
  
当 DBCC SHRINKFILE 操作失败时，则会引发错误。
  
 其他用户可以在文件收缩期间使用数据库，数据库不必处于单用户模式。 无需在单用户模式下运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，即可收缩系统数据库。  
  
## <a name="shrinking-a-log-file"></a>收缩日志文件  

对于日志文件，[!INCLUDE[ssDE](../../includes/ssde-md.md)] 使用 target_size 计算整个日志的目标大小。 因此，target_size 是执行收缩操作后的日志可用空间。 随后，整个日志的目标大小转换为每个日志文件的目标大小。 DBCC SHRINKFILE 尝试立即将每个物理日志文件收缩到其目标大小。 但是，如果部分逻辑日志位于超出目标大小的虚拟日志中，则[!INCLUDE[ssDE](../../includes/ssde-md.md)]将释放尽可能多的空间，并发出一条信息性消息。 该消息说明需要执行哪些操作来将逻辑日志移出位于文件末尾的虚拟日志。 执行这些操作以后，DBCC SHRINKFILE 可用于释放剩余空间。
  
因为日志文件只能收缩到虚拟日志文件边界，所以可能无法将日志文件收缩到小于虚拟日志文件（即使没在使用它）。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]在日志文件创建或扩展时，动态选择虚拟日志文件大小。
  
## <a name="best-practices"></a>最佳做法 
 
在计划收缩文件时，请考虑以下信息：
-   在执行会产生大量未用空间的操作（如截断表或删除表操作）后，执行收缩操作最有效。  

-   大多数数据库都需要一些可用空间，以供常规日常操作使用。 如果反复收缩数据库，并且它的大小再次增长，那么常规操作可能需要收缩空间。 在这种情况下，反复收缩数据库是一种无谓的操作。  

-   收缩操作不保留数据库中索引的碎片状态，通常还会在一定程度上增加碎片。 此类碎片是不要反复收缩数据库的另一个原因。  

-   按顺序而非同时缩小同一数据库中的多个文件。 对系统表的争用可能会导致阻塞，进而导致延迟。  
  
## <a name="troubleshooting"></a>疑难解答  
本部分介绍如何诊断和更正在运行 DBCC SHRINKFILE 命令时可能发生的问题。
  
### <a name="the-file-doesnt-shrink"></a>文件不收缩
  
如果在执行无错误收缩操作后文件大小未改变，请尝试执行以下操作，验证文件是否有足够的可用空间：

- 运行以下查询。  
  
```sql
SELECT name ,size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0 AS AvailableSpaceInMB
FROM sys.database_files;
```

-   运行 [DBCC SQLPERF](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md) 命令以返回事务日志中使用的空间。  

如果可用空间不足，收缩操作无法进一步缩小文件大小。
  
通常情况下，似乎不收缩的是日志文件。 导致这种不收缩的原因通常是，日志文件尚未截断。 若要截断日志，可以将数据库恢复模式设置为 SIMPLE，或者先备份日志，再重新运行 DBCC SHRINKFILE 操作。
  
### <a name="the-shrink-operation-is-blocked"></a>收缩操作受阻  

在[基于行版本控制的隔离级别](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)下运行的事务可能会阻止收缩操作。 例如，如果在执行 DBCC SHRINK DATABASE 操作时，正在基于行版本控制的隔离级别下运行大型删除操作，那么收缩操作会等到删除操作完成，然后才会继续。 出现这种阻塞时，DBCC SHRINKFILE 和 DBCC SHRINKDATABASE 操作会将信息性消息（对于 SHRINKDATABASE 为 5202，对于 SHRINKFILE 为 5203）打印输出到 SQL Server 错误日志。 在第一个小时内，此消息每五分钟记录一次，之后每一小时记录一次。 例如，如果错误日志包含以下错误消息，则会发生以下错误：
  
```
DBCC SHRINKFILE for file ID 1 is waiting for the snapshot   
transaction with timestamp 15 and other snapshot transactions linked to   
timestamp 15 or with timestamps older than 109 to finish.  
```  
  
此消息指明，时间戳早于 109（收缩操作完成的最后一个事务）的快照事务正在阻止收缩操作。 它还指明，[sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) 动态管理视图中的 transaction_sequence_num 或 first_snapshot_sequence_num 列包含值 15。 如果 transaction_sequence_num 或 first_snapshot_sequence_num 视图列包含的数字小于收缩操作完成的最后一个事务 (109)，收缩操作会等待这些事务完成。
  
若要解决此问题，请执行下列任务之一：
-   终止阻止收缩操作的事务。
-   终止收缩操作。 如果收缩操作终止，所有已完成的工作都会保留。  
-   不执行任何操作，并允许收缩操作等到阻塞事务完成。  
  
## <a name="permissions"></a>权限  
要求具有 **sysadmin** 固定服务器角色或 **db_owner** 固定数据库角色的成员身份。
  
## <a name="examples"></a>示例  
  
### <a name="shrinking-a-data-file-to-a-specified-target-size"></a>将数据文件收缩到指定的目标大小  
以下示例将 `UserDB` 用户数据库中名为 `DataFile1` 的数据文件的大小收缩到 7 MB。
  
```sql  
USE UserDB;  
GO  
DBCC SHRINKFILE (DataFile1, 7);  
GO  
```  
  
### <a name="shrinking-a-log-file-to-a-specified-target-size"></a>将日志文件收缩到指定的目标大小  
以下示例将 `AdventureWorks` 数据库中的日志文件收缩到 1 MB。 若要允许 DBCC SHRINKFILE 命令收缩文件，首先需要通过将数据库恢复模式设置为 SIMPLE 来截断该文件。
  
```sql  
USE AdventureWorks2012;  
GO  
-- Truncate the log by changing the database recovery model to SIMPLE.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY SIMPLE;  
GO  
-- Shrink the truncated log file to 1 MB.  
DBCC SHRINKFILE (AdventureWorks2012_Log, 1);  
GO  
-- Reset the database recovery model.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY FULL;  
GO  
```  
  
### <a name="c-truncating-a-data-file"></a>C. 截断数据文件  
下面的示例将截断 `AdventureWorks` 数据库中的主数据文件。 需要查询 `sys.database_files` 目录视图以获得数据文件的 `file_id`。
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT file_id, name  
FROM sys.database_files;  
GO  
DBCC SHRINKFILE (1, TRUNCATEONLY);  
```  
  
### <a name="d-emptying-a-file"></a>D. 清空文件  
下面的示例展示了如何清空文件，这样文件就能从数据库中删除。 为了方便此示例进行展示，先创建包含数据的数据文件。
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create a data file and assume it contains data.  
ALTER DATABASE AdventureWorks2012   
ADD FILE (  
    NAME = Test1data,  
    FILENAME = 'C:\t1data.ndf',  
    SIZE = 5MB  
    );  
GO  
-- Empty the data file.  
DBCC SHRINKFILE (Test1data, EMPTYFILE);  
GO  
-- Remove the data file from the database.  
ALTER DATABASE AdventureWorks2012  
REMOVE FILE Test1data;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
[ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DBCC SHRINKDATABASE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md)  
[FILE_ID (Transact-SQL)](../../t-sql/functions/file-id-transact-sql.md)  
[sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
[收缩文件](../../relational-databases/databases/shrink-a-file.md)
  
  

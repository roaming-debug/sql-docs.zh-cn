---
description: DBCC SHRINKDATABASE (Transact-SQL)
title: DBCC SHRINKDATABASE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DBCC_SHRINKDATABASE_TSQL
- DBCC SHRINKDATABASE
- SHRINKDATABASE_TSQL
- SHRINKDATABASE
dev_langs:
- TSQL
helpviewer_keywords:
- data shrinking [SQL Server]
- shrinking files
- shrinking databases
- DBCC SHRINKDATABASE statement
- decreasing database size
- file shrinking [SQL Server]
- database shrinking [SQL Server]
- logs [SQL Server], shrinking
- reducing database size
ms.assetid: fc976afd-1edb-4341-bf41-c4a42a69772b
author: pmasl
ms.author: umajay
monikerRange: = azuresqldb-current ||>= sql-server-2016 ||>= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: 38deaa198679545b3b67a9d93e9a01b12851abd0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480498"
---
# <a name="dbcc-shrinkdatabase-transact-sql"></a>DBCC SHRINKDATABASE (Transact-SQL)
[!INCLUDE [sql-asdb-asa.md](../../includes/applies-to-version/sql-asdb-asa.md)]

收缩指定数据库中的数据文件和日志文件的大小。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DBCC SHRINKDATABASE   
( database_name | database_id | 0   
     [ , target_percent ]   
     [ , { NOTRUNCATE | TRUNCATEONLY } ]   
)  
[ WITH NO_INFOMSGS ]  
```  

```syntaxsql
-- Azure Synapse Analytics

DBCC SHRINKDATABASE   
( database_name   
     [ , target_percent ]   
)  
[ WITH NO_INFOMSGS ]

```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
_database\_name_ | _database\_id_ | 0  
要收缩的数据库名称或 ID。 0 指定使用当前数据库。  
  
_target\_percent_  
数据库收缩后的数据库文件中所需的剩余可用空间百分比。  
  
NOTRUNCATE  
将分配的页面从文件的末尾移动到文件前面的未分配页面。 此操作会压缩文件中的数据。 _target\_percent_ 是可选的。 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 不支持此选项。 
  
文件末尾的可用空间不会返回给操作系统，并且文件的物理大小也不会更改。 因此，指定 NOTRUNCATE 时，数据库似乎不会收缩。  
  
NOTRUNCATE 只适用于数据文件。 NOTRUNCATE 不影响日志文件。  
  
TRUNCATEONLY  
将文件末尾的所有可用空间释放给操作系统。 不移动文件内的任何页面。 数据文件仅收缩到最后指定的盘区。 如果使用 TRUNCATEONLY 指定，则会忽略 _target\_percent_。 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 不支持此选项。
  
TRUNCATEONLY 将影响日志文件。 若要仅截断数据文件，请使用 DBCC SHRINKFILE。  
  
WITH NO_INFOMSGS  
取消严重级别从 0 到 10 的所有信息性消息。  
  
## <a name="result-sets"></a>结果集  
下表对结果集中的列进行了说明。
  
|列名称|说明|  
|-----------------|-----------------|  
|**DbId**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]试图收缩的文件的数据库标识号。|  
|**FileId**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]尝试收缩的文件的文件标识号。|  
|**CurrentSize**|文件当前占用的 8 KB 页数。|  
|**MinimumSize**|文件最低可以占用的 8 KB 页数。 此值与文件的最小大小或最初创建时的大小相对应。|  
|**UsedPages**|文件当前使用的 8 KB 页数。|  
|**EstimatedPages**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]估计文件能够收缩到的 8 KB 页数。|  
  
>[!NOTE]
> [!INCLUDE[ssDE](../../includes/ssde-md.md)]不显示未收缩的文件的行。  
  
## <a name="remarks"></a>备注  

>[!NOTE]
> 不建议运行此命令，因为这是 I/O 密集型操作，可能会使数据仓库离线。 此外，运行此命令后，还会对数据仓库快照产生成本影响。 

若要收缩特定数据库的所有数据和日志文件，请执行 DBCC SHRINKDATABASE 命令。 若要一次收缩一个特定数据库中的一个数据或日志文件，请执行 [DBCC SHRINKFILE](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md) 命令。
  
若要查看数据库中当前的可用（未分配）空间量，请运行 [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)。
  
可在进程中的任一点停止 DBCC SHRINKDATABASE 操作，任何已完成的工作都将保留。
  
数据库不能小于配置的数据库最小大小。 在最初创建数据库时指定最小大小。 或者，最小大小可以是使用文件大小更改操作显式设置的最后大小。 DBCC SHRINKFILE 或 ALTER DATABASE 等操作是文件大小更改操作的示例。 

假设最初创建的数据库大小为 10 MB。 然后，它增长到 100 MB。 即使数据库中的所有数据都已删除，数据库可以减少到的最小大小也为 10 MB。
  
运行 DBCC SHRINKDATABASE 时，请指定 NOTRUNCATE 选项或 TRUNCATEONLY 选项。 如果不这样做，结果与使用 NOTRUNCATE 运行 DBCC SHRINKDATABASE 操作，然后再使用 TRUNCATEONLY 运行 DBCC SHRINKDATABASE 操作相同。
  
收缩数据库不必处于单用户模式。 其他用户可以在数据库收缩时在其中工作，包括系统数据库。
  
备份数据库时，无法收缩数据库。 反之，也不能在数据库执行收缩操作时备份数据库。
  
## <a name="how-dbcc-shrinkdatabase-works"></a>DBCC SHRINKDATABASE 的工作原理  
DBCC SHRINKDATABASE 以每个文件为单位对数据文件进行收缩。然而，DBCC SHRINKDATABASE 在对日志文件进行收缩时，它将视为所有的日志文件都存在于一个连续的日志池中。 文件始终从末尾开始收缩。
  
假设拥有几个日志文件、一个数据文件和一个名为 **mydb** 的数据库。 数据文件和日志文件分别是 10 MB，并且数据文件包含 6 MB 数据。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 计算每个文件的目标大小。 此值是文件要收缩到的大小。 如果使用 _target\_percent_ 指定 DBCC SHRINKDATABASE ，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 计算得出的目标大小为收缩后文件中可用空间的 _target\_percent_ 数量。 

例如，如果为收缩 **mydb** 将 _target\_percent_ 指定为 25，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 计算得出此文件的目标大小为 8 MB（6 MB 数据加上 2 MB 可用空间）。 因此，[!INCLUDE[ssDE](../../includes/ssde-md.md)] 将数据文件后 2 MB 中的所有数据移动到数据文件前 8 MB 的任何可用空间中，然后对该文件进行收缩。
  
假设 mydb 的数据文件包含 7 MB 的数据。 将 _target\_percent_ 指定为 30，以允许将此数据文件收缩到可用空间的 30%。 但是，将 _target\_percent_ 指定为 40 不会收缩数据文件，因为 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将文件收缩到的大小不能小于数据当前占用的空间大小。 

还可以用另一种方法来思考此问题：40% 的所要求可用空间加上 70% 的整个数据文件大小（10 MB 中的 7 MB）超过了 100%。 任何大于 30 的 _target\_size_ 都不会收缩数据文件。 它不会收缩是因为所需的可用百分比加上数据文件当前占用的百分比大于 100%。
  
对于日志文件，[!INCLUDE[ssDE](../../includes/ssde-md.md)] 使用 _target\_percent_ 计算整个日志的目标大小。 这就是为什么 _target\_percent_ 是收缩操作后日志中的可用空间量的原因。 之后，整个日志的目标大小转换为每个日志文件的目标大小。
  
DBCC SHRINKDATABASE 尝试立即将每个物理日志文件收缩到其目标大小。 假设逻辑日志没有任何部分位于超出日志文件目标大小的虚拟日志中。 然后文件成功截断，DBCC SHRINKDATABASE 完成且没有生成任何消息。 但是，如果部分逻辑日志位于超出目标大小的虚拟日志中，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将释放尽可能多的空间，并发出一条信息性消息。 该消息说明需要执行哪些操作来将逻辑日志移出位于文件末尾的虚拟日志。 运行操作后，DBCC SHRINKDATABASE 可用于释放剩余空间。
  
日志文件只能收缩到虚拟日志文件边界。 这就是为什么不可能将日志文件收缩到小于虚拟日志文件大小的原因。 即使未在使用它也可能无法实现。 虚拟日志文件的大小在创建或扩展这些日志文件时由[!INCLUDE[ssDE](../../includes/ssde-md.md)]动态选择。
  
## <a name="best-practices"></a>最佳实践  
当您计划收缩数据库时，请考虑以下信息：
-   在执行会产生未使用空间的操作（如截断表或删除表操作）后，执行收缩操作最有效。
-   大多数数据库都需要一些可用空间，以供常规日常操作使用。 可能会反复收缩数据库，并注意到数据库大小再次增长。 这种增长表明常规操作需要使用收缩的空间。 在这种情况下，反复收缩数据库是一种无谓的操作。  
-   收缩操作不保留数据库中索引的碎片状态，通常还会在一定程度上增加碎片。 此结果是不要反复收缩数据库的另一个原因。  
-   除非有特定要求，否则不要将 AUTO_SHRINK 数据库选项设置为 ON。  
  
## <a name="troubleshooting"></a>疑难解答  
收缩操作可能会被在[基于行版本控制的隔离级别](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)下运行的事务阻止。 例如，在执行 DBCC SHRINK DATABASE 操作时，正在基于行版本控制的隔离级别下运行大型删除操作。 当这种情况发生时，收缩操作会等到删除操作完成后再收缩文件。 收缩操作在等待时，DBCC SHRINKFILE 和 DBCC SHRINKDATABASE 操作会打印输出信息性消息（对于 SHRINKDATABASE 为 5202，对于 SHRINKFILE 为 5203）。 此消息在第一个小时内每五分钟打印到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志一次，然后在后续每个小时打印一次。 例如，如果错误日志包含以下错误消息：  
  
```
DBCC SHRINKDATABASE for database ID 9 is waiting for the snapshot   
transaction with timestamp 15 and other snapshot transactions linked to   
timestamp 15 or with timestamps older than 109 to finish.  
```  
  
此错误表示时间戳早于 109 的快照事务将阻止收缩操作。 该事务是收缩操作完成的最后一个事务。 它还说明 [sys.dm_tran_active_snapshot_database_transactions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) 动态管理视图中的 **transaction_sequence_num** 或 **first_snapshot_sequence_num** 列包含值 15。 该视图中的 **transaction_sequence_num** 或 **first_snapshot_sequence_num** 列可能包含小于收缩操作完成的最后一个事务 (109) 的数字。 如果是这样，收缩操作将等待这些事务完成。
  
若要解决此问题，请执行下列任务之一：
-   终止阻止收缩操作的事务。  
-   终止收缩操作。 所有已完成的工作都会保留。  
-   不执行任何操作，并允许收缩操作等到阻塞事务完成。  
  
## <a name="permissions"></a>权限  
要求具有 **sysadmin** 固定服务器角色或 **db_owner** 固定数据库角色的成员身份。  
  
## <a name="examples"></a>示例  
  
### <a name="a-shrinking-a-database-and-specifying-a-percentage-of-free-space"></a>A. 收缩数据库并指定可用空间的百分比  
以下示例将减小 `UserDB` 用户数据库中数据文件和日志文件的大小，以便在数据库中留出 10% 的可用空间。  
  
```sql  
DBCC SHRINKDATABASE (UserDB, 10);  
GO  
```  
  
### <a name="b-truncating-a-database"></a>B. 截断数据库  
以下示例将 `AdventureWorks` 示例数据库中的数据和日志文件收缩到最后指定的盘区。  
  
```sql  
DBCC SHRINKDATABASE (AdventureWorks2012, TRUNCATEONLY);  
```  
### <a name="c-shrinking-an-azure-synapse-analytics-database"></a>C. 缩小 Azure Synapse Analytics 数据库

```sql
DBCC SHRINKDATABASE (database_A);
DBCC SHRINKDATABASE (database_B, 10); 

```

## <a name="see-also"></a>另请参阅  
[ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DBCC SHRINKFILE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)  
[收缩数据库](../../relational-databases/databases/shrink-a-database.md)
  
  

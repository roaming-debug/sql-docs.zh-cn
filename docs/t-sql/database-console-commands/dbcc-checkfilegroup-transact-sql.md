---
description: DBCC CHECKFILEGROUP (Transact-SQL)
title: DBCC CHECKFILEGROUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKFILEGROUP_TSQL
- DBCC_CHECKFILEGROUP_TSQL
- DBCC CHECKFILEGROUP
- CHECKFILEGROUP
dev_langs:
- TSQL
helpviewer_keywords:
- DBCC CHECKFILEGROUP statement
- database objects [SQL Server], checking
- allocation checks
- integrity [SQL Server], database objects
- filegroups [SQL Server], consistency checks
- table integrity checks [SQL Server]
- checking database objects
ms.assetid: 8c70bf34-7570-4eb6-877a-e35064a1380a
author: pmasl
ms.author: umajay
ms.openlocfilehash: 885a40a622f70649e004226fdfb9f317bad6bbdf
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597092"
---
# <a name="dbcc-checkfilegroup-transact-sql"></a>DBCC CHECKFILEGROUP (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
检查当前数据库的指定文件组中的所有表和索引视图的分配和结构完整性。
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DBCC CHECKFILEGROUP   
[  
    [ ( { filegroup_name | filegroup_id | 0 }   
        [ , NOINDEX ]   
  ) ]  
    [ WITH   
        {   
            [ ALL_ERRORMSGS | NO_INFOMSGS ]   
            [ , TABLOCK ]   
            [ , ESTIMATEONLY ]  
            [ , PHYSICAL_ONLY ]    
            [ , MAXDOP  = number_of_processors ]  
        }   
    ]  
]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 filegroup_name  
 当前数据库中要检查表分配和结构完整性的文件组的名称。 如果不指定此参数或指定了 0 值，则默认值为主文件组。 文件组名称必须遵循有关[标识符](../../relational-databases/databases/database-identifiers.md)的规则。  
 filegroup_name 不能是 FILESTREAM 文件组。  
  
 *filegroup_id*  
 当前数据库中要检查其表分配和结构完整性的文件组标识 (ID) 号。  
  
 NOINDEX  
 指定不应对用户表的非聚集索引执行会占用很大系统开销的检查。 这将减少总执行时间。 NOINDEX 不影响系统表，因为 DBCC CHECKFILEGROUP 总是对所有系统表索引进行检查。  
  
 ALL_ERRORMSGS  
 显示每个对象不受限制的错误数。 默认情况下显示所有错误消息。 指定或省略此选项都不起作用。  
  
 NO_INFOMSGS  
 取消显示所有信息性消息。  
  
 TABLOCK  
 导致 DBCC CHECKFILEGROUP 获取锁，而不使用内部数据库快照。  
  
 ESTIMATE ONLY  
 显示运行包含所有其他指定选项的 DBCC CHECKFILEGROUP 时所需的 tempdb 空间估计数量。  
  
 PHYSICAL_ONLY  
 限制为检查页、记录标头以及 B 树的物理结构的完整性。 此选项旨在以较低的开销检查文件组的物理一致性，同时，此项检查还可以检测可能危及数据安全的残缺页和常见的硬件故障。 DBCC CHECKFILEGROUP 完全运行的时间可能比在早期版本中完全运行的时间长得多。 导致此现象发生的原因如下：  
 -   逻辑检查更加全面。  
 -   要检查的某些基础结构更为复杂。  
 -   引入了许多新的检查以包含新增功能。  
 因此，使用 PHYSICAL_ONLY 选项可能会使 DBCC CHECKFILEGROUP 在大型文件组上运行的时间短得多，所以对需要频繁检查的生产系统，建议使用 DBCC CHECKFILEGROUP。 我们仍然建议定期执行 DBCC CHECKFILEGROUP 的完整运行。 这些运行的执行频率取决于各业务和生产环境特定的因素。 PHYSICAL_ONLY 始终表示 NO_INFOMSGS，不能与任何一个修复选项一同使用。  
  
> [!NOTE]  
>  指定 PHYSICAL_ONLY 会导致 DBCC CHECKFILEGROUP 跳过对 FILESTREAM 数据的所有检查。  
  
 MAXDOP  
 **适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014 SP2 到 [当前版本](../../sql-server/what-s-new-in-sql-server-2016.md)。  
  
 对于语句，替代 sp_configure 的“max degree of parallelism”配置选项 。 MAXDOP 可以超出使用 sp_configure 配置的值。 如果 MAXDOP 超出使用资源调控器配置的值，则数据库引擎会使用资源调控器 MAXDOP 值（如 ALTER WORKLOAD GROUP (Transact-SQL) 中所述）。 当使用 MAXDOP 查询提示时，所有和 max degree of parallelism 配置选项一起使用的语义规则均适用。 有关详细信息，请参阅 [配置 max degree of parallelism 服务器配置选项](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。  
  
> [!CAUTION]  
>  如果 MAXDOP 设置为零，服务器将选择最大并行度。  
  
## <a name="remarks"></a>备注  
DBCC CHECKFILEGROUP 和 DBCC CHECKDB 是相似的 DBCC 命令。 主要差异是 DBCC CHECKFILEGROUP 限于单个指定文件组和所需的表。
DBCC CHECKFILEGROUP 执行以下命令：
-   文件组的 [DBCC CHECKALLOC](../../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md)。  
-   文件组中的每个表和索引视图的 [DBCC CHECKTABLE](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)。  
  
不要求将 DBCC CHECKALLOC 或 DBCC CHECKTABLE 与 DBCC CHECKFILEGROUP 分开运行。
  
## <a name="internal-database-snapshot"></a>内部数据库快照  
DBCC CHECKFILEGROUP 使用内部数据库快照来提供执行这些检查所必需的事务一致性。 有关详细信息，请参阅[查看数据库快照的稀疏文件大小 (Transact-SQL)](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md) 以及 [DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md) 中的“DBCC 内部数据库快照使用情况”部分。
如果无法创建快照，或指定了 TABLOCK 选项，则 DBCC CHECKFILEGROUP 会获取锁以获得所需的一致性。 在这种情况下，需要排他数据库锁才能执行分配检查，需要共享表锁才能执行表检查。 TABLOCK 可在负荷较重的数据库上提高 DBCC CHECKFILEGROUP 的运行速度，但在运行 DBCC CHECKFILEGROUP 时会降低数据库的并发性。
  
> [!NOTE]  
>  对 tempdb 运行 DBCC CHECKFILEGROUP 不会执行任何分配检查，并且必须获得共享表锁才能执行表检查。 这是因为，为了提高性能，不允许对 tempdb 使用数据库快照。 这意味着，无法获得所需的事务一致性。  
  
## <a name="checking-objects-in-parallel"></a>并行检查对象  
默认情况下，DBCC CHECKFILEGROUP 对对象执行并行检查。 并行度由查询处理器自动确定。 最大并行度的配置与配置并行查询相同。 若要限制 DBCC 检查可使用的处理器的最大数目，请使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)。 有关详细信息，请参阅 [配置 max degree of parallelism 服务器配置选项](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。
通过使用跟踪标志 2528 可以禁用并行检查。 有关详细信息，请参阅[跟踪标志 (Transact-SQL)](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。
  
## <a name="nonclustered-indexes-on-separate-filegroups"></a>单独文件组的非聚集索引  
如果指定文件组中的非聚集索引与其他文件组中的表关联，则不会检查该索引，因为不能验证基表。
如果指定文件组中的表在其他文件组中有非聚集索引，则在下列情况下，不检查非聚集索引：
-   基表结构依赖于非聚集索引的结构。 不必扫描非聚集索引就可以验证基表。  
-   DBCC CHECKFILEGROUP 命令只对指定文件组中的对象进行验证。  
聚集索引和表不能属于不同文件组；所以，前述注意事项仅适用于非聚集索引。
  
## <a name="partitioned-tables-on-separate-filegroups"></a>单独文件组上的已分区表  
当已分区表存在于多个文件组上时，DBCC CHECKFILEGROUP 将检查存在于指定文件组上的分区行集，并忽略其他文件组中的行集。 信息性消息 2594 指出未检查的分区。 不会检查未驻留在指定文件组上的非聚集索引。
  
## <a name="understanding-dbcc-error-messages"></a>了解 DBCC 错误消息  
DBCC CHECKFILEGROUP 命令完成后，会将一条消息写入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志。 如果 DBCC 命令成功执行，则消息指示成功完成以及命令运行的时间。 如果 DBCC 命令在完成检查之前由于错误而停止，则消息将指示命令已终止，并指示状态值和命令运行的时间。 下表列出并说明了此消息中可包含的状态值。
  
|状态|描述|  
|-----------|-----------------|  
|0|出现错误号 8930。 这指示导致 DBCC 命令终止的元数据损坏。|  
|1|出现错误号 8967。 存在一个内部 DBCC 错误。|  
|2|在紧急模式数据库修复过程中出错。|  
|3|这指示导致 DBCC 命令终止的元数据损坏。|  
|4|检测到断言或访问违规。|  
|5|出现终止了 DBCC 命令的未知错误。|  
  
## <a name="error-reporting"></a>错误报告  
只要 DBCC CHECKFILEGROUP 检测到损坏错误，就会在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] LOG 目录中创建微型转储文件 (SQLDUMPnnnn.txt)。 如果为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例启用了“功能使用情况数据收集”和“错误报告”功能，该文件将被自动转发给 [!INCLUDE[msCoName](../../includes/msconame-md.md)]。 收集的数据将用于改进 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能。
转储文件包含 DBCC CHECKFILEGROUP 命令的结果以及其他诊断输出数据。 该文件拥有任意访问控制列表 (DACL)。 只有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户和 sysadmin 角色的成员才有权进行访问。 默认情况下，sysadmin 角色包含 Windows BUILTIN\Administrators 组和本地管理员组的所有成员。 如果数据收集进程失败，DBCC 命令不会失败。
  
## <a name="resolving-errors"></a>纠正错误  
如果 DBCC CHECKFILEGROUP 报告了任何错误，建议通过数据库备份还原数据库。 请注意，不能将修复操作指定为 DBCC CHECKFILEGROUP。
如果不存在备份，请运行包含指定的修复选项的 DBCC CHECKDB 来更正报告的错误。 如果报告了错误，则在列表末尾指定要使用的修复选项。 使用 REPAIR_ALLOW_DATA_LOSS 选项更正错误可能需要删除某些页，因而会删除部分数据。
  
## <a name="result-sets"></a>结果集  
DBCC CHECKFILEGROUP 返回以下结果集（值可能有所不同）：
-   在指定了 ESTIMATEONLY 或 NO_INFOMSGS 时除外。  
-   如果未指定数据库，则无论是否指定了任何选项（NOINDEX 除外），都默认为当前数据库。  
  
```
DBCC results for 'master'.  
DBCC results for 'sys.sysrowsetcolumns'.  
There are 630 rows in 7 pages for object 'sys.sysrowsetcolumns'.  
DBCC results for 'sys.sysrowsets'.  
There are 97 rows in 1 pages for object 'sys.sysrowsets'.  
DBCC results for 'sysallocunits'.  
There are 195 rows in 3 pages for object 'sysallocunits'.  
  
There are 2340 rows in 16 pages for object 'spt_values'.  
DBCC results for 'MSreplication_options'.  
There are 2 rows in 1 pages for object 'MSreplication_options'.  
CHECKFILEGROUP found 0 allocation errors and 0 consistency errors in database 'master'.  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
如果指定了 NO_INFOMSGS，则 DBCC CHECKFILEGROUP 返回：
  
```
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
 如果指定了 ESTIMATEONLY，则 DBCC CHECKFILEGROUP 返回（值可能有所不同）：  

```
Estimated TEMPDB space needed for CHECKALLOC (KB)
-------------------------------------------------   
15  
  
(1 row(s) affected)  
  
Estimated TEMPDB space needed for CHECKTABLES (KB)   
--------------------------------------------------   
207  
  
(1 row(s) affected)  
  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>权限  
要求具有 **sysadmin** 固定服务器角色或 **db_owner** 固定数据库角色的成员身份。
  
## <a name="examples"></a>示例  
### <a name="a-checking-the-primary-filegroup-in-the-a-database"></a>A. 检查数据库中的 PRIMARY 文件组  
下面的示例将检查当前数据库的主文件组。
  
```sql
DBCC CHECKFILEGROUP;  
GO  
```  
  
### <a name="b-checking-the-adventureworks-primary-filegroup-without-nonclustered-indexes"></a>B. 检查不含非聚集索引的 AdventureWorks PRIMARY 文件组  
下面的示例通过指定主文件组的标识号并指定 `NOINDEX`，对 `AdventureWorks2012` 数据库主文件组（不包括非聚集索引）进行检查。
  
```sql
USE AdventureWorks2012;  
GO  
DBCC CHECKFILEGROUP (1, NOINDEX);  
GO  
```  
  
### <a name="c-checking-the-primary-filegroup-with-options"></a>C. 检查带有选项的 PRIMARY 文件组  
下面的示例将检查 `master` 数据库主文件组并指定选项 `ESTIMATEONLY`。
  
```sql
USE master;  
GO  
DBCC CHECKFILEGROUP (1)  
WITH ESTIMATEONLY;  
```  
  
## <a name="see-also"></a>另请参阅  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[FILEGROUP_ID (Transact-SQL)](../../t-sql/functions/filegroup-id-transact-sql.md)  
[sp_helpfile (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)  
[sp_helpfilegroup (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpfilegroup-transact-sql.md)  
[sys.sysfilegroups (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-sysfilegroups-transact-sql.md)  
[DBCC CHECKDB (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[DBCC CHECKALLOC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md)  
[DBCC CHECKTABLE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)
  

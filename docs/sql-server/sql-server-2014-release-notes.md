---
title: SQL Server 2014 发行说明 | Microsoft Docs
description: 本发行说明文档介绍了在安装 Microsoft SQL Server 2014 (12.x) 版本或者解决其相关问题之前，应该了解的一些已知问题。
ms.custom: ''
ms.date: 07/22/2020
ms.prod: sql
ms.technology: release-landing
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: bf4c4922-80b3-4be3-bf71-228247f97004
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: adfd91c6771626aa0979da622d95766a2e4e5613
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97409623"
---
# <a name="sql-server-2014-release-notes"></a>SQL Server 2014 Release Notes
[!INCLUDE[tsql-appliesto-ss2014-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md)]
本文介绍 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] 版本的已知问题，包括相关服务包。

## <a name="sql-server-2014-service-pack-2-sp2"></a>SQL Server 2014 Service Pack 2 (SP2)

SQL Server 2014 SP2 包含针对 SQL Server 2014 SP1 CU7 发布的修补程序的汇总。 它包含以性能、可伸缩性以及基于来自客户和 SQL 社区的反馈的诊断为中心的改进。

### <a name="performance-and-scalability-improvements-in-sp2"></a>SP2 中性能和可伸缩性方面的改进

|Feature|说明|更多信息|
|---|---|---|
|自动 Soft NUMA 分区|可在报告每个 NUMA 节点 8 个或更多 CPU 的系统上自动配置 Soft NUMA。|[软件 NUMA (SQL Server)](../database-engine/configure-windows/soft-numa-sql-server.md)|
|缓冲池扩展|启用 SQL Server 缓冲池以缩放 8 TB 以上。|[缓冲池扩展](../database-engine/configure-windows/buffer-pool-extension.md)|
|动态内存对象缩放| 基于节点数和核心数对内存对象进行动态分区。 此增强功能消除了对跟踪标志 8048 post SQL 2014 SP2 的需求。|[Dynamic Memory Object Scaling](/archive/blogs/sql_server_team/dynamic-memory-object-scaling)（动态内存对象缩放）|
|DBCC CHECK* 命令的 MAXDOP 提示|此改进有助于通过除 sp_configure 值之外的 MAXDOP 设置运行 DBCC CHECKDB。|[提示 (Transact-SQL) - 查询](../t-sql/queries/hints-transact-sql-query.md)|
|SOS_RWLock 旋转锁改进|无需使用 SOS_RWLock 旋转锁，改为使用类似于内存中 OLTP 的无锁技术。 |[SOS_RWLock 重新设计](/archive/blogs/psssql/sql-2016-it-just-runs-faster-sos_rwlock-redesign)|
|空间本机实现|空间查询性能有重大改进。|[SQL Server 2012 和 2014 中的空间性能改进](https://support.microsoft.com/help/3107399/spatial-performance-improvements-in-sql-server-2012-and-2014)

### <a name="supportability-and-diagnostics-improvements-in-sp2"></a>SP2 中的可支持性和诊断改进

|Feature|说明|更多信息|
|---|---|---|
|AlwaysON 超时日志记录|添加租约超时消息的新日志记录功能，以便记录当前时间和预期的续订时间。 |[Improved AlwaysOn Availability Group Lease Timeout Diagnostics](/archive/blogs/alwaysonpro/improved-alwayson-availability-group-lease-timeout-diagnostics)（改进的 AlwaysOn 可用性组租约超时诊断）
|AlwaysON XEvent 和性能计数器|新 AlwaysON XEvent 和性能计数器，在对 AlwaysON 的延迟问题进行故障排除时改进诊断。 |[KB 3107172](https://support.microsoft.com/help/3107172/improve-tempdb-spill-diagnostics-by-using-extended-events-in-sql-serve) 和 [KB 3107400](https://support.microsoft.com/help/3107400/improved-tempdb-spill-diagnostics-in-showplan-xml-schema-in-sql-server)
|更改跟踪清除|新存储过程 sp_flush_CT_internal_table_on_demand 根据需要清除更改跟踪内部表。|[KB 3173157](https://support.microsoft.com/help/3173157/adds-a-stored-procedure-for-the-manual-cleanup-of-the-change-tracking)
|数据库克隆|通过克隆架构、元数据和统计信息（数据除外），使用新 DBCC 命令对现有生产数据库进行故障排除。 克隆的数据库并不用于生产环境。|[KB 3177838](https://support.microsoft.com/help/3177838/how-to-use-dbcc-clonedatabase-to-generate-a-schema-and-statistics-only)
|DMF 添加件|新 DMF sys.dm_db_incremental_stats_properties 公开每个分区的增量统计信息。|[KB 3170114](https://support.microsoft.com/help/3170114/update-to-add-dmf-sys-dm-db-incremental-stats-properties-in-sql-server)
|用于检索 SQL Server 中输入缓冲区的 DMF|现提供用于检索会话/请求 (sys.dm_exec_input_buffer) 的输入缓冲区的新 DMF。 它在功能上等同于 DBCC INPUTBUFFER。|[sys.dm_exec_input_buffer](../relational-databases/system-dynamic-management-views/sys-dm-exec-input-buffer-transact-sql.md)
|对复制的 DROP DDL 支持|允许从数据库和出版物中删除以文章形式包含在事务复制出版物中的表。|[KB 3170123](https://support.microsoft.com/help/3170123/supports-drop-table-ddl-for-articles-that-are-included-in-transactiona)
|针对 SQL 服务帐户的 IFI 特权|确定即时文件初始化 (IFI) 在 SQL Server 服务启动时是否有效。|[数据库文件初始化](../relational-databases/databases/database-instant-file-initialization.md)
|内存授予 - 处理问题|可以通过设置诊断提示的内存授予上限，在运行查询时利用诊断提示防止内存争用。|[KB 3107401](https://support.microsoft.com/help/3107401/new-query-memory-grant-options-are-available-min-grant-percent-and-max)
|轻型按运算符查询执行分析 |优化按运算符查询执行统计信息（如实际行数）的收集。|[Developers Choice:Query progress - anytime, anywhere](/archive/blogs/sql_server_team/query-progress-anytime-anywhere)（开发人员之选：随时随地查询进度）
|查询执行诊断|现可在查询执行计划中报告读取的实际行，以帮助改进查询性能故障排除。|[KB 3107397](https://support.microsoft.com/help/3107397/improved-diagnostics-for-query-execution-plans-that-involve-residual-p)
|Tempdb 溢出的查询执行诊断|哈希警告和排序警告现在具有其他列来跟踪物理 I/O 统计信息、使用的内存和受影响的行。 |[改进 temptdb 溢出诊断](https://support.microsoft.com/help/3107172/improve-tempdb-spill-diagnostics-by-using-extended-events-in-sql-serve)
|Tempdb 可支持性 |在服务器启动时，为 tempdb 文件数、tempdb 数据文件更改使用新的错误日志消息。|[KB 2963384](https://support.microsoft.com/help/2963384/fix-sql-server-crashes-when-the-log-file-of-tempdb-database-is-full-in)


此外，请注意以下修补程序：
- Xevent 调用堆栈现包含模块名称和偏移，而不是绝对地址。
- 加强了 XE 和 DMV 诊断之间的关联 - 仅 Query_hash 和 query_plan_hash 用于标识查询。 DMV 将二者定义为 varbinary(8)，而 XEvent 将其定义为 UINT64。 由于 SQL Server 不具有“unsigned bigint”，因此强制转换并非始终有效。 这一改进引入了等效于 query_hash 和 query_plan_hash 的新 XEvent 操作/筛选器列，除非二者均定义为 INT64。 此修补程序有助于关联 XE 和 DMV 之间的查询。
- BULK INSERT 和 BCP 中支持 UTF-8 - 现在 BULK INSERT 和 BCP 中启用了对导出和导入以 UTF-8 字符集编码的数据的支持。

### <a name="download-pages-and-more-information-for-sp2"></a>下载页和 SP2 详细信息

- [下载 Microsoft SQL Server 2014 Service Pack 2](https://www.microsoft.com/download/details.aspx?id=53168)
- [SQL Server 2014 Service Pack 2 现在可用](https://www.microsoft.com/download/details.aspx?id=53167)
- [SQL Server 2012 SP2 Express](https://www.microsoft.com/download/details.aspx?id=53167)
- [SQL Server 2014 SP2 功能包](https://www.microsoft.com/download/details.aspx?id=53164)
- [SQL Server 2014 SP2 报表生成器](https://www.microsoft.com/download/details.aspx?id=53163)
- [用于 Microsoft Sharepoint 的 SQL Server 2014 SP2 Reporting Services 加载项](https://www.microsoft.com/download/details.aspx?id=53162)
- [SQL Server 2014 SP2 语义语言统计信息](https://www.microsoft.com/download/details.aspx?id=53165)
- [SQL Server 2014 Service Pack 2 版本信息](https://support.microsoft.com/help/3171021/sql-server-2014-service-pack-2-release-information)

## <a name="sql-server-2014-service-pack-1-sp1"></a>SQL Server 2014 Service Pack 1 (SP1)

SQL Server 2014 SP1 包含 SQL Server 2014 CU 1 至 CU 5（包含 CU 5）中提供的修补程序，以及以前 SQL Server 2012 SP2 中附带的修补程序汇总。

> [!NOTE]
> 如果 SQL Server 实例已启用 SSISDB 目录，并且在升级到 SP1 时遇到安装错误，请按照有关此问题的说明[安装 SQL Server 2014 SP1 时，出现错误 912 或 3417](https://support.microsoft.com/help/3018269/error-912-or-3417-when-you-install-sql-server-2014-sp1-build-12-0-4050/) 进行操作。

### <a name="download-pages-and-more-information-for-sp1"></a>下载页和 SP1 详细信息

- [下载 Microsoft SQL Server 2014 Service Pack 1](https://www.microsoft.com/download/details.aspx?id=46694)
- [SQL Server 2014 Service Pack 1 已发布 - 已更新](/archive/blogs/sqlreleaseservices/sql-server-2014-service-pack-1-has-released-updated)
- [Microsoft SQL Server 2014 SP1 Express](https://www.microsoft.com/download/details.aspx?id=42299)
- [Microsoft SQL Server 2014 SP1 功能包](https://www.microsoft.com/download/details.aspx?id=46694)


## <a name="before-you-install-sql-server-2014-rtm"></a>安装 SQL Server 2014 RTM 之前

### <a name="limitations-and-restrictions-in-sql-server-2014-rtm"></a>SQL Server 2014 RTM 中的限制和局限

1.  不支持从 SQL Server 2014 CTP 1 升级到 SQL Server 2014 RTM。  
2.  不支持将 SQL Server 2014 CTP 1 与 SQL Server 2014 RTM 并行安装。  
3.  不支持将 SQL Server 2014 CTP 1 数据库附加到 SQL Server 2014 RTM 或将 SQL Server 2014 CTP 1 数据库还原到 SQL Server 2014 RTM。  

**解决方法：** 无。

#### <a name="upgrading-from-sql-server-2014-ctp-2-to-sql-server-rtm"></a>从 SQL Server 2014 CTP 2 升级到 SQL Server RTM
完全支持升级。 具体而言，您可以：

1.  将 SQL Server 2014 CTP 2 数据库附加到 SQL Server 2014 RTM 实例。    
2.  将在 SQL Server 2014 CTP 2 上执行的数据库备份还原到 SQL Server 2014 RTM 实例。    
3.  就地升级到 SQL Server 2014 RTM。
4.  滚动升级到 SQL Server 2014 RTM。 在启动滚动升级前，您需要切换到手动故障转移模式。 有关详细信息，请参考[在停机时间和数据丢失最少的情况下升级和更新可用性组服务器](../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md)。    
5.  通过 SQL Server 2014 CTP 2 中安装的事务性能收集组收集的数据不能通过 SQL Server 2014 RTM 中的 SQL Server Management Studio 查看，反之亦然。
  
#### <a name="downgrading-from-sql-server-2014-rtm-to-sql-server-2014-ctp-2"></a>从 SQL Server 2014 RTM 降级到 SQL Server 2014 CTP 2  
不支持此操作。  
  
**解决方法：** 没有针对降级的解决方法。 我们建议你在升级到 SQL Server 2014 RTM 之前备份数据库。  
  
#### <a name="incorrect-version-of-streaminsight-client-on-sql-server-2014-mediaisocab"></a>SQL Server 2014 介质/ISO/CAB 上 StreamInsight 客户端的版本不正确  
错误版本的 StreamInsight.msi 和 StreamInsightClient.msi 位于 SQL Server 介质/ISO/CAB 上的以下路径中 (StreamInsight\\\<Architecture\>\\\<Language ID\>)。  
  
**解决方法：** 从 [SQL Server 2014 功能包下载页](https://www.microsoft.com/download/details.aspx?id=57474)下载和安装正确的版本。  
  
### <a name="product-documentation-rtm"></a><a name="ProdDoc"></a>产品文档 RTM
  
不提供某些语言的报表生成器和 PowerPivit 内容。 

**问题：** 不提供以下语言的报表生成器内容：  
  
-   希腊语 (el-GR)  
-   挪威语（博克马尔）(nb-NO)  
-   芬兰语 (fi-FI)  
-   丹麦语 (da-DK)  
  
在 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]中，此内容在产品随附的 CHM 文件中提供并且提供这些语言的内容。 这些 CHM 文件不再随产品一起提供，并且报表生成器内容仅在 MSDN 上提供。 MSDN 不支持这些语言。 报表生成器也已从 TechNet 中删除并且在这些支持的语言中不再提供。  
  
**解决方法：** 无。  
  
**问题：** 不提供以下语言的 Power Pivot 内容：
  
-   希腊语 (el-GR)  
-   挪威语（博克马尔）(nb-NO)  
-   芬兰语 (fi-FI)  
-   丹麦语 (da-DK)  
-   捷克语 (cs-CZ)  
-   匈牙利语 (hu-HU)  
-   荷兰语（荷兰）(nl-NL)  
-   波兰语 (pl-PL)  
-   瑞典语 (sv-SE)  
-   土耳其语 (tr-TR)  
-   葡萄牙语（葡萄牙）(pt-PT)  
  
在 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]中，已在 TechNet 上提供此内容并且提供这些语言的内容。 此内容已从 TechNet 中删除并且不再提供这些支持的语言的内容。  
  
**解决方法：** 无。  
  
### <a name="database-engine-rtm"></a><a name="DBEngine"></a>数据库引擎 (RTM)
  
#### <a name="changes-made-for-standard-edition-in-sql-server-2014-rtm"></a>SQL Server 2014 RTM 中针对 Standard Edition 所做的更改  
SQL Server 2014 Standard 具有以下更改：  
  
-   缓冲池扩展功能允许使用的最大大小为所配置内存的 4 倍。    
-   最大内存从 64 GB 增加到 128 GB。  
 
#### <a name="memory-optimization-advisor-flags-default-constraints-as-incompatible"></a>内存优化顾问将默认约束标记为不兼容  
**问题：** SQL Server Management Studio 中的内存优化顾问将所有默认约束标记为不兼容。 内存优化表中不是所有默认约束都支持；顾问并不区分支持和不支持的默认约束类型。 支持的默认约束包括本机编译的存储过程中支持的所有常量、表达式和内置函数。 要查看本机编译的存储过程中支持的函数的列表，请参阅 [本机编译的存储过程中支持的构造](../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md?viewFallbackFrom=sql-server-2014)(#本机编译的存储过程中支持的构造)。  
  
**解决方法：** 如果要使用顾问来识别阻塞程序，请忽略兼容的默认约束。 要使用内存优化顾问迁移具有兼容默认约束但没有其他阻塞程序的表，请按照以下步骤操作：  
  
1.  从表定义中删除默认约束。    
2.  使用顾问生成对表的迁移脚本。    
3.  在迁移脚本中添加回默认约束。    
4.  执行迁移脚本。  
  
#### <a name="informational-message-file-access-denied-incorrectly-reported-as-an-error-in-the-sql-server-2014-error-log"></a>信息性消息“文件访问被拒绝”在 SQL Server 2014 错误日志中不正确地报告为错误  
**问题：** 重启具有包含内存优化表的数据库的服务器时，可在 SQL Server 2014 错误日志中看到以下类型的错误消息：  
  
```  
[ERROR]Unable to delete file C:\Program Files\Microsoft SQL   
Server\....old.dll. This error may be due to a previous failure to unload   
memory-optimized table DLLs.  
```  
此消息实际上是一条信息性消息，不需要用户执行任何操作。  
  
**解决方法：** 无。 这是一条信息性消息。  
  
#### <a name="missing-index-details-incorrectly-report-included-columns-for-memory-optimized-table"></a>缺少索引详细信息不正确地报告内存优化表的包含列  
**问题：** 如果 SQL Server 2014 检测到对内存优化表的查询缺失索引，则会在 SHOWPLAN_XML 中和缺失索引 DMV（如 sys.dm_db_missing_index_details）中报告缺失索引。 在某些情况下，缺少索引详细信息将包含有包含列。 因为所有列都是使用内存优化表的所有索引隐式包含的，所以不允许使用内存优化索引显式指定包含列。  
  
**解决方法：** 不使用内存优化表上的索引指定 INCLUDE 子句。  
  
#### <a name="missing-index-details-omit-missing-indexes-when-a-hash-index-exists-but-is-not-suitable-for-the-query"></a>哈希索引存在但不适用于查询时缺失索引详细信息忽略缺失索引  
**问题：** 如果查询中引用的内存优化表的列中存在哈希索引，但该索引不能用于查询，则 SQL Server 2014 将不会始终在 SHOWPLAN_XML 和 DMV sys.dm_db_missing_index_details 中报告缺失索引。  
  
特别是查询包含涉及索引键列子集的相等谓词或包含涉及索引键列的相等谓词时，哈希索引不能原样使用，这时就需要不同的索引才能有效地执行查询。  
  
**解决方法：** 使用哈希索引时，检查查询和查询计划以确定查询是否可受益于对索引键子集的 Index Seek 操作或对不等谓词的 Index Seek 操作。 如果需要查找索引键子集，请使用非聚集索引或对需要查找的列使用哈希索引。 如果需要查找不等谓词，请使用非聚集索引而不是哈希索引。  
  
#### <a name="failure-when-using-a-memory-optimized-table-and-memory-optimized-table-variable-in-the-same-query-if-the-database-option-read_committed_snapshot-is-set-to-on"></a>在同一查询中使用内存优化表和内存优化表变量时，如果数据库选项 READ_COMMITTED_SNAPSHOT 设置为 ON，就会失败  
**问题：** 如果数据库选项 READ_COMMITTED_SNAPSHOT 设置为 ON，并访问用户事务上下文之外的同一语句中的内存优化表和内存优化表变量，则可能会看到以下错误消息：  
  
```  
Msg 41359  
A query that accesses memory optimized tables using the READ COMMITTED  
isolation level, cannot access disk based tables when the database option  
READ_COMMITTED_SNAPSHOT is set to ON. Provide a supported isolation level  
for the memory optimized table using a table hint, such as WITH (SNAPSHOT).  
```  
  
**解决方法：** 使用包含表变量的表提示 WITH (SNAPSHOT) 或将数据库选项 MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT 设置为 ON，以便使用以下语句：  
  
```  
ALTER DATABASE CURRENT   
SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON  
```  
  
#### <a name="procedure-and-query-execution-statistics-for-natively-compiled-stored-procedures-record-worker-time-in-multiples-of-1000"></a>本机编译存储过程的过程和查询执行统计信息按 1000 倍记录工作线程时间  
**问题：** 使用 sp_xtp_control_proc_exec_stats 或 sp_xtp_control_query_exec_stats 启用本机编译存储过程的过程或查询执行统计信息集合后，将会在 DMV sys.dm_exec_procedure_stats 和 sys.dm_exec_query_stats 中看到按 1000 的倍数报告的 *_worker_time。 工作线程时间少于 500 毫秒的查询执行将按 worker_time 为 0 报告。  
  
**解决方法：** 无。 对于本机编译存储过程中短时间运行的查询，不要依赖执行状态 DMV 中报告的 worker_time。  
  
#### <a name="error-with-showplan_xml-for-natively-compiled-stored-procedures-that-contain-long-expressions"></a>包含长表达式的本机编译存储过程使用 SHOWPLAN_XML 的错误  
**问题：** 如果本机编译存储过程包含长表达式，使用 T-SQL 选项 SET SHOWPLAN_XML ON 或使用 Management Studio 中的“显示估计的执行计划”选项获取过程的 SHOWPLAN_XML 可能导致以下错误：  
  
```  
Msg 41322. MAT/PIT export/import encountered a failure for memory  
optimized table or natively compiled stored procedure with object ID  
278292051 in database ID 6. The error code was  
0xc00cee81.  
```  
  
**解决方法：** 有两个建议的解决方法：  
  
1.  为表达式添加括号，类似以下示例：  
  
    不是：  
  
    ```  
    SELECT @v0 + @v1 + @v2 + ... + @v199  
    ```  
  
    写入：  
  
    ```  
    SELECT((@v0 + ... + @v49) + (@v50 + ... + @v99)) + ((@v100 + ... + @v149) + (@v150 + ... + @v199))  
    ```  
  
2.  使用稍微简化的表达式为显示计划目的创建第二个过程，该计划的形状应大致相同。 例如，不是：  
  
    ```  
    SELECT @v0 +@v1 +@v2 +...+@v199  
    ```  
  
    写入：  
  
    ```  
    SELECT @v0 +@v1  
    ```  
  
#### <a name="using-a-string-parameter-or-variable-with-datepart-and-related-functions-in-a-natively-compiled-stored-procedure-results-in-an-error"></a>在本机编译存储过程中对 DATEPART 和相关函数使用字符串参数或变量会导致错误  
**问题：** 在对内置函数 DATEPART、DAY、MONTH 和 YEAR 使用利用字符串参数或变量的本机编译存储过程时，系统会显示一条错误消息，说明本机编译存储过程不支持 datetimeoffset。  
  
**解决方法：** 将字符串参数或变量分配给一个新的 datetime2 类型的变量，并在函数 DATEPART、DAY、MONTH 或 YEAR 中使用该变量。 例如：  
  
```  
DECLARE @d datetime2 = @string  
DATEPART(weekday, @d)  
```  
  
#### <a name="native-compilation-advisor-flags-delete-from-clauses-incorrectly"></a>本机编译顾问错误标记 DELETE FROM 子句  
**问题：** 本机编译顾问将存储过程内的 DELETE FROM 子句错误标记为不兼容。  
  
**解决方法：** 无。  
  
#### <a name="register-through-ssms-adds-dac-meta-data-with-mismatched-instance-ids"></a>通过 SSMS 注册会为 DAC 元数据添加不匹配的实例 ID  
**问题：** 通过 SQL Server Management Studio 注册或删除数据层应用程序包 (.dacpac) 时，sysdac* 表不会正确更新，使用户无法查询数据库的 dacpac 历史记录。  sysdac_history_internal 和 sysdac_instances_internal 的 instance_id 不匹配，无法联接。  
  
**解决方法：** 此问题已通过 [Data-Tier Application Framework](https://www.microsoft.com/download/details.aspx?id=100297) 的功能包再分发得到修复。  应用更新后，所有的新历史记录条目都将使用 sysdac_instances_internal 表中为 instance_id 列出的值。  
  
如果已经出现 instance_id 值不匹配的问题，纠正不匹配值的唯一方法是以拥有 MSDB 数据库写入权限的用户身份连接到服务器并更新 instance_id 值使其匹配。  如果从同一数据库获取多个注册和撤消注册事件，可能需要查看时间/日期来确定与当前 instance_id 值匹配的记录。  
  
1.  在 SQL Server Management Studio 中使用对 MSDB 具有更新权限的登录名连接到服务器。    
2.  使用 MSDB 数据库打开一个新查询。    
3.  运行此查询查看所有活动的 DAC 实例。  找到你要更正的实例并记下 instance_id：  
  
    `select * from` sysdac_instances_internal  
  
4.  运行此查询可查看所有历史记录条目：  
  
    `select * from` sysdac_history_internal  
  
5.  确定与待修复实例对应的行。 
6.  将 sysdac_history_internal.instance_id 值更新为在步骤 3 中记下的值（来自 sysdac_instances_internal 表）：  
  
    `update` sysdac_history_internal `set` instance_id = '\<value from step 3\>' `where` \<expression that matches the rows you want to update\>  
  
### <a name="reporting-services-rtm"></a><a name="SSRS"></a>Reporting Services (RTM)
  
#### <a name="the-sql-server-2012-reporting-services-native-mode-report-server-cannot-run-side-by-side-with-sql-server-2014-reporting-services-sharepoint-components"></a>SQL Server 2012 Reporting Services 本机模式报表服务器不能与 SQL Server 2014 Reporting Services SharePoint 组件并行运行  
**问题：** 当同一服务器上安装了 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] SharePoint 组件时，[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 本机模式 Windows 服务“SQL Server Reporting Services”(ReportingServicesService.exe) 将无法启动。  
  
**解决方法：** 卸载 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] SharePoint 组件并重启 Microsoft SQL Server 2012 Reporting Services Windows 服务。  
  
**详细信息：**  
  
[!INCLUDE[ssSQL11](../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 本机模式不能在以下条件之一中并行运行：  
  
-   用于 SharePoint 产品的 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 加载项    
-   [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] SharePoint 共享服务  
  
并行安装会阻止 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 本机模式 Windows 服务启动。 Windows 事件日志中将看到类似于此处描述的错误消息：  
  
```  
Log Name:   Application  
Source:          Report Server (<SQL instance ID>)  
Event ID:        117  
Task Category:   Startup/Shutdown  
Level:           Error  
Keywords:        Classic  
Description:     The report server database is an invalid version.  
  
Log Name:      Application  
Source:        Report Server (<SQL instance ID>)  
Event ID:      107  
Task Category: Management  
Level:         Error  
Keywords:      Classic  
Description:   Report Server (DENALI) cannot connect to the report server database.  
```  
  
有关详细信息，请参阅 [SQL Server 2014 Reporting Services 提示、技巧和故障排除](https://go.microsoft.com/fwlink/?LinkID=391254)。  
  
#### <a name="required-upgrade-order-for-multi-node-sharepoint-farm-to-sql-server-2014-reporting-services"></a>多节点 SharePoint 场到 SQL Server 2014 Reporting Services 所需的升级顺序  
**问题：** 如果 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]SharePoint 共享服务实例在 SharePoint 产品的 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 外接程序的所有实例之前升级，多节点场中的报表呈现将失败。  
  
**解决方法：** 在多节点 SharePoint 场中：  
  
1.  首先升级 SharePoint 产品的所有 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 外接程序实例。    
2.  然后再升级 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] SharePoint 共享服务的所有实例。  
  
有关详细信息，请参阅 [SQL Server 2014 Reporting Services 提示、技巧和故障排除](https://go.microsoft.com/fwlink/?LinkID=391254)。  
  
### <a name="sql-server-2014-rtm-on-azure-virtual-machines"></a><a name="AzureVM"></a> Azure 虚拟机上的 SQL Server 2014 RTM  
  
#### <a name="the-add-azure-replica-wizard-returns-an-error-when-configuring-an-availability-group-listener-in-azure"></a>在 Azure 中配置可用性组侦听器时添加 Azure 副本向导返回错误  
**问题：** 如果可用性组具有侦听器，当你尝试在 Azure 中配置可用性组侦听器时，添加 Azure 副本向导将返回错误。  
  
此问题是因为可用性组侦听器需要在每个托管可用性组副本的子网中分配一个 IP 地址，包括 Azure 子网。  
  
**解决方法：**  
  
1.  在“侦听器”页上，在将托管可用性组副本的 Azure 子网中为可用性组侦听器分配一个空闲的静态 IP 地址。  
  
    此解决方法将允许向导在 Azure 中完成添加副本的工作。  
  
2.  向导完成后，将需要按照 [Azure 中 AlwaysOn 可用性组的侦听器配置](/previous-versions/azure/dn376546(v=azure.100))中所述在 Azure 中完成侦听器配置  
  
### <a name="analysis-services-rtm"></a><a name="SSAS"></a>Analysis Services (RTM)
  
#### <a name="msolap5-must-be-downloaded-installed-and-registered-for-a-sharepoint-2010-new-farm-configured-with-sql-server-2014"></a>必须为使用 SQL Server 2014 配置的 SharePoint 2010 新场下载、安装和注册 MSOLAP.5  
**问题：**  
  
-   对于通过 SQL Server 2014 RTM 部署使用 SQL Server 2014 场配置的 SharePoint 2013 新场，必须下载、安装和注册 SharePoint 2010 MSOLAP.5 的情况，PowerPivot 工作簿无法连接到数据模型，因为连接字符串中引用的访问接口未安装。  
  
**解决方法：**  
  
1.  从 [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)] 功能包下载 MSOLAP.5 访问接口。 将访问接口安装在运行 Excel Services 的应用程序服务器上。 有关详细信息，请参阅“用于 Microsoft SQL Server 2012 SP1 的 Microsoft Analysis Services OLE DB 提供程序”[Microsoft SQL Server 2012 SP1 功能包](https://www.microsoft.com/download/details.aspx?id=35575)部分。  
  
2.  将 MSOLAP.5 注册为 SharePoint Excel Services 中的受信任数据访问接口。 有关详细信息，请参阅 [将 MSOLAP.5 添加为 Excel Services 中的受信任数据访问接口](/analysis-services/power-pivot-for-sharepoint-ssas?viewFallbackFrom=sql-server-ver15)。  
  
**详细信息：**  
  
-   [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] 包括 MSOLAP.6。 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 和 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)][!INCLUDE[ssGemini](../includes/ssgemini-md.md)] 工作簿使用 MSOLAP.5。 如果运行 Excel Services 的计算机上没有安装 MSOLAP.5，Excel Services 就无法加载数据模型。  
  
#### <a name="msolap5-must-be-downloaded-installed-and-registered-for-a-sharepoint-2013-new-farm-configured-with-sql-server-2014"></a>必须为使用 SQL Server 2014 配置的 SharePoint 2013 新场下载、安装和注册 MSOLAP.5  
**问题：**  
  
-   对于使用 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] 部署配置的 SharePoint 2013 场，引用 MSOLAP.5 访问接口的 Excel 工作簿无法连接到表格数据模型，因为未安装连接字符串中引用的访问接口。  
  
**解决方法：**  
  
1.  从 [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)] 功能包下载 MSOLAP.5 访问接口。 将访问接口安装在运行 Excel Services 的应用程序服务器上。 有关详细信息，请参阅“用于 Microsoft SQL Server 2012 SP1 的 Microsoft Analysis Services OLE DB 提供程序”[Microsoft SQL Server 2012 SP1 功能包](https://www.microsoft.com/download/details.aspx?id=35575)部分。  
  
2.  将 MSOLAP.5 注册为 SharePoint Excel Services 中的受信任数据访问接口。 有关详细信息，请参阅 [将 MSOLAP.5 添加为 Excel Services 中的受信任数据访问接口](/analysis-services/power-pivot-for-sharepoint-ssas?viewFallbackFrom=sql-server-ver15)。  
  
**详细信息：**  
  
-   [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] 包括 MSOLAP.6。 但 SQL Server 2014 PowerPivot 工作簿使用 MSOLAP.5。 如果运行 Excel Services 的计算机上没有安装 MSOLAP.5，Excel Services 就无法加载数据模型。  
  
#### <a name="corrupt-data-refresh-schedules-rtm"></a>损坏数据刷新计划 (RTM)
**问题：**  
  
-   更新刷新计划后计划损坏且不可用。  
  
**解决方法：**  
  
1.  在 Microsoft Excel 中，清除自定义高级属性。 请参阅以下知识库文章的“解决方法”部分：[KB 2927748](https://support.microsoft.com/kb/2927748)。  
  
**详细信息：**  
  
-    如果刷新计划的序列化长度小于原有计划，则在为工作簿更新数据刷新计划时，缓冲区大小不会正确更新，并且新计划信息将与旧计划信息合并，导致计划损坏。  
  
### <a name="data-quality-services-rtm"></a><a name="DQS"></a>Data Quality Services (RTM)
  
#### <a name="no-cross-version-support-for-data-quality-services-in-master-data-services"></a>Master Data Services 中没有对 Data Quality Services 的跨版本支持  
**问题：** 不支持以下方案：  
  
-   Master Data Services 2014 承载在安装了 Data Quality Services 2012 的 SQL Server 2012 中的 SQL Server 数据库引擎数据库中。  
  
-   Master Data Services 2012 承载在安装了 Data Quality Services 2014 的 SQL Server 2014 中的 SQL Server 数据库引擎数据库中。  
  
**解决方法：** 使用相同版本的 Master Data Services 作为数据库引擎数据库和 Data Quality Services。  
  
### <a name="upgrade-advisor-issues-rtm"></a><a name="UA"></a>升级顾问问题 (RTM)
  
#### <a name="sql-server-2014-upgrade-advisor-reports-irrelevant-upgrade-issues-for-sql-server-reporting-services"></a>SQL Server 2014 升级顾问报告与 SQL Server Reporting Services 不相关的升级问题  
**问题：** SQL Server 2014 介质附带的 SQL Server 升级顾问 (SSUA) 在分析 SQL Server Reporting Services 服务器时不正确地报告多个错误。  
  
**解决方法：** 此问题已在 [SSUA 的 SQL Server 2014 功能包](https://www.microsoft.com/download/details.aspx?id=57474)中提供的 SQL Server 升级顾问中得到修复。  
  
#### <a name="sql-server-2014-upgrade-advisor-reports-an-error-when-analyzing-sql-server-integration-services-server"></a>SQL Server 2014 升级顾问在分析 SQL Server Integration Services 服务器时报告错误  
**问题：** SQL Server 2014 介质附带的 SQL Server 升级顾问 (SSUA) 会在分析 SQL Server Integration Services 服务器时报告错误。  向用户显示的错误如下：  
  
```  
The installed version of Integration Services does not support Upgrade Advisor.   
The assembly information is "Microsoft.SqlServer.ManagedDTS, Version=11.0.0.0,   
Culture=neutral, PublicKeyToken=89845dcd8080cc91  
```  
  
**解决方法：** 此问题已在 [SSUA 的 SQL Server 2014 功能包](https://www.microsoft.com/download/details.aspx?id=57474)中提供的 SQL Server 升级顾问中得到修复。  
  
[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
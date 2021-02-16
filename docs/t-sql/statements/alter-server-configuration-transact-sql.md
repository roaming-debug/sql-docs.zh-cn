---
description: ALTER SERVER CONFIGURATION (Transact-SQL)
title: ALTER SERVER CONFIGURATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER SERVER CONFIGURATION
- ALTER_SERVER_CONFIGURATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER CONFIGURATION, ALTER
- process affinity, setting
- ALTER SERVER CONFIGURATION statement
- setting process affinity
ms.assetid: f3059e42-5f6f-4a64-903c-86dca212a4b4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e1e9186d4b0de1540f4d8e7a1ca54c15a36817ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340621"
---
# <a name="alter-server-configuration-transact-sql"></a>ALTER SERVER CONFIGURATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

修改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中当前服务器的全局配置设置。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  

```syntaxsql
ALTER SERVER CONFIGURATION  
SET <optionspec>   
[;]  
  
<optionspec> ::=  
{  
     <process_affinity>  
   | <diagnostic_log>  
   | <failover_cluster_property>  
   | <hadr_cluster_context>  
   | <buffer_pool_extension>  
   | <soft_numa>  
   | <memory_optimized>
}  
  
<process_affinity> ::=   
   PROCESS AFFINITY   
   {  
     CPU = { AUTO | <CPU_range_spec> }   
   | NUMANODE = <NUMA_node_range_spec>   
   }  
   <CPU_range_spec> ::=   
      { CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]   
  
   <NUMA_node_range_spec> ::=   
      { NUMA_node_ID | NUMA_node_ID TO NUMA_node_ID } [ ,...n ]  
  
<diagnostic_log> ::=   
   DIAGNOSTICS LOG   
   {   
     ON    
   | OFF    
   | PATH = { 'os_file_path' | DEFAULT }    
   | MAX_SIZE = { 'log_max_size' MB | DEFAULT }    
   | MAX_FILES = { 'max_file_count' | DEFAULT }    
   }  
  
<failover_cluster_property> ::=   
   FAILOVER CLUSTER PROPERTY <resource_property>  
   <resource_property> ::=  
      {  
        VerboseLogging = { 'logging_detail' | DEFAULT }    
      | SqlDumperDumpFlags = { 'dump_file_type' | DEFAULT }  
      | SqlDumperDumpPath = { 'os_file_path'| DEFAULT }  
      | SqlDumperDumpTimeOut = { 'dump_time-out' | DEFAULT }  
      | FailureConditionLevel = { 'failure_condition_level' | DEFAULT }  
      | HealthCheckTimeout = { 'health_check_time-out' | DEFAULT }  
      }  
  
<hadr_cluster_context> ::=  
   HADR CLUSTER CONTEXT = { 'remote_windows_cluster' | LOCAL }  
  
<buffer_pool_extension>::=  
    BUFFER POOL EXTENSION   
    { ON ( FILENAME = 'os_file_path_and_name' , SIZE = <size_spec> )   
    | OFF }  
  
    <size_spec> ::=  
        { size [ KB | MB | GB ] }  
  
<soft_numa> ::=  
    SOFTNUMA  
    { ON | OFF }  

<memory-optimized> ::=   
   MEMORY_OPTIMIZED   
   {   
     ON 
   | OFF
   | [ TEMPDB_METADATA = { ON [(RESOURCE_POOL='resource_pool_name')] | OFF }
   | [ HYBRID_BUFFER_POOL = { ON | OFF }
   }  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
**\<process_affinity> ::=**  
  
PROCESS AFFINITY  
使硬件线程与 CPU 相关联。  
  
CPU = { AUTO | \<CPU_range_spec> }  
将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工作线程分发到指定范围内的各 CPU。 超出了指定范围的 CPU 将不会分配线程。  
  
AUTO  
指定没有为 CPU 分配线程。 操作系统可以基于服务器工作负荷在 CPU 之间自由地移动线程。 这是默认设置，也是推荐设置。  
  
\<CPU_range_spec> ::=  
指定要将线程指定给的 CPU 或 CPU 范围。  
  
{ CPU_ID | CPU_ID  TO  CPU_ID } [ ,...n ]  
一个或多个 CPU 的列表。 CPU ID 从 0 开始且为 **integer** 值。  
  
NUMANODE = \<NUMA_node_range_spec>  
将线程分配给属于指定 NUMA 节点或节点范围的所有 CPU。  
  
\<NUMA_node_range_spec> ::=  
指定 NUMA 节点或 NUMA 节点范围。  
  
{ NUMA_node_ID | NUMA_node_ID  TO NUMA_node_ID } [ ,...n ]  
一个或多个 NUMA 节点的列表。 NUMA 节点 ID 从 0 开始且为 **integer** 值。  
  
**\<diagnostic_log> ::=**  
  
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）。  

  
DIAGNOSTICS LOG  
开始或停止记录 sp_server_diagnostics 过程捕获的诊断数据。 此参数还设置 SQLDIAG 日志配置参数，例如日志文件滚动更新计数、日志文件大小和文件位置。 有关详细信息，请参阅 [查看和读取故障转移群集实例诊断日志](../../sql-server/failover-clusters/windows/view-and-read-failover-cluster-instance-diagnostics-log.md)。  
  
ON  
在 PATH 文件选项指定的位置启动对 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 诊断数据的日志记录。 此参数是默认参数。  
  
OFF  
停止对诊断数据的日志记录。  
  
PATH = { 'os_file_path' | DEFAULT }  
指示诊断日志位置的路径。 默认位置是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例的安装文件夹中的 \<\MSSQL\Log>。  
  
MAX_SIZE = { 'log_max_size' MB | DEFAULT }  
每个诊断日志可以增长到的最大大小 (MB)。 默认值为 100 MB。  
  
MAX_FILES = { 'max_file_count' | DEFAULT }  
可以存储在计算机上的诊断日志文件的最大数量，超过该数量后这些文件将被新的诊断日志所取代。  
  
**\<failover_cluster_property> ::=**  
  
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）。    
  
FAILOVER CLUSTER PROPERTY  
修改 SQL Server 资源专用的故障转移群集属性。  
  
VERBOSE LOGGING = { 'logging_detail' | DEFAULT }  
设置 SQL Server 故障转移群集的日志记录级别。 可以通过启用该属性在错误日志中提供更多详细信息以排除故障。  
  
-   0 - 关闭日志记录（默认值）  
  
-   1 - 仅限错误  
  
-   2 - 错误和警告  
  
在资源故障转移方案中，SQL Server 资源 DLL 可以在发生故障转移之前获取转储文件。 这同时适用于 FCI 和可用性组技术。 SQL Server 资源 DLL 确定 SQL Server 资源失败时，SQL Server 资源 DLL 使用 Sqldumper.exe 实用程序获取 SQL Server 进程的转储文件。 若要确保 Sqldumper.exe 实用程序在资源故障转移时成功生成转储文件，必须将以下三个属性设置为先决条件：SqlDumperDumpTimeOut、SqlDumperDumpPath、SqlDumperDumpFlags。

SQLDUMPEREDUMPFLAGS  
确定 SQL Server SQLDumper 实用工具生成的转储文件的类型。 默认设置为 0。 此设置使用十进制值而不是十六进制值。 对于小型转储使用 288，对于具有间接内存的小型转储使用 296，对于已筛选转储使用 33024。 有关详细信息，请参阅 [SQL Server Dumper 实用工具知识库文章](/troubleshoot/sql/tools/use-sqldumper-generate-dump-file)。  
  
SQLDUMPERDUMPPATH = { 'os_file_path' | DEFAULT }  
SQLDumper 实用工具存储转储文件的位置。 有关详细信息，请参阅 [SQL Server Dumper 实用工具知识库文章](/troubleshoot/sql/tools/use-sqldumper-generate-dump-file)。  
  
SQLDUMPERDUMPTIMEOUT = { 'dump_time-out' | DEFAULT }  
SQLDumper 实用程序在发生 SQL Server 故障时生成转储的超时值（以毫秒为单位）。 默认值为 0，表示完成该转储没有时间限制。 有关详细信息，请参阅 [SQL Server Dumper 实用工具知识库文章](/troubleshoot/sql/tools/use-sqldumper-generate-dump-file)。  
  
 FAILURECONDITIONLEVEL = { 'failure_condition_level' | DEFAULT }  
 SQL Server 故障转移群集实例应在什么状况下进行故障转移或重启。 默认值是 3，表示 SQL Server 资源将在出现严重的服务器错误时进行故障转移或重启。 有关此故障条件级别和其他故障条件级别的详细信息，请参阅[配置 FailureConditionLevel 属性设置](../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md)。  
  
HEALTHCHECKTIMEOUT = { 'health_check_time-out' | DEFAULT }  
超时值，即 SQL Server 数据库引擎资源 DLL 在认定 SQL Server 实例不响应之前应等待服务器运行状况信息的时间。 该超时值用毫秒表示。 默认值为 60,000 毫秒（60 秒）。  
  
**\<hadr_cluster_context> ::=**  
  
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）。   
  
HADR CLUSTER CONTEXT = { 'remote\_windows\_cluster' | LOCAL }   
将服务器实例的 HADR 群集上下文切换到指定的 Windows Server 故障转移群集 (WSFC)。 “HADR 群集上下文”用于确定什么 WSFC 管理由服务器实例承载的可用性副本的元数据。 仅在 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 跨群集迁移到新 WSFC r 上的 [!INCLUDE[ssSQL11SP1](../../includes/sssql11sp1-md.md)] 或更高版本实例的过程中，才使用 SET HADR CLUSTER CONTEXT 选项。  
  
只能将 HADR 群集上下文从本地 WSFC 切换到远程 WSFC。 然后可以选择从远程 WSFC 切换回本地 WSFC。 只有在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例不托管任何可用性副本时，才能将 HADR 群集上下文切换到远程群集。  
  
远程 HADR 群集上下文随时可以切换回本地群集。 但是，只要服务器实例在托管任何可用性副本，该上下文就无法再进行切换。 
  
若要标识目标群集，请指定下列值之一：  
  
*windows_cluster*  
WSFC 的网络名称。 您可以指定短名称或者完整的域名称。 为了找到短名称的目标 IP 地址，ALTER SERVER CONFIGURATION 使用 DNS 解析。 在某些情况下，短名称可能导致混淆，DNS 可能返回错误的 IP 地址。 我们建议您指定完整的域名。  
  
> [!NOTE] 
> 不再支持使用此设置的跨群集迁移。 要执行跨群集迁移，请使用分布式可用性组或其他一些方法（如日志传送）。 
  
LOCAL  
本地 WSFC。  
  
有关详细信息，请参阅[更改服务器实例的 HADR 群集上下文 (SQL Server)](../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md)。  
  
**\<buffer_pool_extension>::=**  
  
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）。    
  
ON  
启用缓冲池扩展选项。 此选项通过使用永久性存储扩展缓冲池的大小。 诸如固态硬盘 (SSD) 之类的永久性存储可使池中的数据页面保持干净。 有关此功能的详细信息，请参阅[缓冲池扩展](../../database-engine/configure-windows/buffer-pool-extension.md)。不是所有 SQL Server 版本都提供缓冲池扩展。 有关详细信息，请参阅 [SQL Server 2016 的各版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)。  
  
FILENAME = 'os_file_path_and_name'  
定义缓冲池扩展缓存文件的目录路径和名称。 文件扩展名必须指定为 .BPE。 禁用 BUFFER POOL EXTENSION 后才能修改 FILENAME。  
  
SIZE = *size* [ **KB** | MB | GB ]  
定义缓存大小。 默认大小规格是 KB。 最小大小为 Max Server Memory 的大小。 最大限制为最大服务器内存大小的 32 倍。 有关最大服务器内存的详细信息，请参阅 [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)。  
  
禁用 BUFFER POOL EXTENSION 后才能修改文件大小。 若要指定小于当前大小的大小，必须重新启动 SQL Server 实例以回收内存。 否则，指定的大小必须与当前大小相同或大于当前大小。  
  
OFF  
禁用缓冲池扩展选项。 需要先禁用缓冲池扩展选项，然后才能修改关联的任何参数，如文件大小或名称。 禁用此选项会从注册表中删除所有相关配置信息。  
  
> [!WARNING]  
>  因为禁用缓冲池扩展会大大缩小缓冲池大小，所以可能会对服务器性能产生负面影响。  
  
**\<soft_numa>**  

适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 开始）。  
  
ON  
启用自动分区，将大型 NUMA 硬件节点拆分为较小的 NUMA 节点。 更改当前使用的值需要重新启动数据库引擎。  
  
OFF  
禁用将大型 NUMA 硬件节点拆分为较小 NUMA 节点的自动软件分区。 更改当前使用的值需要重新启动数据库引擎。  

> [!WARNING]
> 使用 SOFT NUMA 选项和 SQL Server 代理时，ALTER SERVER CONFIGURATION 语句的行为存在一些已知问题。  下面是推荐的操作顺序：  
> 1) 停止 SQL Server 代理实例。  
> 2) 执行 ALTER SERVER CONFIGURATION SOFT NUMA 选项。  
> 3) 重新启动 SQL Server 实例。  
> 4) 启动 SQL Server 代理实例。  
  
**详细信息：** 如果在重启 SQL Server 服务之前使用 SET SOFTNUMA 命令运行 ALTER SERVER CONFIGURATION，则在 SQL Server 代理服务停止时，它会运行 T-SQL RECONFIGURE 命令，将 SOFTNUMA 设置还原回执行 ALTER SERVER CONFIGURATION 之前的状态。 

\<memory_optimized> ::=

适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 开始）。

ON <br>
启用属于[内存数据库](../../relational-databases/in-memory-database.md)功能的所有实例级功能。 当前这些功能包括[内存优化的 tempdb 元数据](../../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata)和[混合缓冲池](../../database-engine/configure-windows/hybrid-buffer-pool.md)。 需要重启才能生效。

OFF <br>
禁用属于内存数据库功能系列的所有实例级功能。 需要重启才能生效。

TEMPDB_METADATA = ON | OFF <br>
仅启用或禁用内存优化的 tempdb 元数据。 需要重启才能生效。 

RESOURCE_POOL='resource_pool_name' <br>
与 TEMPDB_METADATA = ON 结合使用时，指定须用于 tempdb 的用户定义资源池。 如果未指定，tempdb 将使用默认池。 该池必须已存在。 如果在重新启动服务时池不可用，则 tempdb 将使用默认池。


HYBRID_BUFFER_POOL = ON | OFF <br>
在实例级别启用或禁用混合缓冲池。 需要重启才能生效。


## <a name="general-remarks"></a>一般备注  
除非另有明确说明，否则此语句不需要重启 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 如果是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例，则不需要重启 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 群集资源。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
此语句不支持 DDL 触发器。  
  
## <a name="permissions"></a>权限  
需要：
- `ALTER SETTINGS` 权限（对于进程关联选项）。
- `ALTER SETTINGS` 和 `VIEW SERVER STATE` 权限（对于诊断日志和故障转移群集属性选项）。
- `CONTROL SERVER` 权限（对于 HADR 群集上下文选项）。  
- `ALTER SERVER STATE` 权限（对于缓冲池扩展选项）。  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssDE](../../includes/ssde-md.md)] 资源 DLL 以本地系统帐户身份运行。 因此，本地系统帐户必须对诊断日志选项中的指定路径具有读写访问权限。  
  
## <a name="examples"></a>示例  
  
|类别|作为特征的语法元素|  
|--------------|------------------------------|  
|[设置进程关联](#Affinity)|CPU • NUMANODE • AUTO|  
|[设置诊断日志选项](#Diagnostic)|ON • OFF • PATH • MAX_SIZE|  
|[设置故障转移群集属性](#Failover)|HealthCheckTimeout|  
|[更改可用性副本的群集上下文](#ChangeClusterContextExample)|**'** *windows_cluster* **'**|  
|[设置缓冲池扩展](#BufferPoolExtension)|BUFFER POOL EXTENSION| 
|[设置内存数据库选项](#MemoryOptimized)|MEMORY_OPTIMIZED|

  
###  <a name="setting-process-affinity"></a><a name="Affinity"></a> 设置进程关联  
本节中的示例显示如何设置与 CPU 和 NUMA 节点的进程关联。 该示例假定服务器包含 256 个 CPU，这些 CPU 分为四组，每组各有 16 个 NUMA 节点。 线程未分配给任何 NUMA 节点或 CPU。  
  
-   组 0：NUMA 节点 0 到 3，CPU 0 到 63  
-   组 1：NUMA 节点 4 到 7，CPU 64 到 127  
-   组 2：NUMA 节点 8 到 12，CPU 128 到 191  
-   组 3：NUMA 节点 13 到 16，CPU 192 到 255  
  
#### <a name="a-setting-affinity-to-all-cpus-in-groups-0-and-2"></a>A. 设置与 0 和 2 组中所有 CPU 的关联  
下面的示例设置与 0 和 2 组中所有 CPU 的关联。  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY CPU=0 TO 63, 128 TO 191;  
```  
  
#### <a name="b-setting-affinity-to-all-cpus-in-numa-nodes-0-and-7"></a>B. 设置与 NUMA 节点 0 和 7 中所有 CPU 的关联  
下面的示例仅设置与节点 `0` 和 `7` 的 CPU 关联。  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY NUMANODE=0, 7;  
```  
  
#### <a name="c-setting-affinity-to-cpus-60-through-200"></a>C. 设置与 CPU 60 到 200 的关联  
下面的示例设置与 CPU 60 到 200 的关联。  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY CPU=60 TO 200;  
```  
  
#### <a name="d-setting-affinity-to-cpu-0-on-a-system-that-has-two-cpus"></a>D. 设置与具有两个 CPU 的系统上的 CPU 0 的关联  
下面的示例设置与具有两个 CPU 的计算机上的 `CPU=0` 的关联。 在执行以下语句前，内部关联位掩码为 00。  
  
```sql  
ALTER SERVER CONFIGURATION SET PROCESS AFFINITY CPU=0;  
```  
  
#### <a name="e-setting-affinity-to-auto"></a>E. 设置与 AUTO 的关联  
下面的示例将关联设置为 `AUTO`。  
  
```sql  
ALTER SERVER CONFIGURATION  
SET PROCESS AFFINITY CPU=AUTO;  
```  
  
###  <a name="setting-diagnostic-log-options"></a><a name="Diagnostic"></a> Setting diagnostic log options  
  
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）。    
  
本节中的示例说明如何设置诊断日志选项的值。  
  
#### <a name="a-starting-diagnostic-logging"></a>A. 启动诊断日志记录  
下面的示例启动诊断数据的日志记录。  
  
```sql  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG ON;  
```  
  
#### <a name="b-stopping-diagnostic-logging"></a>B. 停止诊断日志记录  
下面的示例停止诊断数据的日志记录。  
  
```sql  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG OFF;  
```  
  
#### <a name="c-specifying-the-location-of-the-diagnostic-logs"></a>C. 指定诊断日志的位置  
下面的示例将诊断日志的位置设置为指定的文件路径。  
  
```sql  
ALTER SERVER CONFIGURATION  
SET DIAGNOSTICS LOG PATH = 'C:\logs';  
```  
  
#### <a name="d-specifying-the-maximum-size-of-each-diagnostic-log"></a>D. 指定每个诊断日志的最大大小  
下面的示例将每个诊断日志的最大大小设置为 10 MB。  
  
```sql  
ALTER SERVER CONFIGURATION   
SET DIAGNOSTICS LOG MAX_SIZE = 10 MB;  
```  
  
###  <a name="setting-failover-cluster-properties"></a><a name="Failover"></a> 设置故障转移群集属性  
  
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）。   
  
下面的示例说明如何设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集资源属性的值。  
  
#### <a name="a-specifying-the-value-for-the-healthchecktimeout-property"></a>A. 指定 HealthCheckTimeout 属性的值  
下面的示例将 `HealthCheckTimeout` 选项设置为 15,000 毫秒（15 秒）。  
  
```sql  
ALTER SERVER CONFIGURATION   
SET FAILOVER CLUSTER PROPERTY HealthCheckTimeout = 15000;  
```  
  
###  <a name="b-changing-the-cluster-context-of-an-availability-replica"></a><a name="ChangeClusterContextExample"></a> B. 更改可用性副本的群集上下文  
下面的示例更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的 HADR 群集上下文。 为了指定目标 WSFC 群集 (`clus01`)，本示例指定完整的群集对象名称 (`clus01.xyz.com`)。  
  
```sql  
ALTER SERVER CONFIGURATION SET HADR CLUSTER CONTEXT = 'clus01.xyz.com';  
```  
  
### <a name="setting-buffer-pool-extension-options"></a>设置缓冲池扩展选项  
  
####  <a name="a-setting-the-buffer-pool-extension-option"></a><a name="BufferPoolExtension"></a> A. 设置缓冲池扩展选项  
  
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始）。    
  
下面的示例启用缓冲池扩展选项并指定文件名和大小。  
  
```sql  
ALTER SERVER CONFIGURATION   
SET BUFFER POOL EXTENSION ON  
    (FILENAME = 'F:\SSDCACHE\Example.BPE', SIZE = 50 GB);  
```  
  
#### <a name="b-modifying-buffer-pool-extension-parameters"></a>B. 修改缓冲池扩展参数  
下面的示例修改缓冲池扩展文件的大小。 修改任何参数前必须先禁用缓冲池扩展选项。  
  
```sql  
ALTER SERVER CONFIGURATION   
SET BUFFER POOL EXTENSION OFF;  
GO  
EXEC sp_configure 'max server memory (MB)', 12000;  
GO  
RECONFIGURE;  
GO  
ALTER SERVER CONFIGURATION  
SET BUFFER POOL EXTENSION ON  
    (FILENAME = 'F:\SSDCACHE\Example.BPE', SIZE = 60 GB);  
GO   
```  

### <a name="setting-in-memory-database-options"></a><a name="MemoryOptimized"></a> 设置内存数据库选项

适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 开始）。

#### <a name="a-enable-all-in-memory-database-features-with-default-options"></a>A. 使用默认选项启用所有内存数据库功能

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED ON;
GO
```

#### <a name="b-enable-memory-optimized-tempdb-metadata-using-the-default-resource-pool"></a>B. 使用默认资源池启用内存优化的 tempdb 元数据

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON;
GO
```

#### <a name="c-enable-memory-optimized-tempdb-metadata-with-a-user-defined-resource-pool"></a>C. 使用用户定义的资源池启用内存优化的 tempdb 元数据

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON (RESOURCE_POOL = 'pool_name');
GO
```

#### <a name="d-enable-hybrid-buffer-pool"></a>D. 启用混合缓冲池

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED HYBRID_BUFFER_POOL = ON;
GO
```

## <a name="see-also"></a>另请参阅  
[Soft-NUMA (SQL Server)](../../database-engine/configure-windows/soft-numa-sql-server.md)   
[更改服务器实例的 HADR 群集上下文 (SQL Server)](../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md)   
[sys.dm_os_schedulers (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md)   
[sys.dm_os_memory_nodes (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-nodes-transact-sql.md)   
[sys.dm_os_buffer_pool_extension_configuration (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-pool-extension-configuration-transact-sql.md)   
[缓冲池扩展](../../database-engine/configure-windows/buffer-pool-extension.md)  

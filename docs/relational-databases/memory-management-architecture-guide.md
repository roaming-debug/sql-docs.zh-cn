---
title: 内存管理体系结构指南 | Microsoft Docs
description: 了解 SQL Server 中的内存管理体系结构，包括对以前版本中内存管理的更改。
ms.custom: ''
ms.date: 01/09/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- guide, memory management architecture
- memory management architecture guide
- PMO
- Partitioned Memory Objects
- cmemthread
- AWE
- SPA, Single Page Allocator
- MPA, Multi Page Allocator
- memory allocation, SQL Server
- memory pressure, SQL Server
- stack size, SQL Server
- buffer manager, SQL Server
- buffer pool, SQL Server
- resource monitor, SQL Server
ms.assetid: 7b0d0988-a3d8-4c25-a276-c1bdba80d6d5
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a8165d82fa5db393b3f2f66737910ba4de9d11a8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473878"
---
# <a name="memory-management-architecture-guide"></a>内存管理体系结构指南

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

## <a name="windows-virtual-memory-manager"></a>Windows 虚拟内存管理器  
提交的地址空间区域由 Windows 虚拟内存管理器 (VMM) 映射到可用的物理内存。  
  
有关不同操作系统所支持的物理内存量的详细信息，请参阅介绍 [Windows 版本的内存限制](/windows/desktop/Memory/memory-limits-for-windows-releases)的 Windows 文档。  
  
虚拟内存系统允许虚拟内存超过物理内存，这样虚拟内存与物理内存的比率可以大于 1:1。 因此，大型程序在计算机上运行时可以具有多种物理内存配置。 但是，使用比所有进程的平均组合工作集大得多的虚拟内存可能会导致性能降低。 

## <a name="sql-server-memory-architecture"></a>SQL Server 内存体系结构

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将根据需要动态获取并释放内存。 虽然该选项仍然存在且在有些环境下需要用到，但通常情况下管理员不必指定为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]分配多少内存。

所有数据库软件的主要设计目标之一是尽量减少磁盘 I/O，因为磁盘的读取和写入操作占用大量资源。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 在内存中生成缓冲池，用于保存从数据库读取的页。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中的大量代码专门用于尽可能减少磁盘与缓冲池之间的物理读写次数。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 设法在以下两个目标之间达到平衡：

* 防止缓冲池变得过大，从而导致整个系统内存不足。
* 尽量增加缓冲池达的大小，以便尽量减少数据库文件的物理 I/O。

> [!NOTE]
> 在负载过重的系统中，某些在运行时需要大量内存的大型查询不能获取所需的最小内存量，并在等待内存资源时收到超时错误。 若要解决此问题，请增大 [query wait 选项](../database-engine/configure-windows/configure-the-query-wait-server-configuration-option.md)。 对于并行查询，请考虑减小 [最大并行度选项](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。
 
> [!NOTE]
> 在负载过重而内存不足的系统中，对于查询计划中带有合并联接、排序和位图的查询，如果无法获得位图所需的最小内存量，可以删除位图。 这会影响查询性能，并且如果排序过程无法容纳在内存中，就会增加 tempdb 数据库中工作表的使用量，从而导致 tempdb 增大。 若要解决此问题，可添加物理内存或优化查询以使用其他更迅速的查询计划。
 
### <a name="providing-the-maximum-amount-of-memory-to-ssnoversion"></a>为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 提供最大内存量

通过使用 AWE 和“锁定内存中的页”权限，可为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 数据库引擎提供下列内存量。 

> [!NOTE]
> 下表包含一个 32 位版本的列，这些版本不再可用。

|内存策略|32 位 <sup>1</sup> |64 位|
|-------|-------|-------| 
|常规内存 |所有 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本。 最大处理虚拟地址空间限制： <br>- 2 GB<br>- 3 GB，带有 /3gb 引导参数 <sup>2</sup> <br>- 4 GB，在 WOW64 <sup>3</sup>上 |所有 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本。 最大处理虚拟地址空间限制： <br>- 7 TB，带有 IA64 体系结构（ [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 及更高版本中不支持 IA64）<br>- 操作系统支持的最大值，带有 x64 体系结构 <sup>4</sup>
|AWE 机制（允许 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 在 32 位平台上超过处理虚拟地址空间限制。） |[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Standard、Enterprise 和 Developer 版本：缓冲池支持访问最多 64 GB 的内存。|不适用 <sup>5</sup> |
|“锁定内存页”操作系统 (OS) 权限（允许锁定物理内存，防止 OS 对锁定的内存进行分页。）<sup>6</sup> |[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Standard、Enterprise 和 Developer 版本：[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 进程使用 AWE 机制所需。 通过 AWE 机制分配的内存不能出页。 <br> 授予此权限但未启用 AWE 不会对服务器产生影响。 | 仅在必要时使用，即有迹象表明正在换出 sqlservr 进程时。在这种情况下，错误日志将报告错误 17890，类似于以下示例：`A significant part of sql server process memory has been paged out. This may result in a performance degradation. Duration: #### seconds. Working set (KB): ####, committed (KB): ####, memory utilization: ##%.`|

<sup>1</sup> 32 位版本不可用 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)]作为开头。  
<sup>2</sup> /3gb 是一个操作系统启动参数。  
<sup>3</sup> WOW64 (Windows on Windows 64) 是 32 位 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 在 64 位操作系统上运行的一种模式。  
<sup>4</sup> [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Standard Edition 最大支持 128 GB。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise Edition 支持操作系统最大值。  
<sup>5</sup> 请注意，sp_configure awe enabled 选项存在于 64 位 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]上，但将被忽略。    
<sup>6</sup> 如果授予“锁定内存中的页”权限 (LPIM)（在支持 AWE 的 32 位系统上或单独在 64 位系统上），建议也要设置最大服务器内存。 有关 LPIM 的详细信息，请参阅[“服务器内存”服务器配置选项](../database-engine/configure-windows/server-memory-server-configuration-options.md#lock-pages-in-memory-lpim)

> [!NOTE]
> 旧版 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 可在 32 位操作系统上运行。 在 32 位操作系统上访问超过 4 GB 的内存需要地址窗口化扩展插件 (AWE) 对内存进行管理。 如果 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 在 64 位操作系统上运行，则不需要。 有关 AWE 的详细信息，请参阅 [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] 文档中的[进程地址空间](/previous-versions/sql/sql-server-2008-r2/ms189334(v=sql.105))和[管理大型数据库的内存](/previous-versions/sql/sql-server-2008-r2/ms191481(v=sql.105))。   

<a name="changes-to-memory-management-starting-2012-11x-gm"></a>

## <a name="changes-to-memory-management-starting-with-sssql11"></a>自 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 以来对内存管理的更改

在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的早期版本（[!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)]、[!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../includes/ssKilimanjaro-md.md)]）中，使用五种不同的机制分配内存：
-  单页分配器 (SPA)，它仅包含 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 进程中小于或等于 8 KB 的内存分配。 “max server memory (MB)”和“min server memory (MB)”这两个配置选项确定 SPA 消耗的物理内存的限制 。 同时，缓冲池也是用于 SPA 的机制，并且还是最大的单页分配使用者。
-  **多页分配器 (MPA)** ，用于需要 8 KB 以上的内存分配。
-  **CLR 分配器**，包含 CLR 初始化期间创建的 SQL CLR 堆及其全局分配。
-  用于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 进程中的 **[线程堆栈](../relational-databases/memory-management-architecture-guide.md#stacksizes)** 的内存分配。
-  **直接 Windows 分配 (DWA)** ，用于直接向 windows 进行的内存分配请求。 包括由加载到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 进程中的模块使用 Windows 堆和直接进行虚拟分配。 此类内存分配请求的示例包括从扩展存储过程 DLL 分配、使用自动化过程（sp_OA 调用）创建的对象以及从链接服务器提供程序分配。

从 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 开始，单页分配、多页分配和 CLR 分配全部合并到“任意大小”页分配器中，受到由“最大服务器内存(MB)”和“最小服务器内存(MB)”配置选项控制的内存限制 。 此更改使通过 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 内存管理器的所有内存要求能更准确地调整大小。 

> [!IMPORTANT]
> 升级到 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 至 [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] 后，请仔细检查当前的“max server memory (MB)”和“min server memory (MB)”配置 。 这是因为从 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 开始，与早期版本相比，这些配置现在包括并用于更多内存分配。 这些更改适用于 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 和 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] 的 32 位和 64 位版本，以及 [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] 到 [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] 的 64 位版本。

下表指示特定类型的内存分配是否受“max server memory (MB)和“min server memory (MB)”配置选项控制 ：

|内存分配的类型| [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)]、[!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../includes/ssKilimanjaro-md.md)]| 自 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 起|
|-------|-------|-------|
|单页分配|是|是，合并到“任意大小”页分配|
|多页分配|否|是，合并到“任意大小”页分配|
|CLR 分配|否|是|
|线程堆栈内存|否|否|
|从 Windows 直接分配|否|否|

从 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 开始，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 可能会分配比 max server memory 设置中指定的值更多的内存。 当 Total Server Memory (KB) 值已达到 Total Server Memory (KB)（由 max server memory 指定）时，可能会出现这种情况。 如果因内存碎片造成连续空闲内存不足，无法满足多页内存请求的需求（超过 8 KB），[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 可以执行超额承诺使用量，而不是拒绝内存请求。 

只要执行此分配，资源监视器后台任务就会开始向所有内存消耗者发送信号，指示其释放已分配的内存，并尝试使 Target Server Memory (KB) 值低于 Target Server Memory (KB) 规范  。 因此，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 内存使用情况可短暂超过 max server memory 设置。 在这种情况下，Total Server Memory (KB) 性能计数器读取将超过 max server memory 和 Target Server Memory (KB) 设置 。

在以下操作中通常会观察到此行为： 
-  大型列存储索引查询。
-  列存储索引（重新）生成，该操作使用大量内存来执行哈希和排序操作。
-  需要较大内存缓冲区的备份操作。
-  需要存储较大输入参数的跟踪操作。

<a name="#changes-to-memory-management-starting-with-includesssql11includessssql11-mdmd"></a>
## <a name="changes-to-memory_to_reserve-starting-with-sssql11"></a>自 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 以来对“memory_to_reserve”的更改
在早期版本的 SQL Server（[!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)]、[!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../includes/ssKilimanjaro-md.md)]）中，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 内存管理器保留了一部分进程虚拟地址空间 (VAS)，供多页分配器 (MPA)、CLR 分配器、用于 SQL Server 进程中的线程堆栈的内存分配以及直接 Windows 分配 (DWA) 使用   。 这一部分虚拟地址空间也称为“Mem-To-Leave”或“非缓冲池”区域。

为这些分配保留的虚拟地址空间是由 _**memory\_to\_reserve**_ 配置选项确定的。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 使用的默认值是 256 MB。 要替代该默认值，请使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -g 启动参数。 有关 -g 启动参数的信息，请参阅介绍[数据库引擎服务启动选项](../database-engine/configure-windows/database-engine-service-startup-options.md)的文档页。

因为从 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 开始，新的“任何大小”页分配器也处理大于8 KB 的分配，所以 memory_to_reserve 值不包括多页分配。 除了此更改以外，其他设置都与此配置选项相同。

下表指示特定类型的内存分配是否属于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 进程虚拟地址空间的 memory_to_reserve 区域：

|内存分配的类型| [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)]、[!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../includes/ssKilimanjaro-md.md)]| 自 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 起|
|-------|-------|-------|
|单页分配|否|否，合并到“任意大小”页分配|
|多页分配|是|否，合并到“任意大小”页分配|
|CLR 分配|是|是|
|线程堆栈内存|是|是|
|从 Windows 直接分配|是|是|

## <a name="dynamic-memory-management"></a><a name="dynamic-memory-management"></a> 动态内存管理
[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]的默认内存管理行为是获取尽可能多的内存而不会造成系统内存短缺。 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]通过使用 Microsoft Windows 中的内存通知 API 来实现这一点。

当 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 动态使用内存时，它会定期查询系统以确定可用内存量。 保持此可用内存可避免操作系统 (OS) 进行分页。 如果可用内存较少， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将会释放内存以供操作系统使用。 如果有更多的内存可用， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 可能会分配更多的内存。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 仅在其工作负荷需要更多内存时才增加内存；空闲的服务器不会增加其虚拟地址空间的大小。  
  
[Max server memory](../database-engine/configure-windows/server-memory-server-configuration-options.md) 控制 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 内存分配、编译内存、所有缓存（包括缓冲池）、[查询执行内存授予](#effects-of-min-memory-per-query)、[锁管理器内存](#memory-used-by-sql-server-objects-specifications)和 CLR<sup>1</sup> 内存（实质上是 [sys.dm_os_memory_clerks](../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md) 中存在的所有内存分配器） 。 

<sup>1</sup> 从 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 开始，在 max_server_memory 分配下管理 CLR 内存。

以下查询返回有关当前分配内存的信息：  
  
```sql  
SELECT 
  physical_memory_in_use_kb/1024 AS sql_physical_memory_in_use_MB, 
    large_page_allocations_kb/1024 AS sql_large_page_allocations_MB, 
    locked_page_allocations_kb/1024 AS sql_locked_page_allocations_MB,
    virtual_address_space_reserved_kb/1024 AS sql_VAS_reserved_MB, 
    virtual_address_space_committed_kb/1024 AS sql_VAS_committed_MB, 
    virtual_address_space_available_kb/1024 AS sql_VAS_available_MB,
    page_fault_count AS sql_page_fault_count,
    memory_utilization_percentage AS sql_memory_utilization_percentage, 
    process_physical_memory_low AS sql_process_physical_memory_low, 
    process_virtual_memory_low AS sql_process_virtual_memory_low
FROM sys.dm_os_process_memory;  
```  
 
<a name="stacksizes"></a>线程堆栈<sup>1</sup>、CLR<sup>2</sup>、扩展过程 .dll 文件、分布式查询引用的 OLE DB 提供程序以及[!INCLUDE[tsql](../includes/tsql-md.md)] 语句中引用的自动化对象的内存以及由非 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] DLL 分配的任何内存均不受最大服务器内存控制。

<sup>1</sup> 有关为当前主机中给定数量的关联 CPU 计算得出的默认工作线程数的信息，请参阅介绍如何[“配置最大工作线程数”服务器配置选项](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md)的文档页。面。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 堆栈大小如下所示：

|SQL Server 体系结构|OS 体系结构|堆栈大小|  
|--------------------|----------------------|----------------------|
|x86（32 位）|x86（32 位）|512 KB|
|x86（32 位）|x64 （64 位）|768 KB| 
|x64 （64 位）|x64 （64 位）|2048 KB|
|IA64 (Itanium)|IA64 (Itanium)|4096 KB|

<sup>2</sup> 从 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 开始，在 max_server_memory 分配下管理 CLR 内存。

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 使用内存通知 API QueryMemoryResourceNotification 来确定 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 内存管理器何时可以分配内存和释放内存。  

启动 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 时，它将基于多个参数（例如系统的物理内存量、服务器线程数和各个引导参数）计算缓冲池的虚拟地址空间的大小。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将为缓冲池保留计算得到的进程虚拟地址空间量，但它仅为当前负荷获取（提交）所需的物理内存量。

然后实例将继续获取支持工作负荷所需的内存。 随着用户连接和运行查询的逐步增多， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将按需获取更多的物理内存。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例将继续获取物理内存，直到达到自己的最大服务器内存分配目标或操作系统指示不再有可用剩余内存；如果该实例获取的内存超过最小服务器内存设置，并且操作系统指示可用内存短缺，将释放内存。 

随着在运行 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]实例的计算机上启动其他应用程序，这些应用程序将会占用内存，从而使可用物理内存量降到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 目标以下。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例将调整其内存使用量。 如果另一个应用程序已停止，并且可用内存增多， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的实例会增加其内存分配的大小。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 每秒可释放和获取几 MB 的内存，从而根据内存分配变化快速做出调整。

## <a name="effects-of-min-and-max-server-memory"></a>min server memory 和 max server memory 的影响
“min server memory”和“max server memory”配置选项建立 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 数据库引擎的缓冲池和其他缓存使用的内存量的上限和下限 。 缓冲池并不立即获取最小服务器内存中指定的内存量。 缓冲池启动时只使用初始化所需的内存。 随着[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]工作负荷的增加，它将继续获取支持工作负荷所需的内存。 在达到最小服务器内存中指定的内存量之前，缓冲池不会释放它获取的任何内存。 达到最小服务器内存后，缓冲池将使用标准算法，根据需要来获取和释放内存。 唯一的区别是缓冲池从不将内存分配降到最小服务器内存所指定的水平下，也从不获取超过最大服务器内存所指定水平的内存。

> [!NOTE]
> 作为进程的[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将获取超过最大服务器内存选项所指定的内存。 内部和外部组件都可以分配缓冲池以外的内存，这将占用额外内存，但是分配给缓冲池的内存通常仍表示 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 占用的内存的最大部分。

[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]获取的内存量完全取决于放置在实例上的工作负荷。 不处理很多请求的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例可能永远不会达到最小服务器内存。

如果为 min server memory 和 max server memory 指定相同的值，则一旦分配给 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 的内存达到该值，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 将停止为缓冲池动态释放和获取内存。

如果在运行 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例的计算机上频繁启动或停止其他应用程序，启动这些应用程序所需的时间可能会因 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例分配和释放内存而延长。 另外，如果 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 是几个在一台计算机上运行的服务器应用程序中的一个，系统管理员可能需要控制分配给 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的内存量。 在这些情况下，可以使用最小服务器内存和最大服务器内存选项控制 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 可使用的内存量。 “min server memory”和“max server memory”选项均以 MB 为单位指定 。 有关详细信息，请参阅 [服务器内存配置选项](../database-engine/configure-windows/server-memory-server-configuration-options.md)。

## <a name="memory-used-by-sql-server-objects-specifications"></a>SQL Server 对象规范使用的内存
以下列表介绍了 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]中不同对象所用内存量的近似值。 列出的数值为估计值，根据环境以及创建对象的方式可能有所不同：

* 锁（由锁管理器维护）：每个所有者 64 字节 + 32 字节   
* 用户连接：约为 (3 \* network_packet_size + 94 kb)    

网络数据包大小是表格数据模式 (TDS) 数据包的大小，该数据包用于应用程序和 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 数据库引擎之间的通信。 默认的数据包大小为 4 KB，由“网络数据包大小”配置选项控制。

启用多个活动的结果集 (MARS) 时，用户连接约为 (3 + 3 \*num_logical_connections)\* network_packet_size + 94 KB

## <a name="effects-of-min-memory-per-query"></a>min memory per query 的影响
min memory per query 配置选项设定将为执行查询分配的最小内存量 (KB)。 这也称为最小内存授予。 所有查询必须等待，直到：可确保提供请求的最小内存，可以启动执行，或者已超过查询等待服务器配置选项中指定的值。 此方案中累积的等待类型是 RESOURCE_SEMAPHORE。

> [!IMPORTANT]
> 不要将 min memory per query 服务器配置选项设置过高，尤其是在非常繁忙的系统上，因为这样做可能导致：         
> - 内存资源竞争加剧。         
> - 即使所需的内存在运行时低于此配置，也会通过增加每次单个查询的内存量来降低并发。    
>    
> 有关使用此配置的建议，请参阅[配置 min memory per query 服务器配置选项](../database-engine/configure-windows/configure-the-min-memory-per-query-server-configuration-option.md#Recommendations)。

### <a name="memory-grant-considerations"></a><a name="memory-grant-considerations"></a>内存授予注意事项
对于“行模式执行”，任何情况下都不得超过初始内存授予。 如果执行哈希或排序操作需要的内存多于初始授予，这些内存将溢出到磁盘 。 溢出的哈希操作由 TempDB 中的 Workfile 支持，而溢出的排序操作由[工作表](../relational-databases/query-processing-architecture-guide.md#worktables)支持。   

排序操作期间发生的溢出称为 [Sort Warning](../relational-databases/event-classes/sort-warnings-event-class.md)。 Sort Warning 指示排序操作的内存不足。 这不包括涉及创建索引的排序操作，只包括查询内的排序操作（例如 `SELECT` 语句中使用的 `ORDER BY` 子句）。

哈希操作期间发生的溢出称为 [Hash Warning](../relational-databases/event-classes/hash-warning-event-class.md)。 在哈希操作过程中发生哈希递归或哈希终止（哈希释放）时，会出现此情况。
-  当生成输入无法装入可用内存时，会发生哈希递归，这将导致输入分割成单独处理的多个分区。 如果这些分区中任何一个仍然大于可用内存，则该分区再拆分成子分区分别进行处理。 此拆分过程将一直持续到每个分区都小于可用内存，或达到最大递归级数。
-  当哈希操作达到其最大递归级数并转换到替换计划以处理剩余的分区数据时发生哈希释放。 这些事件可导致服务器性能降低。

对于“批模式执行”，默认情况下，初始内存授予可以动态增加到特定内部阈值。 此动态内存授予机制旨在允许在批模式下运行哈希或排序操作的内存驻留执行 。 如果这些操作仍然内存不足，则将溢出到磁盘。

有关执行模式的详细信息，请参阅[查询处理体系结构指南](../relational-databases/query-processing-architecture-guide.md#execution-modes)。

## <a name="buffer-management"></a>缓冲区管理
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 数据库的主要用途是存储和检索数据，因此，大量磁盘 I/O 是该数据库引擎的一个核心特点。 此外，完成磁盘 I/O 操作要消耗许多资源并且耗时较长，所以 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 侧重于提高 I/O 效率。 缓冲区管理是实现高效 I/O 操作的关键环节。 缓冲区管理组件由下列两种机制组成：用于访问及更新数据库页的缓冲区管理器和用于减少数据库文件 I/O 的缓冲区高速缓存（又称为“缓冲池”）  。 

### <a name="how-buffer-management-works"></a>缓冲区管理的工作原理
一个缓冲区就是一个 8 KB 大小的内存页，其大小与一个数据页或索引页相当。 因此，缓冲区缓存被划分为多个 8 KB 页。 缓冲区管理器负责将数据页或索引页从数据库磁盘文件读入缓冲区高速缓存中，并将修改后的页写回磁盘。 缓冲区缓存中会保留一页，直到缓冲区管理器需要该缓冲区读入更多数据。 数据只有在被修改后才重新写入磁盘。 在将缓冲区缓存中的数据写回磁盘之前，可对其进行多次修改。 有关详细信息，请参阅 [读取页](../relational-databases/reading-pages.md) 和 [写入页](../relational-databases/writing-pages.md)。

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 启动时，它将基于大量参数（例如系统的物理内存量、配置的最大服务器线程数和各个启动参数）计算缓冲区缓存的虚拟地址空间的大小。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将为缓冲区缓存保留此计算得到的进程虚拟地址空间量（称为“内存目标”），但它仅为当前负荷获取（提交）所需的物理内存量。 可查询 **sys.dm_os_sys_info** 目录视图中的 **bpool_commit_target** 和 [bpool_committed columns](../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) 列，分别返回保留为内存目标的页数以及缓冲区缓存中当前提交的页数。

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 启动与缓冲区缓存获得其内存目标之间的间隔称为“增长期”。 在此期间，读取请求将根据需要填充缓冲区。 例如，一个 8 KB 的页面读取请求填充一个缓冲区页。 也就是说，增长期取决于客户端请求的数量和类型。 通过将单页读取请求转换为对齐的八页请求加快增长（组成一个盘区）。 这样可更快地完成增长，尤其是那些内存容量很大的机器。 有关页和盘区的详细信息，请参阅[页和盘区体系结构指南](../relational-databases/pages-and-extents-architecture-guide.md#pages-and-extents)。

因为缓冲区管理器将多数内存用于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 进程，所以它会与内存管理器协作以使其他组件能使用其缓冲区。 缓冲区管理器主要与下列组件交互：

* 资源管理器。此交互用于控制内存的整体使用情况以及 32 位平台中的地址空间使用情况。  
* 数据库管理器和 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 操作系统 (SQLOS)。此交互用于低级文件 I/O 操作。  
* 日志管理器。此交互用于预写日志记录。  

### <a name="supported-features"></a>支持的功能
缓冲区管理器支持以下功能：

* 缓冲区管理器可识别非一致性内存访问 (NUMA)。 缓冲区高速缓存页将跨硬件 NUMA 节点进行分布，它允许线程访问分配到本地 NUMA 节点上的缓冲区页，而不是从外部内存访问。 
* 缓冲区管理器支持热添加内存，用户无需重新启动服务器即可添加物理内存。 
* 缓冲区管理器在 64 位平台上支持大型页。 页大小因 Windows 版本不同而异。

  > [!NOTE]
  > 在 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 之前，启用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中的大型页面需要[跟踪标志 834](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。  

* 缓冲区管理器提供通过动态管理视图显示的其他诊断信息。 您可以使用这些视图来监视 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]特有的各种操作系统资源。 例如，可以使用 [sys.dm_os_buffer_descriptors](../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-descriptors-transact-sql.md) 视图来监视缓冲区缓存中的页。   

### <a name="disk-io"></a>磁盘 I/O
缓冲区管理器仅对数据库执行读写操作。 其他文件和数据库操作（如打开、关闭、扩展和收缩）则由数据库管理器和文件管理器组件执行。 

缓冲区管理器的磁盘 I/O 操作具有以下特点：

* 所有 I/O 操作均异步执行。这样，在后台进行 I/O 操作的同时，即可调用线程继续处理。
* 所有 I/O 操作均在调用线程中发出，除非 affinity I/O 选项处于使用状态。 Affinity I/O mask 选项将 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 磁盘 I/O 绑定到指定的 CPU 子集。 在高端 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 联机事务处理 (OLTP) 环境中，此扩展可以提高 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 线程执行 I/O 的性能。
* 可通过散播-聚集 I/O 实现多页 I/O，散播-聚集 I/O 允许数据传入或传出非连续内存区域。 这意味着 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 可以快速填充或刷新缓冲区高速缓存，同时避免多个物理 I/O 请求。 

#### <a name="long-io-requests"></a>长时 I/O 请求  
缓冲区管理器报告任何经过 15 秒或更长时间仍未完成的 I/O 请求。 这可以帮助系统管理员区分 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 问题和 I/O 子系统问题。 将报告错误消息 833 并且该消息在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 错误日志中显示如下：

`SQL Server has encountered ## occurrence(s) of I/O requests taking longer than 15 seconds to complete on file [##] in database [##] (#). The OS file handle is 0x00000. The offset of the latest long I/O is: 0x00000.` 

长时 I/O 可以是读或写，不过当前消息并未指明。 长时 I/O 消息是警告而不是错误。 它们并不表示 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 存在问题，而是基础 I/O 系统存在问题。 报告这些消息是为了帮助系统管理员更快地找到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 响应缓慢的原因，并找出 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]无法控制的问题。 因此，不需要为它们执行任何操作，但系统管理员应调查 I/O 请求耗时很长的原因以及耗时是否合理。

#### <a name="causes-of-long-io-requests"></a>长时 I/O 请求的原因  
长时 I/O 消息可能指示 I/O 永久阻塞并且永远无法完成（称为“I/O 丢失”），或者只是它尚未完成。 虽然 I/O 丢失往往会导致闩锁超时，但无法根据消息确定是哪种情况。

长时 I/O 往往指示磁盘子系统的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 工作负荷过于密集。 以下情况可能会指示磁盘子系统不足：

* [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 工作负荷很大时，错误日志中出现多个长时 I/O 消息。
* Perfmon 计数器显示磁盘长时间滞后、磁盘队列长或无磁盘空闲时间。  

长时 I/O 还可能因以下原因所致：I/O 路径中的某个组件（如驱动程序、控制器或固件）不断延迟为早期 I/O 请求提供服务，而为距离当前磁头位置较近的新请求提供服务。 这种对距离读/写磁头当前位置最近的请求进行优先处理的常见技术称为“电梯式查找”。 它可能很难与 Windows 系统监视器 (PERFMON.EXE) 工具配合使用，因为多数 I/O 是立即获得服务的。 执行大量连续 I/O 的工作负荷可能会使长时 I/O 请求情况更严重，如备份和还原、表扫描、排序、创建索引、大容量加载以及清零文件。

单独出现的长时 I/O 如果与上述情况无关，则可能是由硬件或驱动程序问题所致。 系统事件日志可能会包含有助于进行问题诊断的相关事件。

### <a name="memory-pressure-detection"></a>内存压力检测
内存压力是由内存不足造成的一种情况，可导致：
- 额外的 I/O（如非常活跃的 lazy writer 后台线程）
- 重新编译比升高
- 运行查询延长（如果存在内存授予等待）
- 额外的 CPU 周期

这种情况可能由外部或内部原因引发。 外部原因包括：
- 可用物理内存 (RAM) 不足。 这会导致系统调整当前正在运行的进程的工作集，并可能导致整体速度下降。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 可能减少缓冲池的提交目标，并更频繁地开始调整内部缓存。 
- 总体可用系统内存（包括系统页面文件）不足。 这可能导致系统无法分配内存，因为无法对当前已分配的内存进行页面换出。
内部原因包括：
- 在 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 设置较低内存使用情况上限时响应外部内存压力。
- 通过减少 max server memory 配置手动降低内存设置。 
- 多个缓存之间的内部组件的内存分配发生变化。

[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 实现了一个专门用于检测和处理内存压力的框架，包含在动态内存管理中。 此框架包含名为“资源监视器”的后台任务。 “资源监视器”任务可监视外部和内部内存指示器的状态。 一旦某个指示器改变状态，它会计算相应的通知并进行广播。 这些通知是每个引擎组件中的内部消息，存储在环形缓冲区中。 

两个环形缓冲区保留与动态内存管理相关的信息： 
- “资源监视器”环形缓冲区，用于跟踪“资源监视器”活动，例如是否有内存压力信号。 此环形缓冲区包含关于 RESOURCE_MEMPHYSICAL_HIGH、RESOURCE_MEMPHYSICAL_LOW、RESOURCE_MEMPHYSICAL_STEADY 或 RESOURCE_MEMVIRTUAL_LOW 的当前状况的状态信息   。
- “内存代理”环形缓冲区，其中包含每个 Resource Governor 资源池的内存通知记录。 检测到内部内存压力时，将为分配内存的组件启用内存不足通知，以触发用于平衡缓存之间的内存的操作。 

内存代理监视每个组件对内存的需求消耗，然后根据收集的信息计算所有这些组件的最佳内存值。 每个 Resource Governor 资源池都有一组代理。 它们随后可将此信息广播给每个组件，使组件根据需要增加或减少用量。
有关内存代理的详细信息，请参阅 [sys.dm_os_memory_brokers](../relational-databases/system-dynamic-management-views/sys-dm-os-memory-brokers-transact-sql.md)。 

### <a name="error-detection"></a>错误检测  
数据库页可使用下面两种可选机制之一来保证页在从写入磁盘到再次读取期间的完整性：残缺页保护和校验和保护。 这些机制允许采用独立方法验证数据存储以及诸如控制器、驱动程序、电缆等硬件组件甚至操作系统的正确性。 在即将把页写入磁盘之前将向页添加保护，并在从磁盘读取页后对它进行验证。

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将对因校验和、页撕裂或其他 I/O 错误而失败的任何读取都重试四次。 如果在其中一次重试中读取成功，则会向错误日志中写入一条消息，且触发读取的命令将继续执行。 如果重试失败，则该命令失败，且显示错误消息 824。 

页保护类型是包含此页的数据库的属性之一。 校验和保护是在 [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] 和更高版本中创建的数据库的默认保护。 页保护机制是在创建数据库时指定的，并且可以使用 ALTER DATABASE SET 进行更改。 可通过查询 [sys.databases](../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的 page_verify_option 列或 [DATABASEPROPERTYEX](../t-sql/functions/databasepropertyex-transact-sql.md) 函数的 IsTornPageDetectionEnabled 属性来确定当前的页保护设置 。 

> [!NOTE]
> 如果页保护设置发生变化，新设置不会立即影响到整个数据库。 相反，每当下一次写入时，这些页才会采用数据库的当前保护级别。 这意味着数据库可能包含带有不同保护的页。 

#### <a name="torn-page-protection"></a>残缺页保护  
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2000 中引入的“残缺页保护”其实是一种对电源故障导致的页损坏进行检测的方法。 例如，意外电源故障可能导致只有部分页写入磁盘。 使用残缺页保护时，在将 8 KB 的数据库页写入磁盘时，该页的每个 512 字节的扇区都有一个特定的 2 位签名模式存储在数据库的页头中。 从磁盘中读取页时，页头中存储的残缺位将与实际的页扇区信息进行比较。 每次进行写操作时，这个签名模式在二进制数 01 和 10 之间交替，这样始终可以确定是否只有部分扇区写到磁盘：如果稍后读取页时发现某个位的状态不正确，则说明该页没有被正确写入并会因此检测到残缺页。 残缺页检测使用的资源最少，但是它无法检测到磁盘硬件故障导致的所有错误。 有关设置残缺页保护的信息，请参阅 [ALTER DATABASE SET 选项 (Transact-SQL)](../t-sql/statements/alter-database-transact-sql-set-options.md#page_verify)。

#### <a name="checksum-protection"></a>校验和保护  
[!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] 中引入的“校验和保护”提供了更强大的数据完整性检查。 此方法将对写入每一页中的数据进行校验和计算并将其值存储在页头中。 每次从磁盘读取存储了校验和的页时，数据库引擎将重新计算页中数据的校验和。如果新的校验和不同于已存储的校验和，则引发错误 824。 校验和保护比残缺页保护能捕获到更多的错误，因为它受到页中每个字节的影响，但它对资源的消耗较多。 启用校验和后，当缓冲区管理器从磁盘读取页时均可以检测到因电源故障以及硬件或固件故障导致的错误。 有关设置校验和的信息，请参阅 [ALTER DATABASE SET 选项 (Transact-SQL)](../t-sql/statements/alter-database-transact-sql-set-options.md#page_verify)。

> [!IMPORTANT]
> 在用户数据库或系统数据库升级到 [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] 或更高版本后，将保留 [PAGE_VERIFY](../t-sql/statements/alter-database-transact-sql-set-options.md#page_verify) 值（NONE 或 TORN_PAGE_DETECTION）。 强烈建议使用 CHECKSUM。
> TORN_PAGE_DETECTION 可能使用较少资源，但提供的 CHECKSUM 保护最少。

## <a name="understanding-non-uniform-memory-access"></a>了解非一致性内存访问
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]  能识别非一致性内存访问 (NUMA)，无需特殊配置便可在 NUMA 硬件上顺利地执行。 随着处理器时钟速度的提高和处理器数量的增加，使用这种额外处理能力所需的内存滞后时间越来越难以减少。 为了避开这一问题，硬件供应商提供了大型的 L3 缓存，但这只是一种有限的解决方案。 NUMA 体系结构为此问题提供了可缩放的解决方案。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 已设计为利用基于 NUMA 的计算机而无需更改任何应用程序。 有关详细信息，请参阅[操作说明：将 SQL Server 配置为使用软件 NUMA](../database-engine/configure-windows/soft-numa-sql-server.md)。

## <a name="dynamic-partition-of-memory-objects"></a>内存对象的动态分区
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中的堆分配器称为内存对象，[!INCLUDE[ssde_md](../includes/ssde_md.md)] 可通过它们从堆中分配内存。 可以使用 [sys.dm_os_memory_objects](../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md) DMV 跟踪它们。 CMemThread 是一个线程安全内存对象类型，它允许从多个线程并发分配内存。 为实现正确的跟踪，CMemThread 对象依赖于同步构造（互斥体），以确保同一时间只有一个线程更新重要信息。 

> [!NOTE]
> 在整个 [!INCLUDE[ssde_md](../includes/ssde_md.md)] 代码库中，CMemThread 对象类型用于许多不同的分配，并可按节点或 CPU 进行全局分区。   

但是，如果很多线程以高度并发的方式从同一内存对象进行分配，则使用互斥体可能会导致争用。 因此，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 具有分区内存对象 (PMO) 的概念，每个分区由单个 CMemThread 对象表示。 内存对象的分区是静态定义的，创建后不可更改。 由于内存分配模式因硬件和内存使用情况等方面迥然相异，因此不可能提前设定出完美的分区模式。 在绝大多数情况下，使用单个分区就足够了，但在某些情况下，这可能会导致争用，只能通过高度分区的内存对象阻止这种争用。 对每个内存对象进行分区是不理想的，因为分区增多可能导致其他方面的效率低下并增加内存碎片。

> [!NOTE]
> 在 [!INCLUDE[ssSQL15](../includes/sssql15-md.md)]之前，可以使用跟踪标志 8048 将基于节点的 PMO 强制变为基于 CPU 的 PMO。 从 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] SP2 和 [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] 开始，此行为是动态的，由引擎控制。

从 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] SP2 和 [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] 开始，[!INCLUDE[ssde_md](../includes/ssde_md.md)] 可以动态检测特定 CMemThread 对象上的争用，并将对象提升为基于每个节点或每个 CPU 的实现。  升级后，PMO 会保持升级状态，直到重新启动 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 进程。 [sys.dm_os_wait_stats](../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md) DMV 中 CMEMTHREAD 等待数过多可指示 CMemThread 争用，可通过观察以下 [sys.dm_os_memory_objects](../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md) DMV 列来发现它：*contention_factor* *partition_type* *exclusive_allocations_count* 和 *waiting_tasks_count*。

## <a name="see-also"></a>另请参阅
[“服务器内存”服务器配置选项](../database-engine/configure-windows/server-memory-server-configuration-options.md)   
[读取页](../relational-databases/reading-pages.md)   
[写入页](../relational-databases/writing-pages.md)   
[如何：将 SQL Server 配置为使用软件 NUMA](../database-engine/configure-windows/soft-numa-sql-server.md)   
[使用内存优化表的要求](../relational-databases/in-memory-oltp/requirements-for-using-memory-optimized-tables.md)   
[使用内存优化表来解决内存不足问题](../relational-databases/in-memory-oltp/resolve-out-of-memory-issues.md)
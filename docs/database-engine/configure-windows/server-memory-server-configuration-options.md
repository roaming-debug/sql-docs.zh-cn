---
title: 服务器内存配置选项 | Microsoft Docs
ms.custom: contperf-fy20q4
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
description: 了解如何配置 SQL Server 内存管理器分配给 SQL Server 进程的内存量。 查看内存管理方法和示例。
helpviewer_keywords:
- Virtual Memory Manager
- max server memory option
- virtual memory [SQL Server]
- VMM
- server memory options [SQL Server]
- servers [SQL Server], memory
- buffer pools [SQL Server]
- min server memory option
- manual memory options [SQL Server]
- memory [SQL Server], servers
ms.assetid: 29ce373e-18f8-46ff-aea6-15bbb10fb9c2
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 71cab9c11aaa4f3dc569761fdfef407601162b1c
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878897"
---
# <a name="server-memory-configuration-options"></a>服务器内存配置选项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

重新配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例使用的 SQL Server 进程的内存量（以 MB 为单位）。  有两个服务器内存选项：min server memory 和 max server memory。 这些选项会更改 SQL Server 内存管理器可分配给 SQL Server 进程的内存量。
  
这些选项的默认设置和允许的最小值为：

|选项  |  默认 | 允许的最小值  |
|---------|---------|---------|
|**min server memory**     |    0     |    0     |
|**max server memory**     |     2,147,483,647 兆字节 (MB)     |  128 MB       |

默认情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的内存要求会根据可用系统资源的情况动态变化。 有关详细信息，请参阅[动态内存管理](../../relational-databases/memory-management-architecture-guide.md#dynamic-memory-management)。

> [!IMPORTANT]  
> 将“max server memory”值设置得太高可能导致一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例与同一主机上承载的其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例争用内存。 但是，将此值设置得太低可能会导致极大的内存压力和性能问题。 将“max server memory”设置为最小值甚至可能导致无法启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 如果在更改此选项之后无法启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，请使用“-f”启动选项启动它，并将“max server memory”重置为以前的值 。 有关详细信息，请参阅 [Database Engine Service Startup Options](../../database-engine/configure-windows/database-engine-service-startup-options.md)。  
    
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可动态使用内存；但也可以手动设置内存选项并限制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以访问的内存量。 在设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的内存量之前，请通过从总物理内存中减去操作系统所需的内存（即不受 max_server_memory 设置控制的内存分配）以及任何其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例所需的内存（如果计算机并非完全由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 专用，则还要减去其他系统使用的内存量）。 这个差值就是可以分配给当前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例使用的最大内存量。  
 
## <a name="set-options-manually"></a><a name="manually"></a> 手动设置选项  
可以将 min server memory 和 max server memory 设置成一个内存范围 。 在需要兼顾同一台主机上运行的其他应用程序或其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的内存要求时，此方法对于配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的系统或数据库管理员来说非常有用。

> [!NOTE]
> **min server memory** 和 **max server memory** 选项都是高级选项。 如果使用 **sp_configure** 系统存储过程来更改这些设置，则只有在“显示高级选项”设置为 1 时才能更改它们。 这些设置更改后会立即生效，不需要重新启动服务器。  
  
<a name="min_server_memory"></a> 使用 min_server_memory 可以保证可供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存管理器使用的最小内存量。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不会在启动时立即分配 **min server memory** 中指定的内存量。 不过，除非降低 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] min server memory **的值，否则当内存使用量由于客户端负荷而达到该值后，** 不能释放内存。 例如，同一个主机中可同时存在多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时，为了给实例保留内存，请设置 min_server_memory 参数而不是 max_server_memory。 此外，为了确保来自基础主机的内存压力不会尝试从来宾 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 虚拟机 (VM) 上的缓冲池释放超过可接受性能所需的内存，在虚拟环境中设置 min_server_memory 值非常必要。

>[!NOTE]
>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 并不一定分配“最小服务器内存”中指定的内存量。 如果服务器上的负荷从不需要分配 **min server memory** 指定的内存量，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将以较少的内存运行。

<a name="max_server_memory"></a> 使用 max_server_memory 来保证 OS 不会遇到不利的内存压力。 若要设置 max server memory 配置，请监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程的总体消耗，以确定内存要求。 使单个实例的这些计算更准确：
- 从 OS 总内存中，为 OS 自身保留 1GB - 4GB。
- 然后，减去等于在“最大服务器内存”控制范围外的潜在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存分配的值，即堆栈大小<sup>1</sup> \* 计算出的最大工作线程数 <sup>2</sup> 。 所得结果就是一个实例设置的 max_server_memory 设置。

<sup>1</sup> 有关每个体系结构的线程堆栈大小的信息，请参阅[内存管理体系结构指南](../../relational-databases/memory-management-architecture-guide.md#stacksizes)。

<sup>2</sup> 有关为当前主机中给定数量的关联 CPU 计算得出的默认工作线程数的信息，请参阅介绍如何[配置最大工作线程数服务器配置选项](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md)的文档页。

## <a name="use-ssmanstudiofull"></a>使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
使用“min server memory”和“max server memory”这两个服务器内存选项重新配置由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存管理器为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例管理的内存量 (MB)。 默认情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的内存要求会根据可用系统资源的情况动态变化。  
  
### <a name="procedure-for-configuring-a-fixed-amount-of-memory-not-recommended"></a>配置固定内存量的过程（不推荐）  
设置固定内存量：  
  
1.  在对象资源管理器中，右键单击服务器并选择 **“属性”** 。  
  
2.  单击 **“内存”** 节点。  
  
3.  在 **“服务器内存选项”** 中，为 **“最小服务器内存”** 和 **“最大服务器内存”** 输入所需的内存量。  
  
     使用默认设置，将允许 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 根据可用系统资源动态更改其内存需求。 建议按[上述内容](#max_server_memory)设置 max server memory。 

下面的屏幕截图演示了所有这三个步骤： 

:::image type="content" source="media/server-memory-server-configuration-options/configure-memory-in-ssms.png" alt-text="在 SSMS 中配置内存":::

  
## <a name="lock-pages-in-memory-lpim"></a>锁定内存页 (LPIM) 
此 Windows 策略将确定哪些帐户可以使用进程将数据保留在物理内存中，从而阻止系统将数据分页到磁盘的虚拟内存中。 锁定内存中的页可以在发生将内存分页到磁盘时保持服务器的响应能力。 已向有权运行 sqlservr.exe 的帐户授予 Windows 锁定内存页 (LPIM) 用户权限时，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard Edition 和更高版本的实例中将“锁定内存页”选项设置为“打开”。  
  
若要对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**禁用**“锁定内存页”选项，请为有权运行 sqlservr.exe（[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动帐户）启动帐户的帐户删除“锁定内存页”用户权限。  
 
设置此选项可实现根据其他内存分配器的请求扩大或缩小内存，不影响 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [动态内存管理](../../relational-databases/memory-management-architecture-guide.md#dynamic-memory-management)。 使用“锁定内存页”用户权限时，建议按[如上所述](#max_server_memory)，为 max server memory 设置一个上限。

> [!IMPORTANT]
> 应仅在必要时设置此选项，即有迹象表明正在换出 sqlservr 进程时。在这种情况下，错误日志将报告错误 17890，类似于以下示例：`A significant part of sql server process memory has been paged out. This may result in a performance degradation. Duration: #### seconds. Working set (KB): ####, committed (KB): ####, memory utilization: ##%.`
> 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始，Standard Edition 不需要[跟踪标志 845 ](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)来使用“锁定页”。 
  
### <a name="to-enable-lock-pages-in-memory"></a>启用“锁定内存页”  
启用“锁定内存页”选项：  
  
1.  在 **“开始”** 菜单上，单击 **“运行”** 。 在“打开”  框中，键入 **gpedit.msc**  
  
     将打开 **“组策略”** 对话框。  
  
2.  在 **“组策略”** 控制台上，展开 **“计算机配置”** ，再展开 **“Windows 设置”** 。  
  
3.  展开 **“安全设置”** ，再展开 **“本地策略”** 。  
  
4.  选择 **“用户权利指派”** 文件夹。  
  
     细节窗格中随即显示出策略。  
  
5.  在该窗格中，双击“锁定内存页”。  
  
6.  在“本地安全策略设置”对话框中，添加有权运行 sqlservr.exe （[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动帐户）的帐户。  
  
## <a name="multiple-instances-of-ssnoversion"></a>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的多个实例  
 当运行多个 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例时，可以使用三种方法来管理内存：  
  
-   使用“max server memory”控制内存使用量，[如上所述](#max_server_memory)。 为每个实例建立最大设置，注意总的允许设置值不能大于计算机上的物理总内存。 可能需要为每个实例提供与预期的工作负荷或数据库大小成正比的内存。 这种方法的优势体现在：当启动新的进程或实例时，可以立即为这些进程或实例提供可用内存。 这种方法的缺点为：如果没有运行所有实例，则所有运行中的实例都无法使用剩余的可用内存。  
  
-   使用“min server memory”控制内存使用量，[如上所述](#min_server_memory)。 为每个实例建立最小设置，以使这些最小值的和比计算机上总的物理内存小 1-2 GB。 此外，可能需要建立与该实例的预期负荷成正比的最小值。 这种方法的优势体现在：如果没有同时运行所有实例，则运行中的实例可以使用剩余的可用内存。 当计算机上存在其他占用大量内存的进程时，这种方法也十分有用，因为它可确保 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 至少获得合理的内存量。 这种方法的缺点是：当启动新的实例（或任何其他进程）时，运行的实例可能会花费一些时间来释放内存，如果实例必须将修改后的页写回到数据库中来释放内存，则花费的时间可能会更长。  
  
-   不执行任何操作（不推荐）。 带有工作负荷的第一个实例通常分配所有的内存。 空闲实例或稍后启动的实例最终可能会只使用最少的可用内存量运行。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不会尝试均衡分配各个实例的内存使用量。 但是，所有实例均将响应 Windows 内存通知信号以调整它们内存需求量的大小。 Windows 不会使用内存通知 API 来平衡各个应用程序使用的内存。 它只提供有关系统内存可用性的全局反馈。  
  
 您可以在不重新启动实例的情况下更改这些设置，以便可以轻松地进行尝试以找到适合使用模式的最佳设置。  
  
## <a name="provide-the-maximum-amount-of-memory"></a>提供最大内存量

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的所有版本中，内存最大可配置为进程虚拟地址空间限制。 有关详细信息，请参阅 [Windows 和 Windows Server 版本的内存限制](/windows/desktop/Memory/memory-limits-for-windows-releases#physical-memory-limits-windows-server-2016)。

## <a name="examples"></a>示例

### <a name="example-a-set-the-max-server-memory-option-to-4-gb"></a>示例 A. 将最大服务器内存选项设置为 4 GB。
 以下示例将 `max server memory` 选项设置为 4 GB。  请注意，虽然 `sp_configure` 将选项的名称指定为 `max server memory (MB)`，但该示例的演示省略了 `(MB)`。

```sql
sp_configure 'show advanced options', 1;
GO
RECONFIGURE;
GO
sp_configure 'max server memory', 4096;
GO
RECONFIGURE;
GO
```
此操作输出的语句类似于：

> 配置选项“最大服务器内存 (MB)”从 2147483647 更改为 4096。 请运行 RECONFIGURE 语句进行安装。

### <a name="example-b-determining-current-memory-allocation"></a>示例 B. 确定当前内存分配  
 以下查询返回有关当前分配内存的信息。  

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

### <a name="example-c-determining-value-for-max-server-memory-mb"></a>示例 C. 确定“最大服务器内存 (MB)”的值
以下查询返回有关当前配置的值和 SQL Server 使用的值的信息。  无论“显示高级选项”是否为 true，此查询都将返回结果。

```sql
SELECT c.value, c.value_in_use
FROM sys.configurations c WHERE c.[name] = 'max server memory (MB)'
```
  
## <a name="next-steps"></a>后续步骤
 [内存管理体系结构指南](../../relational-databases/memory-management-architecture-guide.md)   
 [监视和优化性能](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [数据库引擎服务启动选项](../../database-engine/configure-windows/database-engine-service-startup-options.md)   
 [SQL Server 2016 的各版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md#Cross-BoxScaleLimits)   
 [SQL Server 2017 的各版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md#Cross-BoxScaleLimits)   
 [Linux 上 SQL Server 2017 的各版本和支持的功能](../../linux/sql-server-linux-editions-and-components-2017.md#Cross-BoxScaleLimits)   
 [Windows 和 Windows Server 版本的内存限制](/windows/desktop/Memory/memory-limits-for-windows-releases)

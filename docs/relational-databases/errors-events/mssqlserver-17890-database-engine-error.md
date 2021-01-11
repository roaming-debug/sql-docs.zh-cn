---
description: MSSQLSERVER_17890
title: MSSQLSERVER_17890
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 17890 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 6a854486d1867e84bcd9b13cb148d3026a41d51f
ms.sourcegitcommit: d819173fb91af6f20ca6ee59686c35c71b060fbc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/28/2020
ms.locfileid: "97797754"
---
# <a name="mssqlserver_17890"></a>MSSQLSERVER_17890
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|17890|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|SRV_WS_TRIMMED|
|消息正文|大部分 SQL Server 进程内存已分页。这可能会导致性能下降。 持续时间: %d 秒。 工作集 (KB): %I64d，已提交 (KB): %I64d，内存使用率: %d%%。|
||

## <a name="explanation"></a>说明

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志或 Windows 应用程序事件日志中可能会出现以下错误消息。

> 大部分 SQL Server 进程内存已分页。这可能会导致性能下降。 持续时间：0 秒。 工作集 (KB)：3383250，已提交 (KB)：9112480，内存利用率：37%。

你还可能会注意到，SQL Server 上的查询执行和所有其他操作的性能突然降低。

## <a name="cause"></a>原因

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 监视有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程的各种内存相关信息。 在这种情况下，它检测到进程的工作集小于已提交的进程内存的 50%。 因此会打印此警告。 此警告的正常原因如下：

- 操作系统会将大部分 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 已提交内存分页到页面文件中。
- 这可能是由于其他应用程序或操作系统突然增加了对内存的需求。
- 当某些设备驱动程序请求按需求进行连续内存分配时，也会发生这种情况。

## <a name="user-action"></a>用户操作

可以通过在物理内存中锁定为缓冲池分配的内存来阻止 Windows 操作系统对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程的缓冲池内存进行分页。 可以通过将“锁定内存页”用户权限分配给用作 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的启动帐户的用户帐户来锁定内存。 但在实现此解决方案之前，请先查看 [SQL Server 内存分页的原因](#what-causes-sql-server-memory-to-be-paged-out)和重要注意事项，然后再为 SQL Server 实例分配“锁定内存页”用户权限

> [!NOTE]
> 使用“锁定内存页”可确保 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理的内存不会进行分页。但是，OS 仍可将线程堆栈、EXE 和任何 DLL 映像、堆内存、CLR 内存分页。
>
> 从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008 SP1 累积更新 2 开始，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard Edition 和 Enterprise Edition 都可以使用“锁定内存页”用户权限。 有关对锁定页面的支持的详细信息，请查看 [KB970070 - 对 SQL Server Standard Edition（64 位）系统上锁定页面的支持](https://support.microsoft.com/help/970070)。

若要分配“锁定内存页”用户权限，请执行以下步骤：

1. 依次单击“开始”和“运行”，键入“gpedit.msc”，然后单击“确定”。
1. 请注意，将显示“组策略”对话框。
1. 依次展开“计算机配置”和“Windows 设置”。
1. 展开 **“安全设置”** ，再展开 **“本地策略”** 。
1. 单击“用户权限分配”，然后双击“锁定内存页”。
1. 在“本地安全策略设置”对话框中，单击“添加用户”或“组”。
1. 在“选择用户”或“组”对话框中，添加有权运行 Sqlservr.exe 文件的帐户，然后单击“确定”。
1. 将关闭“组策略”对话框。
1. 重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务。

分配“锁定内存页”用户权限并重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务后，Windows 操作系统将不再对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程中的缓冲池内存进行分页。 但是，Windows 操作系统仍可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程中对非缓冲池内存进行分页。

可以通过确保在启动时在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志中写入以下消息，来验证 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例是否使用了用户权限：“将锁定页用于缓冲池”

此消息仅适用于 SQL Server。 有关错误日志中此消息的详细信息，请访问以下内容：[是否必须为本地系统中的内存特权分配锁定页面](https://techcommunity.microsoft.com/t5/sql-server-support/do-i-have-to-assign-the-lock-pages-in-memory-privilege-for-local/ba-p/315426)

当 Windows 操作系统页对非缓冲池内存进行分页时，你可能仍会遇到性能问题。 但是，在“说明”部分中提到的错误消息不会记录在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志中。

## <a name="what-causes-sql-server-memory-to-be-paged-out"></a>导致 SQL Server 内存分页的原因

有三种类别的问题会导致此问题：

- 与应用程序相关的问题：所有应用程序一起耗尽了可用的物理内存，操作系统必须为新的应用程序资源请求释放一些内存。 通常情况下，这里的方法是找出哪些应用程序正在耗尽内存，并采取必要步骤平衡它们之间的内存，而不导致 RAM 耗尽。
- 设备驱动程序问题：如果驱动程序不正确地调用内存分配函数，则设备驱动程序可能会导致所有进程的工作集分页。
- 操作系统问题

可以在下面找到有关每个类别的信息

- 与应用程序相关的问题：应用程序一起可能会消耗系统上的所有 RAM。 如果有新的内存请求，则 OS 会尝试满足这些请求，如果没有可用内存，它将修整正在运行的应用程序的工作集以满足内存请求。 在这种情况下，你可能会注意到大多数（如果不是所有）应用程序的工作集都显著下降。 为此，请为系统上的所有应用程序收集以下性能监视器计数器：

  - 性能对象：进程
  - 计数器：工作集
  
  此外，监视以下计数器以关联系统上可用的物理内存量。
  
  - 性能对象：内存
  - 计数器：可用内存 (MB)
  
  你可能观察到的典型行为是：可用内存减少到接近 0 MB，同时系统上大多数（所有）进程的工作集计数器骤降。 如果观察到此类行为，则可能需要采取措施来减少系统上的内存使用量，例如减少 SQL Server 的最大服务器内存。
  
    应用程序也可能占用过多系统缓存，从而导致系统缓存大量增长。 为了响应系统缓存的增长，系统会对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程或其他应用程序的工作集进行分页。 如果遇到此问题，你可以在应用程序中使用一些内存管理函数。 这些函数控制文件 I/O 操作可在应用程序中使用的系统缓存空间。 例如，你可以使用 SetSystemFileCacheSize 函数和 GetSystemFileCacheSize 函数来控制文件 I/O 操作可以使用的系统缓存空间。
  
    可以使用内存性能对象来查看此对象中各种计数器的值，以确定系统缓存工作集是否占用太多内存。 例如，可以查看缓存字节和系统缓存驻留字节数计数器。 有关此主题的详细信息，请参阅：
  
    - [缓存太多](/archive/blogs/ntdebugging/too-much-cache)
    - [Microsoft Windows 动态缓存服务](/archive/blogs/ntdebugging/microsoft-windows-dynamic-cache-service)
    - [当系统文件缓存占用大部分物理 RAM，应用程序和服务会出现性能问题](https://support.microsoft.com/help/976618)
  
    可以下载并部署“Microsoft Windows 动态缓存服务”来控制系统缓存使用的内存。

- 设备驱动程序问题：如果设备驱动程序使用 `MmAllocateContiguousMemory` 函数，并且它将 HighestAcceptableAddress 参数的值设置为小于 4 GB，则 Windows 操作系统可能会对系统上进程的工作集进行分页，包括 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程。 若要解决此问题，请与设备驱动程序的供应商联系以获取驱动程序更新。

    当设备驱动程序尝试分配内存，Windows 操作系统可能会将其他应用程序的工作集分页。 利用此 Windows 修补程序，你可以使用事件跟踪查找导致问题的设备驱动程序。 若要查找有关导致工作集修整行为的特定驱动程序的详细信息，请参阅[识别分配连续内存的驱动程序](/previous-versions/windows/desktop/xperf/identifying-drivers-that-allocate-contiguous-memory)。

- 操作系统问题：若要解决导致 Windows 操作系统对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程工作集进行分页的已知问题，请应用以下 Microsoft 知识库文章中描述的修补程序。

  > [!NOTE]
  > 修补程序是累积的。 修补程序的更高版本包含该修补程序的早期版本。

  - 当系统使用一些高级 TCP 功能时，可能修整 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 集。 有关详细信息，请参阅[如何排查 RSS 和 NetDMA 等高级网络性能功能问题](/troubleshoot/windows-server/networking/troubleshoot-network-performance-features-rss-netdma)。

  - 如果在 Windows Server 2008 上运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，则必须对可能会导致其他操作系统组件对工作集进行修整或占用不必要的过度内存的已知问题应用修补程序。 有关详细信息，请查看以下文章：[在使用 Active Directory 诊断模板运行 Perfmon.exe 以在基于 Windows Server 2008 的域控制器上生成报表时，报表生成进程可能会停止响应](/troubleshoot/windows-server/performance/report-generation-process-stops-responding)。

  - 如果在 Windows Serve 2008 R2 上运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，则必须对可能导致工作集修整的已知问题应用修补程序。 有关详细信息，请查看下列文章：

    - [运行大型应用程序时，运行 Windows 7 或 Windows Server 2008 R2 的计算机变得无响应](https://support.microsoft.com/help/979149)
    - [如果一个线程请求的大量内存在前 4 GB 以内，那么运行 Windows Server 2008 R2 或 Windows 7 的基于 NUMA 的处理器的计算机将出现性能不佳问题](https://support.microsoft.com/help/2155311)
    - [当在 Windows Server 2008 R2 中使用 Storport 驱动程序时，计算机间歇性地运行不佳或停止响应](https://support.microsoft.com/help/2468345)

## <a name="important-considerations-before-you-assign-the-lock-pages-in-memory-user-right"></a>分配“锁定内存页”用户权限之前需要注意的重要事项

在分配“锁定内存页”用户权限之前，还应考虑其他问题。 如果在配置不正确的系统上分配此用户权限，则系统可能会变得不稳定或者降低整个系统的性能。 此外，事件日志中可能会记录事件 ID 333。

如果你联系 Microsoft 客户支持服务 (CSS) 来解决这些问题，CSS 工程师可能会要求你为用作 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的启动帐户的用户帐户吊销此用户权限。 此步骤可能需要收集一些重要的性能数据，CSS 工程师可以使用这些数据来进行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和系统上运行的其他应用程序的各种选项所需的配置。 CSS 工程师收集性能数据后，可以将“锁定内存页”用户权限分配到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的启动帐户。

在分配“锁定内存页”用户权限之前，请确保捕获性能监视器日志来确定系统上安装的各种应用程序和服务的内存需求。 这些应用程序还包括 SQL Server。 要确定内存需求，请收集以下基线信息：

- 请确保正确设置最大服务器内存选项和最小服务器内存选项。 这些选项仅反映 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程的缓冲池的内存需求。 这些选项不包括为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程中的其他组件分配的内存。 这些组件包括以下各项：

  - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工作线程数
  - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程的地址空间中加载的各种 DLL 和组件
  - 备份和还原操作

- DLL 和组件包括各种 OLE DB 提供程序、扩展存储过程、用于 sp_OACreate 存储过程的 Microsoft COM 对象、链接服务器和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CLR。 为这些组件分配的内存位于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程的地址空间的非缓冲池区域下。 为了更好地确定整个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程可以使用的最大内存量，必须将为不使用缓冲池的组件分配的内存从你希望 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程使用的总内存量中减去。 然后，可以使用余数值设置最大服务器内存选项。 在设置最大服务器内存选项和最小服务器内存选项之前，应仔细查看 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书中的“手动设置内存选项”主题。

- 确定其他应用程序和 Windows 操作系统组件的内存需求。 应用程序可以包括其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件，例如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 复制代理、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Reporting Services、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Analysis Services、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Integration Services 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 全文搜索。 执行备份操作和文件复制操作的应用程序可能会使用大量内存。 考虑大容量复制和生成文件 IO 的快照代理等操作。 确定最大服务器内存选项的值和最小服务器内存选项的值时，必须考虑所有这些应用程序的内存需求。 可以在每个进程的进程对象下使用专用字节计数器和工作集计数器来确定特定进程的内存需求。

- 默认情况下，已将“锁定内存页”用户权限分配给内置的本地系统帐户。 有关详细信息，请访问以下 Microsoft 网站：[是否必须为本地系统分配“锁定内存页”权限？](https://techcommunity.microsoft.com/t5/sql-server-support/do-i-have-to-assign-the-lock-pages-in-memory-privilege-for-local/ba-p/315426?advanced=false&collapse_discussion=true&search_type=thread)

- 如果对域中的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程全局使用 Windows 用户帐户，请确定使用组策略配置分配的用户权限。 32 位 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程可以使用此帐户作为启动帐户。 但是，此帐户需要“锁定内存页”用户权限才能启用 `Address Windowing Extensions` (AWE) 功能。 有关详细信息，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书中的“为 SQL Server 提供最大内存量”主题。

- 在为多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例配置最大服务器内存选项和最小服务器内存选项之前，请考虑每个 SQL Server 实例的非缓冲池的内存需求。 然后，为每个 SQL Server 实例配置这些选项。

理想情况下，你可以在负峰值负载期间收集此基准信息。 因此，你可以确定各个应用程序和组件的内存要求，以支持峰值负载。 内存要求因系统而异，这取决于系统上运行的活动和应用程序。 你可以查询动态管理视图 sys.dm_os_process_memory 中提供的信息，以了解系统是否遇到内存不足的情况。 有关详细信息，请参阅 [sys.dm_os_process_memory (Transact-SQL)](/sql/relational-databases/system-dynamic-management-views/sys-dm-os-process-memory-transact-sql)。

## <a name="improvements-added-in-windows-server-2008-and-r2-version"></a>Windows Server 2008 和 R2 版本中的新增改进

Windows Server 2008 和 Windows Server 2008 R2 改进了连续内存分配机制。 这种改进使得 Windows Server 2008 和 Windows Server 2008 R2 在一定程度上减少了在收到新内存请求时对应用程序工作集分页的影响。

下面是来自 Microsoft 白皮书“Windows 中的内存管理改进”的改进说明：

*在 Windows Server 2008 中，大大增强了物理连续内存的分配。分配连续内存的请求更有可能成功，因为内存管理器现在会动态替换页面，通常不需要修整工作集或执行 I/O 操作。此外，许多其他类型的页面（如内核堆栈和文件系统元数据页等）现在都是替换的候选页面。因此，在任何给定时间，都可以使用更多的连续内存。此外，获得此类分配的成本显著降低。*

有关详细信息，请查看 [SQL Server 工作集修整问题](https://techcommunity.microsoft.com/t5/sql-server-support/sql-server-working-set-trim-problems-consider/ba-p/315462?advanced=false&collapse_discussion=true&search_type=thread)。

本文所讨论的第三方产品由独立于 Microsoft 的公司生产。 Microsoft 对这些产品的性能和可靠性不作任何明示或默示担保。

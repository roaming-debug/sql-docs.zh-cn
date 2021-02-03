---
description: MSSQLSERVER_833
title: MSSQLSERVER_833 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 833 (Database Engine error)
ms.assetid: 14129cc4-be80-4772-9e3f-0e5da4d0696b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d562e3ffcde77c706d0cf56a92f188b895eeb457
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197881"
---
# <a name="mssqlserver_833"></a>MSSQLSERVER_833
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sql-asdbmi.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|833|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|BUF_LONG_IO|  
|消息正文|SQL Server 已 %d 次遇到了针对数据库 `[%ls] (%d)` 中文件 [%ls] 的、所需完成时间超过 %d 秒的 I/O 请求。  OS 文件句柄是 0x%p。  最新的长时间 I/O 操作的偏移量是: %#016I64x。|  
  
## <a name="explanation"></a>说明  
该消息指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 已从磁盘发出读取或写入请求，并且表明该请求返回所用的时间已超过 15 秒。 此错误是由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 报告，表明 I/O 子系统有问题。 你可能还会注意到与此消息关联的其他症状：PAGEIOLATCH 等待时间很长、系统事件日志中有警告或错误、系统监视计数器中有磁盘延迟问题迹象。 请监视 [sys.dm_io_virtual_file_stats](../system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql.md) 并为存储吞吐量选择适当的存储层和 IOPS。 
  
### <a name="possible-causes"></a>可能的原因  
这种问题可能是由于以下原因所致：操作系统性能问题、硬件错误、固件错误、设备驱动程序问题或在 I/O 进程或数据库文件存储路径中存在筛选器驱动程序干预。 SQL Server 记录它发起 I/O 请求的时间，并记录 I/O 完成的时间。 如果时间差为 15 秒或更长时间，则会检测到这种情况。 这也意味着 SQL Server 不是导致此消息描述和报告的 I/O 延迟情况的原因。 这种情况称为“停滞的 I/O”。 大多数磁盘请求在磁盘的典型速度内发生。 这种典型的磁盘速度通常称为“磁盘寻道时间”。 大多数标准磁盘的磁盘寻道时间是 10 毫秒或更短。 因此，对于系统 I/O 路径返回到 SQL Server 来说，15 秒是一个非常长的时间。 
  
## <a name="user-action"></a>用户操作  
通过检查系统事件日志获得硬件相关错误消息来纠正引错误。 并且，如果有特定于硬件的日志，也要进行检查。 应使用必要的方法和技术来确定操作系统、驱动程序或 I/O 硬件中出现延迟的原因。 此问题的解决方法可能涉及更新所有设备驱动程序和固件，或执行与磁盘系统关联的其他诊断。 
  
使用性能监视器检查以下计数器：  
  
-   **Average Disk Sec/Transfer**  
  
-   **Average Disk Queue Length**  
  
-   **Current Disk Queue Length**  
  
例如，运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机上的 **Average Disk Sec/Transfer** 时间通常少于 15 毫秒。 如果 **Average Disk Sec/Transfer** 值增加，这表明 I/O 子系统未能完全满足 I/O 需求。

还可以使用像 [Storport ETW 日志记录](/archive/blogs/ntdebugging/storport-etw-logging-to-measure-requests-made-to-a-disk-unit)这样的工具来度量向磁盘单元发出的请求的延迟情况。 另一种类似的磁盘 I/O 故障排除工具包是作为 [Windows Performance Recorder](/windows-hardware/test/wpt/introduction-to-wpr) 的内置配置文件提供的。
  
> [!NOTE]  
> 防病毒程序可能会减慢磁盘访问速度。 若要提高访问速度，请将错误消息中指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据文件从实时病毒扫描中排除。 可以使用 [fltmc.exe 命令行实用工具](/windows-hardware/drivers/ifs/development-and-testing-tools#fltmcexe-control-program)来查询系统上安装的所有筛选器驱动程序，并了解它对数据库文件的存储路径执行的功能。 
  
有关 I/O 错误的详细信息，请参阅 [Microsoft SQL Server I/O 基础知识，第 2 章](/previous-versions/sql/sql-server-2005/administrator/cc917726(v=technet.10))以及 [https://support.microsoft.com/kb/897284/en-us](https://support.microsoft.com/kb/897284/en-us) 上的知识库文章。  

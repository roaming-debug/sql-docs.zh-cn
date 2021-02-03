---
title: MSSQLSERVER 错误 823 | mssqlserver_823
description: Microsoft SQL Server 错误 823 (mssqlserver_823) 的描述和一些常见解决方案。这是一种严重的系统级错误条件，可威胁到数据库的完整性，必须立即解决。
ms.custom: ''
ms.date: 01/27/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 823 (Database Engine error)
ms.assetid: 0d9fce3c-3772-46ce-a7a3-4f4988dc6cae
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7ed748bdf748aa51813d11348859bc769525ed35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197898"
---
# <a name="mssqlserver-error-823"></a>MSSQLSERVER 错误 823
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|823|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|B_HARDERR|  
|消息正文|在文件 '%ls' 中、偏移量为 %#016I64x 的位置执行 %S_MSG 期间，操作系统已经向 SQL Server 返回了错误 %ls。 SQL Server 错误日志和系统事件日志中的其他消息中可能有更详细的信息。 这是一个威胁数据库完整性的严重系统级错误条件，必须立即纠正。 请运行一次完整的数据库一致性检查 (DBCC CHECKDB)。 此错误可能是由多种因素导致的；有关详细信息，请参阅 SQL Server 联机丛书。|  
  
## <a name="explanation"></a>说明  
SQL Server 使用 Windows API（例如 [ReadFile](/windows/win32/api/fileapi/nf-fileapi-readfile)、[WriteFile](/windows/win32/api/fileapi/nf-fileapi-writefile)、[ReadFileScatter](/windows/win32/api/fileapi/nf-fileapi-readfilescatter)、[WriteFileGather](/windows/win32/api/fileapi/nf-fileapi-writefilegather)）执行文件 I/O 操作。 执行这些 I/O 操作后，SQL Server 检查所有与这些 API 调用相关联的错误条件。 如果 API 调用因操作系统错误而失败，则 SQL Server 报告错误 823。

 823 错误消息包含以下信息：
 - 对其执行 I/O 操作的数据库文件
 - 尝试执行 I/O 操作的文件中的偏移。 这是距文件开头的物理字节偏移。 如果将此数字除以 8192，将得到受错误影响的逻辑页码。
 - I/O 操作是一个读取请求还是写入请求
 - 操作系统错误代码和括号中的错误说明
 

**操作系统错误：** 读取或写入 Windows API 调用失败，SQL Server 遇到与 Windows API 调用相关的操作系统错误。 下面是 823 错误的示例消息：

```
Error: 823, Severity: 24, State: 2.
2010-03-06 22:41:19.55 spid58 The operating system returned error 1117 (The request could not be performed because of an I/O device error.) to SQL Server during a read at offset 0x0000002d460000 in file 'e:\program files\Microsoft SQL Server\mssql\data\mydb.MDF'. Additional messages in the SQL Server error log and system event log may provide more detail. This is a severe, system-level error condition that threatens database integrity and must be corrected immediately. It is recommended to complete a full database consistency check (DBCC CHECKDB). This error can be caused by many factors; for more information, see SQL Server Books Online.
```

在错误消息中，你可能会在与文件相关联的数据库上看到 DBCC CHECKDB 语句的错误。 出现 823 错误时，你可运行 DBCC CHECKDB 语句。 如果 DBCC CHECKDB 语句未报告任何错误，则可能会出现间歇性系统问题或磁盘问题。

使用跟踪标志 818 时，可能会向 SQL Server 错误日志文件中写入 823 错误的其他诊断信息。
有关详细信息，请参阅 [KB 826433：添加了其他 SQL Server 诊断以检测未报告的 I/O 问题](https://support.microsoft.com/help/826433/sql-server-diagnostics-added-to-detect-unreported-i-o-problems-due-to)


## <a name="cause"></a>原因
823 错误消息通常指示基础存储系统或硬件出现问题，或 I/O 请求路径中的驱动程序出现问题。 如果文件系统中存在不一致，或者数据库文件已损坏，则可能遇到此错误。 如果读取了文件，SQL Server 在返回 823 之前已重试了四次该读取请求。 如果重试操作成功，则查询就不会失败，但会将消息 [825](mssqlserver-825-database-engine-error.md) 写入错误日志和事件日志。

## <a name="user-action"></a>用户操作  
 - 查看 MSDB 中的 [suspect_pages](../system-tables/suspect-pages-transact-sql.md) 表中是否存在遇到此问题的其他页（在同一数据库或不同数据库中）。
 - 使用 DBCC CHECKDB 命令检查位于同一卷（823 消息中所报告的卷）上的数据库的一致性。 如果发现 DBCC CHECKDB 命令不一致，请使用[如何解决 DBCC CHECKB 报告的数据库一致性错误](https://support.microsoft.com/help/2015748/how-to-troubleshoot-database-consistency-errors-reported-by-dbcc-check)中的指南。 
 - 查看 Windows 事件日志中是否存在从操作系统或存储设备/设备驱动程序中报告的错误或消息。 如果它们在一定程度上与此错误相关，请先解决这些错误。 例如，除了 823 消息以外，你还可能会注意到事件日志中的磁盘源报告的“驱动程序检测到 \Device\Harddisk4\DR4 上的控制器错误”之类的事件。 在这种情况下，你须评估此设备上是否存在此文件，然后先更正这些磁盘错误。
 - 使用 [SQLIOSim](https://support.microsoft.com/help/231619/how-to-use-the-sqliosim-utility-to-simulate-sql-server-activity-on-a-d) 实用工具确定这些 823 错误是否可在常规 SQL Server I/O 请求之外重现。 SQLIOSim 实用工具随 SQL Server 2008 及更高版本进行装配，因此无需单独下载。 通常可以在 `C:\Program Files\Microsoft SQL Server\MSSQLxx.MSSQLSERVER\MSSQL\Binn` 文件夹中找到它。
 - 与硬件供应商或设备制造商合作，确保：
   - 硬件设备和配置符合 SQL Server 的 I/O 要求
   - I/O 路径中所有设备的设备驱动程序和其他支持软件组件都是最新的
 - 如果硬件供应商或设备制造商为你提供了一些诊断实用工具，请用这些实用工具评估 I/O 系统的运行状况
 - 评估在遇到问题的这些 I/O 请求的路径中是否存在[筛选器驱动程序](https://support.microsoft.com/help/2454053/use-of-system-filter-drivers-can-lead-to-sql-server-database-engine-pe)。
   - 检查这些筛选器驱动程序是否有任何更新
   - 能否删除或禁用这些筛选器驱动程序，以观察导致 823 错误的问题是否消失  

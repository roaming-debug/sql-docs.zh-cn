---
description: MSSQLSERVER_855
title: MSSQLSERVER_855
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 855 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 6e68601a2690149b61fcea661d26843f16079a03
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180981"
---
# <a name="mssqlserver_855"></a>MSSQLSERVER_855
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|855|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|BAD_MEMORY_OUTSIDE_BPOOL|
|消息正文|检测到无法纠正的硬件内存损坏。 系统可能会变得不稳定。 有关更多详细信息，请查看 Windows 事件日志|
||

## <a name="explanation"></a>说明

此消息指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在缓冲池外部的缓存对象中检测到错误的内存页面。 在支持从内存错误中恢复的系统上会出现此消息。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法在出现这些情况时恢复，而且无法记录此消息。

## <a name="user-action"></a>用户操作

应运行硬件或系统检查，确定是否存在内存或 CPU 问题。 确保所有系统驱动程序、操作系统更新和硬件更新均已应用到系统。 考虑运行任何硬件制造商诊断，包括与内存相关的测试。 每次看到此错误时，都请考虑针对此实例中的所有数据库运行 `DBCC CHECKDB`。

## <a name="more-information"></a>详细信息

在具有较新硬件且运行 Windows Server 2012 或更高版本的计算机上，硬件可通知操作系统和应用程序，使其知道内存页面（操作系统页面）已被标记为错误或已损坏。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 等应用程序可使用以下 API 集来注册这些“内存页面错误”通知：

- `GetMemoryErrorHandlingCapabilities`
- `RegisterBadMemoryNotification`
- `BadMemoryCallbackRoutine`

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在 Microsoft [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本中增加了对这些通知的支持。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动过程中，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会检查硬件是否支持此新功能。 此外，你会在错误日志中收到以下消息：

> \<Datetime> 服务器计算机支持内存错误恢复。 会启用 SQL 内存保护以从内存损坏中恢复。

当前，只有当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 收到这些通知时，缓冲池才会执行操作。 当它收到通知时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 必须遍历整个缓冲池，并发现每个已分配的缓冲区的地址。 然后，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 `QueryWorkingSetEX` API 来检查是否有任何支持数据页面的内存页面被标记为错误。 如果报告有任何损坏，则与该内存页面相对应的 `PSAPI_WORKING_SET_EX_BLOCK` 输出结构会将其成员错误设置为 1。

如果相应缓冲池或数据页面当前未更改或未处理 I/O，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可放弃并取消提交数据页页面。 然后，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会记录以下消息：

> SQL Server 检测到数据库“%ls”中的硬件内存损坏（文件 ID 为 %u、页面 ID为 %u、内存地址为0x%I64x），并且已成功恢复页面。

当查询再次需要相应数据页面时，缓冲池可从磁盘读回数据页面，并将内容恢复到缓冲池。 页面的磁盘版本也可能会处于已损坏状态。 在这种情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能会记录其他错误，例如错误 824。

如果错误页面并非由缓冲池使用，而是由某些其他缓存的对象或结构使用，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会记录以下消息：

> 检测到无法纠正的硬件内存损坏。 系统可能会变得不稳定。 有关更多详细信息，请查看 Windows 事件日志。

如果服务器报告内存错误，则你应与计算机硬件供应商联系，并执行相应的操作，例如执行内存诊断、更新 BIOS 和固件，以及更换错误的内存模块。

可使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 跟踪标志 849 来阻止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 向操作系统注册以获取内存错误通知。 但请注意，启用跟踪标志 849 将阻止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从操作系统接收内存错误通知。 因此，建议不要在典型情况下使用此跟踪标志。

另请注意，默认情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在受支持的硬件上接收这些通知。

还应注意，当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 注册以获取这些内存错误通知时，惰性写入器系统进程不会执行固定页面检查。 有关固定页面检查的详细信息，请参阅[如何对 SQL Server 中的消息 832（固定页面已更改）进行故障排除](https://support.microsoft.com/help/2015759)。

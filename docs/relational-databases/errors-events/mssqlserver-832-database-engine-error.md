---
description: MSSQLSERVER_832
title: MSSQLSERVER_832
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 832 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: bba8712825dc3ef14617ad4e8d8f7312b479e936
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180294"
---
# <a name="mssqlserver_832"></a>MSSQLSERVER_832
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|832|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|B_CONSTPAGECHANGED|
|消息正文|本应保持不变的页面已更改（预期校验和 \<expected value>、实际校验和 \<actual value>、数据库 \<dbid>、文件 \'<filename>' 和页面 \<pageno>）。 此错误通常指示存在内存故障或其他硬件损坏或操作系统损坏。|
||

## <a name="explanation"></a>说明

外部因素导致数据库页面在用于更改数据库页面的常规 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 引擎代码之外被修改。  条件可能是：  

- 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程中运行的线程在数据库页面上执行错误写入操作。 这通常称为“涂鸦器”
- 硬件或操作系统问题是，支持数据库页面的内存遭到错误地修改或损坏  

当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 检测到此行为时，将引发错误 832。

## <a name="user-action"></a>用户操作

若要找出错误原因，请考虑以下选项：

- 应运行任何正常的硬件或系统检查，确定是否存在内存、CPU 或其他与硬件相关的问题。 确保所有系统驱动程序、操作系统更新和硬件更新均已应用到系统。 考虑运行任何硬件制造商诊断，包括与内存相关的测试。
- 评估 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中可能加载了哪些会导致此问题的“外部”DLL，包括扩展存储过程、COM 对象或其他可能错误地修改了为数据库页面保留的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存的 DLL。  

每次看到此错误时，都应立即考虑针对错误消息中 \<dbid> 引用的数据库运行 `DBCC CHECKDB`。

## <a name="more-information"></a>详细信息

此错误是由通常称为 LazyWriter 的后台任务检测到的。 （此任务的“命令”被视为 LAZY WRITER）。 因此，此错误不会返回到客户端应用程序。 此错误将以 EventID=832 的形式写入 Windows 应用程序事件日志。  

仅检查缓存中当前未修改（或“更新”）的页面。 此消息使用“固定”一词的原因是，自从磁盘读取页面以来从未对该页面进行更改。 此外，此页面是在“干净”模式下从磁盘读取的，因为它在页面上具有校验和值，但从未遇到校验和失败（消息 824）。 但是，可能会在此错误发生后的某个时间点修改此页面，然后以不正确的修改写入磁盘。 如果发生此错误，则在修改写入磁盘之前，将基于所有修改来计算新的校验和。 因此，磁盘上的页面可能已损坏，但随后从磁盘读取可能不会触发校验和失败。 请务必在此错误引用的所有数据库上运行 `DBCC CHECKDB`。  

即使 `DBCC CHECKDB` 在写入磁盘后，也可能不会报告此状态下的页面错误。 这是因为不正确的修改可能位于既不包含任何数据，也不包含任何重要的页面或行结构信息的页面位置，或者可能是对 CHECKDB 无法检测到的数据所做的修改。  

有关消息 832 的更多详细信息，另请参阅白皮书 [SQL Server I/O 基础知识，第 2 章](/previous-versions/sql/sql-server-2005/administrator/cc917726(v=technet.10))。
---
description: MSSQLSERVER_854
title: MSSQLSERVER_854
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 854 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: f0bf9061b452329280f0625bcc1339469372f4df
ms.sourcegitcommit: ead0b8c334d487a07e41256ce5d6acafa2d23c9d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "92418703"
---
# <a name="mssqlserver_854"></a>MSSQLSERVER_854
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|854|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|HARDWARE_MEMORY_SCRUBBER|
|消息正文|计算机支持内存错误恢复。 会启用 SQL 内存保护以从内存损坏中恢复|
||

## <a name="explanation"></a>说明

此消息表示操作系统中的硬件支持从内存错误中恢复。 在具有较新硬件且运行 Windows Server 2012 或更高版本的计算机上，硬件可通知操作系统和应用程序，使其知道内存页面（操作系统页面）已被标记为错误或已损坏。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 等应用程序可使用以下 API 集来注册这些“内存页面错误”通知：

- `GetMemoryErrorHandlingCapabilities`
- `RegisterBadMemoryNotification`
- `BadMemoryCallbackRoutine`

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012 及更高版本中增加了对这些通知的支持。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动过程中，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会检查硬件是否支持此新功能。 此外，你会在错误日志中收到以下消息：

> \<Datetime> 服务器计算机支持内存错误恢复。 会启用 SQL 内存保护以从内存损坏中恢复。

## <a name="user-action"></a>用户操作

检查是否遇到其他错误（如 855 和 856），并采取适当的纠正措施。

## <a name="more-information"></a>详细信息

可使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 跟踪标志 849 来阻止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 向操作系统注册以获取内存错误通知。 但请注意，启用跟踪标志 849 将阻止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从操作系统接收内存错误通知。 因此，建议不要在典型情况下使用此跟踪标志。

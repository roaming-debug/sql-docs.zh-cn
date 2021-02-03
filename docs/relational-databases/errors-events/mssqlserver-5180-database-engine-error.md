---
description: MSSQLSERVER_5180
title: MSSQLSERVER_5180
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5180 (Database Engine error)
ms.assetid: ''
author: rgward
ms.author: ramakoni
ms.openlocfilehash: f80f4dafad6b0570837728e15a79eebdd7d6f41c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185531"
---
# <a name="mssqlserver_5180"></a>MSSQLSERVER_5180
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|5180|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|DSK_BAD_FCB_FILEID|
|消息正文|对于数据库 '%.*ls' 中无效的文件 ID %d，无法打开文件控制区(FCB)。 请验证文件位置。 执行 DBCC CHECKDB。|
||

## <a name="explanation"></a>说明

当 [!INCLUDE[ssDENoVersion](../../includes/ssdenoversion_md.md)] 引用了无效的文件 ID 时，某项查询或操作可能会失败，并出现错误 5180。 下面是一个示例：

> 消息 5180，级别 22，状态 1，行 1  
对于数据库 '%.*ls' 中无效的文件 ID %d，无法打开文件控制区(FCB)。 请验证文件位置。 执行 DBCC CHECKDB。

由于引发错误的严重性为 22，系统将断开用户的会话。 此错误消息将写入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志和 EventID = 5180 的 Windows 应用程序事件日志中。

## <a name="possible-causes"></a>可能的原因

[!INCLUDE[ssDENoVersion](../../includes/ssdenoversion-md.md)] 会在很多不同情况下引用文件 ID，主要是在引用页面 ID 时（因为文件 ID 是页面 ID 的第一部分）。 如果由于任何原因，引用的文件 ID 小于 0 或不是数据库中的有效文件 ID（根据系统目录视图中列出的有效文件 ID，例如 sys.database_files），则可能会遇到 5180 错误。

一种可能的原因是存储的文件 ID 无效。 由于行中转发的记录引用了其他页面，访问该页面且文件 ID 无效时，可能会遇到 5180 错误。 这种情况下，具有转发记录的页面上会出现数据库损坏错误。 （在此示例中，`DBCC CHECKDB` 将报告消息 8993）。

另一个示例是页面 ID 中的无效文件 ID，该文件 ID 存储在页面标头中用于下一页或上一页 ID。 此字段用于链接一系列页面，例如在聚集索引中。 如果文件 ID 对于上一页或下一页无效，则当引擎必须引用此 ID 才能遍历下一页或上一页时，可能会遇到 5180 错误。 （在此示例中，`DBCC CHECKDB` 报告消息 8981）。

如果问题不是出在由于存储页面 ID 损坏而导致的无效文件 ID，则问题可能出在 [!INCLUDE[ssDENoVersion](../../includes/ssdenoversion-md.md)] 中。

## <a name="user-action"></a>用户操作

如果遇到此错误，则应针对错误消息中列出的数据库运行 `DBCC CHECKDB`。 如果发现错误，则应从已知的完好备份中还原。 如果无法从备份还原，则还可选择使用其输出建议的 `DBCC CHECKDB` 修复选项。 在大多数情况下，修复此类错误将导致数据丢失。 有关使用数据库及其损坏原因的详细信息，请参阅下文：[如何排查 DBCC CHECKDB 报告的数据库一致性错误](https://support.microsoft.com/kb/2015748)。

如果 `DBCC CHECKDB` 未报告任何错误，但问题仍然存在，则应联系 Microsoft 技术支持人员寻求帮助。 准备提供遇到 5180 错误的查询。 请参阅[详细信息](#more-information)部分，了解如何确定遇到错误的查询。

## <a name="more-information"></a>详细信息

文件控制块 (FCB) 是一种内部存储结构，用于跟踪与数据库关联的文件的相关重要信息。 文件 ID 用于唯一标识数据库的 FCB。 如果服务器引擎尝试引用无效的文件 ID，则找不到触发 5180 错误条件的 FCB 结构。

若要找出遇到此错误的查询，可使用以下方法：

- 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008 及更高版本，请查看 system_health 会话是否有错误记录，该记录中应包含查询文本。 有关 system_health 会话的详细信息，请查看以下资源：[支持 SQL Server 2008：system_health 会话](https://techcommunity.microsoft.com/t5/sql-server-support/supporting-sql-server-2008-the-system-health-session/ba-p/315509)。
- 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 探查器，并捕获 `SQL:BatchStarting`、`RPC:Starting` 和异常事件。 查找与异常事件关联的会话的 5180 异常事件之前的查询。

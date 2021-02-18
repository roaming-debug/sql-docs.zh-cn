---
description: SSMS 使用情况和诊断数据收集的本地审核
title: 使用情况和诊断数据
ms.custom: seo-lt-2019
ms.date: 04/16/2019
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c28ffa44-7b8b-4efa-b755-c7a3b1c11ce4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b2e188200daf0469f80cf74db901bcc240fd8706
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340696"
---
# <a name="local-audit-for-ssms-usage-and-diagnostic-data-collection"></a>SSMS 使用情况和诊断数据收集的本地审核
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SQL Server Management Studio (SSMS) 包含已启用 Internet 的功能，这些功能可收集并向 Microsoft 发送匿名功能使用情况和诊断数据。 SSMS 可能会收集标准计算机信息以及有关使用情况和性能的信息，并可能会将这些信息传输给 Microsoft 进行分析，以便改进 SSMS 的质量、安全性和可靠性。 我们不会收集你的姓名、地址或其他联系信息。 有关详细信息，请参阅 [Microsoft 隐私声明](https://privacy.microsoft.com/privacystatement)和 [SQL Server 隐私补充](../sql-server/sql-server-privacy.md)。

## <a name="audit-feature-usage-and-diagnostic-data"></a>审核功能使用情况和诊断数据

若要查看 SSMS 收集的功能使用情况数据，请执行以下步骤：

1.  启动 SSMS。
2.  单击“查看”，然后单击主菜单中的“输出”以显示“输出”窗口。 
3.  看到“输出”窗口后，选择“显示输出来源:”菜单中的“遥测”。

使用 SSMS 与数据库进行交互时，“输出”窗口显示收集的数据。

## <a name="enable-or-disable-usage-and-diagnostic-data-collection-in-ssms"></a>在 SSMS 中启用或禁用使用情况和诊断数据收集

若要选择启用或禁用 SSMS 使用情况数据收集，请执行以下操作：

- 对于 SQL Server Management Studio 17：

  `Subkey = HKEY_CURRENT_USER\Software\Microsoft\SQL Server Management Studio\14.0`

  注册表项名称 = `UserFeedbackOptIn`

  条目类型 `DWORD`：`0` 表示选择禁用；`1` 表示选择启用

  此外，SSMS 17.x 基于 Visual Studio 2015 shell，且 Visual Studio 安装默认支持客户反馈。  

  若要将 Visual Studio 配置为对各台计算机禁用客户反馈，请将以下注册表子项的值更改为字符串 `0`：`HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\VisualStudio\SQM OptIn`

  例如，将子项更改为下面的内容：  
  `HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\VisualStudio\SQM OptIn `=` 0`

  SQL Server 2017 使用情况和诊断数据收集遵循这些注册表子项上基于注册表的组策略。

- 对于 SQL Server Management Studio 18：

  `Subkey = HKEY_CURRENT_USER\Software\Microsoft\SQL Server Management Studio\18.0_IsoShell`

  注册表项名称 = `UserFeedbackOptIn`

  条目类型 `DWORD`：`0` 表示选择禁用；`1` 表示选择启用

## <a name="see-also"></a>请参阅

- [配置 SQL Server 的使用情况和诊断数据收集](../sql-server/usage-and-diagnostic-data-configuration-for-sql-server.md)
- [SQL Server 使用情况和诊断数据收集的本地审核](../sql-server/usage-and-diagnostic-data-in-local-audit.md)
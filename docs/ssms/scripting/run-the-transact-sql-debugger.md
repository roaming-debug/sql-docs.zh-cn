---
title: 运行 Transact-SQL 调试器
description: 了解如何自定义 Transact-SQL 调试器，以及如何使用它调试 Transact-SQL 代码。 可以在另一台计算机上的数据库引擎实例上运行调试器。
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, sysadmin requirement
- Transact-SQL debugger, supported versions
- Query Editor [Database Engine], right-click menu
- debugging [SQL Server], T-SQL debugger
- Transact-SQL debugger, Query Editor shortcut menu
- Transact-SQL debugger, stopping
- Transact-SQL debugger, Debug menu
- debugging [SQL Server]
- Transact-SQL debugger, Debug toolbar
- Transact-SQL debugger, keyboard shortcuts
- Transact-SQL debugger, starting
ms.assetid: 386f6d09-dbec-4dc7-9e8a-cd9a4a50168c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e9fe76a738bc38eafc1c778fef6a6f91d93492e2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466278"
---
# <a name="run-the-transact-sql-debugger"></a>运行 Transact-SQL 调试器

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

在打开 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询编辑器窗口之后，即可启动 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 调试器。 然后，您可在调试模式下运行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码，直到您停止调试器。 您可以设置选项以自定义调试器的运行方式。

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="starting-and-stopping-the-debugger"></a>启动和停止调试器

启动 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器的要求如下：

- 如果您的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器连接到其他计算机上的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，则必须配置调试器进行远程调试。 有关详细信息，请参阅 [运行 TSQL 调试器之前配置防火墙规则](./configure-firewall-rules-before-running-the-tsql-debugger.md)。
  
- [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 必须在作为 sysadmin 固定服务器角色成员的 Windows 帐户下运行。

- [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器窗口必须使用 Windows 身份验证来连接，或使用作为 sysadmin 固定服务器角色成员的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证登录名来连接。
  
- [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器窗口必须从 [!INCLUDE[ssDE](../../includes/ssde-md.md)] Service Pack 2 (SP2) 或更高版本连接到 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 实例。 如果查询编辑器窗口连接到处于单用户模式下的实例，您将无法运行调试器。  
  
 我们建议在测试服务器上调试 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码，而不要在生产服务器上调试，原因如下：
  
- 调试是一项需要高特权的操作。 因此只允许 sysadmin 固定服务器角色成员在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中进行调试。
  
- 当您调查多个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的运行时，调试会话通常会运行很长时间。 会话获取的锁（如更新锁）可能会持有很长时间，直到终止会话或者提交或回滚事务。  
  
 启动 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器可将查询编辑器窗口置于调试模式。 在查询编辑器窗口进入调试模式时，调试器会在第一个代码行处暂停。 然后，您可以单步执行代码，在特定 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句上暂停执行，并使用调试器窗口来查看当前执行状态。 可以通过在 **“查询”** 工具栏上单击 **“调试”** 按钮，或在 **“调试”** 菜单上单击 **“启动调试”** 来启动调试器。  
  
 查询编辑器窗口会保持在调试模式下，直到查询编辑器窗口中的最后一个语句完成或您停止调试模式。 可以使用以下任何一种方法来停止调试模式和语句执行：  
  
- 在 **“调试”** 菜单中，单击 **“停止调试”** 。  
  
- 在 **“调试”** 工具栏上，单击 **“停止调试”** 按钮。  
  
- 在 **“查询”** 菜单上，单击 **“取消执行查询”** 。  
  
- 在 **“查询”** 工具栏上，单击 **“取消执行查询”** 按钮。  
  
 也可在 [!INCLUDE[tsql](../../includes/tsql-md.md)] “调试” **菜单上单击** “全部分离” **，以停止调试模式，但允许剩余的** 语句完成执行。  
  
## <a name="controlling-the-debugger"></a>控制调试器

 可以使用以下命令、工具栏和快捷方式控制 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器的运行方式：  
  
- **“调试”** 菜单和 **“调试”** 工具栏。 在焦点放到打开的查询编辑器窗口中之前， **“调试”** 菜单和 **“调试”** 工具栏处于不活动状态。 它们将保持活动状态，直到关闭当前项目。  
  
- 调试器键盘快捷键。  
  
- 查询编辑器快捷菜单。 当右键单击查询编辑器窗口中的某一行时，会显示快捷菜单。 当查询编辑器窗口处于调试模式时，快捷菜单会显示适用于所选的行或字符串的调试器命令。  
  
- 调试器打开的窗口中的菜单项和上下文命令，例如 **“监视”** 或 **“断点”** 。  
  
 下表显示了调试器菜单命令、工具栏按钮和键盘快捷键。  
  
|调试菜单命令|编辑器快捷方式命令|工具栏按钮|键盘快捷键|操作|  
|------------------------|-----------------------------|--------------------|-----------------------|------------|  
|**窗口/断点**|不可用|**“断点”**|Ctrl+Alt+B|显示 **“断点”** 窗口，您可在其中查看和管理断点。|  
|**窗口/监视/监视1**|不可用|**断点/监视/监视1**|CTRL+ALT+W, 1|显示 **“监视1”** 窗口。|  
|**窗口/监视/监视2**|不可用|**断点/监视/监视2**|CTRL+ALT+W, 2|显示 **“监视2”** 窗口。|  
|**窗口/监视/监视3**|不可用|**断点/监视/监视3**|CTRL+ALT+W, 3|显示 **“监视3”** 窗口。|  
|**窗口/监视/监视4**|不可用|**断点/监视/监视4**|Ctrl+Alt+W，4|显示 **“监视4”** 窗口。|  
|**窗口/局部变量**|不可用|**断点/局部变量**|Ctrl+Alt+V，L|显示 **“局部变量”** 窗口。|  
|**窗口/调用堆栈**|不可用|**断点/调用堆栈**|Ctrl+Alt+C|显示 **“调用堆栈”** 窗口。|  
|**窗口/线程**|不可用|**断点/线程**|Ctrl+Alt+H|显示 **“线程”** 窗口。|  
|**继续**|不可用|**继续**|Alt+F5|运行到下一个断点。 在将焦点放在处于调试模式的查询编辑器窗口上之前， **“继续”** 处于不活动状态。|  
|**“调试”**|不可用|**“调试”**|Alt+F5|将查询编辑器窗口置于调试模式，并运行到第一个断点。 如果将焦点放在处于调试模式的查询编辑器窗口上，则 **“启动调试”** 将由 **“继续”** 替代。|  
|**全部中断**|不可用|**全部中断**|Ctrl+Alt+Break|[!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器不使用此功能。|  
|**停止调试**|不可用|**停止调试**|Shift+F5|使查询编辑器窗口脱离调试模式，并将其恢复到常规模式。|  
|**菜单上单击**|不可用|不可用|不可用|停止调试模式，但在查询编辑器窗口中执行剩余的语句。|  
|**步入**|不可用|**步入**|F11|运行下一个语句，如果下一个语句运行存储过程、触发器或函数，还将打开处于调试模式的新查询编辑器窗口。|  
|**步越**|不可用|**步越**|F10|与 **“逐语句”** 相同，只不过它不会调试函数、存储过程或触发器。|  
|**步出**|不可用|**步出**|SHIFT+F11|执行触发器、函数或存储过程中的剩余代码，而不在任何断点处暂停。 当控件返回到调用该模块的代码时，常规调试模式恢复。|  
|不可用|**运行至光标处**|不可用|Ctrl+F10|从上次停止位置开始执行所有代码，一直到当前光标位置，在断点处不停止。|  
|**快速监视**|**快速监视**|不可用|Ctrl+Alt+Q|显示 **“快速监视”** 窗口。|  
|**切换断点**|**断点/插入断点**|不可用|F9|在当前或选定的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句上设置一个断点。|  
|不可用|**断点/删除断点**|不可用|不可用|从选定行上删除断点。|  
|不可用|**断点/禁用断点**|不可用|不可用|禁用选定行上的断点。 断点在代码行中保留，但不会停止执行，直到重新启用断点。|  
|不可用|**断点/启用断点**|不可用|不可用|启用选定行上的断点。|  
|**删除全部断点**|不可用|不可用|CTRL+SHIFT+F9|删除所有断点。|  
|**禁用所有断点**|不可用|不可用|不可用|禁用所有断点。|  
|不可用|**添加监视**|不可用|不可用|将选定的表达式添加到 **“监视”** 窗口中。|  
  
## <a name="see-also"></a>另请参阅

- [Transact-SQL 调试器](./transact-sql-debugger.md)
- [逐句通过 Transact-SQL 代码](./step-through-transact-sql-code.md)
- [Transact-SQL 调试器信息](./transact-sql-debugger-information.md)
- [数据库引擎查询编辑器 (SQL Server Management Studio)](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [实时查询统计信息](../../relational-databases/performance/live-query-statistics.md)
---
title: Transact-SQL 代码
description: 了解如何使用 Transact-SQL 调试器控制在数据库引擎查询编辑器窗口中运行哪些 Transact-SQL 语句。
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, debugging code
- Transact-SQL debugger, step over
- Transact-SQL debugger, step out
- Transact-SQL debugger, step into
ms.assetid: e09079b8-c4c9-42b4-821b-4ce81a98a086
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3d86d2b94410f1ffbdd9d0b15226d278d28b3a5c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97476858"
---
# <a name="step-through-transact-sql-code"></a>Transact-SQL 代码

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器，您可以控制在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询编辑器窗口中运行哪些 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 语句。 可在各个语句上暂停调试器，然后查看该位置的代码元素的状态。  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="breakpoints"></a>断点

断点表示调试器对特定 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句暂停执行。 有关断点的详细信息，请参阅 [Transact-SQL 断点](./transact-sql-breakpoints.md)。  
  
## <a name="controlling-statement-execution"></a>控制语句执行

在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器中，可以指定从 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码中的当前语句开始执行的以下选项：

- 运行到下一个断点。

- 单步执行下一个语句。  

    如果下一个语句调用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程、函数或触发器，则调试器会显示一个包含该模块的代码的新查询编辑器窗口。 该窗口处于调试模式，并在模块中的第一个语句上暂停执行。 然后，您可以在模块代码中移动，例如，设置断点或逐句通过代码。

- 逐过程执行下一个语句。

    执行下一个语句。 但是，如果该语句调用了存储过程、函数或触发器，则模块代码会运行，直到完成执行，并将结果返回到调用代码。 如果确认存储过程中没有错误，可以逐过程执行。 在调用存储过程、函数或触发器的后面的语句上暂停执行。

- 跳出存储过程、函数或触发器。  

    在调用存储过程、函数或触发器的后面的语句上暂停执行。  

- 从当前位置运行到指针的当前位置，并忽略所有断点。  

 下表列出了用于控制在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器中执行语句的各种方法。  
  
|操作|执行操作：|  
|------------|---------------------|  
|运行当前语句到下一个断点之间的所有语句|在  “调试”菜单上单击“继续  ”。<br /><br /> 在  “调试”工具栏上单击“继续”  按钮。|  
|单步执行下一个语句或模块|在  “调试”菜单上单击“单步跳入”  。<br /><br /> 在“调试”  工具栏上单击“单步跳入”  按钮。<br /><br /> 按 F11。|  
|逐过程执行下一个语句或模块|在  “调试”菜单上单击“单步跳过”  。<br /><br /> 在“调试”  工具栏上单击“单步跳过”  按钮。<br /><br /> 按 F10。|  
|跳出模块|在  “调试”菜单上单击“单步跳出”  。<br /><br /> 在“调试”  工具栏上单击“单步跳出”  按钮。<br /><br /> 按 Shift+F11。|  
|运行到当前光标位置|右键单击查询编辑器窗口，然后单击“运行至光标处”。<br /><br /> 按 Ctrl+F10。|  
  
## <a name="see-also"></a>另请参阅

- [Transact-SQL 调试器信息](./transact-sql-debugger-information.md)
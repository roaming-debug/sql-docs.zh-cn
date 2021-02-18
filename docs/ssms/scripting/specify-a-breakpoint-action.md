---
title: 指定断点操作
description: 了解如何指定断点操作 - 即在命中断点且满足某些其他条件时 Transact-SQL 调试器要执行的自定义任务。
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, breakpoint action
- Transact-SQL debugger, breakpoint when hit action
ms.assetid: f97f0097-6f51-40c1-b2e0-294a93ce1e1b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 42902b55150c4efe5d9c81eb5698b3cff59b7aab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344142"
---
# <a name="specify-a-breakpoint-action"></a>指定断点操作

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

断点 **“命中条件”** 操作指定 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器针对某个断点执行的自定义任务。 如果达到指定的命中计数并满足所有指定的断点条件，则调试器将执行为断点指定的操作。

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]
  
##  <a name="action-considerations"></a><a name="BKMK_ActionConsiderations"></a> 操作注意事项

当命中计数和断点条件都得到满足时，断点的默认操作为中断执行。 **调试器中** “命中条件” [!INCLUDE[tsql](../../includes/tsql-md.md)] 操作的主要用途是通过指定打印消息，将信息打印到调试器的 **“输出”** 窗口。  
  
打印消息在 **“打印消息”** 选项中指定并被指定为一个文本字符串，其中包含的表达式包括正在调试的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中的信息。 表达式包括：  
  
-   包含在大括号 ({}) 中的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 表达式。 该表达式可以包含 [!INCLUDE[tsql](../../includes/tsql-md.md)] 变量、参数和内置函数。 示例包括 {@MyVariable}、{@NameParameter}、{@@SPID} 或 {SERVERPROPERTY(‘ProcessID’)}。  
  
-   以下关键字之一：  
  
    1.  $ADDRESS 返回在其中设置断点的存储过程或用户定义函数的名称。 如果在编辑器窗口中设置断点，$ADDRESS 将返回正在编辑的脚本文件的名称。 $ADDRESS 和 $FUNCTION 在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器中返回相同的信息。  
  
    2.  $CALLER 返回调用存储过程或函数的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码单元的名称。 如果在编辑器窗口中设置断点，则 $CALLER 返回 \<No caller available>。 如果断点位于从编辑器窗口中的代码调用的存储过程或用户定义函数中，$CALLER 将返回正在编辑的文件的名称。 如果断点位于从其他存储过程或函数中调用的存储过程或用户定义函数中，$CALLER 将返回此调用过程或函数的名称。  
  
    3.  $CALLSTACK 返回链中调用当前存储过程或用户定义函数的函数的调用堆栈。 如果在编辑器窗口中设置断点，$CALLSTACK 将返回正在编辑的脚本文件的名称。  
  
    4.  $FUNCTION 返回在其中设置断点的存储过程或用户定义函数的名称。 如果在编辑器窗口中设置断点，$FUNCTION 将返回正在编辑的脚本文件的名称。  
  
    5.  $PID 和 $PNAME 返回正在运行数据库引擎实例（其中正在运行 [!INCLUDE[tsql](../../includes/tsql-md.md)] ）的操作系统进程的 ID 和名称。 $PID 返回与 SERVERPROPERTY(‘ProcessID’) 相同的 ID，但 $PID 是一个十六进制值，而 SERVERPROPERTY(‘ProcessID’) 是一个十进制值。  
  
    6.  $TID 和 $TNAME 返回运行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理的操作系统线程的 ID 和名称。 该线程是一个与运行数据库引擎实例的进程关联的线程。 $TID 返回的值与 SELECT kpid FROM sys.sysprocesses WHERE spid = @@SPID 返回的值相同，不同的是 $TID 是一个十六进制值，而 kpid 是一个十进制值。  
  
-   还可以使用反斜杠字符 (\\) 作为转义字符，以允许在消息中使用大括号和反斜杠： \\{、 \\} 和 \\\\。  
  
#### <a name="to-specify-a-when-hit-action"></a>指定命中条件操作  
  
1.  在编辑器窗口中，右键单击断点符号，然后在快捷菜单上单击“命中条件”。  
  
     -或-  
  
     在“断点”窗口中，右键单击断点符号，然后在快捷菜单上单击“命中条件”。  
  
2.  在 **“当命中断点时”** 对话框中，选择您需要的行为：  
  
    1.  选择 **“打印消息”** ，以便当命中断点时在调试器的输出窗口中打印消息。  
  
    2.  **“运行宏”** 选项不可用于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器中而呈灰显状态。  
  
    3.  如果您不想暂停执行断点，请选择 **“继续执行”** 。 仅当选择了 **“打印消息”** 选项时，此选项才可用。  
  
3.  单击 **“确定”** 实施更改，或单击 **“取消”** 退出而不应用更改。  
  
## <a name="see-also"></a>另请参阅  
 [指定断点条件](./specify-a-breakpoint-condition.md)   
 [指定命中计数](./specify-a-hit-count.md)
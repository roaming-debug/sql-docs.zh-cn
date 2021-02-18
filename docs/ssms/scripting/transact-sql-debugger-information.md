---
title: Transact-SQL 调试器信息
description: 了解如何查看 Transact-SQL 调试器输出，其中包括调用堆栈、线程、断点、代码、变量和命令等信息。
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, Locals Window
- Transact-SQL debugger, Watch Window
- Transact-SQL debugger, Threads Window
- Transact-SQL debugger, Call Stack Window
- Transact-SQL debugger, QuickWatch
- Transact-SQL debugger, viewing information
ms.assetid: b99819cc-f388-41a1-b304-36e78ce24147
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 412ae1788542d4fa00df6a17df1b6450f7605851
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340999"
---
# <a name="transact-sql-debugger---information"></a>Transact-SQL 调试器 - 信息

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

每次当调试器对特定 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句暂停执行时，您可以使用不同调试器窗口来查看当前执行状态。 

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="debugger-windows"></a>调试器窗口  

在调试器模式下，调试器会在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 主窗口底部打开两个窗口。 调试器在这两个窗口中显示其所有信息。 每个调试器窗口都有选项卡，可选择这些选项卡以控制在窗口中显示哪些信息。 左侧调试器窗口包含 **“局部变量”** 、 **“监视1”** 、 **“监视2”** 、 **“监视3”** 和 **“监视4”** 选项卡。 右侧调试器窗口包含 **“调用堆栈”** 、 **“线程”** 、 **“断点”** 、 **“命令窗口”** 和 **“输出”** 选项卡。  
  
> [!NOTE]  
>  以上说明适用于调试器窗口的默认位置。 可以拖动选项卡，将其从一个窗口移到另一个窗口，或者取消停靠选项卡以创建一个新窗口，可以将该窗口放在所需的任何地方。  
  
 默认情况下，并非所有选项卡或窗口都处于活动状态。 可以使用下列方法之一来打开特定窗口：  
  
-   在 **“调试”** 菜单上，单击 **“窗口”** ，然后选择所需窗口。  
  
-   在 **“调试”** 工具栏上，单击 **“断点”** ，然后选择所需窗口。  
  
## <a name="transact-sql-expressions"></a>Transact-SQL 表达式  
 表达式是 [!INCLUDE[tsql](../../includes/tsql-md.md)] 子句，其计算结果为单个标量值，例如变量或参数。 左侧的调试器窗口可以显示当前赋给表达式的数据值，这些值最多可在 5 个选项卡或窗口中显示： **“局部变量”、“监视1”**、 **“监视2”**、 **“监视3”** 和 **“监视4”**。  
  
 **“局部变量”** 窗口显示 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器当前作用域中局部变量的信息。 调试器在代码的不同部分运行时， **“局部变量”** 窗口中列出的表达式集也会发生变化。  
  
 **“快速监视”** 和四个 **“监视”** 窗口中的表达式并不仅限于列出变量的标识符。 您可以通过指定 [!INCLUDE[tsql](../../includes/tsql-md.md)] 表达式计算出单个值（如向变量添加数字），或使用 SELECT 语句计算出单个值。 示例包括：  
  
-   变量名称，如 @IntegerCounter。  
  
-   变量的算术运算，如 @IntegerCounter + 1。  
  
-   两个字符变量的字符串运算，如 @FirstName + @LastName。  
  
-   返回单个值的 SELECT 语句，如 SELECT CharCol FROM MyTable WHERE PrimaryKey = 1。  
  
 可以使用 **“快速监视”** 窗口查看 [!INCLUDE[tsql](../../includes/tsql-md.md)] 表达式的值，然后将该表达式保存到 **“监视”** 窗口。 若要在 **“快速监视”** 窗口中选择表达式，请在 **“表达式”** 框中选择或输入该表达式的名称。  
  
 四个 **“监视”** 窗口显示所选变量和表达式的信息。 在列表中添加或删除表达式之前， **“监视”** 窗口中列出表达式集的不会变化。  
  
 若要将表达式添加到 **“监视”** 窗口，可在 **“快速监视”** 对话框中选择 **“添加监视”** ，或在 **“监视”** 窗口中空行的 **“名称”** 列中输入该表达式的名称。  
  
 在“局部变量”、“监视”或“快速监视”窗口中，右键单击行，然后选择“编辑值”，即可设置变量的数据值。 **“局部变量”** 窗口、 **“监视”** 窗口和 **“快速监视”** 对话框中的 **“值”** 列都支持文本、XML 和 HTML 数据可视化程序。 可视化程序由 **“值”** 列右侧的放大镜数据提示来表示。 在与数据类型匹配的显示结果中，可以使用可视化程序查看文本、XML 或 HTML 数据值，例如在浏览器窗口中查看 XML 文件。  
  
 在调试模式中，当您将鼠标指针移至某个标识符上方时，将弹出 **“快速信息”** ，显示表达式的名称及其当前值。 有关详细信息，请参阅[快速信息 (IntelliSense)](./quick-info-intellisense.md)。  
  
## <a name="breakpoints"></a>断点  
 可以使用 **“断点”** 窗口来查看和管理当前设置的断点。 有关详细信息，请参阅 [逐句通过 Transact-SQL 代码](./step-through-transact-sql-code.md)。  
  
## <a name="call-stacks"></a>调用堆栈  
 “调用堆栈”窗口显示当前执行位置，以及关于执行如何从原始编辑器窗口开始，通过所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 模块（函数、存储过程或触发器），到达当前执行位置的信息。 **“调用堆栈”** 窗口中的每一行称为一个堆栈帧，表示以下任何一项：  
  
-   当前的执行位置。  
  
-   从一个模块对另一个模块的调用。  
  
-   从编辑器窗口对 [!INCLUDE[tsql](../../includes/tsql-md.md)] 模块的调用。  
  
 堆栈的顺序与调用模块的顺序相反。 当前执行位置在堆栈的顶部，原始调用则在底部。 堆栈帧左侧空白处中的黄色箭头标识调试器暂停执行所在的帧。  
  
 **“名称”** 列记录以下信息：  
  
-   包含调用了下一级的代码行的源模块。  
  
-   调用了堆栈上下一个模块的代码行。  
  
-   如果调用带参数的存储过程或函数，则还会列出所有参数的名称、数据类型和值。  
  
 **“局部变量”** 、 **“监视”** 和 **“快速监视”** 窗口中的表达式都是针对当前堆栈帧求值的。 默认情况下，当前堆栈帧是堆栈中的顶帧，调试器在此处暂停执行。 当指定其他堆栈帧作为当前帧时，则 **“局部变量”**、 **“监视”** 和 **“快速监视”** 窗口中的表达式将针对新堆栈帧重新求值。 可以通过双击帧，或者单击帧并选择“切换到帧”来更改当前堆栈帧。 此时， **“局部变量”** 、 **“监视”** 和 **“快速监视”** 窗口中的表达式将针对新帧重新求值。 只要当前堆栈帧不是堆栈中的顶帧，堆栈帧的左侧空白处就会有一个绿色箭头标识当前堆栈帧。  
  
 当你右键单击某个堆栈帧并选择“转到源代码”时，该帧的代码会显示在“查询编辑器”窗口中。 但是，该帧不会成为当前帧， **“局部变量”** 、 **“监视”** 和 **“快速监视”** 窗口中的内容也不会更改。  
  
## <a name="system-information-and-transact-sql-results"></a>系统信息和 Transact-SQL 结果  
 调试器在 **“输出”** 窗口中列出其状态和事件消息。 其中包括调试器何时附加到其他进程或调试器线程何时结束等信息。  
  
 处于调试模式下时， **“结果”** 和 **“消息”** 选项卡在查询编辑器中仍处于活动状态。 **“结果”** 选项卡继续显示调试会话过程中执行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的结果集。 **“消息”** 选项卡继续显示系统消息，例如 *xx* 行受影响，以及 PRINT 和 RAISERROR 语句的输出。  
  
## <a name="see-also"></a>另请参阅  
 [“局部变量”窗口](./transact-sql-debugger-locals-window.md)   
 [“监视”窗口](./transact-sql-debugger-watch-window.md)   
 [“快速监视”对话框](./transact-sql-debugger-quickwatch-dialog-box.md)   
 [“断点”窗口](./transact-sql-debugger-breakpoints-window.md)   
 [“调用堆栈”窗口](./transact-sql-debugger-call-stack-window.md)   
 [“线程”窗口](./transact-sql-debugger-threads-window.md)   
 [输出窗口](./transact-sql-debugger-output-window.md)   
 [Transact-SQL 调试器](./transact-sql-debugger.md)  
  

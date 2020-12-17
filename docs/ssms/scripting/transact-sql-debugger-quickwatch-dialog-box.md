---
title: “快速监视”对话框
description: 了解如何在调试代码时使用“快速监视”对话框快速查看一个 Transact-SQL 表达式（如变量）的数据类型和值。
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vs.debug.quickwatch
helpviewer_keywords:
- QuickWatch Dialog [Transact-SQL]
ms.assetid: d6bbb373-1452-41f2-bdc5-86ae689c3dc0
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 79905aa908372f19653f548253d8312b5f760a48
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480618"
---
# <a name="transact-sql-debugger---quickwatch-dialog-box"></a>Transact-SQL 调试器 -“快速监视”对话框

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

使用“快速监视”对话框可以在调试 **代码时快速查看一个** 表达式（例如变量或参数）的数据类型和值。 [!INCLUDE[tsql](../../includes/tsql-md.md)][!INCLUDE[tsql](../../includes/tsql-md.md)] 若要监视多个表达式，也可以将这些表达式添加到 **“监视”** 窗口。  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="task-list"></a>任务列表

 **访问“快速监视”对话框**  
  
-   在 **“调试”** 菜单上，单击 **“快速监视”** 。  
  
 **查看有关表达式的信息**  
  
1.  在 **“表达式”** 列表框中，键入或选择所需的表达式。 支持以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 表达式：  
  
    -   变量。  
  
    -   参数。  
  
    -   名称以 @@ 开头的系统函数。  
  
    -   通过向一个或多个变量、参数或系统函数应用运算符而生成的表达式，如 @IntegerCounter + 1 或 FirstName + LastName。  
  
    -   返回单个值的 Transact-SQL 语句，例如：SELECT CharacterCol FROM MyTable WHERE PrimaryKey = 1。  
  
2.  单击 **“重新计算”** 。  
  
 **将“快速监视”表达式添加到“监视”窗口**  
  
-   单击 **“添加监视”** 。  
  
 **更改“快速监视”表达式的值**  
  
-   右键单击表达式，然后选择“编辑值”。  
  
## <a name="options"></a>选项  
 **表达式列表**  
 显示当前选定的表达式。 该下拉列表包含一组表达式，您可以从中选择要显示的表达式。 该列表中的表达式是在当前从 **“调用堆栈”** 窗口中选择的堆栈帧的范围内可用的表达式。 若要显示另一个表达式，可以输入该表达式或从列表中选择它。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器支持的表达式有：变量、参数和名称以 @@ 开头的系统函数。  
  
 **值网格**  
 显示当前所监视的表达式的属性。  
  
 **名称**  
 正在监视的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 表达式。  
  
 **值**  
 显示当前向该表达式分配的值。 如果该表达式当前不具有任何值，则显示空白。  
  
 如果表达式的长度超过了 **“值”** 列的宽度，则将指针移动到该表达式的 **“值”** 单元格上方可显示一条工具提示，指示该表达式的完整值。  
  
 **“值”** 单元格中的放大镜图标指示 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器可视化工具可用。 在该列表中，可以指定 **“文本可视化工具”** 、 **“XML 可视化工具”** 或 **“HTML 可视化工具”** 。 若要启动调试器可视化工具，请单击此放大镜图标。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器将打开一个对话框，该对话框以适合相应数据类型的格式显示这些数据。  
  
 类型  
 显示表达式的数据类型。  
  
## <a name="see-also"></a>另请参阅  
 [Transact-SQL 调试器](./transact-sql-debugger.md)   
 [Transact-SQL 调试器信息](./transact-sql-debugger-information.md)   
 [“监视”窗口](./transact-sql-debugger-watch-window.md)   
 [“局部变量”窗口](./transact-sql-debugger-locals-window.md)   
 [“调用堆栈”窗口](./transact-sql-debugger-call-stack-window.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)  
  

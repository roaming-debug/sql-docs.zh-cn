---
title: “监视”窗口
description: 了解“监视”窗口（一次多达四个），其中显示有关所选表达式的信息。 此信息仅在调试模式下显示。
titleSuffix: T-SQL Debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Watch Window [Transact-SQL]
ms.assetid: 23f3baa4-14c2-4262-92f7-3f43fcfa0436
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.reviewer: ''
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0c677071eb146b7d1a0335108a41f168326d0350
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480588"
---
# <a name="transact-sql-debugger---watch-window"></a>Transact-SQL 调试器 -“监视”窗口

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

**“监视”** 窗口显示有关所选表达式的信息。 最多可以有四个“监视”窗口：“监视 1”、“监视 2”、“监视 3”和“监视 4”  。 这些表达式是在 **“调用堆栈”** 窗口中选择的当前调用堆栈帧范围内求值的。 只有在调试模式下才可监视变量和表达式。  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="task-list"></a>任务列表

**访问“监视”窗口**  
  
-   在 **“调试”** 菜单上依次单击 **“窗口”** 、 **“监视”** ，然后再单击 **“监视 1”** 、 **“监视 2”、“监视 3”** 或 **“监视 4”** 。  
  
 **更改表达式的值**  
  
-   右键单击表达式，然后选择“编辑值”。  
  
## <a name="columns"></a>列  
 **名称**  
 是由 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器列出的表达式。 支持以下表达式：  
  
-   变量。  
  
-   参数。  
  
-   名称以 @@ 开头的系统函数。  
  
-   通过向一个或多个变量、参数或系统函数应用运算符而生成的表达式，如 @IntegerCounter + 1 或 FirstName + LastName。  
  
-   返回单个值的 Transact-SQL 语句，例如：SELECT CharacterCol FROM MyTable WHERE PrimaryKey = 1。  
  
 **值**  
 显示 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器对“名称”中指定的表达式求值后所返回的值。  
  
 如果表达式的长度超过了 **“值”** 列的宽度，则将指针移动到该表达式的 **“值”** 单元格上方可显示一条工具提示，指示该表达式的完整值。  
  
 **“值”** 单元格中的放大镜图标指示 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器可视化工具可用。 在该列表中，可以指定 **“文本可视化工具”** 、 **“XML 可视化工具”** 或 **“HTML 可视化工具”** 。 若要启动调试器可视化工具，请单击此放大镜图标。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器将打开一个对话框，该对话框以适合相应数据类型的格式显示这些数据。  
  
 类型  
 显示表达式的数据类型。  
  
## <a name="see-also"></a>另请参阅  
 [Transact-SQL 调试器](./transact-sql-debugger.md)   
 [Transact-SQL 调试器信息](./transact-sql-debugger-information.md)   
 [“局部变量”窗口](./transact-sql-debugger-locals-window.md)   
 [“调用堆栈”窗口](./transact-sql-debugger-call-stack-window.md)   
 [“快速监视”对话框](./transact-sql-debugger-quickwatch-dialog-box.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)
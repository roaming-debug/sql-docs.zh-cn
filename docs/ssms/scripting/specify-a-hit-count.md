---
title: 指定命中计数
description: 了解如何为断点设置命中次数，使调试器在达到命中次数前不会在该断点处中断。
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vs.debug.breakpt.hitcount
helpviewer_keywords:
- Transact-SQL debugger, breakpoint hit count
ms.assetid: 24836939-94ed-4e57-aa85-5d6938d859e4
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ae3ee2fe3e8219229d3975cfb59819f4fa8e1513
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466218"
---
# <a name="specify-a-hit-count"></a>指定命中计数

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

断点命中计数是每次到达断点时由 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器递增的计数器。 如果达到指定的命中计数并满足所有指定的断点条件，则调试器将执行为断点指定的操作。  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="hit-count-considerations"></a>命中计数注意事项

 默认情况下，每次到达断点时即中断执行。 您可以在下面的选项之间进行选择：  
  
-   总是中断（默认值）。  
  
-   当命中计数等于指定值时中断。  
  
-   当命中计数等于指定值的倍数时中断。  
  
-   当命中计数大于或等于指定值时中断。  
  
 中断命中计数在调试会话范围内递增。 所有命中计数在每个调试会话开始时均设置为零。  
  
 如果您想要跟踪命中断点的次数，但不执行断点中断，请将命中计数指定为一个非常高的值，以便断点永远不会中断。  
  
 当命中计数和断点条件都得到满足时，断点的默认操作为中断执行。 有关指定其它操作的信息，请参阅 [指定断点操作](./specify-a-breakpoint-action.md)。  
  
#### <a name="to-specify-a-hit-count"></a>指定命中计数  
  
1.  在编辑器窗口中，右键单击断点符号，然后单击快捷菜单上的“命中计数”。  
  
     -或-  
  
     在“断点”窗口中，右键单击断点符号，然后单击快捷菜单上的“命中计数”。  
  
2.  在 **“断点命中计数”** 对话框中，从 **“命中断点时”** 框中选择所需行为。  
  
     如果您选择除 **“总是中断”** 以外的任何设置，则列表的右侧会出现一个文本框。 在文本框中输入一个整数以指定所需的命中计数。  
  
3.  单击 **“确定”** 实施更改，或单击 **“取消”** 退出而不应用更改。  
  
#### <a name="to-view-or-reset-the-current-hit-count"></a>查看或重置当前命中计数  
  
1.  在编辑器窗口中，右键单击断点符号，然后单击快捷菜单上的“命中计数”。  
  
     -或-  
  
     在“断点”窗口中，右键单击断点符号，然后单击快捷菜单上的“命中计数”。  
  
2.  在 **“断点命中计数”** 对话框中， **“当前命中计数:”** 显示在 **“重置”** 按钮的正上方。  
  
3.  如果您想要将当前命中计数设置为零，请单击 **“重置”** 。  
  
4.  单击 **“确定”** 或 **“取消”** 以退出对话框。  
  
## <a name="see-also"></a>另请参阅  
 [指定断点条件](./specify-a-breakpoint-condition.md)  
  

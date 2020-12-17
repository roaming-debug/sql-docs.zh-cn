---
title: “查找结果”窗口
description: 有两个“查找结果”窗口可用于显示“在文件中查找”和“在文件中替换”操作找到的匹配项。 了解如何显示这些窗口，以及如何显示与匹配项对应的代码文件和行。
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- vs.findresults2
helpviewer_keywords:
- Find Results Windows dialog box
ms.assetid: 3b68dbb7-26d6-4bc9-bd2c-c27e5dc385c3
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 74b2483037d44436bb0ae3ef6f1c3de2fe9fa852
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466348"
---
# <a name="find-results-windows"></a>“查找结果”窗口
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  两个“查找结果”窗口显示通过使用 **“查找和替换”** 对话框中的 **“在文件中查找”** 或 **“在文件中替换”** 选项卡找到的匹配项。 **“在文件中查找”** 和 **“在文件中替换”** 选项卡中的 **“结果选项”** 命令允许您选择“查找结果”窗口，其中显示找到的所有匹配项。  
  
 只要找到匹配项，即会自动打开所选的“查找结果”窗口。 若要手动显示“查找结果”窗口，请单击 **“视图”** 菜单上的 **“其他窗口”** ，再单击 **“查找结果 1”** 或 **“查找结果 2”**。  
  
 若要显示代码文件并跳转到匹配项所在的行，请双击结果列表中的任意行。 此时，相应的源文件会显示在代码编辑器中，并且插入点位于匹配文本的起始位置。 在编辑器的指示器边距中会显示一个符号，以标记包含匹配项的行，并且状态栏会显示其完整文本。  
  
## <a name="toolbar-buttons"></a>工具栏按钮  
 使用工具栏按钮，可以帮助浏览结果列表，并跳转到匹配项所在的代码行。  
  
 **页标志 + 上箭头**  
 转到所选匹配项所在的行。  
  
 **页标志 + 左箭头**  
 转到上一个匹配项所在的行。  
  
 **页标志 + 右箭头**  
 转到下一个匹配项所在的行。  
  
 **全部清除**  
 从“结果”列表中删除所有匹配项。  
  
## <a name="shortcut-keys"></a>快捷键  
 使用以下快捷键，可以帮助快速浏览找到的匹配项。  
  
 Ctrl+Home  
 滚动到“查找结果”窗口的顶部。  
  
 Ctrl+End  
 滚动到“查找结果”窗口的底部。  
  
 Page Up  
 向上滚动到上一组匹配项。  
  
 Page Down  
 向下滚动到下一组匹配项。  
  
 向上键  
 选择上一个匹配项。  
  
 向下键  
 选择下一个匹配项。  
  
## <a name="search-result-entries"></a>搜索结果项  
 结果列表中的每一项都提供有以下信息：  
  
-   完整路径  
  
-   文件名  
  
-   行号  
  
-   包含匹配项的行的完整文本  
  
> [!TIP]  
>  您可以使用 **“快速查找”** 在很长的结果列表中进行浏览。 打开并停靠“查找结果”窗口，然后单击 **“查找”** 选项卡上的三角形 **“视图”** 按钮，再切换到 **“快速查找”** 。 将搜索的 **“查找范围”** 字段设置为 **“活动窗口”** ，输入一个 **“查找内容”** 字符串，再单击 **“查找下一个”** 。 这样，您就可以在结果列表中查找在特定文件夹或文件中找到的匹配项，或查找与某个其他关键术语在代码行中同时出现的匹配项。  
  
## <a name="summary-lines"></a>摘要行  
 每组搜索结果都以一个说明搜索参数的行开始，并以一个包含统计信息的行结束。 例如，如果通过使用 **“在文件中查找”** 在所有打开的文档中搜索匹配正则表达式“`var[1-3]&par`”的任何字符串，其结果列表可能以如下的搜索参数行开始：  
  
 `Find all "var[1-3]&par" Regular Expression, All Open Documents`  
  
 并且可能以此行统计信息结束：  
  
 `Total found: 57  Matching files: 23  Total files searched: 59`  
  
 如果通过使用 **“在文件中替换”** 在所有打开的文档中搜索，用字符串“`var[1-3]&par`”替换匹配正则表达式“`sample`”的任何字符串，其结果列表可能以如下的搜索参数行开始：  
  
 `Replace "var[1-3]&par", "sample", Regular Expression, All Open Documents`  
  
 并且可能以此行统计信息结束：  
  
 `Total replaced: 57  Matching files: 23  Total files searched: 59`  

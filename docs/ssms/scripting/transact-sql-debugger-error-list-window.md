---
title: “错误列表”窗口
description: 了解如何使用错误列表查看在编辑 Transact-SQL 脚本时 Intellisense 生成的错误和警告。
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- error list window
- SQL Server Management Studio [SQL Server], error list window
ms.assetid: fae6327d-e268-44ae-a474-4a8f8f843129
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5045ff2aa128b9e1c34f3031ae1fcd487b511481
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354511"
---
# <a name="transact-sql-debugger---error-list-window"></a>Transact-SQL 调试器 -“错误列表”窗口

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]“错误列表”用于显示由 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器中的 IntelliSense 代码生成的语法和语义错误。  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="features-of-the-error-list"></a>“错误列表”的功能  

**“错误列表”** 提供下列功能：  
  
-   编辑脚本时， **“错误列表”** 可显示由 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器中的 IntelliSense 生成的错误和警告。  
  
-   可以通过双击任意错误消息项来着重查看生成此错误的脚本文件的选项卡，并移至出错的位置。  
  
-   可以筛选要显示哪些项以及为每一项显示哪些列的信息。  
  
-   修复某错误后，相应错误项将从 **“错误列表”** 中删除。  
  
-   关闭某个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本文件的选项卡后，与该文件相关的错误将从 **“错误列表”** 中删除。  
  
## <a name="working-with-the-error-list"></a>使用“错误列表”  
 若要显示 **“错误列表”** ，请执行下列操作之一：  
  
-   在 **“视图”** 菜单上单击 **“错误列表”** 。  
  
-   使用键盘快捷键 CTRL+\\和 CTRL+E。  
  
 打开 **“错误列表”** 后，可以通过执行以下操作自定义视图：  
  
-   若要对列表进行排序，请单击任一列标题。 若要按其他列对列表进行进一步排序，请按住 Shift 键然后单击其他列标题。  
  
-   若要选择显示的列和隐藏的列，请从快捷菜单中选择 **“显示列”** 。  
  
-   若要更改列显示的顺序，请将任一列标题向左或向右拖动。  
  
 **“错误列表”** 中不包含指向有关特定错误的附加信息的链接。  
  
## <a name="transact-sql-errors-in-management-studio"></a>Management Studio 中的 Transact-SQL 错误  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在以下位置显示 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本的错误：  
  
-   **“错误列表”** 包含 IntelliSense 在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 编辑器中找到的所有语法和语义错误。 当您编辑 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本时，此错误列表将动态更新。 此列表包括编辑器在每个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本中找到的所有错误。 在脚本中遇到错误后，编辑器并不会停止分析文件。 在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]中， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 编辑器中的 IntelliSense 不支持所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法元素。 **“错误列表”** 仅包含 IntelliSense 支持的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法所产生的错误。  
  
-   **查询编辑器底部的** “消息” [!INCLUDE[ssDE](../../includes/ssde-md.md)] 选项卡显示执行 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 脚本时 [!INCLUDE[tsql](../../includes/tsql-md.md)] 返回的所有错误和警告。 只有再次执行该脚本此列表才会发生变化。 当 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 找到一个或两个编译错误后，它将停止分析批处理；因此， **“消息”** 选项卡中可能不会列出脚本中的所有错误。  
  
 有时候错误会同时在上述两个位置列出。 例如，某脚本文件可能存在已在 **“错误列表”** 中列出的语法错误。 如果在纠正此错误之前执行了该脚本，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 分析器会检测到相同的错误情形并在 **“消息”** 选项卡中返回此错误消息。  
  
> [!NOTE]  
>  “错误列表”仅显示来源于 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器的错误，而不显示来源于 MDX、DMX 或 XML/A 编辑器的错误。 所有 MDX、DMX 和 XML/A 错误均显示在这些编辑器的“消息”选项卡中。  
  
## <a name="ui-element-list"></a>UI 元素列表  
 **“错误列表”** 后，将在以下列中显示相关信息：  
  
 **默认顺序**  
 显示一个整数，该整数指示相应项的生成次序。  
  
 **说明**  
 显示相应错误项的文本。 较长的说明会自动换行。  
  
 **File**  
 显示生成相应错误的脚本文件的名称。  
  
 **线条**  
 显示一个整数，该整数指示包含相应错误的代码行。  
  
 **列**  
 显示一个整数，该整数指示错误在相应代码行中的位置。  
  
 **项目**  
 显示包含相应脚本文件的项目的名称。  

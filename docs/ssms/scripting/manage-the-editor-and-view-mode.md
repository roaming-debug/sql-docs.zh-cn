---
title: 管理编辑器和视图模式
description: 了解如何选择 SQL Server Management Studio 两种模式中的其中一种：选项卡式文档模式和多文档界面模式。 同时了解拆分视图、自动换行、虚拟空间模式、显示行号、全屏模式和全部自动隐藏等功能。
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Query Editor [SQL Server Management Studio], managing window behavior
- workbench view modes [SQL Server Management Studio]
- full screen mode [SQL Server Management Studio]
- fonts [SQL Server Management Studio]
- word wraps [SQL Server Management Studio]
- virtual space mode [SQL Server Management Studio]
- splitting document views
- displaying line numbers
- view modes [SQL Server Management Studio]
ms.assetid: 25c58a14-9f94-4296-9770-7d84c6bc3969
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 65ca95f3aa2cef72b49a9f80bdbf615e890a85d0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478768"
---
# <a name="manage-the-editor-and-view-mode"></a>管理编辑器和视图模式

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

利用编辑器，您可以通过多种方法来控制代码视图。  

## <a name="changing-the-view-mode"></a>更改视图模式  

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 提供了一种名为 **“选项卡式文档”** 的视图模式。通过这种视图模式，您可以同时打开多个编辑器和文档，并通过编辑器顶部的选项卡来对其进行访问。 另外，您也可以在多文档界面 (MDI) 模式下打开 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 环境，这种模式将不带选项卡的窗口联接起来，并允许对每个窗口执行平铺、最小化等操作。  
  
#### <a name="to-switch-between-view-modes"></a>切换视图模式  
  
1.  在 **“工具”** 菜单上单击 **“选项”** 。  
  
2.  单击 **“环境”** 。 单击 **“常规”** 。  
  
3.  单击 **“选项卡式文档”** 或 **“MDI 环境”** 。  
  
    > [!NOTE]  
    >  直到重新启动 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 时，更改才会生效。  
  
## <a name="splitting-the-view"></a>拆分视图  
 为了便于编辑，可以将“编辑器”窗口拆分为两个单独的部分。  
  
#### <a name="to-split-a-window"></a>拆分窗口  
  
1.  单击拆分栏（位于滚动条的上方）。  
  
2.  向下拖动拆分栏。  
  
3.  若要恢复到单一窗格，可双击分隔两窗格的拆分栏。  
  
 新窗格中包含与旧窗格相同的文档，只要两个窗格显示的是文档中的同一位置，则对一个窗格所做的任何更改都会反映到另一个窗格中。  
  
## <a name="word-wrap"></a>自动换行  
 激活自动换行功能时，水平滚动条将消失，且当代码行的长度超过编辑器的窗口宽度时，会自动转到下一个显示行，而不会滚动到窗口之外。  
  
#### <a name="to-activate-word-wrap"></a>激活自动换行功能  
  
1.  在 **“工具”** 菜单上单击 **“选项”** 。  
  
2.  单击 **“文本编辑器”** 。  
  
3.  打开相应的语言文件夹（或打开“所有语言”以使操作对所有语言生效）。  
  
4.  选择 **“自动换行”** 。  
  
## <a name="enabling-virtual-space-mode"></a>启用虚拟空间模式  
 在 **“虚拟空间”** 模式下，编辑器看起来就像在每行末尾都包含无数的空间，这样，代码行的长度就可以超出屏幕可视区域的范围。  
  
#### <a name="to-enable-virtual-space-mode"></a>启用虚拟空间模式  
  
1.  在 **“工具”** 菜单上单击 **“选项”** 。  
  
2.  单击 **“文本编辑器”** 。  
  
3.  打开相应的语言文件夹（或打开“所有语言”以使操作对所有语言生效）。  
  
4.  选择 **“启用虚拟空间”** 。  
  
 如果未启用虚拟空间模式，则光标会在到达一行的结尾时转到下一行的第一个字符（或相反）。  
  
## <a name="displaying-line-numbers"></a>显示行号  
 您可以在代码中打开编号功能。 行号在代码定位时非常有用。 有关详细信息，请参阅 [导航代码和文本](./navigate-code-and-text.md)。  
  
> [!NOTE]  
>  打开编号功能并不意味着在打印文档时会打印行号。 若要打印行号，则必须在 **“文件”** 菜单上的 **“页面设置”** 命令中选中 **“行号”** 复选框。  
  
#### <a name="to-display-line-numbers-in-code"></a>在代码中显示行号  
  
1.  在 **“工具”** 菜单上单击 **“选项”** 。  
  
2.  单击 **“文本编辑器”** 。  
  
3.  单击 **“所有语言”** 。  
  
4.  单击 **“常规”** 。  
  
5.  选择 **“行号”** 。  
  
 若仅为某些编程语言指定编号，则可在相应文件夹中选择 **“行号”** 。  
  
## <a name="enabling-full-screen-mode"></a>启用全屏显示模式  
 通过启用全屏显示模式，可以选择隐藏所有工具窗口，而只查看文档窗口。  
  
#### <a name="to-enable-full-screen-mode"></a>启用全屏显示模式  
  
1.  按 Alt+Shift+Enter 可切换到全屏显示模式。  
  
## <a name="using-auto-hide-all"></a>使用“全部自动隐藏”  
  
#### <a name="to-hide-all-the-tool-windows-at-once"></a>同时隐藏所有工具窗口  
  
1.  选择 **“窗口”** 菜单上的 **“全部自动隐藏”** 。  
  

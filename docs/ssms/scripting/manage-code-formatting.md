---
title: 管理代码格式
description: 了解文本缩进的三种样式及其使用方式，并了解如何创建可单击的 URL。
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- indenting code [SQL Server]
- displaying URLs
- code formatting [SQL Server Management Studio]
- collapsing text
- formats [SQL Server], code formatting in SQL Server Management Studio
- hiding text
- formats [SQL Server]
- text [SQL Server], code formats
- automatic indentation
- converting text to lower case
- Query Editor [SQL Server Management Studio], managing code formats
- URL displayed in code [SQL Server Management Studio]
- converting text to upper case
- text [SQL Server]
- unindenting code
ms.assetid: ddbac4d2-6bdc-4467-a352-e869ec880eed
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 550f34f022262b24cd7992c7b3ebe056b946f4cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354621"
---
# <a name="manage-code-formatting"></a>管理代码格式

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

使用编辑器可以用缩进、隐藏文本、URL 等来设置代码的格式。 还可以使用智能缩进在键入时自动格式化代码。  
  
## <a name="indenting"></a>缩进  
 可以选择三种不同样式的文本缩进。 此外，还可以指定由多少个空格组成一个缩进或制表符，以及在缩进时编辑器是使用制表符，还是使用空格。  
  
#### <a name="to-choose-an-indenting-style"></a>选择缩进样式  
  
1.  在“工具”  菜单上，单击“选项” 。  
  
2.  单击 **“文本编辑器”** 。  
  
3.  单击文件夹，并选择 **“所有语言”** ，可以为所有语言设置缩进。  
  
4.  单击 **“选项卡”** 。  
  
5.  单击下列选项之一：  
  
    -   **无**。 光标转到下一行的开始位置。  
  
    -   **块**。 光标在下一行时与在上一行时的位置对齐。  
  
    -   “智能”（默认）。 由语言服务确定要使用的适当缩进样式。  
  
    > [!NOTE]  
    >  某些语言不提供所有这三种缩进选项。  
  
#### <a name="to-change-indent-tab-settings"></a>更改缩进制表符设置  
  
1.  在“工具”  菜单上，单击“选项” 。  
  
2.  单击 **“文本编辑器”** 。  
  
3.  选择 **“所有语言”** 的文件夹，可以为所有语言设置缩进。  
  
4.  单击 **“选项卡”** 。  
  
5.  若要指定制表符和缩进操作的制表符字符数，请单击 **“保留制表符”** 。 若要指定空格数，请选择 **“插入空格”** 。  
  
     如果选择 **“插入空格”** ，请在 **“制表符大小”** 或 **“缩进大小”** 下分别输入每个制表符或缩进所代表的空格数。  
  
#### <a name="to-indent-code"></a>缩进代码  
  
1.  选择要缩进的文本。  
  
2.  按 Tab 键，或在标准工具栏上单击 **“缩进”** 按钮。  
  
#### <a name="to-unindent-code"></a>取消代码的缩进  
  
1.  选择要取消缩进的文本。  
  
2.  按 Shift+Tab，或在标准工具栏上单击“取消缩进”按钮。  
  
#### <a name="to-automatically-indent-all-of-your-code"></a>自动缩进所有代码  
  
1.  在“工具”  菜单上，单击“选项” 。  
  
2.  单击 **“文本编辑器”** 。  
  
3.  单击 **“所有语言”** 。  
  
4.  单击 **“选项卡”** 。  
  
5.  单击 **“智能”** 。  
  
> [!NOTE]  
>  “智能”选项对某些语言不可用。  
  
#### <a name="to-convert-white-space-to-tabs"></a>将空白空格转换成制表符  
  
1.  选择要将其空白空格转换成制表符的文本。  
  
2.  在 **“编辑”** 菜单上，指向 **“高级”** ，并单击 **“制表符替换空格”** 。  
  
#### <a name="to-convert-tabs-to-spaces"></a>将制表符转换成空格  
  
1.  选择想要将其制表符转换成空格的文本。  
  
2.  在 **“编辑”** 菜单上，指向 **“高级”** ，并单击 **“空格替换制表符”** 。  
  
 这些命令的行为取决于在 **“选项”** 对话框中的制表符设置。 例如，如果制表符设置是 4，则 **“制表符替换空格”** 为每 4 个连续空格创建一个制表符，而 **“空格替换制表符”** 则为每个制表符创建 4 个空格。  
  
## <a name="converting-text-to-upper-and-lower-case"></a>转换文本的大小写  
 可以使用命令将文本转换为全部大写或小写。  
  
#### <a name="to-switch-text-to-upper-or-lower-case"></a>将文本切换为大写或小写  
  
1.  选择要转换的文本。  
  
2.  若要将文本转换为大写，请按 Ctrl+Shift+U，或单击“编辑”菜单的“高级”子菜单上的“转换为大写”。  
  
3.  若要将文本转换为小写，请按 Ctrl+Shift+L，或单击“编辑”菜单的“高级”子菜单上的“转换为小写”。  
  
> [!NOTE]  
>  有关键盘快捷键的完整列表，请参阅 [SQL Server Management Studio 键盘快捷键](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)。  
  
## <a name="displaying-and-linking-to-urls"></a>显示和链接到 URL  
 可以在代码中创建并显示可单击的 URL。 默认情况下，URL：  
  
-   带下划线。  
  
-   鼠标指针移到它们上面时，指针更改为手形。  
  
-   如果 URL 有效，单击时将打开 URL。  
  
#### <a name="to-display-a-clickable-url"></a>显示可单击的 URL  
  
1.  在“工具”  菜单上，单击“选项” 。  
  
2.  单击 **“文本编辑器”** 。  
  
3.  若要只更改一种语言的选项，请单击该语言文件夹，再单击 **“常规”** 。 若要更改所有语言的选项，请单击 **“所有语言”** ，再单击 **“常规”** 。  
  
4.  选择“启用单击 URL 定位”。  
  
  

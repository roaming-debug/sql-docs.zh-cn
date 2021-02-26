---
title: 代码和文本定位
description: 了解如何使用各种方法在文档中导航：为某个位置设置书签，以便轻松返回该位置；增量式搜索；使用鼠标和键盘；使用 Go To 命令通过指定某行的行号来转到该行。
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- searches [SQL Server Management Studio], incremental
- mouse [SQL Server Management Studio]
- bookmarks [SQL Server Management Studio]
- Query Editor [SQL Server Management Studio], navigating code
- Query Editor [SQL Server Management Studio], Go To Command
- incremental searches [SQL Server Management Studio]
- Query Editor [SQL Server Management Studio], bookmarks
- Query Editor [SQL Server Management Studio], mouse
- navigating code
- Go To command
ms.assetid: f63247ff-9751-4e99-8ee3-0772ad4009d0
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f6242e7716e8de999e54d994a7e496a0a8225b12
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354612"
---
# <a name="navigate-code-and-text"></a>代码和文本定位

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

可以使用以下方法在整个文本中移动：  
  
-   书签。  
  
-   渐进式搜索。  
  
-   鼠标和导航键。  
  
-   **Go To** 命令。  
  
> [!NOTE]  
>  有关键盘快捷键的完整列表，请参阅 [SQL Server Management Studio 键盘快捷键](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)。  
  
## <a name="navigating-with-bookmarks"></a>使用书签定位  
 若要在其他位置编辑文档，然后再返回初始位置，则请添加书签。 可以设置书签并使用键盘快捷键移至书签。 在书签窗口中查看书签。  
  
## <a name="incremental-search"></a>渐进式搜索  
 通过渐进式搜索，当您输入搜索字符时，可直接定位到当前文档中的位置。 可以使用键盘快捷键访问渐进式搜索。  
  
## <a name="navigating-with-the-mouse-and-keyboard"></a>使用鼠标和键盘定位  
 最常用的文本定位方法是使用鼠标和导航键：  
  
-   使用向左键和向右键一次可以移动一个字符，如果与 Ctrl 键组合使用，则一次可以移动一个词。 使用向上键和向下键一次可以移动一行。  
  
-   单击某个位置可以将光标置于该位置。  
  
-   使用滚动条或鼠标上的滚轮可以在整个文本中移动。  
  
-   使用 Home、End、Page Up 和 Page Down 键也可以在整个文本中移动。  
  
-   使用 Ctrl+Page Up 和 Ctrl+Page Down 可以分别将插入点移到窗口的顶部或底部。  
  
-   使用 Ctrl+向上键和 Ctrl+向下键可以滚动视图而不移动插入点。  
  
## <a name="go-to-command"></a>Go To 命令  
 使用 **GoTo** 命令可以转到特定的行号。 若要显示编号，请在 **“选项”** 对话框中依次单击 **“文本编辑器”** 、 **“所有语言”** 和 **“常规”** ，然后选择 **“行号”** 。  
  
 **转到特定行号**  
  
1.  单击 **“编辑”** 菜单中的 **“转到”**  
  
2.  输入要查看的行号
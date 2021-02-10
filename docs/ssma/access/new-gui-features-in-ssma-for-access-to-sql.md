---
description: SSMA 中的新 GUI 功能，用于访问 SQL
title: SSMA 中用于访问 SQL 的新 GUI 功能 |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 95b95de2-db05-4422-825d-43968ecfd01c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 75bddda078465537e7d68047d6c9c690beafaea4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066572"
---
# <a name="new-gui-features-in-ssma-for-access-to-sql"></a>SSMA 中的新 GUI 功能，用于访问 SQL
本章介绍 SSMA 用户界面的新功能  
  
## <a name="layouts"></a>布局  
此功能可让你选择两个预定义的 windows 布局之一或创建自己的布局。 若要访问布局子菜单，请在 "视图" 菜单上指向 "布局"。 可以在其中选择一个现有布局、添加当前布局或管理布局。  
  
### <a name="add-current-layout"></a>添加当前布局  
若要保存当前的 windows 布局，请在 "视图" 菜单上指向 "布局"，然后单击 "添加当前布局"。  
  
### <a name="choose-predefined-layout"></a>选择预定义布局  
若要选择一个预定义布局，请在 "视图" 菜单上指向 "布局"，然后单击 "默认布局" 或 "不包含资源管理器"。 你还可以分别对预定义的布局使用快捷方式 Ctrl + Alt + 1 或 Ctrl + Alt + 2。  
  
### <a name="choose-user-defined-layout"></a>选择用户定义的布局  
若要选择用户定义的布局，请在 "视图" 菜单上指向 "布局"，然后单击用户定义的布局之一。 你还可以使用为布局定义的快捷方式。  
  
### <a name="manage-layouts"></a>管理布局  
要打开 "管理布局" 对话框，请在 "视图" 菜单上指向 "布局"，然后单击 "管理布局"。 在 "管理布局" 对话框中，可在对话框左侧找到现有布局的列表。 可以在其中选择要更改其设置的布局。 此外，还可以更改列表中的布局顺序，或使用列表顶部的按钮删除布局。 在对话框的右侧，可以更改以下布局设置：  
  
-   布局名称  
  
-   元数据资源管理器同步  
  
-   源和目标元数据资源管理器的可见性和宽度  
  
-   源或目标窗口的可见性及其大小  
  
-   辅助窗口的可见性和高度  
  
## <a name="bookmarks"></a>书签  
此功能允许您在源或目标代码中设置一个或多个书签，通过使用快捷方式快速找到书签，使用友好对话框管理书签。  
  
### <a name="toggle-bookmark"></a>切换书签  
可以通过以下方式设置/删除书签：  
  
-   在源或目标 SQL 窗口的顶部使用按钮切换书签  
  
-   单击 SQL 窗口左侧的灰色区域  
  
-   使用 Ctrl + Shift + &lt; 0-9 &gt; 设置编号书签  
  
### <a name="bookmark-navigation"></a>书签导航  
可以通过以下方式遍历书签：  
  
-   在 SQL 窗口的顶部使用 "下一书签" 按钮、上一个书签  
  
-   使用 Ctrl + &lt; 0 ... 9 &gt; 查找编号书签  
  
-   使用按钮在 "管理书签" 对话框中转到或查看源  
  
### <a name="removing-bookmark"></a>删除书签  
可以通过以下方式删除书签：  
  
-   使用 "SQL" 窗口顶部的 "按钮清除" 删除当前文档中的所有书签  
  
-   使用 "管理书签" 对话框中的 "删除" 或 "全部删除" 按钮  
  
### <a name="manage-bookmarks"></a>管理书签  
若要打开 "管理书签" 对话框，请在 "编辑" 菜单上单击 "管理书签"。 在对话框中，您将看到现有书签的列表。 您可以使用对话框右侧的按钮来管理书签。  
  
## <a name="object-history"></a>对象历史记录  
GUI 对象历史记录可让你在导航对象时具有以下优势：  
  
-   可以使用 "后退" 和 "前进" 按钮导航已访问的对象  
  
-   返回到该对象后，返回到已有的相同选项卡  
  
-   返回到对象，并且选项卡为 SQL 时，将返回到已离开的相同游标位置  
  
## <a name="advanced-search-capabilities"></a>高级搜索功能  
高级搜索功能提供功能强大且灵活的搜索功能，使你能够查找对象声明、获取对象信息、执行快速搜索、使用模式等在类别中执行高级对象搜索。  
  
### <a name="get-quick-information"></a>获取快速信息  
可以通过以下方式获取有关光标位置处的对象的快速信息：  
  
-   单击 "SQL" 窗口顶部的 "按钮快速信息"  
  
-   选择右键单击弹出菜单中的 "快速信息"  
  
-   按 Ctrl + Shift + 空格键  
  
### <a name="find-declaration"></a>查找声明  
可以通过以下方式在光标位置处中转到对象的声明：  
  
-   单击 "切换到 SQL" 窗口顶部的 "跳到声明"  
  
-   选择右键单击弹出菜单中的 "切换到声明"  
  
-   按 F12  
  
### <a name="quick-search"></a>快速搜索  
您可以使用以下功能执行快速文本搜索：  
  
-   您可以使用 Ctrl + F 快捷方式开始搜索  
  
-   可以通过使用 F3  
  
-   你可以使用 Shift + F3 向后移动最后一次搜索  
  
-   您可以使用 Ctrl + F3 查找光标所在位置的下一个匹配项  
  
-   您可以使用 Ctrl + Shift + F3 在光标位置查找该词的上一个匹配项  
  
-   你还可以通过菜单项执行所有这些操作。  
  
### <a name="advanced-search"></a>高级搜索  
若要打开 "高级搜索" 对话框，请在 "编辑" 菜单点上查找，然后单击 "高级搜索"。 在对话框中，可以使用模式查找任何对象。 在对话框的顶部，可以选择 "搜索区域" 和 "对象类别"。  
  

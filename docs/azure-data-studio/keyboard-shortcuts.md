---
title: 创建和自定义键盘快捷方式
description: 了解如何使用基于 Visual Studio Code 的功能查看、编辑和创建 Azure Data Studio 中的键盘快捷方式。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 26d16927004a9a34e0b0d50ca5ebe28abac599a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040157"
---
# <a name="keyboard-shortcuts-in-azure-data-studio"></a>Azure Data Studio 中的键盘快捷方式

本文提供了在 Azure Data Studio 中快速查看、编辑和创建键盘快捷方式的步骤。

因为 Azure Data Studio 从 Visual Studio Code 继承其键绑定功能，有关使用不同键盘布局等高级自定义的详细信息，请参阅 [Visual Studio Code 的键绑定](https://code.visualstudio.com/docs/getstarted/keybindings)一文。 某些键绑定功能可能不可用（例如，Azure Data Studio 中不支持键映射扩展）。

## <a name="open-the-keyboard-shortcuts-editor"></a>打开“键盘快捷方式”编辑器

查看所有当前定义的键盘快捷方式：

从“文件”菜单中打开“键盘快捷方式”编辑器 ：“文件” > “首选项” > “键盘快捷方式”（Mac 上为“Azure Data Studio” > “首选项” > “键盘快捷方式”）     。

除显示当前键绑定外，“键盘快捷方式”编辑器还会列出未定义键盘快捷方式的可用命令。 借助“键盘快捷方式”编辑器，可以轻松更改、删除、重置和定义新的键绑定。  

## <a name="edit-existing-keyboard-shortcuts"></a>编辑现有的键盘快捷方式

更改现有键盘快捷方式的键绑定：

1. 使用搜索框或浏览列表，找到要更改的键盘快捷方式。
   > [!TIP]
   > 按键、按命令、按源等方式搜索，以返回所有相关的键盘快捷方式。

2. 右键单击所需的条目，然后选择“更改键绑定”

   ![编辑键盘快捷方式](media/keyboard-shortcuts/change-keybinding.png)

3. 按下所需的键组合，然后按 Enter 进行保存。 

   ![保存键盘快捷方式](media/keyboard-shortcuts/save-keybinding.png)

## <a name="create-new-keyboard-shortcuts"></a>创建新的键盘快捷方式

创建新的键盘快捷方式：

1. 右键单击没有任何键绑定的命令，然后选择“添加键绑定”。

   ![创建键盘快捷方式](media/keyboard-shortcuts/add-keybinding.png)

2. 按下所需的键组合，然后按 Enter 进行保存。
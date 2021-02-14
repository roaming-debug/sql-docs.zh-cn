---
description: '可实现从“查找和替换”窗口进行复制的解决方法 '
title: 启用从“查找和替换”窗口复制
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c28ffa44-7b8b-4efa-b755-c7a3b1c11ce4
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan, sstein
ms.custom: seo-lt-2019
ms.date: 11/03/2020
ms.openlocfilehash: f7a11c952fa20b720ad37abc204c7dbb31f0e96f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058602"
---
# <a name="workaround-to-enable-copying-from-find-and-replace-window"></a>可实现从“查找和替换”窗口进行复制的解决方法

[!INCLUDE[Applies to](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

需要采用对应的方法才能启用从“查找和替换”窗口复制。  如果无法从 SQL Server Management Studio 中的“查找和替换”窗口进行复制，请按下面的[解决方法](#workaround)操作。

## <a name="error-message"></a>错误消息

尝试从 SQL Server Management Studio 中的“查找和替换”窗口复制文本时，会收到一条错误消息。

> 无法从杂项文件项目将未保存的文档剪切或复制到剪贴板。 在剪切或复制之前，必须保存未保存的文档。

![“错误”对话框表示的内容：无法从杂项文件项目将未保存的文档剪切或复制到剪贴板。 在剪切或复制之前，必须保存未保存的文档](../media/troubleshoot/unable-copy-find-replace-window.png)

## <a name="workaround"></a>解决方法

若要实现从“查找和替换”窗口中复制文本的功能，请执行以下步骤：

1. 在“工具”菜单中，打开“选项” 。

2. 在“环境”>“文档”下，取消选中“在解决方案资源管理器中显示杂项文件”项 

3. 关闭并重新打开 SQL Server Management Studio

![未选中“在解决方案资源管理器中显示杂项文件”的选项窗口](../media/troubleshoot/fix-copy-find-replace-window.png)


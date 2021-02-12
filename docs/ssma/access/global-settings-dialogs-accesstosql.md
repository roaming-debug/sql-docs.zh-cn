---
description: '全局设置 (对话框)  (AccessToSQL) '
title: 全局设置 (对话框)  (AccessToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 6c2204f2-d49e-49ba-9c0f-f14cf07fa561
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 15dd9656a263640b598389de650b779704b8dc41
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078888"
---
# <a name="global-settings-dialogs-accesstosql"></a>全局设置 (对话框)  (AccessToSQL) 
使用 " **全局设置** " 对话框的 "对话框" 页可以指定 SSMA 的默认用户操作和警告设置。  
  
若要访问 " **工具** " 菜单上的对话框设置，请选择 " **全局设置**"，单击左侧窗格底部的 " **GUI** "，然后选择 " **对话框**"。  
  
## <a name="options"></a>选项  
**启动时显示迁移向导**  
在 SSMA for Access 中，你可以选择在启动 SSMA 应用程序时启用或禁用 **迁移向导** 。 默认情况下，此选项为 **True**。  
  
-   如果将此选项设置为 **True**，则在打开 SSMA for Access 应用程序时，将最初显示 "迁移向导" 对话框。  
  
-   如果将此选项设置为 " **False**"，则不会显示迁移向导，如果需要，你必须从 " **文件** " 菜单中手动访问该向导。  
  
**覆盖对象之前发出警告**  
当 SSMA 将对象转换为 SQL Server 时，某些对象可能已经存在于项目的 SQL Server 元数据中。 这些对象可能已被转换，或者对象在目标架构中的名称可能与要转换的对象的名称相同。  
  
使用此选项可指定 SSMA 是否应提示您覆盖重复的对象定义：  
  
-   如果选择 " **True**"，则在遇到重复对象时，SSMA 将显示一个警告对话框。 在此对话框中，您可以指定覆盖单个对象或所有重复对象，或跳过单个对象或所有重复的对象。  
  
-   如果选择 " **False**"，则会出现 " **对象覆盖默认操作** " 选项，您可以在其中指定默认操作。  
  
**对象覆盖默认操作**  
如果为 "**覆盖对象之前警告**" 选项选择 " **False** "，则会显示此选项。  
  
使用此选项指定默认对象覆盖行为：  
  
-   如果选择 " **True**"，则 SSMA 会自动覆盖 SQL Server 项目元数据中具有相同名称并且与要转换的对象位于同一目标架构中的对象。  
  
-   如果选择 **False**，则 SSMA 在转换过程中不会覆盖对象元数据。  
  

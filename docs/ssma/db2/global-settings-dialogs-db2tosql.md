---
description: '全局设置 (对话框)  (DB2ToSQL) '
title: 全局设置 (对话框)  (DB2ToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 360e01bb-6347-4e2b-acda-0daa161ed33b
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 51b60367cd773ecddf75d2c24676eefe835180e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060442"
---
# <a name="global-settings-dialogs-db2tosql"></a>全局设置 (对话框)  (DB2ToSQL) 
使用 " **全局设置** " 对话框的 "对话框" 页可以指定 SSMA 的默认用户操作和警告设置。  
  
若要访问 " **工具** " 菜单上的对话框设置，请选择 " **全局设置**"，单击左侧窗格底部的 " **GUI** "，然后选择 " **对话框**"。  
  
## <a name="options"></a>选项  
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
  

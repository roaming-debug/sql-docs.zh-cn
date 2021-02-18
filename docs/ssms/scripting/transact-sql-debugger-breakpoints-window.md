---
title: “断点”窗口
description: 了解如何使用数据库引擎查询编辑器的“断点”窗口管理 Transact-SQL 调试器断点。
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Breakpoints Window [Transact-SQL]
ms.assetid: bad88d10-fdd5-4d3d-b5ea-a4f063847485
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 07/22/2020
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: a9de728e7b50f27558e53b9b4db2fbcda1ed6e1e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340677"
---
# <a name="transact-sql-debugger---breakpoints-window"></a>Transact-SQL 调试器 -“断点”窗口

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

**“断点”** 窗口列出在当前 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器中设置的所有断点。 若要管理断点，请使用“断点”窗口中的工具栏。  断点是代码中的某些位置，在调试模式下执行在这些位置暂停，以便您可以查看调试数据。

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="task-list"></a>任务列表

**访问“断点”窗口**

- 在“调试”菜单上，选择“Windows”，然后选择“断点”  。

## <a name="breakpoints-window-columns"></a>“断点”窗口中包含的列

默认情况下， **“断点”** 窗口列出以下列。  

**名称**  
显示断点名称。 断点名称是由调试器提供的。 该名称含有包含此断点的数据库引擎查询编辑器窗口的名称和查询编辑器中设有此断点的行的行号。  

**条件**  
显示“(无条件)”。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器不支持设置断点条件。

**命中计数**  
显示“总是中断”。

在 **“列”** 列表中选择以下列后，可以添加或删除这些列。  

**筛选器**  
显示“(无)”。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器不支持设置断点筛选器。

**命中条件**  
显示“中断”。

**语言**  
如果是 **，则显示** Transact-SQL [!INCLUDE[tsql](../../includes/tsql-md.md)]。  

**Function**  
显示设有断点的行的行号。  

**File**  
显示包含断点的源文件的名称和设有此断点的行的行号。

**Address**  
[!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器不支持此功能。  

**处理**  
显示“[SQL]”，则表明这是一个 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 进程。 后面跟随代码在其中执行的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例的名称。

## <a name="breakpoints-window-toolbar"></a>“断点”窗口工具栏

如果当前 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器窗口具有活动断点，则 **“断点”** 窗口将显示一个可用于管理这些断点的工具栏。

**删除**  
删除所选断点。

**删除全部断点**  
删除“断点”窗口中显示的全部断点。  

**禁用所有断点**  
禁用所有断点，以使其不再中断代码执行，但保留断点。 禁用所有断点后，该按钮即变为 **“启用所有断点”** 。

**“启用所有断点”**  
启用所有断点，以使它们中断代码执行。 启用所有断点后，该按钮即变为 **“禁用所有断点”** 。  

**转到源代码**  
将光标定位在查询编辑器中包含所选断点的行。

**“列”**  
列出所有可以在“断点”窗口中显示的列。 复选框指示所显示的列。 若要在 **“断点”** 窗口中添加或删除某一列，请在此列表中选择该列。

## <a name="see-also"></a>另请参阅

[Transact-SQL 调试器](./transact-sql-debugger.md)
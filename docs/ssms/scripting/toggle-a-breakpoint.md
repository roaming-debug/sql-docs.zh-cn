---
title: 切换断点
description: 了解如何切换断点以突出显示关联的 Transact-SQL 语句，以及如何对该语句执行各种操作（例如编辑）。
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c477ab89-a1cd-4f2c-aa7c-40525041100f
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0de50770b4be70a79ebddffd3cd1a983dc2524ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339355"
---
# <a name="toggle-a-breakpoint"></a>切换断点

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句上设置断点的操作称为切换断点。  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="breakpoints"></a>断点

设置断点后，语句左侧的灰色栏中会出现一个图标来表示该断点。 该图标称为断点符号。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 断点适用于整个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 打开断点时，调试器会突出显示相关联的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
 如果一行中存在多个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，可以为每个语句切换一个断点。 单击窗口左侧的灰色栏可以在行中的第一个语句上切换断点。 您可以通过以下方法在后续语句中切换断点：突出显示该语句的任何部分，或将光标移动到该语句中，然后按 F9 或单击 **“调试”** 菜单上的 **“切换断点”** 。 如果一行中有多个断点，则只有一个断点符号位于左侧的灰色栏中。  
  
 在切换某一断点后，您可以对该断点执行不同的操作，例如编辑其属性或临时禁用该断点。 有关详细信息，请参阅 [Transact-SQL 断点](./transact-sql-breakpoints.md)。  
  
## <a name="toggle-a-breakpoint"></a>切换断点  
 **切换 Transact-SQL 语句上的断点**  
  
1.  单击 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句左侧的灰色栏。  
  
2.  或者，突出显示语句的任何部分，或将光标移到语句上，然后执行下列两个操作之一：  
  
    -   按 F9。  
  
    -   在 **“调试”** 菜单上，单击 **“切换断点”** 。  
  

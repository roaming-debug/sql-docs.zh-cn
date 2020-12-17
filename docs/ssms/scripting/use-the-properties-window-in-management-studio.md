---
title: 使用 Management Studio 中的“属性”窗口
description: 了解如何使用“属性”窗口查看 SQL Server Management Studio 项的相关信息，例如连接和数据库对象。
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- viewing properties
- Properties window [SQL Server Management Studio]
- complex properties [SQL Server Management Studio]
ms.assetid: 903d4aca-f57c-43d9-a893-702eceaa7004
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9f18cec0f6ac8754fc5826e75325c810e965b692
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480578"
---
# <a name="use-the-properties-window-in-management-studio"></a>使用 Management Studio 中的“属性”窗口
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  “属性”窗口用于说明 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中的项（如连接或 Showplan 运算符）的状态，以及有关数据库对象（如表、视图和设计器等）的信息。  
  
 可以使用“属性”窗口查看当前连接的属性。 许多属性在“属性”窗口中是只读的，但可以在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]的其他地方更改。 例如，查询的数据库属性在“属性”窗口中是只读的，但可在工具栏上更改。  
  
### <a name="to-view-properties-using-the-properties-window"></a>使用“属性”窗口查看属性  
  
1.  如果“属性”窗口不可见，请在 **“查看”** 菜单中单击 **“属性窗口”** ，或者按 F4。  
  
2.  将要查看的对象设置为焦点。  
  
3.  在“属性”窗口中查找特定的属性。  
  
### <a name="to-view-connection-properties-of-a-query-window"></a>查看查询窗口的连接属性  
  
1.  如果“属性”窗口不可见，请在 **“查看”** 菜单中单击 **“属性窗口”** ，或者按 F4。  
  
2.  在“属性”窗口中，可以看到所有连接属性。  
  
### <a name="to-view-the-properties-of-a-showplan-operator"></a>查看 Showplan 运算符的属性  
  
1.  在 **“查询”** 菜单上，单击 **“包括实际的执行计划”** 。  
  
2.  在 SQL 查询编辑器中，键入并执行一个查询。  
  
3.  如果“属性”窗口不可见，请在 **“查看”** 菜单中单击 **“属性窗口”** ，或者按 F4。  
  
4.  在 SQL 查询编辑器的 **“执行计划”** 选项卡中单击运算符的图标，可在“属性”窗口中查看有关这些运算符的信息。  
  
## <a name="see-also"></a>另请参阅  
 [属性窗口 (Management Studio)](../properties-window-management-studio.md)  
  

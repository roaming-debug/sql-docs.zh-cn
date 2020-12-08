---
title: 显示实际执行计划 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 生成实际的图形化执行计划。 实际的图形化执行计划包含运行时信息。
ms.custom: ''
ms.date: 11/21/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- displaying execution plans
- actual execution plans
- viewing execution plans
- execution plans [SQL Server], displaying
ms.assetid: 9e583a18-5f4a-4054-bfe1-4b2a76630db6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a2130a33cee0b0c5f070a45bb3b902a903dd82ee
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505277"
---
# <a name="display-an-actual-execution-plan"></a>显示实际执行计划
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本主题介绍了如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]生成实际的图形化执行计划。 执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询或批处理后，将生成实际的执行计划。 为此，实际的执行计划包含运行时信息，例如实际的资源使用量度量值和运行时警告（如果有）。 生成的执行计划会显示 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 用于执行查询的实际查询执行计划。  
  
 若要使用此功能，用户必须具有相应权限来执行要为其生成图形化执行计划的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询，并且对于查询所引用的所有数据库，用户必须被授予 SHOWPLAN 权限。  
  
## <a name="to-include-an-execution-plan-for-a-query-during-execution"></a>在查询执行中包括其执行计划  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 工具栏中，单击 **“数据库引擎查询”** 。 通过单击 **“打开文件”** 工具栏按钮，再定位到该现有查询，也可以打开一个现有查询并显示估计的执行计划。 
  
2.  输入要显示其实际执行计划的查询。  
  
3.  在“查询”菜单中，单击“包括实际的执行计划”或单击“包括实际的执行计划”工具栏按钮  。

    ![工具栏上的“实际执行计划”按钮](../../relational-databases/performance/media/actualexecplantoolbar.png "工具栏上的“实际执行计划”按钮")   
  
4.  通过单击 **“执行”** 工具栏按钮执行查询。 查询优化器使用的计划将显示在结果窗格的 **“执行计划”** 选项卡中。 

    ![实际执行计划](../../relational-databases/performance/media/actualexecplan.png "实际执行计划")   

5.  将鼠标悬停在逻辑和物理运算符上，通过选择根节点运算符（上图中的 SELECT 节点），在显示的工具提示中查看运算符的描述和属性，包括整个执行计划的属性。   
  
    另外，还可以在“属性”窗口中查看运算符属性。 如果属性不可见，请右键单击一个运算符并单击“属性”。 选择要查看其属性的运算符。  

    ![右键单击计划运算符中的“属性”](../../relational-databases/performance/media/planproperties.png "右键单击计划运算符中的“属性”")    
  
6.  可以通过右键单击执行计划并选择“放大”、“缩小”、“自定义显示比例”或“缩放到合适大小”来更改执行计划的显示。    **“放大”** 和 **“缩小”** 可以放大或缩小执行计划， **“自定义显示比例”** 使您可以定义自己需要的显示比例，例如缩放到 80%。 **“缩放到合适大小”** 会放大执行计划以适应结果窗格。 或者，使用 Ctrl 键和鼠标滚轮的组合来激活动态缩放。  

7.  若要导航执行计划的显示，请使用垂直和水平滚动条，或单击并按住执行计划的任何空白区域，然后拖动鼠标 。 或者，在右下角的执行计划窗口中单击并按住加号 (+)，以显示整个执行计划的缩略图。

> [!NOTE] 
> 或者，使用 [SET STATISTICS XML](../../t-sql/statements/set-statistics-xml-transact-sql.md) 在执行每条语句后返回该语句的执行计划信息。 如果在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中使用，“结果”选项卡中将包含用于以图形格式打开执行计划的链接。   
> 有关详细信息，请参阅[查询分析基础结构](../../relational-databases/performance/query-profiling-infrastructure.md)。
  
## <a name="see-also"></a>另请参阅  
 [执行计划](../../relational-databases/performance/execution-plans.md)    
 [查询处理体系结构指南](../../relational-databases/query-processing-architecture-guide.md)  

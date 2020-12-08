---
title: 比较执行计划 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio“计划比较”功能比较实际图形执行计划之间的异同。
ms.custom: ''
ms.date: 11/21/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- comparing execution plans
- compare execution plans
- plan comparison
- execution plans [SQL Server], comparing
- Query Store, comparing plans
ms.assetid: 9e583a18-5f4a-4054-bfe1-4b2a76630db6
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 4f3c78649a8934af4add9bf0696aaf91f017e687
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505349"
---
# <a name="compare-execution-plans"></a>比较执行计划
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
本主题介绍如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]“计划比较”功能比较实际图形执行计划之间的异同。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v16 及更高版本支持此功能。
  
> [!NOTE]
> 执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询或批处理后，将生成实际的执行计划。 为此，实际的执行计划包含运行时信息，例如实际的行数、资源使用量度量值和运行时警告（如果有）。 有关详细信息，请参阅[显示实际执行计划](../../relational-databases/performance/display-an-actual-execution-plan.md)。
  
比较计划的能力是指数据库专业人员出于故障排除的需要所需具备的能力：
-   查明为何查询或批处理速度突然降低。
-   了解查询重写的影响。
-   了解架构设计中引入的特定性能增强更改（如新索引）是如何有效改变执行计划的。  
 
“计划比较”菜单选项允许并行比较两个不同的执行计划，以便更轻松地确定可解释针对上述所有原因的不同行为的相似之处与变化之处。 此选项可比较：
- 两个以前保存的执行计划文件（.sqlplan 扩展名）。
- 一个活动的执行计划和一个以前保存的查询执行计划。
- [查询存储](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)中两个选定的查询计划。

> [!TIP]
> 计划比较适用于所有 .sqlplan 文件，包括较旧版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 文件。 此外，此选项还支持脱机比较，因此无需连接 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 

比较两个执行计划时，本质上执行相同操作的计划区域会以相同颜色和样式突出显示。 如果单击一个计划中的某个彩色区域，焦点会同时集中在另一个计划的匹配节点上。 仍可以对不同执行计划中不匹配的运算符和节点进行比较，但在这种情况下，必须手动选择运算符才能比较。

> [!IMPORTANT]
> 只有那些会改变计划整体形状的节点会用来检查计划之间的相似性。 因此，两个位于计划中同一子分区的节点之间可能存在一个未着色的节点。 这种缺少颜色的情况表示，在检查分区是否相等时，未考虑该节点。
  
## <a name="to-compare-execution-plans"></a>比较执行计划
  
1.  使用“文件”菜单打开以前保存的查询执行计划文件 (.sqlplan) 并单击“打开文件”或将计划文件拖到 [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] 窗口 。 或者，如果刚执行了查询并选择显示其执行计划，请移动到结果窗格中的“执行计划”选项卡中。 

2.  右键单击执行计划的空白区域，然后单击“比较显示计划”。 

    ![右键单击“比较显示计划”](../../relational-databases/performance/media/plancomparisonmenuoption.png "右键单击“比较显示计划”")   

3.  选择要比较的第二个查询文件。 第二个文件随即打开，以便执行计划比较。

4.  将为要比较的计划打开新窗口，默认情况下，一个在顶部，一个在底部。 默认选择内容为首次在各比较的计划中出现的相同运算符或节点，但会显示计划间差异。 所有突出显示的运算符和节点同时存在于两个被比较的计划中。 选择上面或下面的计划中突出显示的运算符将自动选择另一个计划中对应的运算符。 此外，选择任意被比较的计划中的根节点运算符还将选择另一被比较计划中相应的根节点运算符。

    ![两个已保存计划文件的计划比较](../../relational-databases/performance/media/plancomparison-plans.png "两个已保存计划文件的计划比较")  

     > [!TIP]
     > 可以通过右键单击执行计划的空白区域并选择“切换拆分器方向”，将执行计划比较的显示方式切换为并排显示。

     > [!TIP]
     > 可用于执行计划的所有缩放和导航选项均在计划比较模式下工作。 有关详细信息，请参阅[显示实际执行计划](../../relational-databases/performance/display-an-actual-execution-plan.md)。

5.  在默认选择范围内，右侧还会显示一个双属性窗口。 如果属性同时存在于两个被比较的运算符中但有差异，属性前面会带有不等号 (&ne;)，这样更易识别。

    ![双属性窗口](../../relational-databases/performance/media/plancomparison-properties.png "双属性窗口")  

6.  底部还会显示“显示计划分析”比较导航窗口。 有三个可用的选项卡：

    1.  在“语句选项”选项卡中，默认选中“突出显示类似运算”，并且比较的计划中突出显示的相同运算符或节点会采用相同的颜色和线条样式。 通过单击某个线条模式，可浏览比较的计划中的类似区域。 此外，还可通过选择“突出显示不匹配相似段的运算”，突出显示计划中的差异而不是相似之处。 
    
       > [!NOTE]
       > 默认情况下，比较计划时将忽略数据库名称，以允许比较为名称不同但架构相同的数据库所捕获的计划。 例如，比较数据库 ProdDB 和 TestDB 中的计划 。 可以使用“比较运算符时忽略数据库名称”选项来更改此行为。

       ![显示计划分析窗口](../../relational-databases/performance/media/plancomparison-analysis.png "显示计划分析窗口") 

    2.  使用多语句比较计划时，“多语句”选项卡非常有用。它可以比较正确的语句对。

        ![比较的计划中的多个语句](../../relational-databases/performance/media/plancomparison-multiple.png "比较的计划中的多个语句")  

    3.  在“方案”选项卡中，可以找到关于部分最相关方面的自动分析，查看与比较的计划间的[基数估算](../../relational-databases/performance/cardinality-estimation-sql-server.md)差异相关的内容。 对于在左窗格中列出的每个运算符，右侧窗格会在“单击此处了解有关此方案的更多信息”链接中显示该方案的详细信息，并列出制定该方案的可能理由。 

        ![不同的估计行数](../../relational-databases/performance/media/plancomparison-scenarios.png "不同的估计行数")  

    如果此窗口为关闭状态，右键单击某个比较的计划的空白区域，并选择“显示计划比较选项”以重新打开此窗口。

    ![计划比较选项](../../relational-databases/performance/media/plancomparison-options.png "计划比较选项")  

## <a name="to-compare-execution-plans-in-query-store"></a>比较查询存储中的执行计划

1.  在查询存储中，确定具有多个执行计划的查询。 有关查询存储方案的详细信息，请参阅[查询存储使用方案](../../relational-databases/performance/query-store-usage-scenarios.md#identify-and-tune-top-resource-consuming-queries)。

2.  结合使用 Shift 键和鼠标，为同一查询选择两个计划。 

    ![在查询存储中选择两个计划](../../relational-databases/performance/media/plancomparison-querystore.png "在查询存储中选择两个计划")   

3.  使用“在单独的窗口中比较选定查询的计划”按钮开始计划比较。 这时可使用“比较执行计划”的步骤 4 到 6。 

    ![在查询存储中比较显示计划](../../relational-databases/performance/media/plancomparison-querystoreoption.png "在查询存储中比较显示计划") 

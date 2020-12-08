---
title: 分析实际执行计划 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 计划分析功能分析包含运行时信息的实际图形执行计划。
ms.custom: ''
ms.date: 10/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- analyzing execution plans
- analyzing actual execution plans
- execution plans [SQL Server], analyzing
ms.assetid: 9e583a18-5f4a-4054-bfe1-4b2a76630db6
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 461f2ef3dc8617531513272d032eb30c6b59d790
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505409"
---
# <a name="analyze-an-actual-execution-plan"></a>分析实际执行计划

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题介绍了如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 计划分析功能来分析实际的图形化执行计划。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v17.4 及更高版本支持此功能。 我们通常建议[安装最新版本的 SSMS](../../ssms/download-sql-server-management-studio-ssms.md)。

> [!NOTE]
> 执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询或批处理后，将生成实际的执行计划。 为此，实际的执行计划包含运行时信息，例如实际的行数、资源使用量度量值和运行时警告（如果有）。 有关详细信息，请参阅[显示实际执行计划](../../relational-databases/performance/display-an-actual-execution-plan.md)。
  
查询性能故障排除需要具备大量有关查询处理和执行计划方面的专业知识，以便可以实际找到和修复根本原因。

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 包括的功能可在实际执行计划分析任务中实现某种程度的自动化，特别是对于大型的复杂计划。 目标是更容易找到不准确的[基数估计](../../relational-databases/performance/cardinality-estimation-sql-server.md)的情况，并获得可能的缓解措施建议。

> [!IMPORTANT]
> 请确保先对建议的缓解措施进行适当测试，然后再将其应用到生产环境。
  
## <a name="to-analyze-an-execution-plan-for-a-query"></a>分析查询的执行计划  
  
1.  使用“文件”菜单打开以前保存的查询执行计划文件 (.sqlplan) 并单击“打开文件”或将计划文件拖到 [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] 窗口 。 或者，如果刚执行了查询并选择显示其执行计划，请移动到结果窗格中的“执行计划”选项卡中。 

2.  右键单击执行计划的空白区域，然后单击“分析实际执行计划”。 

    ![右键单击“分析实际执行计划”](../../relational-databases/performance/media/plananalysismenuoption.png "右键单击“分析实际执行计划”")   

3.  “显示计划分析”窗口在底部打开。 通过分析正确的语句，使用多语句分析计划时“多语句”选项卡非常有用。

4.  选择“方案”选项卡，查看针对实际执行计划发现的问题详细信息。 对于在左窗格中列出的每个运算符，右侧窗格会在“单击此处了解有关此方案的更多信息”链接中显示该方案的详细信息，并列出制定该方案的可能理由。

    ![执行计划分析结果](../../relational-databases/performance/media/plananalysis-scenarios.png "执行计划分析结果") 

---
description: 使用自定义表达式汇总或聚合值 (Visual Database Tools)
title: 使用自定义表达式汇总或聚合值
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- summarizing query results
- custom expressions to aggregate values [SQL Server]
ms.assetid: 34130ac1-0106-4766-b324-acb0b7bb6f6e
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 2b50489a7a063f81e68de788ed829a0ff728c6e3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350411"
---
# <a name="summarize-or-aggregate-values-using-custom-expressions-visual-database-tools"></a>使用自定义表达式汇总或聚合值 (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
除了使用聚合函数来聚合数据之外，还可以创建自定义表达式来生成聚合值。 可在聚合查询中的任何位置使用自定义表达式来替代聚合函数。  
  
例如，在 `titles` 表中，您可能希望创建一个查询，不仅显示平均价格，而且还显示打折时的平均价格。  
  
所包括的表达式不能基于只涉及表中单个行的计算，表达式必须基于聚合值，因为在计算表达式时只有聚合值是可用的。  
  
### <a name="to-specify-a-custom-expression-for-a-summary-value"></a>为汇总值指定自定义表达式  
  
1.  为查询指定组。 有关详细信息，请参阅[对查询结果中的行进行分组 (Visual Database Tools)](../../ssms/visual-db-tools/group-rows-in-query-results-visual-database-tools.md)。  
  
2.  移动到“条件”窗格中的空白行，然后在“列”列中键入表达式。  
  
    [查询和视图设计器](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md)将自动为表达式分配列别名，以在查询输出中创建有用的列标题。 有关详细信息，请参阅[创建列别名 (Visual Database Tools)](../../ssms/visual-db-tools/create-column-aliases-visual-database-tools.md)。  
  
3.  在表达式的“分组依据”列中，选择“表达式”。  
  
4.  运行该查询。  
  
## <a name="see-also"></a>另请参阅  
[对查询结果进行排序和分组 (Visual Database Tools)](../../ssms/visual-db-tools/sort-and-group-query-results-visual-database-tools.md)  
[汇总查询结果 (Visual Database Tools)](../../ssms/visual-db-tools/summarize-query-results-visual-database-tools.md)  
  

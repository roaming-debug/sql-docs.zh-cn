---
description: 汇总或聚合表中所有行的值 (Visual Database Tools)
title: 汇总或聚合表中所有行的值
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- summarizing query results
- aggregate functions [SQL Server], summarizing query results
ms.assetid: f5af876e-f937-4110-ba09-07999c35a699
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 88c4808e8418c248f54c9da8966306020fbb72f0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350424"
---
# <a name="summarize-or-aggregate-values-for-all-rows-in-a-table-visual-database-tools"></a>汇总或聚合表中所有行的值 (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
## <a name="aggregate-function"></a>聚合函数
使用聚合函数，可为表中所有值创建汇总。 例如，可创建如下所示的查询，以显示 `titles` 表中所有书籍的总价：  
  
```  
SELECT SUM(price)  
FROM titles  
```  
  
通过对多个列使用聚合函数，可在同一查询中创建多个聚合。 例如，可创建计算 `price` 列的合计以及 `discount` 列的平均值的查询。  
  
也可在同一查询中用不同方式聚合同一列（如合计、计数以及求平均值）。 例如，下面的查询将对 `price` 表的 `titles` 列求平均值并进行汇总：  
  
```  
SELECT AVG(price), SUM(price)  
FROM titles  
```  
  
如果添加搜索条件，则可聚合满足该条件的行子集。  

**注意！** 您还可以对表中的所有行或满足特定条件的行进行计数。 有关详细信息，请参阅[如何计算表中的行数 (Visual Database Tools)](../../ssms/visual-db-tools/count-rows-in-a-table-visual-database-tools.md)。  
  
  
当为表中的所有行创建单个聚合值时，将仅显示聚合值本身。 例如，如果对 `price` 表的 `titles` 列的值进行合计，将无法同时显示单个书名、出版商名称等。  
 
 **!** 如果进行小计（即创建组），则可显示每个组的列值。 有关详细信息，请参阅[对查询结果中的行进行分组 (Visual Database Tools)](../../ssms/visual-db-tools/group-rows-in-query-results-visual-database-tools.md)。  

## <a name="aggregate-values-for-all-rows"></a>对所有行的值进行聚合  
  
1.  请确保您要聚合的表已经包含在“关系图”窗格中。  
  
2.  右键单击“关系图”窗格的背景，再从快捷菜单中选择“分组依据”。 [查询和视图设计器](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md)会在“条件”窗格的网格中添加一个“分组依据”列。  
  
3.  将要聚合的列添加到“条件”窗格中。 确保将该列标记为输出。  
  
    查询及视图设计器将自动为要汇总的列分配列别名。 您可以用更有意义的名称替换此别名。 有关详细信息，请参阅[创建列别名 (Visual Database Tools)](../../ssms/visual-db-tools/create-column-aliases-visual-database-tools.md)。  
  
4.  在“分组依据”网格列中，选择适当的聚合函数，例如：**Sum**、**Avg**、**Min**、**Max** 和 **Count**。 如果只希望聚合结果集中的唯一行，请选择带 DISTINCT 选项的聚合函数，如 **Min Distinct**。 不要选择 **Group By**、 **Expression** 或 **Where**，因为这些选项不适用于聚合所有行。  
  
    查询和视图设计器将用指定的聚合函数替换 [“SQL”窗格](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md) 内语句中的列名。 例如，SQL 语句可能类似以下形式：  
  
    ```  
    SELECT SUM(price)  
    FROM titles  
    ```  
  
5.  如果希望在查询中创建多个聚合，请重复第 3 和第 4 步。  
  
    当向查询输出列表或排序依据列表中添加另一列时，查询和视图设计器会自动在网格的“分组依据”列中填充 **Group By** 一词。 请选择适当的聚合函数。  
  
6.  添加搜索条件（如果有的话），以指定要汇总的行子集。  
  
当您执行查询时，“结果”窗格将显示指定的聚合。  
  
> [!NOTE]  
> 查询和视图设计器将聚合函数一直作为 SQL 窗格中 SQL 语句的一部分进行维护，直到您显式关闭“分组依据”模式为止。 因此，如果您通过更改查询类型或更改“关系图”窗格中显示的表或表值对象来修改查询，那么所生成的查询可能包含无效的聚合函数。  
  
## <a name="see-also"></a>另请参阅  
[对查询结果进行排序和分组 (Visual Database Tools)](../../ssms/visual-db-tools/sort-and-group-query-results-visual-database-tools.md)  
[汇总查询结果 (Visual Database Tools)](../../ssms/visual-db-tools/summarize-query-results-visual-database-tools.md)  
  

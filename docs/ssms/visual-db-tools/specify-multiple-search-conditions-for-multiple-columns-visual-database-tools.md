---
description: 为多个列指定多个搜索条件 (Visual Database Tools)
title: 为多个列指定多个搜索条件
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- search criteria [SQL Server], multiple conditions
- multiple search conditions
- search conditions [SQL Server], multiple
- OR operator
- AND, Criteria pane
ms.assetid: 06617729-0d0b-4da2-9890-b7e2f5cdbc7b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 4831d45d1e258855a70e8171a0d07c46a8a56d41
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350433"
---
# <a name="specify-multiple-search-conditions-for-multiple-columns-visual-database-tools"></a>为多个列指定多个搜索条件 (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
通过在搜索条件中包括多个数据列，可以扩大或缩小查询范围。 例如，你可能希望：  
  
-   搜索在公司工作五年以上或出任某些职位的雇员。  
  
-   搜索由特定出版商出版的有关烹饪方面的书籍。  
  
若要创建在两个（或更多）列中的任一列中搜索值的查询，请指定 OR 条件。 若要创建必须满足两个（或更多）列中所有条件的查询，请指定 AND 条件。  
  
## <a name="specifying-an-or-condition"></a>指定 OR 条件  
若要创建使用 OR 链接的多个条件，请将每个独立的条件放在“条件”窗格的不同列中。  
  
#### <a name="to-specify-an-or-condition-for-two-different-columns"></a>为两个不同的列指定 OR 条件  
  
1.  在 [“条件”窗格](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md)中，添加要搜索的列。  
  
2.  在要搜索的第一个列的“筛选器”列中，指定第一个条件。  
  
3.  在要搜索的第二个数据列的 **Or...** 列中，指定第二个条件，将“筛选器”列保留为空白。  
  
    查询和视图设计器将创建包含 OR 条件的 WHERE 子句，如下所示：  
  
    ```  
    SELECT job_lvl, hire_date  
    FROM employee  
    WHERE (job_lvl >= 200) OR   
      (hire_date < '01/01/1998')  
    ```  
  
4.  对每个要添加的其他条件重复第 2 和第 3 步。 对每个新条件使用不同的 **Or...** 列。  
  
## <a name="specifying-an-and-condition"></a>指定 AND 条件  
若要使用由 AND 链接的条件搜索不同的数据列，请将所有条件都放在网格的“筛选器”列中。  
  
#### <a name="to-specify-an-and-condition-for-two-different-columns"></a>为两个不同的列指定 AND 条件  
  
1.  在 [“条件”窗格](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md)中，添加要搜索的列。  
  
2.  在要搜索的第一个数据列的“筛选器”列中，指定第一个条件。  
  
3.  在第二个数据列的“筛选器”列中，指定第二个条件。  
  
    查询和视图设计器将创建包含 AND 条件的 WHERE 子句，如下所示：  
  
    ```  
    SELECT pub_id, title  
    FROM titles  
    WHERE (pub_id = '0877') AND (title LIKE '%Cook%')  
    ```  
  
4.  对每个要添加的其他条件重复第 2 和第 3 步。  
  
## <a name="see-also"></a>另请参阅  
[在 AND 优先时合并条件](../../ssms/visual-db-tools/combine-conditions-when-and-has-precedence-visual-database-tools.md)  
[在 OR 优先时组合条件](../../ssms/visual-db-tools/combine-conditions-when-or-has-precedence-visual-database-tools.md)  
[在“条件”窗格中组合搜索条件的约定](../../ssms/visual-db-tools/conventions-combine-search-conditions-in-criteria-pane-visual-db-tools.md)  
[指定搜索条件](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
  

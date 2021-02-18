---
description: 使用表以外的对象创建查询 (Visual Database Tools)
title: 使用表以外的对象创建查询
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- user-defined functions [SQL Server], queries
- queries [SQL Server], creating
ms.assetid: 8e4a1f0a-8a42-4733-be8d-e21d6dbddb33
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 95a432ca8912b4d306538906d11bb2a3d5464e5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350451"
---
# <a name="create-queries-using-something-besides-a-table-visual-database-tools"></a>使用表以外的对象创建查询 (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
每次编写检索查询时，必须明确说明所需的列和行以及查询处理器应从何处查找原始数据。 一般情况下，此类原始数据由一个表或几个联接在一起的表组成。 不过，原始数据也可以来自表以外的源。 事实上，它可以来自视图、查询、同义词或可返回表的用户定义函数。  
  
## <a name="using-a-view-in-place-of-a-table"></a>使用视图代替表  
您可以从视图选择行。 例如，假设数据库包含一个名为“ExpensiveBooks”的视图，在该视图中，每行描述一个价格超过 19.99 美元的书名。 视图定义可能类似以下形式：  
  
```  
SELECT *  
FROM titles  
WHERE price > 19.99  
```  
  
这样，只需从 ExpensiveBooks 视图中选择心理学书籍，就可以选择较贵的心理学书籍。 生成的 SQL 结果可能类似以下形式：  
  
```  
SELECT *  
FROM ExpensiveBooks  
WHERE type = 'psychology'  
```  
  
同样，视图也可以参与 JOIN 操作。 例如，只需将 sales 表联接到 ExpensiveBooks 视图，就可以查找较贵的书籍的销售额。 生成的 SQL 结果可能类似以下形式：  
  
```  
SELECT *  
FROM sales   
         INNER JOIN   
         ExpensiveBooks   
         ON sales.title_id   
         =  ExpensiveBooks.title_id  
```  
  
有关向查询添加视图的详细信息，请参阅[向查询中添加表 (Visual Database Tools)](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md)。  
  
## <a name="using-a-query-in-place-of-a-table"></a>使用查询代替表  
您可以从查询选择行。 例如，假设已编写了一个查询，用于检索合著书籍（有一个以上作者的书）的书名和标识符。 该 SQL 可能类似以下形式：  
  
```  
SELECT   
     titles.title_id, title, type  
FROM   
     titleauthor   
         INNER JOIN  
         titles   
         ON titleauthor.title_id   
         =  titles.title_id   
GROUP BY   
     titles.title_id, title, type  
HAVING COUNT(*) > 1  
```  
  
您随后可以编写另一个基于此结果的查询。 例如，您可以编写一个检索合著的心理学书籍的查询。 若要编写这个新查询，则可以将现有查询用作这个新查询的数据源。 生成的 SQL 结果可能类似以下形式：  
  
```  
SELECT   
    title  
FROM   
    (  
    SELECT   
        titles.title_id,   
        title,   
        type  
    FROM   
        titleauthor   
            INNER JOIN  
            titles   
            ON titleauthor.title_id   
            =  titles.title_id   
    GROUP BY   
        titles.title_id,   
        title,   
        type  
    HAVING COUNT(*) > 1  
    )   
    co_authored_books  
WHERE     type = 'psychology'  
```  
  
加粗的文本表示用作新查询的数据源的现有查询。 请注意，新查询使用了现有查询的别名（“co_authored_books”）。 有关别名的详细信息，请参阅[创建表别名 (Visual Database Tools)](../../ssms/visual-db-tools/create-table-aliases-visual-database-tools.md) 和[创建列别名 (Visual Database Tools)](../../ssms/visual-db-tools/create-column-aliases-visual-database-tools.md)。  
  
同样，查询也可以参与 JOIN 操作。 例如，只需将 ExpensiveBooks 视图联接到检索合著书籍的查询，就可以查找较贵的合著书籍的销售额。 生成的 SQL 结果可能类似以下形式：  
  
```  
SELECT   
    ExpensiveBooks.title  
FROM   
    ExpensiveBooks   
        INNER JOIN  
        (  
        SELECT   
            titles.title_id,   
            title,   
            type  
        FROM   
            titleauthor   
                INNER JOIN  
                titles   
                ON titleauthor.title_id   
                =  titles.title_id   
        GROUP BY   
            titles.title_id,   
            title,   
            type  
        HAVING COUNT(*) > 1  
        )  
```  
  
有关向查询添加查询的详细信息，请参阅[向查询中添加表 (Visual Database Tools)](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md)。  
  
## <a name="using-a-user-defined-function-in-place-of-a-table"></a>使用用户定义函数代替表  
在 SQL Server 2000 或更高版本中，可以创建返回表的用户定义函数。 此类函数对执行复杂逻辑操作或逐步逻辑操作很有用。  
  
例如，假设 employee 表包含一个附加列 employee.manager_emp_id，且存在从 manager_emp_id 到 employee.emp_id 的外键。 在 employee 表的每个行中，在 manager_emp_id 列显示该雇员的老板。 更确切地说，它显示该雇员的老板的 emp_id。 您可以创建用户定义函数以返回这样的表：在某个特定高级经理的所辖部门层次结构中工作的每个雇员在该表中各占一行。 可以调用函数 fn_GetWholeTeam，并对其进行设计以获得输入变量（待检索部门的经理的 emp_id）。  
  
随后可以编写将 fn_GetWholeTeam 函数用作数据源的查询。 生成的 SQL 结果可能类似以下形式：  
  
```  
SELECT *   
FROM   
     fn_GetWholeTeam ('VPA30890F')  
```  
  
“VPA30890F”是待检索部门的经理的 emp_id。 有关向查询添加用户定义函数的详细信息，请参阅[向查询中添加表 (Visual Database Tools)](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md)。 有关用户定义函数的完整说明，请参阅 [用户定义函数](../../relational-databases/user-defined-functions/user-defined-functions.md)。  

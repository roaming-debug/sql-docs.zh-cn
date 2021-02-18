---
description: 在“条件”窗格中组合搜索条件的约定 (Visual Database Tools)
title: 在“条件”窗格中组合搜索条件的约定
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- search conditions [SQL Server], combining
- precedence [SQL Server], Criteria pane
- search criteria [SQL Server], combining conditions
- multiple OR clauses
- combining search conditions
- OR operator
- AND, Criteria pane
- multiple AND clauses
ms.assetid: d4859be5-ff5b-48b2-a101-ad40c6dbcc68
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: fe9c75b0b173bf2f72437233d2c06305b5041132
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355201"
---
# <a name="conventions-for-combining-search-conditions-in-the-criteria-pane-visual-database-tools"></a>在“条件”窗格中组合搜索条件的约定 (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
您可以创建包含用任意多个 AND 和 OR 运算符链接的任意多个搜索条件的查询。 具有 AND 和 OR 子句组合的查询会变得非常复杂。因此，了解在执行此类查询时如何对其进行解释，以及在[“条件”窗格](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md)和 [SQL 窗格](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md)中如何表示此类查询，是非常有用的。  
  
> [!NOTE]  
> 有关仅包含一个 AND 或 OR 运算符的搜索条件的详细信息，请参阅[为同一列指定多个搜索条件 (Visual Database Tools)](../../ssms/visual-db-tools/specify-multiple-search-conditions-for-one-column-visual-database-tools.md) 和 [为多个列指定多个搜索条件 (Visual Database Tools)](../../ssms/visual-db-tools/specify-multiple-search-conditions-for-multiple-columns-visual-database-tools.md)。  
  
在本文稍后部分中，您将了解以下相关信息：  
  
-   在包含 AND 和 OR 的查询中，AND 和 OR 的优先级。  
  
-   AND 和 OR 子句中的条件在逻辑上如何相关。  
  
-   查询和视图设计器在“条件”窗格中如何表示同时包含 AND 和 OR 的查询。  
  
为帮助理解下面的讨论内容，本文假设您正使用包含 `employee` 、 `hire_date`和 `job_lvl`列的 `status`表。 这些示例假设您需要知道如下信息：雇员在公司工作了多长时间（即雇员的雇佣日期），雇员承担何种工作（职位等级是什么），以及雇员的状况（例如，是否已退休）。  
  
## <a name="precedence-of-and-and-or"></a>AND 和 OR 的优先级  
当执行查询时，它首先计算与 AND 链接的子句，然后计算与 OR 链接的子句。  
  
> [!NOTE]  
> NOT 运算符的优先级高于 AND 和 OR 运算符。  
  
例如，若要查找在公司工作五年以上的低级职位雇员，或者查找中级职位的雇员而不考虑他们的雇佣日期，可构造如下的 WHERE 子句：  
  
```  
WHERE   
   hire_date < '01/01/95' AND   
   job_lvl = 100 OR  
   job_lvl = 200  
```  
  
若要改变 AND 高于 OR 的默认优先级，可在 SQL 窗格中将特定的条件用圆括号括起来。 括号里的条件始终先进行计算。 例如，若要查找在公司工作五年以上的低级职位或中级职位的所有雇员，可构造如下的 WHERE 子句：  
  
```  
WHERE   
   hire_date < '01/01/95' AND   
   (job_lvl = 100 OR job_lvl = 200)  
```  
  
> [!TIP]  
> 为清楚起见，建议在组合 AND 和 OR 子句时始终使用圆括号，而不要依赖默认的优先级。  
  
## <a name="how-and-works-with-multiple-or-clauses"></a>AND 如何与多个 OR 子句一起使用  
了解组合 AND 和 OR 子句时它们的相关方式有助于构造和理解查询和视图设计器中的复杂查询。  
  
如果使用 AND 链接多个条件，则与 AND 链接的第一组条件适用于第二组的所有条件。 也就是说，用 AND 与另一个条件链接的条件将分布到第二组的所有条件。 例如，下面的表达式简单表示了与一组 OR 条件链接的 AND 条件：  
  
```  
A AND (B OR C)  
```  
  
上面的表达式与下面的简单表达式在逻辑上是等效的，后者表示 AND 条件如何分布到第二组条件中：  
  
```  
(A AND B) OR (A AND C)  
```  
  
这种分布式原则将影响您对查询和视图设计器的使用方式。 例如，假设您要查找在公司工作五年以上的低级或中级职位的所有雇员。 您可在 SQL 窗格内的语句中输入以下 WHERE 子句：  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
   (job_lvl = 100 OR job_lvl = 200)  
```  
  
用 AND 链接的子句适用于用 OR 链接的两个子句。 为清楚起见，可以为 OR 子句中的每个条件重复一次 AND 条件。 下面的语句比前面的语句更清楚明了（也更长），但逻辑上是等效的：  
  
```  
WHERE    (hire_date < '01/01/95' ) AND  
  (job_lvl = 100) OR   
  (hire_date < '01/01/95' ) AND   
  (job_lvl = 200)  
```  
  
将 AND 子句分布到链接的 OR 子句的原则也适用于涉及多个条件的情况。 例如，假设您要查找在公司工作五年以上或已退休的中级或更高职位的雇员。 该 WHERE 子句可能类似以下形式：  
  
```  
WHERE   
   (job_lvl = 200 OR job_lvl = 300) AND  
   (hire_date < '01/01/95' ) OR (status = 'R')  
```  
  
在分布用 AND 链接的条件后，WHERE 子句将类似以下形式：  
  
```  
WHERE   
   (job_lvl = 200 AND hire_date < '01/01/95' ) OR  
   (job_lvl = 200 AND status = 'R') OR  
   (job_lvl = 300 AND hire_date < '01/01/95' ) OR  
   (job_lvl = 300 AND status = 'R')  
```  
  
## <a name="how-multiple-and-and-or-clauses-are-represented-in-the-criteria-pane"></a>在“条件”窗格中如何表示多个 AND 和 OR 子句  
查询和视图设计器可以在 [“条件”窗格](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md)中显示搜索条件。 但是，在需要用 AND 和 OR 链接多个子句的某些情况下，“条件”窗格中的表示形式可能会与预期形式不一样。 另外，如果在“条件”窗格或 [“关系图”](../../ssms/visual-db-tools/diagram-pane-visual-database-tools.md)窗格中修改查询，你可能会发现生成的 SQL 语句与输入的语句不同。  
  
一般情况下，以下规则将决定 AND 和 OR 子句在“条件”窗格中的显示方式：  
  
-   所有用 AND 链接的条件都显示在“筛选器”网格列中或同一个“或...”列中。  
  
-   所有用 OR 链接的条件都显示在不同的“或...”列中。  
  
-   如果 AND 和 OR 子句组合的逻辑结果是将 AND 分布到多个 OR 子句中，则“条件”窗格将根据需要重复 AND 子句以明确地表示结果。  
  
例如，在 SQL 窗格中可创建如下搜索条件，其中用 AND 链接的两个子句的优先级高于用 OR 链接的第三个子句：  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  (job_lvl = 100) OR   
  (status = 'R')  
```  
  
查询和视图设计器在“条件”窗格中按以下形式显示此 WHERE 子句：  
  
![“条件”窗格中的 OR 子句优先级](../../ssms/visual-db-tools/media/vs_criteriapane1.gif "“条件”窗格中的 OR 子句优先级")  
  
但是，如果链接的 OR 子句优先级高于 AND 子句，则每个 OR 子句都会重复 AND 子句。 这将导致 AND 子句分布到每个 OR 子句中。 例如，在 SQL 窗格中可以创建如下 WHERE 子句：  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  ( (job_lvl = 100) OR   
  (status = 'R') )  
```  
  
查询和视图设计器在“条件”窗格中按以下形式显示此 WHERE 子句：  
  
![“条件”窗格中的多个 AND 子句和 OR 子句](../../ssms/visual-db-tools/media/vs_criteriapane2.gif "“条件”窗格中的多个 AND 子句和 OR 子句")  
  
如果链接的 OR 子句仅涉及一个数据列，查询和视图设计器会将整个 OR 子句放置到单个的网格单元格中，以避免重复 AND 子句。 例如，在 SQL 窗格中可以创建如下 WHERE 子句：  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  ((status = 'R') OR (status = 'A'))  
```  
  
查询和视图设计器在“条件”窗格中按以下形式显示此 WHERE 子句：  
  
![“条件”窗格中定义的链接 OR 子句](../../ssms/visual-db-tools/media/vs_criteriapane3.gif "“条件”窗格中定义的链接 OR 子句")  
  
如果更改查询（如更改“条件”窗格中的某个值），查询和视图设计器将在 SQL 窗格中重新创建 SQL 语句。 重新创建的 SQL 语句将与“条件”窗格的显示形式相似，而不是与原始语句相似。 例如，如果“条件”窗格中包含分布式的 AND 子句，则会用清楚明了的分布式 AND 子句重新创建 SQL 窗格中的结果语句。 有关详细信息，请参阅本主题前面部分中的“AND 如何与多个 OR 子句一起使用”。  
  
## <a name="see-also"></a>另请参阅  
[指定搜索条件 (Visual Database Tools)](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
  

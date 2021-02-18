---
description: 为同一列指定多个搜索条件 (Visual Database Tools)
title: 为一个列指定多个搜索条件
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
ms.assetid: 2c006e36-56b1-4992-89b4-c6c0b19808f3
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: ddb90eb0beaabac86e8d422fac24fa3cf8c66168
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346072"
---
# <a name="specify-multiple-search-conditions-for-one-column-visual-database-tools"></a>为同一列指定多个搜索条件 (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
在某些情况下，可能希望对同一数据列应用多个搜索条件。 例如，您可能希望：  
  
-   在 `employee` 表中搜索几个不同的名字或搜索位于不同薪金范围内的雇员。 这种类型的搜索需要使用 OR 条件。  
  
-   搜索以单词“The”开头并包含单词“Cook”的书名。 这种类型的搜索需要使用 AND 条件。  
  
> [!NOTE]  
> 本主题中的信息对查询的 WHERE 和 HAVING 子句中的搜索条件都适用。 这些示例主要讨论创建 WHERE 子句，但其原理适用于这两种类型的搜索条件。  
  
若要在同一数据列中搜索可选值，可指定 OR 条件。 若要搜索同时满足几个条件的值，可指定 AND 条件。  
  
## <a name="specifying-an-or-condition"></a>指定 OR 条件  
使用 OR 条件，您可以指定要在列中搜索的多个可选值。 此选项扩展了搜索范围，可以比搜索单一值返回更多的行。  
  
> [!TIP]  
> 通常可以改用 IN 运算符在同一数据列中搜索多个值。  
  
#### <a name="to-specify-an-or-condition"></a>指定 OR 条件  
  
1.  在[“条件窗格”](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md)中，添加要搜索的列。  
  
2.  在刚刚添加的数据列的“筛选器”列中，指定第一个条件。  
  
3.  在同一数据列的“或...”列中，指定第二个条件。  
  
查询和视图设计器将创建包含 OR 条件的 WHERE 子句，如下所示：  
  
```  
SELECT fname, lname  
FROM employees  
WHERE (salary < 30000) OR (salary > 100000)  
```  
  
## <a name="specifying-an-and-condition"></a>指定 AND 条件  
使用 AND 条件，您可以指定某列中的值必须同时满足两个（或更多）条件，才能使该行包含在结果集中。 此选项缩小了搜索范围，通常会比搜索单一值返回更少的行。  
  
> [!TIP]  
> 若要搜索一定范围内的值，可使用 BETWEEN 运算符替代 AND 来链接两个条件。  
  
#### <a name="to-specify-an-and-condition"></a>指定 AND 条件  
  
1.  在“条件”窗格中，添加要搜索的列。  
  
2.  在刚刚添加的数据列的“筛选器”列中，指定第一个条件。  
  
3.  将同一数据列再次添加到“条件”窗格中，将其放在网格的空行中。  
  
4.  在数据列的第二个实例的“筛选器”列中，指定第二个条件。  
  
查询设计器将创建包含 AND 条件的 WHERE 子句，如下所示：  
  
```  
SELECT title_id, title  
FROM titles  
WHERE (title LIKE '%Cook%') AND   
  (title LIKE '%Recipe%')  
```  
  
## <a name="see-also"></a>另请参阅  
[在“条件”窗格中组合搜索条件的约定 (Visual Database Tools)](../../ssms/visual-db-tools/conventions-combine-search-conditions-in-criteria-pane-visual-db-tools.md)  
[指定搜索条件 (Visual Database Tools)](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
  

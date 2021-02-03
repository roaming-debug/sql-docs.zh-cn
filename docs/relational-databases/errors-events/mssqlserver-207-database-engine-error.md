---
title: MSSQLSERVER_207 | Microsoft Docs
description: SQL Server 中关于无效列名的查询错误。 请查看错误的说明和可能的解决方法。
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 207 (Database Engine error)
ms.assetid: d1ab00c7-0331-437a-84fe-bae53b82feec
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7c39bf5d5b820a906cc5b14fd270ed59aa3f4084
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196425"
---
# <a name="mssqlserver_207"></a>MSSQLSERVER_207
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|207|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|SQ_BADCOL|  
|消息正文|列名“%.*ls”无效。|  
  
## <a name="explanation"></a>说明  
此查询错误可能是由以下问题之一导致的：  
  
-   列名称拼写错误，或任一指定的表中不存在该列。  
  
-   数据库的排序规则区分大小写，在查询中指定的列名与表中所定义的列名的大小写不同。 例如，如果将某列在表中定义为 **LastName** 而且数据库使用区分大小写的排序规则，那么，以 **Lastname** 或 **lastname** 形式引用该列的查询将因列名不匹配而导致返回错误 207。  
  
-   在 SELECT 子句中定义的列别名在另一个如 WHERE 或 GROUP BY 之类的子句中被引用。 例如，以下查询在 SELECT 子句中定义列别名 `Year`，并在 GROUP BY 子句中将其引用。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT DATEPART(yyyy,OrderDate) AS Year, SUM(TotalDue) AS Total  
    FROM Sales.SalesOrderHeader  
    GROUP BY Year;  
    ```  
  
    由于查询子句逻辑上处理的顺序，该示例返回错误 207。 处理顺序如下所示：  
  
    1.  FROM  
  
    2.  ON  
  
    3.  JOIN  
  
    4.  WHERE  
  
    5.  GROUP BY  
  
    6.  WITH CUBE 或 WITH ROLLUP  
  
    7.  HAVING  
  
    8.  SELECT  
  
    9. DISTINCT  
  
    10. ORDER BY  
  
    11. TOP  
  
    因为列别名是在处理 SELECT 子句后定义的，所以当处理 GROUP BY 子句时列别名是未知的。  
  
-   当 *<merge_matched>* 子句引用源表中的列，但该源表在 WHEN NOT MATCHED BY SOURCE 子句中未返回任何行时，MERGE 语句将引发此错误。 该错误的发生是因为当未返回任何行到查询中时源表中的列不能被访问。 例如，如果无法访问源表中的 `WHEN NOT MATCHED BY SOURCE THEN UPDATE SET TargetTable.Col1 = SourceTable.Col1`，`Col1` 子句可能导致该语句失败。  
  
## <a name="user-action"></a>用户操作  
查看以下信息并根据需要更正该语句。  
  
-   表中存在此列名且该列名拼写正确。 下面的示例查询 **sys.columns** 目录视图以返回给定表的所有列名称。  
  
    ```  
    SELECT name FROM sys.columns WHERE object_id = OBJECT_ID('schema_name.table_name');  
    ```  
  
-   数据库排序规则区分大小写。 以下语句返回指定数据库的排序规则。  
  
    ```  
    SELECT collation_name FROM sys.databases WHERE name = 'database_name';  
    ```  
  
    排序规则名称中的缩写 CS 指示排序规则区分大小写。 例如，Latin1_General_CS_AS 是一个区分大小写和重音符号的排序规则。 更改列名使之与表中定义的列名的大小写相同。  
  
-   列别名引用不正确。 通过重复在相应子句中定义别名的表达式或通过使用派生表，对语句进行更改。 下面的示例重复在 GROUP BY 子句中定义 `Year` 别名的表达式。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT DATEPART(yyyy,OrderDate) AS Year ,SUM(TotalDue) AS Total  
    FROM Sales.SalesOrderHeader  
    GROUP BY DATEPART(yyyy,OrderDate);  
    ```  
  
    以下示例使用派生表使得别名可用于查询中的其他子句。 请注意，`Year` 别名是在首先处理的 FROM 子句中定义的，所以可以在查询中的其他子句中使用该别名。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT d.Year, SUM(TotalDue) AS Total  
    FROM (SELECT DATEPART(yyyy,OrderDate) AS Year, TotalDue  
          FROM Sales.SalesOrderHeader)AS d  
    GROUP BY Year;  
    ```  
  
-   MERGE 语句中的 WHEN NOT MATCHED BY SOURCE 子句引用的是可以访问的值。 修改 MERGE 语句，使源表至少在 WHEN NOT MATCHED BY SOURCE 子句中返回一行。 例如，您可能需要添加或修订为子句指定的搜索条件。 或者，也可以更改该子句以指定没有引用源表的值。 例如，`WHEN NOT MATCHED BY SOURCE THEN UPDATE SET TargetTable.Col1 = <expression, or other available value>`。  
  
## <a name="see-also"></a>另请参阅  
[MERGE (Transact-SQL)](~/t-sql/statements/merge-transact-sql.md)  
[FROM (Transact-SQL)](~/t-sql/queries/from-transact-sql.md)  
[SELECT (Transact-SQL)](~/t-sql/queries/select-transact-sql.md)  
[UPDATE (Transact-SQL)](~/t-sql/queries/update-transact-sql.md)  
  

---
description: 动态 SQL
title: 动态 SQL |Microsoft Docs
ms.custom: ''
ms.date: 11/16/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- SQL [ODBC], embedded SQL
- SQL statements [ODBC], dynamic SQL
- SQL statements [ODBC], embedded SQL
- dynamic SQL [ODBC]
- SQL [ODBC], dynamic SQL
- embedded SQL [ODBC]
ms.assetid: 0bfb9ab7-9c15-4433-93bc-bad8b6c9d287
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 86164b1bc5d98475d4e2f0980ce433d33e5d858f
ms.sourcegitcommit: a2182276ba00c48dc1475b9c7dfa45179d4416dc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "94704172"
---
# <a name="dynamic-sql"></a>动态 SQL
虽然静态 SQL 在许多情况下都可以很好地工作，但有一类应用程序无法提前确定数据访问。 例如，假设电子表格允许用户输入查询，电子表格随后会将查询发送到 DBMS 以检索数据。 编写电子表格程序时，程序员不知道此查询的内容。  
  
 若要解决此问题，电子表格将使用名为动态 SQL 的嵌入式 SQL 形式。 与程序中硬编码的静态 SQL 语句不同，可以在运行时生成动态 SQL 语句，并将其放置在字符串主机变量中。 然后将它们发送到 DBMS 进行处理。 由于 DBMS 必须在运行时为动态 SQL 语句生成访问计划，因此动态 SQL 通常比静态 SQL 慢。 当编译包含动态 SQL 语句的程序时，不会从程序中提取动态 SQL 语句，如静态 SQL 语句中所示。 相反，它们被传递给 DBMS 的函数调用替代;同一程序中的静态 SQL 语句会正常处理。  
  
 执行动态 SQL 语句的最简单方法是使用 EXECUTE IMMEDIATE 语句。 此语句将 SQL 语句传递到 DBMS 进行编译和执行。  
  
 EXECUTE 直接语句的一个缺点是，DBMS 必须每执行一次语句就 [处理 SQL 语句的五个步骤](processing-a-sql-statement.md) 。 如果动态执行了很多语句，此过程所涉及的开销会很大，并且如果这些语句相似，它也会浪费。 为了解决这种情况，动态 SQL 提供一种名为 "准备好的执行" 的优化形式，它使用以下步骤：  
  
1.  该程序会在缓冲区中构造一条 SQL 语句，与执行直接语句时相同。 问号 (？ ) 可以替换语句文本中任意位置的常量，以指示稍后将提供常数的值，而不是主机变量。 问号称为参数标记。  
  
2.  该程序使用 PREPARE 语句将 SQL 语句传递给 DBMS，请求 DBMS 对语句进行分析、验证和优化，并为其生成执行计划。 然后，该程序使用 EXECUTE 语句 (执行语句) ，稍后执行 PREPARE 语句。 它通过名为 SQL 数据区域或 SQLDA 的特殊数据结构传递语句的参数值。  
  
3.  该程序可以重复使用 EXECUTE 语句，在每次执行动态语句时提供不同的参数值。  
  
 准备好的执行仍与静态 SQL 不同。 在静态 SQL 中，处理 SQL 语句的前四个步骤在编译时进行。 在准备好的执行中，这些步骤仍会在运行时进行，但它们只执行一次;只有在调用 EXECUTE 时才会执行计划。 这有助于消除动态 SQL 体系结构固有的一些性能缺点。

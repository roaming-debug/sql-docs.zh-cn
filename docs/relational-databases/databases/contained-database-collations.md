---
title: 包含数据库的排序规则 | Microsoft Docs
description: 了解排序规则在包含数据库和非包含数据库中的工作方式。 了解会话在跨越包含和非包含上下文时可能出现的问题。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- contained database, collations
ms.assetid: 4b44f6b9-2359-452f-8bb1-5520f2528483
author: stevestein
ms.author: sstein
ms.openlocfilehash: 260c4afa90b28aa8efc22a15fbda13cfa7f35417
ms.sourcegitcommit: 98acedd435aecfda1b3c4c23d3f0c3c1a12682a4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102532319"
---
# <a name="contained-database-collations"></a>包含数据库的排序规则
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  许多属性会影响文本数据的排序顺序和相等语义，包括区分大小写、区分重音以及所用的基本语言。 对于这些特性，可通过选择数据的排序规则来表示给 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关排序规则本身的更深入讨论，请参阅 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。  
  
 排序规则不仅适用于用户表中存储的数据，还适用于由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 处理的所有文本，包括元数据、临时对象、变量名称等。在这些内容的处理方面，包含数据库和非包含数据库采用不同的方式。 此更改不会影响很多用户，而且有助于提供独立而统一的实例。 但是，此更改也可能导致某些混淆，并可能使同时访问包含数据库和非包含数据库的会话出现问题。  
  
 本主题阐明更改的内容，并考察这一更改可能导致问题的领域。  
  
## <a name="non-contained-databases"></a>非包含数据库  
 所有数据库都有一种默认的排序规则（可在创建或更改数据库时设置）。 此排序规则用于数据库中的所有元数据，以及数据库中所有字符串列的默认值。 通过使用 **COLLATE** 子句，用户可为任何特定的列选择不同的排序规则。  
  
### <a name="example-1"></a>示例 1  
 例如，如果我们在北京工作，则可能会使用中文排序规则：  
  
```sql  
ALTER DATABASE MyDB COLLATE Chinese_Simplified_Pinyin_100_CI_AS;  
```  
  
 现在，如果我们创建一个列，其默认排序规则将是此中文排序规则，但我们可以根据需要选择其他排序规则：  
  
```sql  
CREATE TABLE MyTable  
      (mycolumn1 nvarchar,  
      mycolumn2 nvarchar COLLATE Frisian_100_CS_AS);  
GO  
SELECT name, collation_name  
FROM sys.columns  
WHERE name LIKE 'mycolumn%' ;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```sql  
name            collation_name  
--------------- ----------------------------------  
mycolumn1       Chinese_Simplified_Pinyin_100_CI_AS  
mycolumn2       Frisian_100_CS_AS  
```  
  
 这看起来比较简单，但会引发几个问题。 由于列的排序规则取决于创建表的数据库，因此如果使用存储在 **tempdb** 中的临时表，就会出现问题。 **tempdb** 的排序规则通常与实例的排序规则匹配，而不必与数据库的排序规则匹配。  
  
### <a name="example-2"></a>示例 2  
 假设在排序规则为 **Latin1_General** 的实例上使用上述（中文）数据库：  
  
```sql  
CREATE TABLE T1 (T1_txt nvarchar(max)) ;  
GO  
CREATE TABLE #T2 (T2_txt nvarchar(max)) ;  
GO  
```  
  
 乍看起来这两个表似乎具有相同的架构，但由于数据库的排序规则不同，它们的值实际上并不兼容：  
  
```  
SELECT T1_txt, T2_txt  
FROM T1   
JOIN #T2   
    ON T1.T1_txt = #T2.T2_txt  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 消息 468，级别 16，状态 9，第 2 行  
  
 无法解决等于运算中“Latin1_General_100_CI_AS_KS_WS_SC”与“Chinese_Simplified_Pinyin_100_CI_AS”之间的排序规则冲突。  
  
 通过显式排列临时表可修复此问题。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 为 **COLLATE** 子句提供 **DATABASE_DEFAULT** 关键字，使操作在一定程度上得到了简化。  
  
```sql  
CREATE TABLE T1 (T1_txt nvarchar(max)) ;  
GO  
CREATE TABLE #T2 (T2_txt nvarchar(max) COLLATE DATABASE_DEFAULT);  
GO  
SELECT T1_txt, T2_txt  
FROM T1   
JOIN #T2   
    ON T1.T1_txt = #T2.T2_txt ;  
```  
  
 现在此代码将可以正常运行。  
  
 我们还可以看到，变量的行为也依赖于排序规则。 请考虑以下函数：  
  
```  
CREATE FUNCTION f(@x INT) RETURNS INT  
AS BEGIN   
      DECLARE @I INT = 1  
      DECLARE @İ INT = 2  
      RETURN @x * @i  
END;  
```  
  
 这是一个相当特殊的函数。 在区分大小写的排序规则中，return 子句中的 @i 无法绑定到 @I 或 @İ。 在不区分大小写的 Latin1_General 排序规则中，@i 绑定到 @I，该函数返回 1。 而在不区分大小写的 Turkish 排序规则中，@i 绑定到 @İ，该函数返回 2。 如果在采用不同排序规则的实例之间移动数据库，则会给数据库造成严重的破坏。  
  
## <a name="contained-databases"></a>包含的数据库  
 由于包含数据库的设计目标是让自身实现独立，因此必须切断它们对实例和 **tempdb** 排序规则的依赖。 为此，包含数据库引入了目录排序规则的概念。 目录排序规则适用于系统元数据和临时对象。 下面将详细介绍这一概念。  
  
 如果某个包含数据库的目录排序规则是 **Latin1_General_100_CI_AS_WS_KS_SC**。 则所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上的所有包含数据库都采用此排序规则，而且不能更改。  
  
 数据库排序规则将得到保留，但只能用作用户数据的默认排序规则。 默认情况下，数据库排序规则等同于 model 数据库排序规则，但在非包含数据库中，用户可通过 **CREATE** 或 **ALTER DATABASE** 命令更改数据库排序规则。  
  
 新关键字 **CATALOG_DEFAULT** 适用于 **COLLATE** 子句。 此关键字用作包含数据库和非包含数据库中当前元数据排序规则的快捷方式。 换言之，在非包含数据库中， **CATALOG_DEFAULT** 将返回当前的数据库排序规则，因为元数据是按数据库排序规则排列的。 在包含数据库中，这两个值可能是不同的，因为用户可以更改数据库排序规则，以使其不同于目录排序规则。  
  
 下表总结了非包含数据库和包含数据库中各个对象的行为：  
  
|Item|非包含数据库|包含数据库|  
|-|-|-|  
|用户数据（默认）|COLLATE|COLLATE|  
|临时数据（默认）|TempDB 排序规则|COLLATE|  
|元数据|DATABASE_DEFAULT/CATALOG_DEFAULT|CATALOG_DEFAULT|  
|临时元数据|TempDB 排序规则|CATALOG_DEFAULT|  
|变量|实例排序规则|CATALOG_DEFAULT|  
|Goto 标签|实例排序规则|CATALOG_DEFAULT|  
|游标名称|实例排序规则|CATALOG_DEFAULT|  
  
 通过上述临时表示例可以看出，此排序规则行为使大多数临时表不再需要使用显式 COLLATE 子句。 在包含数据库中，即使数据库和实例采用不同的排序规则，此代码现在也可以正常运行：  
  
```sql  
CREATE TABLE T1 (T1_txt nvarchar(max)) ;  
GO  
CREATE TABLE #T2 (T2_txt nvarchar(max));  
GO  
SELECT T1_txt, T2_txt  
FROM T1   
JOIN #T2   
    ON T1.T1_txt = #T2.T2_txt ;  
```  
  
 之所以能够正常运行，是因为 `T1_txt` 和 `T2_txt` 都按包含数据库的数据库排序规则排列。  
  
## <a name="crossing-between-contained-and-uncontained-contexts"></a>跨越包含和非包含上下文  
 只要包含数据库中的会话仍处于包含状态，就必须保留在它所连接到的数据库内。 此时的行为非常简单。 但是，如果会话跨越包含和非包含上下文，其行为就会变得比较复杂，因为必须将两组规则联系起来。 这种情况可能出现在部分包含数据库中，因为用户可对另一个数据库应用 **USE** 。 在此情况下，排序规则之间的差异按以下原则处理。  
  
-   批处理的排序规则行为由开始执行批处理的数据库决定。  
  
 请注意，此决定是在发出任何命令之前做出的，包括最初的 **USE**。 也即，如果批处理是在包含数据库中开始执行的，而应用于非包含数据库的第一个命令是 **USE** ，则仍对该批处理应用包含数据库的排序规则行为。 鉴于此，引用（例如对变量的引用）可能会产生多种可能的结果：  
  
-   引用可能只找到一个匹配项。 在此情况下，该引用可正常操作。  
  
-   引用在当前排序规则中可能找不到匹配项，而原本存在匹配项。 这将引发一个错误，指示该变量不存在，即使已明确创建该变量也不例外。  
  
-   引用可能会找到多个原本不同的匹配项。 这也将引发错误。  
  
 我们将用几个示例加以说明。 在这些示例中，假定存在一个名为 `MyCDB` 的部分包含数据库；其数据库排序规则设置为默认排序规则 **Latin1_General_100_CI_AS_WS_KS_SC**。 我们假设实例排序规则为 **Latin1_General_100_CS_AS_WS_KS_SC**。 两个排序规则的区别只在于是否区分大小写。  
  
### <a name="example-1"></a>示例 1  
 下面的示例演示引用只找到一个匹配项的情况。  
  
```  
USE MyCDB;  
GO  
  
CREATE TABLE #a(x int);  
INSERT INTO #a VALUES(1);  
GO  
  
USE master;  
GO  
  
SELECT * FROM #a;  
GO  
  
Results:  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
x  
-----------  
1  
```  
  
 在此示例中，标识的 #a 同时绑定在不区分大小写的目录排序规则和区分大小写的实例排序规则中，代码可以正常运行。  
  
### <a name="example-2"></a>示例 2  
 下面的示例演示当前排序规则中原本有匹配项而引用却找不到匹配项的情况。  
  
```  
USE MyCDB;  
GO  
  
CREATE TABLE #a(x int);  
INSERT INTO #A VALUES(1);  
GO  
```  
  
 在此示例中，#A 绑定到不区分大小写的默认排序规则中的 #a，而且插入部分可正常运行。  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
(1 row(s) affected)  
```  
  
 但如果继续运行脚本...  
  
```  
USE master;  
GO  
  
SELECT * FROM #A;  
GO  
```  
  
 将显示一个错误，因为我们尝试绑定到区分大小写的实例排序规则中的 #A；  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 消息 208，级别 16，状态 0，第 2 行  
  
 对象名“#A”无效。  
  
### <a name="example-3"></a>示例 3  
 下面的示例演示引用找到多个原本不同的匹配项的情况。 首先，我们在 **tempdb** （与实例具有相同的区分大小写的排序规则）中开始并执行以下代码。  
  
```  
USE tempdb;  
GO  
  
CREATE TABLE #a(x int);  
GO  
CREATE TABLE #A(x int);  
GO  
INSERT INTO #a VALUES(1);  
GO  
INSERT INTO #A VALUES(2);  
GO  
```  
  
 此操作成功，因为采用此排序规则的表是不同的。  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
(1 row(s) affected)  
(1 row(s) affected)  
```  
  
 但是，一旦进入包含数据库，我们就会发现无法再绑定到这些表。  
  
```  
USE MyCDB;  
GO  
SELECT * FROM #a;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 消息 12800，级别 16，状态 1，第 2 行  
  
 对临时表名称“#a”的引用不明确，无法解析。 可能的候选项是“#a”和“#A”。  
  
## <a name="conclusion"></a>结束语  
 与非包含数据库相比，包含数据库的排序规则行为略有不同。 此行为通常是有益的，因为它可以提供独立而简单的实例。 某些用户可能会遇到问题，特别是当会话同时访问包含数据库和非包含数据库时。  
  
## <a name="see-also"></a>另请参阅  
 [包含的数据库](../../relational-databases/databases/contained-databases.md)  
  
  

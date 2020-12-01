---
description: 计算列上的索引
title: 计算列上的索引 | Microsoft Docs
ms.custom: ''
ms.date: 11/19/2018
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- computed columns, index creation
- index creation [SQL Server], computed columns
- imprecise columns
- persisted computed columns
- precise [SQL Server]
ms.assetid: 8d17ac9c-f3af-4bbb-9cc1-5cf647e994c4
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ea5366a8080ccbfb4d48c8bcfda8928ed54932d1
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88408113"
---
# <a name="indexes-on-computed-columns"></a>计算列上的索引
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

只要满足下列要求就可以为计算列定义索引：  
  
-   所有权要求  
-   确定性要求  
-   精度要求  
-   数据类型要求  
-   SET 选项要求  
  
#### <a name="ownership-requirements"></a>所有权要求
  
计算列中的所有函数引用必须与表具有相同的所有者。  
  
## <a name="determinism-requirements"></a>确定性要求  

如果对于一组指定的输入表达式始终返回相同的结果，则说明表达式具有确定性。 **COLUMNPROPERTY** 函数的 [IsDeterministic](../../t-sql/functions/columnproperty-transact-sql.md) 属性报告 *computed_column_expression* 是否具有确定性。  
*Computed_column_expression* 必须具有确定性。 如果下列所有项均为真，则 computed_column_expression 具有确定性：  
  
-   表达式引用的所有函数都具有确定性，并且是精确的。 这些函数包括用户定义函数和内置函数。 有关详细信息，请参阅 [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)。 如果计算列是 PERSISTED，则函数可能不精确。 有关详细信息，请参阅本主题后面的 [对持久化计算列创建索引](#BKMK_persisted) 。  
  
-   表达式引用的所有列都来自包含计算列的表。  
  
-   没有列引用从多行中请求数据。 例如，聚合函数（如 SUM 或 AVG）依靠来自多行的数据，这使 *computed_column_expression* 具有不确定性。  
  
-   *computed_column_expression* 没有系统数据访问或用户数据访问。  
  
任何包含公共语言运行时 (CLR) 表达式的计算列都必须具有确定性并标记为 PERSISTED，这样才能为该列创建索引。 允许在计算列定义中使用 CLR 用户定义类型的表达式。 类型为 CLR 用户定义类型的计算列只要其类型是可比较的，就可以在该列上创建索引。 有关详细信息，请参阅 [CLR 用户定义类型](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)。  

#### <a name="cast-and-convert"></a>CAST 和 CONVERT

如果您在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]内的索引计算列中引用 date 数据类型的字符串文字，则建议使用确定性日期格式样式将该文字显式转换为所需的日期类型。 有关确定性日期格式样式的列表，请参阅 [CAST and CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md)。 

有关详细信息，请参阅[文字日期字符串转换为日期值的不确定性转换](../../t-sql/data-types/nondeterministic-convert-date-literals.md)。

#### <a name="compatibility-level"></a>兼容性级别

非 Unicode 字符数据在排序规则间的隐式转换被视为具有不确定性，除非兼容性级别设置为 80 或更低。  

当数据库兼容级别设置为 90 时，不能对包含这些表达式的计算列创建索引。 但在已升级的数据库中，包含这些表达式的现有计算列是可以维护的。 如果使用包含字符串到日期的隐式转换的索引计算列，则为了避免损坏索引，请确保数据库和应用程序中的 LANGUAGE 和 DATEFORMAT 设置一致。

兼容性级别 90 对应于 SQL Server 2005。



## <a name="precision-requirements"></a>精度要求
  
 *computed_column_expression* 必须精确。 如果下列一项或多项为真，则 *computed_column_expression* 是精确的：  
  
-   表达式的数据类型不是 **float** 或 **real** 。  
-   表达式定义中没有使用 **float** 或 **real** 数据类型。 例如，在下列语句中，列 `y` 为 **int** 且具有确定性，但不精确。  
  
    ```sql  
    CREATE TABLE t2 (a int, b int, c int, x float,   
       y AS CASE x   
             WHEN 0 THEN a   
             WHEN 1 THEN b   
             ELSE c   
          END);  
    ```  
  
> [!NOTE]  
> 任何 **float** 或 **real** 表达式都被认为是不精确的，不能作为索引键； **float** 或 **real** 表达式可以在索引视图中使用，但不能作为键使用。 对于计算列同样如此。 如果任何函数、表达式或用户定义函数包含任何 **float** 或 **real** 表达式，则被认为是不精确的。 这也包括逻辑表达式（比较）。  
  
COLUMNPROPERTY 函数的 **IsPrecise** 属性报告 *computed_column_expression* 是否精确。  


## <a name="data-type-requirements"></a>数据类型要求
  
-   为计算列定义的 *computed_column_expression* 不能求值为 **text**、 **ntext** 或 **image** 数据类型。  
-   只要计算列的数据类型可以作为索引键列，从 **image**、 **ntext**、 **text**、 **varchar(max)**、 **nvarchar(max)**、 **varbinary(max)** 和 **xml** 数据类型派生的计算列上就可以创建索引。  
-   只要计算列的数据类型可以作为非键索引列，从 **image**、 **ntext** 和 **text** 数据类型派生的计算列就可以作为非聚集索引中的非键（包含性）列。  


## <a name="set-option-requirements"></a>SET 选项要求
  
-   执行定义计算列的 CREATE TABLE 或 ALTER TABLE 语句时，必须将 ANSI_NULLS 连接级选项设置为 ON。 [OBJECTPROPERTY](../../t-sql/functions/objectproperty-transact-sql.md) 函数通过 **IsAnsiNullsOn** 属性报告此选项是否设置为 ON。  
-   对于在其中创建索引的连接和所有尝试执行 INSERT、UPDATE 或 DELETE 语句（将更改索引中的值）的连接，必须将六个 SET 选项设置为 ON，将一个选项设置为 OFF。 如果不具有上述选项设置的连接执行了任何 SELECT 语句，优化器将忽略计算列的索引。  
  
    -   NUMERIC_ROUNDABORT 选项必须设置为 OFF，且下列选项必须设置为 ON：  
    -   ANSI_NULLS  
    -   ANSI_PADDING  
    -   ANSI_WARNINGS  
    -   ARITHABORT  
    -   CONCAT_NULL_YIELDS_NULL  
    -   QUOTED_IDENTIFIER  
  
> [!NOTE]
> 当数据库兼容级别设置为 90 或更高时，如果将 ANSI_WARNINGS 设置为 ON，则将使 ARITHABORT 隐式设置为 ON。  
  
## <a name="creating-indexes-on-persisted-computed-columns"></a><a name="BKMK_persisted"></a> 对持久化计算列创建索引  

有时可以创建一个计算列，并使用一个确定性但不精确的表达式进行定义。 列在 CREATE TABLE 或 ALTER TABLE 语句中被标记为 PERSISTED 时，可以执行此操作。

这意味着 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 在表中存储计算值，并且在计算列所依赖的任何其他列发生更新时更新这些值。 如果 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 对列创建了索引并且该索引由某查询引用，则会使用这些持久值。

当 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 不能准确证明返回计算列表达式的函数（特别是在 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]中创建的 CLR 函数）是否既具有确定性又精确时，使用此选项可以对计算列创建索引。  


  
## <a name="related-content"></a>相关内容  
 [COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)    
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)
  
  

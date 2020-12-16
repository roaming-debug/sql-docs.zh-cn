---
title: 数据库标识符 | Microsoft Docs
description: 熟悉数据库标识符。 了解其排序规则、各种类、分隔要求和命名规则。
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- regular identifiers [SQL Server]
- identifiers [SQL Server]
- names [SQL Server], identifiers
- identifiers [SQL Server], about identifiers
- SQL Server identifiers
- Transact-SQL identifiers
- database objects [SQL Server], names
ms.assetid: 171291bb-f57f-4ad1-8cea-0b092d5d150c
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 62d76e47e88a3d3a660d7be89249a995e919b715
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478428"
---
# <a name="database-identifiers"></a>数据库标识符

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  数据库对象的名称即为其标识符。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的所有内容都可以有标识符。 服务器、数据库和数据库对象（例如表、视图、列、索引、触发器、过程、约束及规则等）都可以有标识符。 大多数对象要求有标识符，但对有些对象（例如约束），标识符是可选的。

 对象标识符是在定义对象时创建的。 标识符随后用于引用该对象。 例如，下列语句创建一个标识符为 `TableX`的表，该表中有两列的标识符分别是 `KeyCol` 和 `Description`：

```sql
CREATE TABLE TableX
(KeyCol INT PRIMARY KEY, Description nvarchar(80));
```

 此表还有一个未命名的约束。 `PRIMARY KEY` 约束没有标识符。

 标识符的排序规则取决于定义标识符的级别。 为实例级对象（如登录名和数据库名）的标识符分配实例的默认排序规则。 为数据库对象（如表、视图和列名）的标识符分配数据库的默认排序规则。 例如，对于名称差别仅在于大小写的两个表，可在使用区分大小写排序规则的数据库中创建，但不能在使用不区分大小写排序规则的数据库中创建。

> [!NOTE]  
> 变量的名称或函数和存储过程的参数必须符合 [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符的规则。

## <a name="classes-of-identifiers"></a>标识符的种类
有两类标识符：

-  常规标识符    
   符合标识符的格式规则。 在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中使用常规标识符时不用将其分隔开。

   ```sql
   USE AdventureWorks
   GO
   SELECT *
   FROM HumanResources.Employee
   WHERE NationalIDNumber = 153479919
   ```

-  分隔标识符    
   包含在双引号 (") 或者方括号 ([ ]) 内。 不会分隔符合标识符格式规则的标识符。 例如：

   ```sql
   USE AdventureWorks
   GO
   SELECT *
   FROM [HumanResources].[Employee] --Delimiter is optional.
   WHERE [NationalIDNumber] = 153479919 --Delimiter is optional.
   ```

在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中，必须对不符合所有标识符规则的标识符进行分隔。 例如：

```sql
USE AdventureWorks
GO
CREATE TABLE [SalesOrderDetail Table] --Identifier contains a space and uses a reserved keyword.
(
    [Order] [int] NOT NULL,
    [SalesOrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [OrderQty] [smallint] NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NOT NULL,
    [UnitPriceDiscount] [money] NOT NULL,
    [ModifiedDate] [datetime] NOT NULL,
  CONSTRAINT [PK_SalesOrderDetail_Order_SalesOrderDetailID] PRIMARY KEY CLUSTERED 
  ([Order] ASC, [SalesOrderDetailID] ASC)
);
GO

SELECT *
FROM [SalesOrderDetail Table]  --Identifier contains a space and uses a reserved keyword.
WHERE [Order] = 10;            --Identifier is a reserved keyword.
```

常规标识符和分隔标识符包含的字符数都必须在 1 到 128 之间。 对于本地临时表，标识符最多可以有 116 个字符。

## <a name="rules-for-regular-identifiers"></a>常规标识符规则
 变量、函数和存储过程的名称必须符合 [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符的规则。

1.  第一个字符必须是下列字符之一：

    -   Unicode 标准 3.2 定义的字母， Unicode 中定义的字母包括拉丁字符 a-z 和 A-Z，以及来自其他语言的字母字符。

    -   下划线 (\_)、at 符号 (@) 或数字符号 (#)。

        在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，某些位于标识符开头位置的符号具有特殊意义。 以 at 符号开头的常规标识符始终表示局部变量或参数，并且不能用作任何其他类型的对象的名称。 以一个数字符号开头的标识符表示临时表或过程。 以两个数字符号 (##) 开头的标识符表示全局临时对象。 虽然数字符号或两个数字符号字符可用作其他类型对象名的开头，但是我们建议不要这样做。

        某些 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数的名称以两个 at 符号 (@@) 开头。 为了避免与这些函数混淆，不应使用以 @@ 开头的名称。

2.  后续字符可以包括：

    -   Unicode 标准 3.2 定义的字母。

    -   基本拉丁字符或其他国家/地区字符中的十进制数字。

    -   at 符号、美元符号 ($)、数字符号或下划线。

3.  标识符必须不能是 [!INCLUDE[tsql](../../includes/tsql-md.md)] 保留字。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 保留保留字的大写和小写版本。 在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中使用标识符时，不符合这些规则的标识符必须由双引号或括号分隔。 保留字依赖于数据库兼容级别。 可通过使用 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) 语句设置该级别。

4.  不允许嵌入空格或特殊字符。

5.  不允许使用[增补字符](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters)。

 在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中使用标识符时，不符合这些规则的标识符必须由双引号或括号分隔。

> [!NOTE]
> 一些常规标识符格式规则取决于数据库兼容级别。 该级别可以使用 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)设置。

## <a name="see-also"></a>另请参阅
[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
[CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)   
[CREATE DEFAULT (Transact-SQL)](../../t-sql/statements/create-default-transact-sql.md)   
[CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)   
[CREATE RULE (Transact-SQL)](../../t-sql/statements/create-rule-transact-sql.md)   
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
[CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
[CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md)   
[DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
[DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
[INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
[保留关键字 (Transact-SQL)](../../t-sql/language-elements/reserved-keywords-transact-sql.md)   
[SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
[UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)
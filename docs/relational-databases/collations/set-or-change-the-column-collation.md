---
description: 设置或更改列排序规则
title: 设置或更改列排序规则 | Microsoft Docs
ms.custom: ''
ms.date: 12/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- tempdb database [SQL Server], collations
- collations [SQL Server], column
ms.assetid: d7a9638b-717c-4680-9b98-8849081e08be
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3f408175a59484aa162c0db654ebf4b1d5656901
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460580"
---
# <a name="set-or-change-the-column-collation"></a>设置或更改列排序规则
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  可以覆盖 **char**、 **varchar**、 **text**、 **nchar**、 **nvarchar** 和 **ntext** 数据的数据库排序规则，方法是为表的特定列指定不同的排序规则并使用以下方式之一：  
  
-   [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) 和 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) 的 COLLATE 子句，如以下示例所示。 

    -   **就地转换。** 请考虑下面定义的一个现有表：

        ```sql
        -- NVARCHAR column is encoded in UTF-16 because a supplementary character enabled collation is used
        CREATE TABLE dbo.MyTable (CharCol NVARCHAR(50) COLLATE Latin1_General_100_CI_AI_SC);

        -- VARCHAR column is encoded the Latin code page and therefore is not Unicode capable
        CREATE TABLE dbo.MyTable (CharCol VARCHAR(50) COLLATE Latin1_General_100_CI_AI);
        ```

        要将列就地转换为使用 UTF-8，请运行 `ALTER COLUMN` 语句，该语句将设置所需的数据类型和支持 UTF-8 的排序规则：

        ```sql 
        ALTER TABLE dbo.MyTable 
        ALTER COLUMN CharCol VARCHAR(50) COLLATE Latin1_General_100_CI_AI_SC_UTF8
        ```

        此方法易于实现，但是可能会阻止操作，这对于大型表和繁忙的应用程序可能会成为问题。

    -   **复制和替换。** 请考虑下面定义的一个现有表：

        ```sql
        -- NVARCHAR column is encoded in UTF-16 because a supplementary character enabled collation is used
        CREATE TABLE dbo.MyTable (CharCol NVARCHAR(50) COLLATE Latin1_General_100_CI_AI_SC);
        GO

        -- VARCHAR column is encoded using the Latin code page and therefore is not Unicode capable
        CREATE TABLE dbo.MyTable (CharCol VARCHAR(50) COLLATE Latin1_General_100_CI_AI);
        GO
        ```

        要将列转换为使用 UTF-8，请将数据复制到一个新表（该表的目标列应已设置为所需的数据类型和支持 UTF-8 的排序规则），然后替换旧表：

        ```sql
        CREATE TABLE dbo.MyTableNew (CharCol VARCHAR(50) COLLATE Latin1_General_100_CI_AI_SC_UTF8);
        GO
        INSERT INTO dbo.MyTableNew 
        SELECT * FROM dbo.MyTable;
        GO
        DROP TABLE dbo.MyTable;
        GO
        EXEC sp_rename 'dbo.MyTableNew', 'dbo.MyTable';
        GO
        ```

        此方法比就地转换要快得多，但处理包含许多依赖项（FK、PK、触发器、DF）的复杂架构和同步表尾（如果数据库正在使用）需要进行更多规划。
        
    有关详细信息，请参阅 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. 有关详细信息，请参阅[修改列（数据库引擎）](../../relational-databases/tables/modify-columns-database-engine.md#SSMSProcedure)。  
  
-   使用 **管理对象 (SMO) 中的** Column.Collation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 属性。  
  
 如果下列其中之一当前正在引用一个列，则无法更改该列的排序规则。  
  
-   计算列  
-   索引  
-   分发统计信息，可以自动生成，也可以通过 `CREATE STATISTICS` 语句生成  
-   CHECK 约束  
-   FOREIGN KEY 约束  
  
 使用 **tempdb** 时， [COLLATE](~/t-sql/statements/collations.md) 子句包括了 *database_default* 选项，以此来指定对于该连接，临时表中的列使用当前用户数据库的默认排序规则而非 **tempdb** 的排序规则。  
  
## <a name="collations-and-text-columns"></a>排序规则和文本列  
 如果 **text** 列的排序规则不同于数据库默认排序规则的代码页，则可以在该列中插入值或更新该列中的值。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会隐式地将这些值转换为该列的排序规则。  
  
## <a name="collations-and-tempdb"></a>排序规则和 tempdb  
 每次启动 **时，都会创建** tempdb [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。该数据库与 **model** 数据库具有相同的默认排序规则。 这通常与实例的默认排序规则相同。 如果为创建的用户数据库指定的默认排序规则与 **model** 的排序规则不同，则该用户数据库与 **tempdb** 的默认排序规则也不同。 所有临时存储过程或临时表都创建和存储在 **tempdb** 中。 这意味着临时表中的所有隐式列以及临时存储过程中的所有强制默认常量、变量和参数都具有与可比较对象（在永久表和存储过程中创建）不同的排序规则。  
  
 这将导致用户定义的数据库和系统数据库对象之间的排序规则出现互相不匹配的问题。 例如， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例使用 Latin1_General_CS_AS 排序规则，而您执行以下语句：  
  
```sql  
CREATE DATABASE TestDB COLLATE Estonian_CS_AS;  
USE TestDB;  
CREATE TABLE TestPermTab (PrimaryKey int PRIMARY KEY, Col1 nchar );  
```  
  
 在此系统中， **tempdb** 数据库使用代码页 1252 的 Latin1_General_CS_AS 排序规则， `TestDB` 和 `TestPermTab.Col1` 使用代码页 1257 的 `Estonian_CS_AS` 排序规则。 例如：  
  
```sql  
USE TestDB;  
GO  
-- Create a temporary table with the same column declarations  
-- as TestPermTab  
CREATE TABLE #TestTempTab (PrimaryKey int PRIMARY KEY, Col1 nchar );  
INSERT INTO #TestTempTab  
         SELECT * FROM TestPermTab;  
GO  
```  
  
 在上述示例中， **tempdb** 数据库使用 Latin1_General_CS_AS 排序规则， `TestDB` 和 `TestTab.Col1` 使用 `Estonian_CS_AS` 排序规则。 例如：  
  
```sql  
SELECT * FROM TestPermTab AS a INNER JOIN #TestTempTab on a.Col1 = #TestTempTab.Col1;  
```  
  
 由于 **tempdb** 使用默认的服务器排序规则，而 `TestPermTab.Col1` 使用其他排序规则，因此 SQL Server 会返回如下错误：“无法解决等于运算中 'Latin1_General_CI_AS_KS_WS' 与 'Estonian_CS_AS' 之间的排序规则冲突。”  
  
 为防止出现此错误，可以使用下列方法之一：  
  
-   指定临时表列使用用户数据库（而不是 **tempdb**）的默认排序规则。 如果系统需要，这可以使临时表在多个数据库中使用具有类似格式的表。  
  
    ```sql  
    CREATE TABLE #TestTempTab  
       (PrimaryKey int PRIMARY KEY,  
        Col1 nchar COLLATE database_default  
       );  
    ```  
  
-   为 `#TestTempTab` 列指定正确的排序规则：  
  
    ```sql  
    CREATE TABLE #TestTempTab  
       (PrimaryKey int PRIMARY KEY,  
        Col1 nchar COLLATE Estonian_CS_AS  
       );  
    ```  
  
## <a name="see-also"></a>另请参阅  
 [设置或更改服务器排序规则](../../relational-databases/collations/set-or-change-the-server-collation.md)   
 [设置或更改数据库排序规则](../../relational-databases/collations/set-or-change-the-database-collation.md)   
 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)  
  
  

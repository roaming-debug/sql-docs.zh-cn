---
description: rowversion (Transact-SQL)
title: rowversion (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- timestamp_TSQL
- timestamp
dev_langs:
- TSQL
helpviewer_keywords:
- rowversion data type
- size [SQL Server], rowversion
- row version-stamping [SQL Server]
- rowversion columns
- version-stamping table rows
- unique rowversion numbers
- unique timestamp numbers
- timestamp data type
- timestamp columns
- size [SQL Server], timestamp
ms.assetid: 65c9cf0e-3e8a-45f8-87b3-3460d96afb0b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 087e3e1d67bce5d8f46f03cdb1cad05578b39b46
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88479875"
---
# <a name="rowversion-transact-sql"></a>rowversion (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

公开数据库中自动生成的唯一二进制数字的数据类型。 rowversion 通常用作给表行加版本戳的机制。 存储大小为 8 个字节。 rowversion 数据类型只是递增的数字，不保留日期或时间。 若要记录日期或时间，请使用 datetime2 数据类型。
  
## <a name="remarks"></a>备注  
每个数据库都有一个计数器，当对数据库中包含 rowversion 列的表执行插入或更新操作时，该计数器值就会增加。 此计数器是数据库行版本。 这可以跟踪数据库内的相对时间，而不是时钟相关联的实际时间。 一个表只能有一个 rowversion 列。 每次修改或插入包含 rowversion 列的行时，就会在 rowversion 列中插入经过增量的数据库 rowversion 值。 这一属性使 rowversion 列不适合作为键使用，尤其是不能作为主键使用。 对行的任何更新都会更改行版本值，从而更改键值。 如果该列属于主键，那么旧的键值将无效，进而引用该旧值的外键也将不再有效。 如果该表在动态游标中引用，则所有更新均会更改游标中行的位置。 如果该列属于索引键，则对数据行的所有更新还将导致索引更新。  使用任何更新语句都会让 rowversion 值递增，即使没有任何行值发生更改。 （例如，如果某列的值为 5，且更新语句将该值设置为 5，即使没有发生任何更改，此操作也被视为更新，并且 rowversion 发生递增。）
  
timestamp 的数据类型为 rowversion 数据类型的同义词，并具有数据类型同义词的行为。 在 DDL 语句中，应尽量使用 rowversion，而不是 timestamp。 有关详细信息，请参阅[数据类型同义词 (Transact-SQL)](../../t-sql/data-types/data-type-synonyms-transact-sql.md)。
  
 timestamp 数据类型不同于在 ISO 标准中定义的 timestamp 数据类型[!INCLUDE[tsql](../../includes/tsql-md.md)] 。
  
> [!NOTE]  
>  不推荐使用 timestamp 语法。 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
在 CREATE TABLE 或 ALTER TABLE 语句中，不必为 timestamp 数据类型指定列名，例如：
  
```sql
CREATE TABLE ExampleTable (PriKey int PRIMARY KEY, timestamp);  
```  
  
如果不指定列名，则 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 将生成 timestamp 列名；但 rowversion 同义词不具有这样的行为。 在使用 rowversion 时，必须指定列名，例如：
  
```sql
CREATE TABLE ExampleTable2 (PriKey int PRIMARY KEY, VerCol rowversion) ;  
```  
  
> [!NOTE]  
>  通过使用在其 SELECT 列表中包含了 rowversion 列的 SELECT INTO 语句，可以生成重复的 rowversion 值。 建议不要以这种方式使用 rowversion。  
  
不可为空的 rowversion 列在语义上等同于 binary(8) 列。 可为空的 rowversion 列在语义上等同于varbinary(8) 列。
  
可以使用某行的 rowversion 列轻松确定自上次读取该行后，是否对该行运行过更新语句。 如果对该行运行过更新语句，则会更新 rowversion 值。 如果没有对该行运行过更新语句，则 rowversion 值将与以前读取该行时的 rowversion 值相同。 若要返回数据库的当前 rowversion 值，请使用 [@@DBTS](../../t-sql/functions/dbts-transact-sql.md)。
  
当多个用户正在同时更新行时，可以在表中添加一个 rowversion 列以帮助维护数据库的完整性。 此外，您可能还想要在不重新查询表的情况下了解有多少行被更新以及哪些行被更新。
  
例如，假定您创建了一个名为 `MyTest` 的表。 通过运行下面的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句在表中填充一些数据。
  
```sql
CREATE TABLE MyTest (myKey int PRIMARY KEY  
    ,myValue int, RV rowversion);  
GO   
INSERT INTO MyTest (myKey, myValue) VALUES (1, 0);  
GO   
INSERT INTO MyTest (myKey, myValue) VALUES (2, 0);  
GO  
```  
  
然后，可以使用以下示例 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句在更新期间实现 `MyTest` 表的乐观并发控制。 脚本使用 `<myRv>` 来表示上次读取该行时的 rowversion 值。 将该值替换为实际 rowversion 值。 一个实际的 rowversion 值的示例是 `0x00000000000007D3`。
  
```sql
DECLARE @t TABLE (myKey int);  
UPDATE MyTest  
SET myValue = 2  
    OUTPUT inserted.myKey INTO @t(myKey)   
WHERE myKey = 1   
    AND RV = <myRv>;  
IF (SELECT COUNT(*) FROM @t) = 0  
    BEGIN  
        RAISERROR ('error changing row with myKey = %d'  
            ,16 -- Severity.  
            ,1 -- State   
            ,1) -- myKey that was changed   
    END;  
```  
  


还可以将示例 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句放在事务中。 通过在事务的作用域中查询 `@t` 变量，可以在不重新查询 `MyTest` 表的情况下检索表的更新后的 `myKey` 列。

以下是使用 timestamp 语法的同一个示例。 将 `<myTS>` 替换为实际的 timestamp 。

```sql
CREATE TABLE MyTest2 (myKey int PRIMARY KEY  
    ,myValue int, TS timestamp);  
GO   
INSERT INTO MyTest2 (myKey, myValue) VALUES (1, 0);  
GO   
INSERT INTO MyTest2 (myKey, myValue) VALUES (2, 0);  
GO  
DECLARE @t TABLE (myKey int);  
UPDATE MyTest2  
SET myValue = 2  
    OUTPUT inserted.myKey INTO @t(myKey)   
WHERE myKey = 1   
    AND TS = <myTS>;  
IF (SELECT COUNT(*) FROM @t) = 0  
    BEGIN  
        RAISERROR ('error changing row with myKey = %d'  
            ,16 -- Severity.  
            ,1 -- State   
            ,1) -- myKey that was changed   
    END;  
```  
  
## <a name="see-also"></a>另请参阅
[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)  
[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)  
[INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)  
[MIN_ACTIVE_ROWVERSION (Transact-SQL)](../../t-sql/functions/min-active-rowversion-transact-sql.md)  
[SET @local_variable (Transact-SQL)](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)
  
  

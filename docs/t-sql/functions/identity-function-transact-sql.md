---
description: IDENTITY（函数）(Transact-SQL)
title: IDENTITY（函数）(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- IDENTITY_TSQL
- IDENTITY
dev_langs:
- TSQL
helpviewer_keywords:
- IDENTITY function
- SELECT statement [SQL Server], IDENTITY function
- inserting identity columns
- columns [SQL Server], creating
- identity columns [SQL Server], IDENTITY function
ms.assetid: ebec77eb-fc02-4feb-b6c5-f0098d43ccb6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 364bcd6eb68ca7c529c56fae70109b79cbc1dc18
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91114728"
---
# <a name="identity-function-transact-sql"></a>IDENTITY（函数）(Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  只用于在带有 INTO 子句的 SELECT 语句中将标识列插入到新表中。 尽管类似，但是 IDENTITY 函数不是与 CREATE TABLE 和 ALTER TABLE 一起使用的 IDENTITY 属性。  
  
> [!NOTE]  
>  要创建一个可在多个表中使用的自动递增数字或者可以从应用程序中调用而不引用任何表的自动递增数字，请参阅[序列号](../../relational-databases/sequence-numbers/sequence-numbers.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
IDENTITY (data_type [ , seed , increment ] ) AS column_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 data_type  
 标识列的数据类型。 标识列的有效数据类型可以是任何整数数据类型类别的数据类型（bit 数据类型除外），也可以是 decimal 数据类型。  
  
 seed  
 要分配给表中第一行的整数值。 为每一个后续行分配下一个标识值，该值等于上一个 IDENTITY 值加上 increment 值。 如果既没有指定 seed，也没有指定 increment，那么它们都默认为 1。  
  
 increment  
 要加到表中后续行的 seed 值上的整数值。  
  
 column_name  
 将插入到新表中的列的名称。  
  
## <a name="return-types"></a>返回类型  
 返回与 data_type 相同的数据类型。  
  
## <a name="remarks"></a>注解  
 因为该函数在表中创建一个列，所以必须用下列方式中的一种在选择列表中指定该列的名称：  
  
```sql  
--(1)  
SELECT IDENTITY(int, 1,1) AS ID_Num  
INTO NewTable  
FROM OldTable;  
  
--(2)  
SELECT ID_Num = IDENTITY(int, 1, 1)  
INTO NewTable  
FROM OldTable;  
```  
  
## <a name="examples"></a>示例  
 下面的示例将来自 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `Contact` 表的所有行都插入到名为 `NewContact` 的新表。 使用 IDENTITY 函数在 `NewContact` 表中从 100 而不是 1 开始编标识号。  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'Person.NewContact', N'U') IS NOT NULL  
    DROP TABLE Person.NewContact;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY BULK_LOGGED;  
GO  
SELECT  IDENTITY(smallint, 100, 1) AS ContactNum,  
        FirstName AS First,  
        LastName AS Last  
INTO Person.NewContact  
FROM Person.Person;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
GO  
SELECT ContactNum, First, Last FROM Person.NewContact;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [@@IDENTITY (Transact-SQL)](../../t-sql/functions/identity-transact-sql.md)   
 [IDENTITY（属性）&#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)   
 [SELECT @local_variable (Transact-SQL)](../../t-sql/language-elements/select-local-variable-transact-sql.md)   
 [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)   
 [sys.identity_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-identity-columns-transact-sql.md)  
  
  

---
description: SELECT 子句 (Transact-SQL)
title: SELECT 子句 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SELECT Clause
- SELECT_Clause_TSQL
- DISTINCT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- parentheses [SQL Server]
- identity columns [SQL Server], SELECT clause
- SELECT clause
- $IDENTITY keyword
- user-defined types [SQL Server], invoking methods and properties
- SELECT statement [SQL Server], processing orders
- clauses [SQL Server], SELECT
- $ROWGUID keyword
- queries [SQL Server], results
ms.assetid: 2616d800-4853-4cf1-af77-d32d68d8c2ef
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2a633f4cfb8a0c9e5158b24c89035c5e97588a46
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207602"
---
# <a name="select-clause-transact-sql"></a>SELECT 子句 (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  指定查询返回的列。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
SELECT [ ALL | DISTINCT ]  
[ TOP ( expression ) [ PERCENT ] [ WITH TIES ] ]   
<select_list>   
<select_list> ::=   
    {   
      *   
      | { table_name | view_name | table_alias }.*   
      | {  
          [ { table_name | view_name | table_alias }. ]  
               { column_name | $IDENTITY | $ROWGUID }   
          | udt_column_name [ { . | :: } { { property_name | field_name }   
            | method_name ( argument [ ,...n] ) } ]  
          | expression  
          [ [ AS ] column_alias ]   
         }  
      | column_alias = expression   
    } [ ,...n ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 **ALL**  
 指定在结果集中可以包含重复行。 ALL 为默认值。  
  
 DISTINCT  
 指定在结果集中只能包含唯一行。 对于 DISTINCT 关键字来说，Null 值是相等的。  
  
 TOP (expression ) [ PERCENT ] [ WITH TIES ]  
 指示只能从查询结果集返回指定的第一组行或指定的百分比数目的行。 *expression* 可以是行数或行的百分比。  
  
 为了能够向后兼容，支持在 SELECT 语句中使用不带括号的 TOP expression，但推荐不这样做。 有关详细信息，请参阅 [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md)。  
  
\< select_list > 要为结果集选择的列。 选择列表是以逗号分隔的一系列表达式。 可在选择列表中指定的表达式的最大数目是 4096。  
  
 \*  
 指定返回 FROM 子句中的所有表和视图中的所有列。 这些列按 FROM 子句中指定的表或视图顺序返回，并对应于它们在表或视图中的顺序。  
  
 table_name | view_name | table_alias.*     
 将 \* 的作用域限制为指定的表或视图。  
  
 column_name  
 要返回的列名。 请限定 column_name 以避免引用不明确，例如，当 FROM 子句中的两个表包含同名的列时会出现这种情况。 例如，[!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中的 SalesOrderHeader 和 SalesOrderDetail 表中均有名为 ModifiedDate 的列。 如果将这两个表加入查询，则可以在选择列表中指定 SalesOrderDetail 项的修改日期，即 SalesOrderDetail.ModifiedDat。  
  
 *expression*  
 常量、函数以及由一个或多个运算符连接的列名、常量和函数的任意组合，或者是子查询。  
  
 $IDENTITY  
 返回标识列。 有关详细信息，请参阅 [IDENTITY（属性）(Transact-SQL)](../../t-sql/statements/create-table-transact-sql-identity-property.md)、[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md) 和 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)。  
  
 如果 FROM 子句中的多个表内都包含具有 IDENTITY 属性的列，则必须使用特定的表名限定 $IDENTITY（如 T1.$IDENTITY）。  
  
 $ROWGUID  
 返回行 GUID 列。  
  
 如果在 FROM 子句中有多个表具有 ROWGUIDCOL 属性，则必须用特定的表名限定 $ROWGUID，如 T1.$ROWGUID。  
  
 udt_column_name  
 要返回的公共语言运行时 (CLR) 用户定义类型列的名称。  
  
> [!NOTE]  
>  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 返回以二进制表示形式表示的用户定义类型值。 若要以字符串或 XML 格式返回用户定义类型值，请使用 [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md) 或 [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md)。  
  
 { . | :: }  
 指定 CLR 用户定义类型的方法、属性或字段。 使用 . 用于实例（非静态）方法、属性或字段。 将 :: 用于静态方法、属性或字段。 若要调用 CLR 用户定义类型的方法、属性或字段，必须对类型具有 EXECUTE 权限。  
  
 property_name  
 udt_column_name 的公共属性。  
  
 field_name  
 udt_column_name 的公共数据成员。  
  
 method_name  
 带一个或多个参数的 udt_column_name 的公共方法。 method_name 不能是赋值函数方法。  
  
 以下示例通过调用名为 `Location` 类型的方法，从 `point` 表中选择 `Cities` 列的值（定义为 `Distance` 类型）：  
  
```sql
CREATE TABLE dbo.Cities (  
     Name VARCHAR(20),  
     State VARCHAR(20),  
     Location POINT);  
GO  
DECLARE @p POINT (32, 23), @distance FLOAT;  
GO  
SELECT Location.Distance (@p)  
FROM Cities;  
```  
  
 column_ alias  
 查询结果集内替换列名的可选名。 例如，可以为名为 quantity 的列指定别名，如 Quantity、Quantity to Date 或 Qty。  
  
 别名还可用于为表达式结果指定名称，例如：  
  
 ```sql
 USE AdventureWorks2012;  
 GO  
 SELECT AVG(UnitPrice) AS [Average Price]  
 FROM Sales.SalesOrderDetail;
 ```  
  
 column_alias 可以用于 ORDER BY 子句。 但不能用于 WHERE、GROUP BY 或 HAVING 子句中。 如果查询表达式是 DECLARE CURSOR 语句的一部分，则不能在 FOR UPDATE 子句中使用 column_alias。  
  
## <a name="remarks"></a>备注  
 选择列表中包含的 text 或 ntext 列返回的数据长度被设置为下列值中的最小值：text 列的实际大小、默认 TEXTSIZE 会话设置或硬编码应用程序限制  。 若要更改会话的返回文本长度，请使用 SET 语句。 默认情况下，用 SELECT 语句返回的文本数据的长度限制是 4,000 字节。  
  
 如果发生下列两种情况中的一种，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 将引发编号为 511 的异常错误并回滚当前运行的语句：  
  
-   SELECT 语句生成超过 8,060 字节的结果行或中间级工作表。  
  
-   尝试对超过 8,060 字节的行执行 DELETE、INSERT 或 UPDATE 语句。  
  
 如果没有为 SELECT INTO 或 CREATE VIEW 语句创建的列指定列名，将会发生错误。  
  
## <a name="see-also"></a>另请参阅  
 [SELECT 示例 (Transact-SQL)](../../t-sql/queries/select-examples-transact-sql.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)  
  
  

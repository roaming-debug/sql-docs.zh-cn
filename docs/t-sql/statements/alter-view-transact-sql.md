---
description: ALTER VIEW (Transact-SQL)
title: ALTER VIEW (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/25/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_VIEW_TSQL
- ALTER VIEW
dev_langs:
- TSQL
helpviewer_keywords:
- indexed views [SQL Server], modifying
- views [SQL Server], modifying
- modifying views
- ALTER VIEW statement
ms.assetid: 03eba220-13e2-49e3-bd9d-ea9df84dc28c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ece59fa96d7fc3f81df663456f2ba276f96708d0
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2021
ms.locfileid: "103575330"
---
# <a name="alter-view-transact-sql"></a>ALTER VIEW (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  修改先前创建的视图。 其中包括索引视图。 ALTER VIEW 不影响相关的存储过程或触发器，并且不会更改权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ALTER VIEW [ schema_name . ] view_name [ ( column [ ,...n ] ) ]   
[ WITH <view_attribute> [ ,...n ] ]   
AS select_statement   
[ WITH CHECK OPTION ] [ ; ]  
  
<view_attribute> ::=   
{   
    [ ENCRYPTION ]  
    [ SCHEMABINDING ]  
    [ VIEW_METADATA ]       
}   
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
ALTER VIEW [ schema_name . ] view_name [  ( column_name [ ,...n ] ) ]   
AS <select_statement>   
[;]  
``` 
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *schema_name*  
 视图所属架构的名称。  
  
 view_name  
 要更改的视图。  
  
 *column*  
 将成为指定视图的一部分的一个或多个列的名称（以逗号分隔）。  
  
> [!IMPORTANT]  
>  只有在 ALTER VIEW 执行前后列名称不变的情况下，列的权限才会保持不变。  
  
> [!NOTE]  
>  在视图的各列中，列名的权限在 CREATE VIEW 或 ALTER VIEW 语句间均适用，与基础数据源无关。 例如，如果在 CREATE VIEW 语句中授予对 SalesOrderID 列的权限，则 ALTER VIEW 语句可以重命名 SalesOrderID 列（例如，重命名为 OrderRef），并且仍然具有与使用 SalesOrderID 的视图关联的权限。  
  
 ENCRYPTION  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。  
  
 加密 [sys.syscomments](../../relational-databases/system-compatibility-views/sys-syscomments-transact-sql.md) 中包含 ALTER VIEW 语句文本的项。 WITH ENCRYPTION 可防止视图作为 SQL Server 复制的一部分进行发布。  
  
 SCHEMABINDING  
 将视图绑定到基础表的架构。 如果指定了 SCHEMABINDING，则不能以可影响视图定义的方式来修改基表。 必须首先修改或删除视图定义本身，然后才能删除要修改的表的相关性。 使用 SCHEMABINDING 时，_select\_statement_ 必须包含所引用的表、视图或用户定义函数的两部分名称 (_schema_ **.** _object_)。 所有被引用对象都必须在同一个数据库内。  
  
 不能删除参与使用 SCHEMABINDING 子句创建的视图的表或视图，除非该视图已被删除或更改，而不再具有架构绑定。 否则，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将引发错误。 另外，如果对参与具有架构绑定的视图的表执行 ALTER TABLE 语句，而这些语句又会影响视图定义，则这些语句将会失败。  
  
 VIEW_METADATA  
 指定为引用视图的查询请求浏览模式的元数据时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例将向 DB-Library、ODBC 和 OLE DB API 返回有关视图的元数据信息，而不返回基表的元数据信息。 浏览模式的元数据是[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例返回到客户端 DB-Library、ODBC 和 OLE DB API 的额外元数据。 如果使用此元数据，客户端 API 将可以实现可更新客户端游标。 浏览模式的元数据包含结果集中的列所属的基表的相关信息。  
  
 对于使用 VIEW_METADATA 创建的视图，浏览模式的元数据在描述结果集内视图中的列时，将返回视图名，而不返回基表名。  
  
 使用 WITH VIEW_METADATA 创建视图时，如果该视图具有 INSERT 或 UPDATE INSTEAD OF 触发器，则视图的所有列（timestamp 列除外）都可更新。 有关详细信息，请参阅 [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md) 中的“注释”部分。  
  
 AS  
 视图要执行的操作。  
  
 select_statement  
 定义视图的 SELECT 语句。  
  
 WITH CHECK OPTION  
 要求对该视图执行的所有数据修改语句都必须符合 select_statement 中所设置的条件。  
  
## <a name="remarks"></a>备注  
 有关 ALTER VIEW 的详细信息，请参阅 [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md) 中的“备注”部分。  
  
> [!NOTE]  
>  如果原来的视图定义是使用 WITH ENCRYPTION 或 CHECK OPTION 创建的，则只有在 ALTER VIEW 中也包含这些选项时，才会启用这些选项。  
  
 如果当前所用的视图使用 ALTER VIEW 来修改，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 使用对该视图的排他架构锁。 在授予锁时，如果该视图没有活动用户，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将从过程缓存中删除该视图的所有副本。 引用该视图的现有计划将继续保留在缓存中，但一旦被调用就会重新编译。  
  
 ALTER VIEW 可应用于索引视图；但是，ALTER VIEW 会无条件地删除视图的所有索引。  
  
## <a name="permissions"></a>权限  
 若要执行 ALTER VIEW，至少需要具有对 OBJECT 的 ALTER 权限。  
  
## <a name="examples"></a>示例  
 以下示例创建一个包含所有雇员及其雇佣日期（称为 `EmployeeHireDate`）的视图。 为该视图授予权限，但是要求更改为选择雇佣日期在某个日期之前的雇员。 然后，使用 `ALTER VIEW` 替换该视图。  
  
```sql 
USE AdventureWorks2012 ;  
GO  
CREATE VIEW HumanResources.EmployeeHireDate  
AS  
SELECT p.FirstName, p.LastName, e.HireDate  
FROM HumanResources.Employee AS e JOIN Person.Person AS  p  
ON e.BusinessEntityID = p.BusinessEntityID ;  
GO  
```  
  
 必须将视图更改为只包括在 `2002` 年之前雇佣的雇员。 如果未使用 ALTER VIEW，而是删除并重新创建视图，则必须重新输入先前用来处理与视图有关的权限的 GRANT 语句和任何其他语句。  
  
```sql  
ALTER VIEW HumanResources.EmployeeHireDate  
AS  
SELECT p.FirstName, p.LastName, e.HireDate  
FROM HumanResources.Employee AS e JOIN Person.Person AS p  
ON e.BusinessEntityID = p.BusinessEntityID  
WHERE HireDate < CONVERT(DATETIME,'20020101',101) ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md)   
 [DROP VIEW (Transact-SQL)](../../t-sql/statements/drop-view-transact-sql.md)   
 [创建存储过程](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [对发布数据库进行架构更改](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)  
  
  

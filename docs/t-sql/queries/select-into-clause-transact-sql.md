---
description: SELECT - INTO 子句 (Transact-SQL)
title: INTO 子句 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- INTO_TSQL
- INSERT_INTO_TSQL
- INSERT INTO
- INTO
- INTO clause
- INTO_clause_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- copying data [SQL Server], into a new table
- INTO clause
- moving data, to a new table
- table creation [SQL Server], INTO clause
- SELECT INTO statement
- inserting rows
- clauses [SQL Server], INTO
- row additions [SQL Server], INTO clause
ms.assetid: b48d69e8-5a00-48bf-b2f3-19278a72dd88
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7afde8d9db8beedc46ea597dc47e31877d170942
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172719"
---
# <a name="select---into-clause-transact-sql"></a>SELECT - INTO 子句 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SELECT…INTO 在默认文件组中创建一个新表，并将来自查询的结果行插入该表中。 要查看完整的 SELECT 语法，请参阅 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
[ INTO new_table ]
[ ON filegroup ]
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 new_table   
 根据选择列表中的列和从数据源选择的行，指定要创建的新表名。  
 
 new_table 的格式通过对选择列表中的表达式进行取值来确定。 new_table 中的列按选择列表指定的顺序创建。 new_table 中的每列与选择列表中的相应表达式具有相同的名称、数据类型、为 Null 性和值。 列的 IDENTITY 属性将被转移，但在“备注”部分的“使用标识列”中定义的情况除外。  
  
 要在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的同一实例上的另一个数据库中创建表，请将 new_table 指定为 database.schema.table_name 形式的完全限定名称 。  
  
 不能在远程计算机上创建 new_table，但可以从远程数据源填充 new_table 。 要从远程源表创建 new_table，请在 SELECT 语句的 FROM 子句中，按照 linked_server、catalog、schema 和 object 的形式使用由四个部分组成的名称，指定源表    。 或者，也可以在 FROM 子句中使用 [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) 或 [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) 函数来指定远程数据源。  
 
 filegroup    
 指定要在其中创建新表的文件组的名称。 指定的文件组应存在于数据库中，否则 SQL Server 引擎会引发错误。   
 
 **适用于：** [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 和更高版本。
  
## <a name="data-types"></a>数据类型  
 FILESTREAM 属性不转移到新表。 FILESTREAM BLOB 作为 varbinary(max) BLOB 复制并存储在新表中。 如果没有 FILESTREAM 属性，则 varbinary(max) 数据类型具有 2 GB 的限制。 如果某个 FILESTREAM BLOB 超过该值，则会引发 7119 错误并停止该语句。  
  
 在选择现有标识列并将其插入到新表时，新列将继承 IDENTITY 属性，除非满足以下条件之一：  
  
-   SELECT 语句包含一个联接。  
  
-   多个 SELECT 语句由 UNION 联接。  
  
-   标识列在选择列表内多次列出。  
  
-   标识列是表达式的一部分。  
  
-   标识列来自远程数据源。  
  
如果这些条件中的一个为真，列将被创建为 NOT NULL 而不继承 IDENTITY 属性。 如果在新表中需要某一标识列，但此类列不可用，或者您需要不同于源标识列的种子或增量值，则使用 IDENTITY 函数在选择列表中定义该列。 请参阅下面的“示例”部分中的“使用 IDENTITY 函数创建标识列”。  

## <a name="remarks"></a>备注  
`SELECT...INTO` 语句的操作分为两个部分：创建新表，然后插入行。  这意味着如果插入失败，它们将全部回滚，但新的（空）表将保留。  如果需要整个操作以整体的形式成功或失败，请使用[显式事务](../language-elements/begin-transaction-transact-sql.md)。
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 不能将表变量或表值参数指定为新表。  
  
 即使已对源表进行分区，也不能使用 `SELECT...INTO` 创建已分区表。 `SELECT...INTO` 不使用源表的分区方案；而是在默认文件组中创建新表。 若要在已分区表中插入行，必须先创建已分区表，然后使用 `INSERT INTO...SELECT...FROM` 语句。  
  
 源表中定义的索引、约束和触发器不会转移到新表中，也不能在 `SELECT...INTO` 语句中指定它们。 如果需要使用这些对象，可以在执行 `SELECT...INTO` 语句后创建它们。  
  
 指定 `ORDER BY` 子句无法确保行将按指定的顺序插入。  
  
 当选择列表中包含稀疏列时，稀疏列属性不会转移到新表中的列。 如果需要在新表中使用该属性，请在执行 SELECT...INTO 语句后更改列定义以包含该属性。  
  
 当选择列表中包含计算列时，新表中的相应列并不是计算列。 新列中的值是在执行 `SELECT...INTO` 时计算所得的值。  
  
## <a name="logging-behavior"></a>日志记录行为  
 `SELECT...INTO` 的日志记录量取决于数据库的有效恢复模式。 在简单恢复模式或大容量日志恢复模式下，大容量操作是最小日志记录操作。 对于最小的日志记录，使用 `SELECT...INTO` 语句可能比创建一个表后再使用 INSERT 语句填充该表的效率更高。 有关详细信息，请参阅 [事务日志 (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)。
 
包含用户定义函数 (UDF) 的 `SELECT...INTO` 语句是完整记录的操作。 如果 `SELECT...INTO` 语句中使用的用户定义函数不执行任何数据访问操作，你可以为用户定义函数指定 SCHEMABINDING 子句，这会将这些用户定义函数的派生 UserDataAccess 属性设置为 0。 进行此更改后，将按最少数量记录 `SELECT...INTO` 语句。 如果 `SELECT...INTO` 语句仍然引用至少一个将此属性设置为 1 的用户定义函数，则将完整记录该操作。
  
## <a name="permissions"></a>权限  
 在目标数据库中要求 CREATE TABLE 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-table-by-specifying-columns-from-multiple-sources"></a>A. 通过指定来自多个源的列，创建一个表  
 下面的示例通过从与雇员或地址有关的各个表中选择七列创建 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的 `dbo.EmployeeAddresses` 表。  
  
```sql  
SELECT c.FirstName, c.LastName, e.JobTitle, a.AddressLine1, a.City,   
    sp.Name AS [State/Province], a.PostalCode  
INTO dbo.EmployeeAddresses  
FROM Person.Person AS c  
    JOIN HumanResources.Employee AS e   
    ON e.BusinessEntityID = c.BusinessEntityID  
    JOIN Person.BusinessEntityAddress AS bea  
    ON e.BusinessEntityID = bea.BusinessEntityID  
    JOIN Person.Address AS a  
    ON bea.AddressID = a.AddressID  
    JOIN Person.StateProvince as sp   
    ON sp.StateProvinceID = a.StateProvinceID;  
GO  
```  
  
### <a name="b-inserting-rows-using-minimal-logging"></a>B. 使用最小日志记录插入行  
 下面的示例创建 `dbo.NewProducts` 表并从 `Production.Product` 表插入行。 此示例假定 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的恢复模式设置为 FULL。 若要确保最小方式记录，应在插入行之前将 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的恢复模式设置为 BULK_LOGGED，并在 SELECT...INTO 语句后重置为 FULL。 此过程确保 SELECT...INTO 语句在事务日志中占用最少的空间并且高效执行。  
  
```sql  
ALTER DATABASE AdventureWorks2012 SET RECOVERY BULK_LOGGED;  
GO  
  
SELECT * INTO dbo.NewProducts  
FROM Production.Product  
WHERE ListPrice > $25   
AND ListPrice < $100;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
GO  
```  
  
### <a name="c-creating-an-identity-column-using-the-identity-function"></a>C. 使用 IDENTITY 函数创建标识列  
 下面的示例使用 IDENTITY 函数在 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的新表 `Person.USAddress` 中创建一个标识列。 这是必需的，因为定义该表的 SELECT 语句包含一个联接，而该联接导致 IDENTITY 属性不转移到新表。 请注意，在 IDENTITY 函数中指定的种子和增量值不同于源表 `AddressID` 的 `Person.Address` 列中的种子和增量值。  
  
```sql  
-- Determine the IDENTITY status of the source column AddressID.  
SELECT OBJECT_NAME(object_id) AS TableName, name AS column_name, 
  is_identity, seed_value, increment_value  
FROM sys.identity_columns  
WHERE name = 'AddressID';  
  
-- Create a new table with columns from the existing table Person.Address. 
-- A new IDENTITY column is created by using the IDENTITY function.  
SELECT IDENTITY (int, 100, 5) AS AddressID,   
       a.AddressLine1, a.City, b.Name AS State, a.PostalCode  
INTO Person.USAddress   
FROM Person.Address AS a  
INNER JOIN Person.StateProvince AS b 
  ON a.StateProvinceID = b.StateProvinceID  
WHERE b.CountryRegionCode = N'US';   
  
-- Verify the IDENTITY status of the AddressID columns in both tables.  
SELECT OBJECT_NAME(object_id) AS TableName, name AS column_name, 
  is_identity, seed_value, increment_value  
FROM sys.identity_columns  
WHERE name = 'AddressID';  
```  
  
### <a name="d-creating-a-table-by-specifying-columns-from-a-remote-data-source"></a>D. 通过指定来自远程数据源的列，创建一个表  
 下面的示例演示从远程数据源在本地服务器上创建新表的三个方法。 该示例从创建指向远程数据源的链接开始。 然后在第一个 SELECT...INTO 语句的 FROM 子句中和第二个 SELECT...INTO 语句的 OPENQUERY 函数中指定链接服务器名称 `MyLinkServer,`。 第三个 SELECT...INTO 语句使用 OPENDATASOURCE 函数，该函数直接指定远程数据源，而非使用链接的服务器名称。  
  
 适用于：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
```sql
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' 
-- or 'server_name\instance_name'.  
EXEC sp_addlinkedserver @server = N'MyLinkServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI',   
    @datasrc = N'server_name',  
    @catalog = N'AdventureWorks2012';  
GO  

USE AdventureWorks2012;  
GO  
-- Specify the remote data source in the FROM clause using a four-part name   
-- in the form linked_server.catalog.schema.object.  
SELECT DepartmentID, Name, GroupName, ModifiedDate  
INTO dbo.Departments  
FROM MyLinkServer.AdventureWorks2012.HumanResources.Department  
GO  
-- Use the OPENQUERY function to access the remote data source.  
SELECT DepartmentID, Name, GroupName, ModifiedDate  
INTO dbo.DepartmentsUsingOpenQuery  
FROM OPENQUERY(MyLinkServer, 'SELECT *  
               FROM AdventureWorks2012.HumanResources.Department');   
GO  
-- Use the OPENDATASOURCE function to specify the remote data source.  
-- Specify a valid server name for Data Source using the format 
-- server_name or server_name\instance_name.  
SELECT DepartmentID, Name, GroupName, ModifiedDate  
INTO dbo.DepartmentsUsingOpenDataSource  
FROM OPENDATASOURCE('SQLNCLI',  
    'Data Source=server_name;Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Department;  
GO  
```  
  
### <a name="e-import-from-an-external-table-created-with-polybase"></a>E. 从使用 PolyBase 创建的外部表导入  
 从 Hadoop 或 Azure 存储空间将数据导入到 SQL Server 进行永久存储。 使用 `SELECT INTO` 导入外部表引用的数据，以便永久存储在 SQL Server 中。 动态创建关系表，然后在第二步中创建基于该表的列存储索引。  
  
 适用对象：[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。  
  
```sql
-- Import data for car drivers into SQL Server to do more in-depth analysis.  
SELECT DISTINCT   
        Insured_Customers.FirstName, Insured_Customers.LastName,   
        Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
INTO Fast_Customers from Insured_Customers INNER JOIN   
(  
        SELECT * FROM CarSensor_Data where Speed > 35   
) AS SensorD  
ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
ORDER BY YearlyIncome;  
```  
### <a name="f-copying-the-data-from-one-table-to-another-and-create-the-new-table-on-a-specified-filegroup"></a>F. 将数据从一个表复制到另一个表，并在指定的文件组上创建新表
以下示例演示如何创建一个新表作为另一个表的副本，并将其加载到用户默认文件组之外的指定文件组。

 **适用于：** [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 和更高版本。

```sql
ALTER DATABASE [AdventureWorksDW2016] ADD FILEGROUP FG2;
ALTER DATABASE [AdventureWorksDW2016]
ADD FILE
(
NAME='FG2_Data',
FILENAME = '/var/opt/mssql/data/AdventureWorksDW2016_Data1.mdf'
)
TO FILEGROUP FG2;
GO
SELECT * INTO [dbo].[FactResellerSalesXL] ON FG2 FROM [dbo].[FactResellerSales];
```
  
## <a name="see-also"></a>另请参阅  
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [SELECT 示例 (Transact-SQL)](../../t-sql/queries/select-examples-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [IDENTITY（函数）(Transact-SQL)](../../t-sql/functions/identity-function-transact-sql.md)  
  
  

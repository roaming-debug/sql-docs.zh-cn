---
description: 设置或更改数据库排序规则
title: 设置或更改数据库排序规则 | Microsoft Docs
ms.custom: ''
ms.date: 10/27/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- collations [SQL Server], database
- database collations [SQL Server]
ms.assetid: 1379605c-1242-4ac8-ab1b-e2a2b5b1f895
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7fbaf22758dcf62d2159e63ee3af3c0507f3f607
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460570"
---
# <a name="set-or-change-the-database-collation"></a>设置或更改数据库排序规则
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 设置和更改数据库排序规则。 如果未指定排序规则，则使用服务器排序规则。  
  
> [!IMPORTANT]
> 不支持对 Azure SQL 数据库执行 `ALTER DATABASE COLLATE` 语句。

 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **若要设置或更改数据库排序规则，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   仅限 Windows Unicode 的排序规则只能与 COLLATE 子句一起使用，以便将排序规则应用于列级别和表达式级别数据上的 **nchar**、 **nvarchar** 和 **ntext** 数据类型。 它们不能与 COLLATE 子句一起使用以更改数据库或服务器实例的排序规则。  
  
-   如果指定的排序规则或者被引用的对象所使用的排序规则使用 Windows 不支持的代码页，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将显示错误。  

###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
你可以在 [Windows 排序规则名称 (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md) 和 [SQL Server 排序规则名称 (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md)中找到支持的排序规则名称，或者可以使用 [sys.fn_helpcollations (Transact-SQL)](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md) 系统函数。  
  
更改数据库排序规则时，需要更改下列内容：  
  
-   将系统表中的任何 **char**、 **varchar**、 **text**、 **nchar**、 **nvarchar** 或 **ntext** 列更改为使用新的排序规则。  
  
-   存储过程和用户定义函数的所有现有 **char**、 **varchar**、 **text**、 **nchar**、 **nvarchar** 或 **ntext** 参数和标量返回值均已更改为新的排序规则。  
  
-   **char**、 **varchar**、 **text**、 **nchar**、 **nvarchar** 或 **ntext** 系统数据类型和基于这些系统数据类型的所有用户定义的数据类型均已更改为新的默认排序规则。  
  
可以使用 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) 语句的 `COLLATE` 子句来更改在用户数据库中创建的任何新对象的排序规则。 使用此语句不能更改任何现有用户定义的表中列的排序规则  。 使用 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) 的 `COLLATE` 子句可以更改这些列的排序规则。  

> [!IMPORTANT]
> 更改数据库或单个列的排序规则时，不会修改已存储在现有表中的基础数据。 除非你的应用程序显式处理不同排序规则之间的数据转换和比较，否则建议将数据库中的现有数据转换为新的排序规则。 这消除了应用程序可能不当修改数据的风险，错误修改会导致结果可能错误或者数据丢失却无提示。   

更改数据库排序规则时，默认情况下，只有新表将继承新的数据库排序规则。 有几种备用方法可将现有数据转换为新的排序规则：
-  就地转换数据。 若要转换现有表中某列的排序规则，请参阅[设置或更改列排序规则](../../relational-databases/collations/set-or-change-the-column-collation.md)。 此操作很容易实现，但可能会造成大型表和繁忙的应用程序受阻的问题。 请查看以下示例，了解将 `MyString` 列就地转换为新的排序规则的情况：

   ```sql
   ALTER TABLE dbo.MyTable
   ALTER COLUMN MyString VARCHAR(50) COLLATE Latin1_General_100_CI_AI_SC_UTF8;
   ```

-  将数据复制到使用新的排序规则的新表中，并替换同一数据库中的原始表。 在当前数据库中创建一个将继承数据库排序规则的新表，在旧表与新表之间复制数据，删除原始表，然后将新表重命名为原始表的名称。 该操作比就地转换的速度要快，但在处理具有依赖项（例如外键约束、主键约束和触发器）的复杂架构时，可能成为一项挑战。 如果应用程序继续更改数据，那么它还要求在最终截断之前，在原始表与新表之间进行最终的数据同步。 请查看以下示例，了解通过“复制替换”方式将 `MyString` 列转换为新的排序规则的情况：

   ```sql
   CREATE TABLE dbo.MyTable2 (MyString VARCHAR(50) COLLATE Latin1_General_100_CI_AI_SC_UTF8); 
   
   INSERT INTO dbo.MyTable2 
   SELECT * FROM dbo.MyTable; 
   
   DROP TABLE dbo.MyTable; 
   
   EXEC sp_rename 'dbo.MyTable2', 'dbo.MyTable’;
   ```

-  将数据复制到使用新的排序规则的数据库，然后替代原始数据库。 使用新的排序规则创建一个新的数据库，再通过 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 等工具或者 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的“导入/导出向导”传输原始数据库中的数据。 对于复杂架构，此方法更简单。 如果应用程序继续更改数据，那么它还要求在最终截断之前，在原始数据库与新数据库之间进行最终的数据同步。
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 若要创建新数据库，需要 master 数据库中的 `CREATE DATABASE` 权限，或者需要 `CREATE ANY DATABASE` 或 `ALTER ANY DATABASE` 权限  。  
  
 若要更改现有数据库的排序规则，需要数据库上的 `ALTER` 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-set-or-change-the-database-collation"></a>设置或更改数据库排序规则  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的实例，再依次展开该实例、 **“数据库”** 。  
  
2.  如果您正在创建一个新数据库，则右键单击 **“数据库”** ，然后单击 **“新建数据库”** 。 如果您不希望使用默认排序规则，则单击 **“选项”** 页，然后从 **“排序规则”** 下拉列表中选择某一排序规则。  
  
     或者，如果数据库已经存在，则右键单击所需数据库，然后单击 **“属性”** 。 单击 **“选项”** 页，然后从 **“排序规则”** 下拉列表中选择某一排序规则。  
  
3.  在完成后，单击 **“确定”** 。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-set-the-database-collation"></a>设置数据库排序规则  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。 此示例演示如何使用 [COLLATE](~/t-sql/statements/collations.md) 子句来指定排序规则名称。 此示例创建使用 `MyOptionsTest` 排序规则的数据库 `Latin1_General_100_CS_AS_SC` 。 在创建数据库后，执行 `SELECT` 语句以验证设置。  
  
```sql  
USE master;  
GO  
IF DB_ID (N'MyOptionsTest') IS NOT NULL  
DROP DATABASE MyOptionsTest;  
GO  
CREATE DATABASE MyOptionsTest  
COLLATE Latin1_General_100_CS_AS_SC;  
GO  
  
--Verify the collation setting.  
SELECT name, collation_name  
FROM sys.databases  
WHERE name = N'MyOptionsTest';  
GO  
```  
  
#### <a name="to-change-the-database-collation"></a>更改数据库排序规则  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。 此示例说明如何在 [ALTER DATABASE](~/t-sql/statements/collations.md) 语句中使用 [COLLATE](../../t-sql/statements/alter-database-transact-sql.md) 子句来更改排序规则名称。 执行 `SELECT` 语句以验证更改。  
  
```sql  
USE master;  
GO  
ALTER DATABASE MyOptionsTest  
COLLATE French_CI_AS ;  
GO  
  
--Verify the collation setting.  
SELECT name, collation_name  
FROM sys.databases  
WHERE name = N'MyOptionsTest';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)   
 [sys.fn_helpcollations (Transact-SQL)](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [SQL Server 排序规则名称 (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md)   
 [Windows 排序规则名称 (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md)   
 [COLLATE (Transact-SQL)](~/t-sql/statements/collations.md)   
 [排序规则优先级 (Transact-SQL)](../../t-sql/statements/collation-precedence-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)   
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)  
  

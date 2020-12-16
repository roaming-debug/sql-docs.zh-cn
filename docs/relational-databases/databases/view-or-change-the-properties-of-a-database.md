---
title: 查看或更改数据库的属性 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中查看或更改数据库的属性。
ms.custom: ''
ms.date: 01/05/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- displaying databases
- database viewing [SQL Server]
- databases [SQL Server], viewing
- viewing databases
ms.assetid: 9e8ac097-84b7-46c7-85e3-c1e79f94d747
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6195e52fba9bf21c4ef7c154dcd5f89d50cca6cd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478338"
---
# <a name="view-or-change-the-properties-of-a-database"></a>查看或更改数据库的属性
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中查看或更改数据库的属性。 更改数据库属性后，修改内容将立即生效。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **查看或更改数据库的属性，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   当 AUTO_CLOSE 为 ON 时，由于该数据库不可用于检索数据，因此 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的某些列和 DATABASEPROPERTYEX 函数将返回 NULL。 若要解决此问题，请打开数据库。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 必须具有对数据库的 ALTER 权限，才能更改数据库的属性。 至少必须有公共数据库角色成员资格，才能查看数据库的属性。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-view-or-change-the-properties-of-a-database"></a>查看或更改数据库的属性  
  
1.  在 **对象资源管理器** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的实例，然后展开该实例。  
  
2.  展开“数据库”，右键单击要查看的数据库，再单击“属性”。  
  
3.  在 **“数据库属性”** 对话框中，选择一个页以查看相应的信息。 例如，选择 **“文件”** 页可以查看数据和日志文件信息。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 Transact-SQL 提供了许多用于查看和更改数据库属性的不同方法。 若要查看数据库属性，可以使用 [DATABASEPROPERTYEX (Transact-SQL)](../../t-sql/functions/databasepropertyex-transact-sql.md) 函数和 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图。 要更改数据库属性，可以使用适用于你环境的 ALTER DATABASE 语句版本：[ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md) 或 [ALTER DATABASE（Azure SQL 数据库）](../../t-sql/statements/alter-database-transact-sql.md)。 若要查看数据库范围属性，使用 [sys.database_scoped_configurations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) 目录视图；若要更改数据库范围属性，则使用 [ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) 语句。  
  
#### <a name="to-view-a-property-of-a-database-by-using-the-databasepropertyex-function"></a>使用 DATABASEPROPERTYEX 函数查看数据库属性  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] ，然后连接到要查看其属性的数据库。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例使用 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 系统函数返回 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中 AUTO_SHRINK 数据库选项的状态。 返回值 1 表示将该选项设置为 ON，返回值 0 表示将该选项设置为 OFF。  
  
    ```sql  
    SELECT DATABASEPROPERTYEX('AdventureWorks2012', 'IsAutoShrink');  
    ```  
  
#### <a name="to-view-the-properties-of-a-database-by-querying-sysdatabases"></a>通过查询 sys.databases 查看数据库的属性  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] ，然后连接到要查看其属性的数据库。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例查询 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图来查看 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的几个属性。 此实例返回数据库 ID 号 (`database_id`)、数据库是只读还是读写的 (`is_read_only`)、数据库的排序规则 (`collation_name`) 和数据库兼容级别 (`compatibility_level`)。  
  
    ```sql  
    SELECT database_id, is_read_only, collation_name, compatibility_level  
    FROM sys.databases WHERE name = 'AdventureWorks2012';  
    ```  
  
#### <a name="to-view-the-properties-of-a-database-scoped-configuration-by-querying-sysdatabases_scoped_configuration"></a>通过查询 sys.databases_scoped_configuration 查看数据库范围的配置的属性  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] ，然后连接到要查看其属性的数据库。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例查询 [sys.database_scoped_configurations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) 目录视图来查看当前数据库的多个属性。  
  
    ```sql  
    SELECT configuration_id, name, value, value_for_secondary  
    FROM sys.database_scoped_configurations;  
    ```  
  
     有关更多示例，请参阅 [sys.database_scoped_configurations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md)  
  
#### <a name="to-change-the-properties-of-a-sql-server-2016-database-using-alter-database"></a>使用 ALTER DATABASE 更改 SQL Server 2016 数据库的属性  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  复制以下示例并将其粘贴到查询窗口中。 此示例确定 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库上的快照隔离状态，更改属性的状态，然后验证更改。  
  
     若要确定快照隔离的状态，请选择第一个 `SELECT` 语句，然后单击 **“执行”** 。  
  
     若要更改快照隔离的状态，请选择 `ALTER DATABASE` 语句，然后单击 **“执行”** 。  
  
     若要验证更改，请选择第二个 `SELECT` 语句，然后单击 **“执行”** 。  
  
     [!code-sql[DatabaseDDL#AlterDatabase9](../../relational-databases/databases/codesnippet/tsql/view-or-change-the-prope_1.sql)]  
  
#### <a name="to-change-the-database-scoped-properties-using-alter-database-scoped-configuration"></a>使用 ALTER DATABASE SCOPED CONFIGURATION 更改数据库范围的属性  
  
1.  连接到 SQL Server 实例中的数据库。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  复制以下示例并将其粘贴到查询窗口中。 下面的示例将辅助数据库的 MAXDOP 设置为主数据库的值。  
  
    ```sql  
    ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET MAXDOP = PRIMARY   
    ```  
  
## <a name="see-also"></a>另请参阅  
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [DATABASEPROPERTYEX (Transact-SQL)](../../t-sql/functions/databasepropertyex-transact-sql.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [ALTER DATABASE（Azure SQL 数据库）](../../t-sql/statements/alter-database-transact-sql.md)   
 [ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)   
 [sys.database_scoped_configurations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md)  

  

---
description: Enable Stretch Database for a table
title: Enable Stretch Database for a table
ms.date: 08/05/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, enabling table
- enabling table for Stretch Database
ms.assetid: de4ac0c5-46ef-4593-a11e-9dd9bcd3ccdc
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 29d4e1d6e2de7c575b9549f13dc325add4f5ca88
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081032"
---
# <a name="enable-stretch-database-for-a-table"></a>Enable Stretch Database for a table
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  若要为 Stretch Database 配置表，请在 SQL Server Management Studio 中为表选择“Stretch | 启用”，以打开“为 Stretch 启用表”向导。 还可以使用 Transact-SQL 在现有表上启用 Stretch Database，或创建已启用 Stretch Database 的新表。  
  
-   如果在单独的某个表中存储了冷数据，可以迁移整个表。  
  
-   如果表同时包含热数据和冷数据，可以指定筛选器函数来选择要迁移的行。    
 
 **先决条件**。 如果针对某个表选择了“**延伸 | 启用**”但尚未为数据库启用 Stretch Database，该向导将先为 Stretch Database 配置数据库。 请执行[通过运行“启用数据库延伸向导”开始](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md)中的步骤，而非本文中的步骤。  
  
 **权限**。 在数据库或表上启用 Stretch Database 需要 db_owner 权限。 对某个表启用 Stretch Database 还要求对该表拥有 ALTER 权限。  

 > [!NOTE]
 > 之后如果要禁用 Stretch Database，请记住，禁用表或数据库的 Stretch Database 不会删除远程对象。 如果希望删除远程表或远程数据库，则需要使用 Azure 管理门户进行删除。 远程对象会继续产生 Azure 成本，直到手动删除它们。
 
##  <a name="use-the-wizard-to-enable-stretch-database-on-a-table"></a><a name="EnableWizardTable"></a> 使用向导在表上启用 Stretch Database  
 **启动向导**  
 1.  在 SQL Server Management Studio 的对象资源管理器中，选择要在其上启用 Stretch 的表。  
  
2.  右键单击并选择“Stretch”，然后选择“启用”，以启动向导。  
  
 **介绍**  
 查看向导和必备组件的用途。  
  
 **选择数据库表**  
 确认已显示并选定你要启用的表。  
  
 你可以迁移整个表，或在向导中指定一个简单的筛选器函数。 如果想要使用不同类型的筛选器函数来选择要迁移的行，请执行以下操作之一。  
  
-   退出向导并运行 ALTER TABLE 语句以对表启用 Stretch 并指定筛选器函数。  
  
-   运行 ALTER TABLE 语句以在退出向导后指定筛选器函数。 有关所需步骤，请参阅 [运行向导后添加筛选器函数](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md#addafterwiz)。  
  
 ALTER TABLE 语法将在本文的后面进行介绍。  
  
 **摘要**  
 查看你输入的值和你在该向导中选择的选项。 然后选择“完成”  以启用 Stretch。  
  
 **结果**  
 查看结果。  
  
##  <a name="use-transact-sql-to-enable-stretch-database-on-a-table"></a><a name="EnableTSQLTable"></a> 使用 Transact-SQL 在表上启用 Stretch Database  
 也可以使用 Transact-SQL 为现有表启用 Stretch Database，或使用启用的 Stretch Database 创建一个新表。  
  
### <a name="options"></a>选项  
 运行 CREATE TABLE 或 ALTER TABLE 时使用以下选项来在表上启用 Stretch Database。  
  
-   根据需要，如果表中同时包含热数据和冷数据，则使用 `FILTER_PREDICATE = <function>` 子句指定一个函数来选择要迁移的行。 该谓词必须调用内联表值函数。 有关详细信息，请参阅 [通过使用筛选器函数选择要迁移的行](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md)。 如果未指定筛选器函数，则将迁移整个表。  
  
    > [!IMPORTANT]  
    > 如果提供的筛选器函数性能不佳，则数据迁移性能也不佳。 Stretch Database 通过使用 CROSS APPLY 运算符将筛选器函数应用到表。  
  
-   指定 `MIGRATION_STATE = OUTBOUND` 以立即开始数据迁移，或指定  `MIGRATION_STATE = PAUSED` 以推迟数据迁移的开始时间。  
  
### <a name="enable-stretch-database-for-an-existing-table"></a>启用现有表的 Stretch Database  
 若要为 Stretch Database 配置现有表，请运行 ALTER TABLE 命令。  
  
 以下示例将迁移整个表并立即开始数据迁移。  
  
```sql  
USE <Stretch-enabled database name>;
GO
ALTER TABLE <table name>  
    SET ( REMOTE_DATA_ARCHIVE = ON ( MIGRATION_STATE = OUTBOUND ) ) ;  
GO
```  
  
 下面是仅迁移由 `dbo.fn_stretchpredicate` 内联表值函数标识的行并推迟数据迁移的示例。 有关筛选器函数的详细信息，请参阅[通过使用筛选器函数选择要迁移的行](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md)。  
  
```sql  
USE <Stretch-enabled database name>;
GO
ALTER TABLE <table name>  
    SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(),  
        MIGRATION_STATE = PAUSED ) ) ;  
 GO
```  
  
 有关详细信息，请参阅 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)。  
  
### <a name="create-a-new-table-with-stretch-database-enabled"></a>创建已启用 Stretch Database 的新表  
 若要创建已启用 Stretch Database 的新表，请运行 CREATE TABLE 命令。  
  
 以下示例将迁移整个表并立即开始数据迁移。  
  
```sql  
USE <Stretch-enabled database name>;
GO
CREATE TABLE <table name>
    ( ... )  
    WITH ( REMOTE_DATA_ARCHIVE = ON ( MIGRATION_STATE = OUTBOUND ) ) ;  
GO
```  
  
 下面是仅迁移由 `dbo.fn_stretchpredicate` 内联表值函数标识的行并推迟数据迁移的示例。 有关筛选器函数的详细信息，请参阅[通过使用筛选器函数选择要迁移的行](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md)。  
  
```sql  
USE <Stretch-enabled database name>;
GO
CREATE TABLE <table name> 
    ( ... )  
    WITH ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(),  
        MIGRATION_STATE = PAUSED ) ) ;  
GO  
```  
  
 有关详细信息，请参阅 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)  
  
  

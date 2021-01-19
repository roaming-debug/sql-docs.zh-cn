---
description: DBCC CLONEDATABASE (Transact-SQL)
title: DBCC CLONEDATABASE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/23/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CLONEDATABASE
- CLONE DATABASE
- DBCC_CLONEDATABASE_TSQL
- DBCC CLONEDATABASE
- DBCC CLONE DATABASE
- CLONEDATABASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database cloning [SQL Server]
- cloning databases
- clone databases
- cloning database
- clone database
- copying databases
- copy databases
- copying database
- copy database
- NO_STATISTICS option
- NO_QUERYSTORE option
- VERIFY_CLONEDB option
- BACKUP_CLONEDB option
- database copying [SQL Server]
- database cloning [SQL Server]
- DBCC CLONEDATABASE statement
ms.assetid: ''
author: bluefooted
ms.author: pamela
manager: amitban
ms.openlocfilehash: 2de4f0e84b39d1384e342eab3b7b3d0bfd101611
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172579"
---
# <a name="dbcc-clonedatabase-transact-sql"></a>DBCC CLONEDATABASE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

使用 DBCC CLONEDATABASE 生成仅限架构的克隆数据库，以调查与查询优化器相关的性能问题。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DBCC CLONEDATABASE   
(  
    source_database_name
    ,  target_database_name
)
    [ WITH { [ NO_STATISTICS ] [ , NO_QUERYSTORE ] [ , VERIFY_CLONEDB | SERVICEBROKER ] [ , BACKUP_CLONEDB ] } ]     
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*source_database_name*  
要复制的数据库的名称。 
  
*target_database_name*  
源数据库要复制到的数据库的名称。 此数据库由 DBCC CLONEDATABASE 创建，创建之前不应存在。 
  
NO_STATISTICS  
指定是否需要将表/索引统计信息排除在克隆范围之外。 如果未指定此选项，表/索引统计信息自动包含在克隆范围之内。 从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3 和 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 开始提供此选项。

NO_QUERYSTORE<br>
指定是否需要将查询存储数据排除在克隆范围之外。 如果未指定此选项，在源数据库中启用了查询存储的情况下，系统会将查询存储数据复制到克隆中。 从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 开始提供此选项。

VERIFY_CLONEDB  
检查新数据库的一致性。  如果打算将克隆数据库用于生产，则需要此选项。  启用 VERIFY_CLONEDB 还会禁用统计信息和查询存储集合，因此它等效于运行 WITH VERIFY_CLONEDB、NO_STATISTICS 和 NO_QUERYSTORE。  从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3、[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 和[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU8 开始提供此选项。

> [!NOTE]  
> 可以使用下面的命令来确认克隆数据库已为生产准备就绪： <br/>`SELECT DATABASEPROPERTYEX('clone_database_name', 'IsVerifiedClone')`


SERVICEBROKER<br>
指定是否应将与 Service Broker 相关的系统目录包含在克隆范围之内。  SERVICEBROKER 选项不能与 VERIFY_CLONEDB 结合使用。  从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3、[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 和[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU8 开始提供此选项。

BACKUP_CLONEDB  
创建并验证克隆数据库的备份。  与 VERIFY_CLONEDB 配合使用时，系统先验证克隆数据库，再进行备份。  从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3、[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 和[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU8 开始提供此选项。
  
## <a name="remarks"></a>备注
DBCC CLONEDATABASE 可执行以下验证。 如果任何验证失败，则该命令失败。
- 源数据库必须是用户数据库。 不允许克隆系统数据库（主数据库、模型数据库、msdb 数据库、tempdb 数据库和分发数据库等）。
- 源数据库必须处于联机状态或可读取。
- 不得存在与克隆数据库使用相同名称的数据库。
- 该命令不在用户事务中。

如果所有验证均成功，则通过下列操作克隆源数据库：
- 根据模型数据库创建一个新的目标数据库，该数据库的文件布局与源数据库相同，但采用的是默认文件大小。
- 创建源数据库的内部快照。
- 将系统元数据从源数据库复制到目标数据库。
- 将所有对象的所有架构从源数据库复制到目标数据库。
- 将所有索引的统计信息从源数据库复制到目标数据库。

> [!NOTE]  
> 通过 DBCC CLONEDATABASE 生成的新数据库主要用于故障排除和诊断。  必须使用 VERIFY_CLONEDB 选项，才能将克隆数据库用作生产数据库。

目标数据库中的所有文件将继承模型数据库的大小和增长设置。 目标数据库的文件名将遵循 source_file_name _underscore_random 编号约定。 如果目标文件夹中已存在生成的文件名，DBCC CLONEDATABASE 将失败。

如果模型数据库中存在以上部分创建的任何用户对象（表、索引、架构、角色等），则 DBCC CLONEDATABASE 不支持创建克隆。 如果模型数据库中存在用户对象，数据库克隆将失败，并显示以下错误消息：

```
Msg 2601, Level 14, State 1, Line 1
Cannot insert duplicate key row in object <system table> with unique index 'index name'. The duplicate key value is <key value>   
```

> [!IMPORTANT]
> 如果有列存储索引，请参阅[在克隆数据库上使用列存储索引优化查询的注意事项](https://techcommunity.microsoft.com/t5/SQL-Server/Considerations-when-tuning-your-queries-with-columnstore-indexes/ba-p/385294)对列存储索引进行更新，然后再运行 DBCC CLONEDATABASE 命令。  自 SQL Server 2019 起，上文中所述的手动步骤将不再是必需的，因为 **DBCC CLONEDATABASE** 命令会自动收集此信息。

<a name="ctp23"></a>

## <a name="stats-blob-for-columnstore-indexes"></a>列存储索引的统计信息 blob

[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]，`DBCC CLONEDATABASE` 自动捕获列存储索引的统计信息 blob，因此不再需要任何手动步骤。`DBCC CLONEDATABASE` 创建仅限架构的数据库副本，其中包括在不复制数据的情况下对查询性能问题进行故障排除所需的所有元素。 在以前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，该命令不会复制所需的统计信息来准确地对列存储索引查询进行故障排除，且需要手动步骤来捕获此信息。

有关克隆数据库的数据安全的信息，请参阅[了解克隆数据库中的数据安全](https://techcommunity.microsoft.com/t5/SQL-Server/Understanding-data-security-in-cloned-databases-created-using/ba-p/385287)。

## <a name="internal-database-snapshot"></a>内部数据库快照
DBCC CLONEDATABASE 使用源数据库的内部数据库快照来实现执行复制所需的事务一致性。 使用此快照可防止在执行这些命令时出现阻塞和并发问题。 如果无法创建快照，DBCC CLONEDATABASE 将失败。 

在复制过程的以下步骤期间，将锁定数据库级别：
- 验证源数据库
- 对源数据库进行 S 锁定
- 创建源数据库的快照
- 创建克隆数据库（从模型数据库继承的空数据库）
- 对克隆数据库进行 X 锁定
- 将元数据复制到克隆数据库
- 解除所有 DB 锁定

命令运行完成后，系统会立即删除内部快照。 克隆数据库上的 TRUSTWORTHY 和 DB_CHAINING 选项处于关闭状态。 

## <a name="supported-objects"></a>支持的对象
目标数据库中仅可克隆下列对象。 加密对象可以克隆，但无法在克隆数据库中使用。 不支持克隆未在以下部分列出的任何对象： 
- APPLICATION ROLE
- AVAILABILITY GROUP
- COLUMNSTORE INDEX
- CDB
- CDC
- CLR（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3、[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 和更高版本开始支持）
- DATABASE PROPERTIES
- DEFAULT
- FILES AND FILEGROUPS
- 全文（从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 CU2 开始支持）
- FUNCTION
- INDEX
- LOGIN
- PARTITION FUNCTION
- PARTITION SCHEME
- PROCEDURE   
> [!NOTE]   
> 从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 开始，所有版本均支持 [!INCLUDE[tsql](../../includes/tsql-md.md)] 过程。 从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3 开始支持 CLR 过程。 从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 开始支持本机编译过程。  

- QUERY STORE（从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 开始支持）   
> [!NOTE]   
> 仅当源数据库上启用了查询存储数据时，才复制查询存储数据。 若要将最新的运行时统计信息作为查询存储的一部分进行复制，请执行 sp_query_store_flush_db，将运行时统计信息刷新至查询存储，然后再执行 DBCC CLONEDATABASE。  

- ROLE
- RULE
- SCHEMA
- SEQUENCE
- SPATIAL INDEX
- STATISTICS
- SYNONYM
- TABLE
- MEMORY OPTIMIZED TABLES（仅限 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 和更高版本）。
- FILESTREAM AND FILETABLE OBJECTS（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3、[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 及更高版本开始支持）。 
- TRIGGER
- TYPE
- UPGRADED DB
- USER
- VIEW
- XML INDEX
- XML SCHEMA COLLECTION  

## <a name="permissions"></a>权限  
要求具有 **sysadmin** 固定服务器角色的成员身份。

## <a name="error-log-messages"></a>错误日志消息
以下消息是克隆过程中记录在错误日志中的消息示例：

```
2018-03-26 15:33:56.05 spid53 Database cloning for 'sourcedb' has started with target as 'sourcedb_clone'.

2018-03-26 15:33:56.46 spid53 Starting up database 'sourcedb_clone'.

2018-03-26 15:33:57.80 spid53 Setting database option TRUSTWORTHY to OFF for database 'sourcedb_clone'.

2018-03-26 15:33:57.80 spid53 Setting database option DB_CHAINING to OFF for database 'sourcedb_clone'.

2018-03-26 15:33:57.88 spid53 Starting up database 'sourcedb_clone'.

2018-03-26 15:33:57.91 spid53 Database 'sourcedb_clone' is a cloned database. A cloned database should be used for diagnostic purposes only and is not supported for use in a production environment.

2018-03-26 15:33:57.92 spid53 Database cloning for 'sourcedb' has finished. Cloned database is 'sourcedb_clone'.
```

## <a name="database-properties"></a>数据库属性
如果使用 DBCC CLONEDATABASE 生成数据库，`DATABASEPROPERTYEX('dbname', 'IsClone')` 将返回 1。

如果使用 WITH VERIFY_CLONEDB 验证数据库成功，`DATABASEPROPERTYEX('dbname', 'IsVerifiedClone')` 将返回 1。

## <a name="examples"></a>示例  
  
### <a name="a-creating-a-clone-of-a-database-that-includes-schema-statistics-and-query-store"></a>A. 创建包含架构、统计信息和查询存储的克隆数据库 
下面的示例创建 AdventureWorks 数据库的克隆，该数据库包含架构、统计信息和查询存储数据（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 及更高版本）

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone);    
GO 
```  
  
### <a name="b-creating-a-schema-only-clone-of-a-database-without-statistics"></a>B. 创建不含统计信息的仅限架构的克隆数据库 
下面的示例创建 AdventureWorks 数据库的克隆，该数据库不含统计信息（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3 及更高版本）

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH NO_STATISTICS;    
GO 
```  

### <a name="c-creating-a-schema-only-clone-of-a-database-without-statistics-and-query-store"></a>C. 创建不含统计信息和查询存储的仅限架构的克隆数据库 
下面的示例创建 AdventureWorks 数据库的克隆，其中不含统计信息和查询存储数据（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 及更高版本）

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH NO_STATISTICS, NO_QUERYSTORE;    
GO 
```  

### <a name="d-creating-a-clone-of-a-database-that-is-verified-for-production-use"></a>D. 创建经验证可用于生产的克隆数据库
下面的示例创建 AdventureWorks 数据库的克隆，该数据库仅限架构、不含统计信息和查询存储数据，并且经验证可用作生产数据库（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 及更高版本）。

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH VERIFY_CLONEDB;    
GO 
```  
  
### <a name="e-creating-a-clone-of-a-database-that-is-verified-for-production-use-that-includes-a-backup-of-the-cloned-database"></a>E. 创建经验证可用于生产且包含克隆数据库的备份的克隆数据库
下面的示例创建 AdventureWorks 数据库的克隆，该克隆数据库仅限架构、不含统计信息和查询存储数据，并且经验证可用作生产数据库。  还将创建经验证的克隆数据库的备份（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 及更高版本）。

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH VERIFY_CLONEDB, BACKUP_CLONEDB;    
GO 
```

## <a name="see-also"></a>另请参阅
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)    
[如何生成所需数据库元数据的脚本以在 SQL Server 中创建仅限统计信息的数据库](https://support.microsoft.com/help/914288)   


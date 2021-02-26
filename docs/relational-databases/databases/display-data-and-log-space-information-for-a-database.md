---
title: 显示数据库的数据和日志空间信息
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中显示数据库的数据和日志空间信息。
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], space
- status information [SQL Server], space
- displaying space information
- disk space [SQL Server], displaying
- databases [SQL Server], space used
- viewing space information
- space allocation [SQL Server], displaying
- data space [SQL Server]
ms.assetid: c7b99463-4bab-4e9b-9217-fcb0898dc757
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 648e33d685e7c97a7e900fd752402ed135fb9ecb
ms.sourcegitcommit: c821c2bdc383a84e45bbdc95ff6fbabf4f54901c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/17/2021
ms.locfileid: "100560945"
---
# <a name="display-data-and-log-space-information-for-a-database"></a>显示数据库的数据和日志空间信息
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中显示数据库的数据和日志空间信息。  

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 执行 **sp_spaceused** 的权限授予 **public** 角色。 只有 db_owner 固定数据库角色的成员可以指定 **updateusage 参数\@** 。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-display-data-and-log-space-information-for-a-database"></a>若要显示数据库的数据和日志空间信息  
  
1. 在对象资源管理器中，连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例，然后展开该实例。  
  
2. 展开 **“数据库”** 。  
  
3. 右键单击某数据库，依次指向“报表”和“标准报表”，然后单击“磁盘使用情况”。  

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL

#### <a name="to-display-data-and-log-space-information-for-a-database-by-using-sp_spaceused"></a>使用 sp_spaceused 显示数据库的数据和日志空间信息
  
1. 连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2. 在标准菜单栏上，单击 **“新建查询”** 。  
  
3. 将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 该示例使用 [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md) 系统存储过程报告整个数据库（表和索引）的磁盘空间信息。  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_spaceused;  
GO  
```  

#### <a name="to-display-data-space-used-by-object-and-allocation-unit-for-a-database"></a>显示数据库的对象和分配单元使用的数据空间
  
1. 连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2. 在标准菜单栏上，单击 **“新建查询”** 。  
  
3. 将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例查询[对象目录视图](../system-catalog-views/object-catalog-views-transact-sql.md)，以报告每个表和每个表内每个[分配单元](../pages-and-extents-architecture-guide.md#IAM)的磁盘空间使用情况。  
  
```sql  
SELECT
  t.object_id,
  OBJECT_NAME(t.object_id) ObjectName,
  sum(u.total_pages) * 8 Total_Reserved_kb,
  sum(u.used_pages) * 8 Used_Space_kb,
  u.type_desc,
  max(p.rows) RowsCount
FROM
  sys.allocation_units u
  join sys.partitions p on u.container_id = p.hobt_id
  join sys.tables t on p.object_id = t.object_id
GROUP BY
  t.object_id,
  OBJECT_NAME(t.object_id),
  u.type_desc
ORDER BY
  Used_Space_kb desc,
  ObjectName
```  

#### <a name="to-display-data-and-log-space-information-for-a-database-by-querying-sysdatabase_files"></a>通过查询 sys.database_files 显示数据库的数据和日志空间信息  
  
1. 连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2. 在标准菜单栏上，单击 **“新建查询”** 。  
  
3. 将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例查询 [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) 目录视图以便返回与 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中的数据和日志文件有关的特定信息。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT file_id, name, type_desc, physical_name, size, max_size  
FROM sys.database_files ;  
GO  
  
```  
  
## <a name="see-also"></a>另请参阅

 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sp_spaceused (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [向数据库中添加数据文件或日志文件](../../relational-databases/databases/add-data-or-log-files-to-a-database.md)   
 [删除数据库中的数据文件或日志文件](../../relational-databases/databases/delete-data-or-log-files-from-a-database.md)  

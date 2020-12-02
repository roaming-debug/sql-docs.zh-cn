---
title: 删除失效文件组 (SQL Server) | Microsoft Docs
description: 本文演示如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中删除失效文件组。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- piecemeal restores [SQL Server], defunct filegroups
- defunct filegroups
- restoring filegroups [SQL Server]
- deferred transactions
- filegroups [SQL Server], defunct
- unrestored filegroups
ms.assetid: 055f9c6a-5c18-4942-98e7-ec918f0ff975
author: cawrites
ms.author: chadam
ms.openlocfilehash: 37c305f6901819ade1c30c2bc71c9d4cb11f6ce5
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130309"
---
# <a name="remove-defunct-filegroups-sql-server"></a>删除失效文件组 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中删除失效的文件组。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
-   [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **若要删除失效的文件组，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   本主题与包含多个文件或文件组的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库相关；在简单模式下，仅与包含只读文件组的数据库相关。  
  
-   删除离线文件组后，文件组中的所有文件都将失效。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   如果未还原的文件组再也不用还原了，您可以将该文件组从数据库中删除，从而使文件组 *失效* 。 失效文件组再也不能还原到此数据库，但其元数据仍然保留。 文件组失效后，可以重新启动数据库，恢复数据库会使数据库在还原的文件组中保持一致。  
  
     例如，可以选择让文件组失效来解决由于数据库中不再需要的脱机文件组而导致的事务延迟。 这样的文件组失效之后，由于这些文件组的脱机而延迟的事务将脱离延迟状态。 有关详细信息，请参阅 [延迟的事务 (SQL Server)](../../relational-databases/backup-restore/deferred-transactions-sql-server.md)中删除失效的文件组。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对数据库拥有 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-remove-defunct-filegroups"></a>删除失效的文件组  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例，然后展开该实例。  
  
2.  展开 **“数据库”** ，右键单击要从其中删除文件的数据库，再单击 **“属性”** 。  
  
3.  选择 **“文件”** 页。  
  
4.  在 **“数据库文件”** 网格中，选择要删除的文件，单击 **“删除”** ，然后单击 **“确定”** 。  
  
5.  选择 **“文件组”** 页。  
  
6.  在 **“行”** 网格中，选择要删除的文件组，单击 **“删除”** ，然后单击 **“确定”** 。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-remove-defunct-filegroups"></a>删除失效的文件组  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。 （注意：  此示例假定文件和文件组已存在。 若要创建这些对象，请参阅 [ALTER DATABASE 和文件组选项](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)主题中的示例 B。）第一个示例通过使用具有 `test1dat3` 子句的 `test1dat4` 语句，从失效的文件组中删除 `ALTER DATABASE` 和 `REMOVE FILE` 文件。 第二个示例通过使用 `Test1FG1`子句，删除失效的文件组 `REMOVE FILEGROUP` 。  
  
```sql  
USE master;  
GO  
ALTER DATABASE AdventureWorks2012  
REMOVE FILE test1dat3 ;  
ALTER DATABASE AdventureWorks2012  
REMOVE FILE test1dat4 ;  
GO  
  
```  
  
```sql  
USE master;  
GO  
ALTER DATABASE AdventureWorks2012  
REMOVE FILEGROUP Test1FG1 ;  
GO  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE 文件和文件组选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)   
 [延迟的事务 (SQL Server)](../../relational-databases/backup-restore/deferred-transactions-sql-server.md)   
 [文件还原（完整恢复模式）](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)   
 [文件还原（简单恢复模式）](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)   
 [联机还原 (SQL Server)](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [还原页 (SQL Server)](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [段落还原 (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
  

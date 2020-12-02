---
title: 管理 suspect_pages 表 (SQL Server) | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中管理 suspect_pages 表。 产生特定错误的页十分可疑。
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- 824 (Database Engine error)
- restoring pages [SQL Server]
- pages [SQL Server], suspect
- pages [SQL Server], restoring
- suspect_pages system table
- suspect pages [SQL Server]
- restoring [SQL Server], pages
ms.assetid: f394d4bc-1518-4e61-97fc-bf184d972e2b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 16255027d2fa0617066fcff0d2a208e70c96a9f9
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96126900"
---
# <a name="manage-the-suspect_pages-table-sql-server"></a>管理 suspect_pages 表 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何使用 **或** 来在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中管理 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] suspect_pages [!INCLUDE[tsql](../../includes/tsql-md.md)]表。 **suspect_pages** 表可用来维护有关可疑页的信息，并且还有助于确定有无必要进行还原。 [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) 表位于 [msdb 数据库](../../relational-databases/databases/msdb-database.md)中。  
  
 如果 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 在试图读取某个数据页时遇到下列错误之一，该页面将被视为“可疑”：  
  
-   由操作系统发出的循环冗余检查 (CRC) 导致的 823 错误，如磁盘错误（某些硬件错误）  
  
-   824 错误，如页撕裂（任何逻辑错误）  
  
 每个可疑页的页 ID 将记录在 **suspect_pages** 表中。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将记录在常规处理过程中发现的所有可疑页，例如在下列情况下：  
  
-   查询必须读取页。  
  
-   在运行 DBCC CHECKDB 的过程中。  
  
-   在备份操作的过程中。  
  
 当执行还原操作、DBCC 修复操作或删除数据库操作时， **suspect_pages** 表也会根据需要进行更新。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **若要管理 suspect_pages 表，可使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   **suspect_pages 表中记录的错误**  
  
     在 **suspect_pages** 表中，由于出现 824 错误而失败的每页占一行，最多为 1,000 行。 下表显示了记录在 **suspect_pages** 表的 **event_type** 列中的错误。  
  
    |错误说明|**event_type** 值|  
    |-----------------------|---------------------------|  
    |由操作系统 CRC 错误造成的 823 错误，或者校验和错误或页撕裂以外的 824 错误（例如，页 ID 错误）|1|  
    |错误的校验和|2|  
    |残缺页|3|  
    |已还原（页在标记为错误后已还原）|4|  
    |已修复（DBCC 修复了页）|5|  
    |已由 DBCC 释放|7|  
  
     暂时性的错误也会记录在 **suspect_pages** 表中。  暂时性错误的来源包含 I/O 错误（例如电缆断开连接）或暂时未通过重复校验和测试的页。  
  
-   **数据库引擎如何更新 suspect_pages 表**  
  
     [!INCLUDE[ssDE](../../includes/ssde-md.md)] 对 **suspect_pages** 表执行下列操作：  
  
    -   如果表未满，则每出现一个 824 错误，该表都会更新以指明出现了错误，且错误计数器也将相应递增。 如果通过修复、还原或释放操作修复后的页仍有错误，则其 **number_of_errors** 计数将会递增，其 **last_update** 列也会更新  
  
    -   列出的页通过还原或修复操作修复之后，该操作将更新 **suspect_pages** 行，以指示此页已修复 (**event_type** = 5) 或已还原 (**event_type** = 4)。  
  
    -   如果运行 DBCC 检查，则该检查会将所有未出错页标记为已修复 (**event_type** = 5) 或已释放 (**event_type** = 7)。  
  
-   **自动更新 suspect_pages 表**  
  
     尝试读取数据文件中的某一页由于以下原因之一失败后，数据库镜像伙伴或 AlwaysOn 可用性副本将更新 **suspect_pages** 表。  
  
    -   由操作系统 CRC 错误导致的 823 错误。  
  
    -   824 错误（像页撕裂这样的逻辑损坏）。  
  
     以下操作还自动更新 **suspect_pages** 表中的行。  
  
    -   DBCC CHECKDB REPAIR_ALLOW_DATA_LOSS 更新 **suspect_pages** 表，以指示已释放或已修复的各页。  
  
    -   完整还原、文件还原或页面还原将页面项标记为已还原。  
  
     以下操作将自动从 **suspect_pages** 表中删除行。  
  
    -   ALTER DATABASE REMOVE FILE  
  
    -   DROP DATABASE  
  
-   **数据库管理员的维护角色**  
  
     数据库管理员负责管理表（主要通过删除旧的行实现）。 **suspect_pages** 表有大小限制，如果此表已满，则不会记录新的错误。 若要防止此表填满，数据库管理员或系统管理员必须通过删除行来手动清除此表中的旧条目。 因此，我们建议你定期删除或归档具有已还原或已修复的 **event_type** 的行或者具有旧的 **last_update** 值的行。  
  
     若要监视对 suspect_pages 表执行的操作，可使用 [Database Suspect Data Page 事件类](../../relational-databases/event-classes/database-suspect-data-page-event-class.md)。 有时，行会因暂时性错误而添加到 **suspect_pages** 表。 如果正在向该表添加很多行，则 I/O 子系统可能出了问题。 如果您注意到正向该表添加的行数突然增加，我们建议您检查一下 I/O 子系统是不是出现了问题。  
  
     数据库管理员还可以插入或更新记录。 例如，如果数据库管理员知道某个特定的可疑页实际上没问题但想要暂时保留记录，更新行可能就很有用。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 任何拥有 **msdb** 访问权限的人员都可以读取 **suspect_pages** 表中的数据。 任何拥有 suspect_pages 表的 UPDATE 权限的人员都可以更新它的记录。 **msdb** 上的 **db_owner** 固定数据库角色或 **sysadmin** 固定服务器角色的成员都可以插入、更新和删除记录。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-manage-the-suspect_pages-table"></a>管理 suspect_pages 表  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的实例，再依次展开该实例、 **“数据库”** 。  
  
2.  依次展开“系统数据库”  、“msdb”  、“表”  和“系统表”  。  
  
3.  展开“dbo.suspect_pages”  ，然后右键单击“编辑前 200 行”  。  
  
4.  在查询窗口中，编辑、更新或删除所需的行。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-manage-the-suspect_pages-table"></a>管理 suspect_pages 表  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击 **“执行”** 。 此示例将删除 `suspect_pages` 表中的一些行。  
  
```  
-- Delete restored, repaired, or deallocated pages.  
DELETE FROM msdb..suspect_pages  
   WHERE (event_type = 4 OR event_type = 5 OR event_type = 7);  
GO  
  
```  
  
 此示例将返回 `suspect_pages` 表中的错误网页。  
  
```  
-- Select nonspecific 824, bad checksum, and torn page errors.  
SELECT * FROM msdb..suspect_pages  
   WHERE (event_type = 1 OR event_type = 2 OR event_type = 3);  
GO  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [DROP DATABASE (Transact SQL)](../../t-sql/statements/drop-database-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)   
 [还原页 (SQL Server)](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [suspect_pages (Transact-SQL)](../../relational-databases/system-tables/suspect-pages-transact-sql.md)   
    
   
  
  




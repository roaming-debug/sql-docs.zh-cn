---
title: 收缩数据库 | Microsoft Docs
description: 了解如何通过 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中使用对象收缩数据库。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.swb.shrinkdatabase.f1
helpviewer_keywords:
- shrinking databases
- databases [SQL Server], shrinking
- decreasing database size
- database shrinking [SQL Server]
- reducing database size
ms.assetid: 83afbf74-fd50-4c39-831c-b1f473a50620
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 94fb9ab933a4bd2ed7d42f1b43f86a48df6b3b35
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98812998"
---
# <a name="shrink-a-database"></a>收缩数据库
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中使用对象收缩数据库。  
  
 收缩数据文件通过将数据页从文件末尾移动到更靠近文件开头的未占用的空间来恢复空间。 在文件末尾创建足够的可用空间后，可以取消对文件末尾的数据页的分配并将它们返回给文件系统。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **收缩数据库，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **跟进：** [收缩数据库](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   收缩后的数据库不能小于数据库的最小大小。 最小大小是在数据库最初创建时指定的大小，或是上一次使用文件大小更改操作（如 DBCC SHRINKFILE）设置的显式大小。 例如，如果数据库最初创建时的大小为 10 MB，后来增长到 100 MB，则该数据库最小只能收缩到 10 MB，即使已经删除数据库的所有数据也是如此。  
  
-   不能在备份数据库时收缩数据库。 反之，也不能在数据库执行收缩操作时备份数据库。
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   若要查看数据库中当前的可用（未分配）空间量。 有关详细信息，请参阅 [显示数据库的数据和日志空间信息](../../relational-databases/databases/display-data-and-log-space-information-for-a-database.md)  
  
-   当您计划收缩数据库时，请考虑以下信息：  
  
    -   在执行会产生许多未使用空间的操作（如截断表或删除表操作）后，执行收缩操作最有效。  
  
    -   大多数数据库都需要一些可用空间，以供常规日常操作使用。 如果反复收缩数据库并注意到数据库大小变大，则表明收缩的空间是常规操作所必需的。 在这种情况下，反复收缩数据库是一种无谓的操作。  
  
    -   收缩操作不会保留数据库中索引的碎片状态，通常还会在一定程度上增加碎片。 这是不要反复收缩数据库的另一个原因。  
  
    -   除非有特定要求，否则不要将 AUTO_SHRINK 数据库选项设置为 ON。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求具有 **sysadmin** 固定服务器角色或 **db_owner** 固定数据库角色的成员身份。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-shrink-a-database"></a>收缩数据库  
  
1.  在 **对象资源管理器** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的实例，然后展开该实例。  
  
2.  展开“数据库”，再右键单击要收缩的数据库。  
  
3.  指向 **“任务”** ，指向 **“收缩”** ，然后单击 **“数据库”** 。  
  
     **Database**  
     显示所选数据库的名称。  
  
     **当前分配的空间**  
     显示所选数据库的总已用空间和未使用空间。  
  
     **可用空间**  
     显示所选数据库的日志和数据文件中可用空间的总和。  
  
     **在释放未使用的空间前重新组织文件**  
     选择此选项等效于执行指定了目标百分比选项的 DBCC SHRINKDATABASE。 清除此选项等效于执行带 TRUNCATEONLY 选项的 DBCC SHRINKDATABASE。 在打开对话框时，默认情况下不选择此选项。 如果选择此选项，用户必须指定目标百分比选项。  
  
     **收缩后文件中的最大可用空间**  
     输入在数据库收缩后数据库文件中剩余可用空间的最大百分比。 值可以介于 0 和 99 之间。  
  
4.  单击“确定”。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-shrink-a-database"></a>收缩数据库  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此实例使用 [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md) 减少 `UserDB` 数据库中数据文件和日志文件的大小并允许数据库中有 `10` ％ 的可用空间。  
  
 [!code-sql[DBCC#DBCC_SHRINKDB1](../../relational-databases/databases/codesnippet/tsql/shrink-a-database_1.sql)]  
  
##  <a name="follow-up-after-you-shrink-a-database"></a><a name="FollowUp"></a> 跟进：在收缩数据库之后  
 被移动用来收缩文件的数据可以分布到文件的任何可用位置。 这将导致索引碎片并使搜索索引范围的查询变慢。 若要消除碎片，请考虑在收缩后重新生成文件的索引。  
  
## <a name="see-also"></a>另请参阅  
 [收缩文件](../../relational-databases/databases/shrink-a-file.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)   
 [DBCC SHRINKFILE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)   
 [数据库文件和文件组](../../relational-databases/databases/database-files-and-filegroups.md)  
  
  

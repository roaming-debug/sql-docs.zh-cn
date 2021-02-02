---
title: 收缩文件 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中收缩数据或日志文件。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.swb.shrinkfile.f1
helpviewer_keywords:
- shrinking files
- decreasing file size
- databases [SQL Server], shrinking
- reducing file size
- size [SQL Server], files
- file size [SQL Server]
ms.assetid: ce5c8798-c039-4ab2-81e7-90a8d688b893
author: stevestein
ms.author: sstein
ms.openlocfilehash: 077f7d98470c385b8333fc9db2f55d04bd13bb44
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98812972"
---
# <a name="shrink-a-file"></a>收缩文件
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何通过使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中收缩数据或日志文件。  
  
 收缩数据文件通过将数据页从文件末尾移动到更靠近文件开头的未占用的空间来恢复空间。 在文件末尾创建足够的可用空间后，可以取消对文件末尾的数据页的分配并将它们返回给文件系统。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **若要收缩数据或日志文件，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   主数据文件不能收缩到小于 model 数据库中的主文件的大小。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   被移动用来收缩文件的数据可以分布到文件的任何可用位置。 这将导致索引碎片并使搜索索引范围的查询变慢。 若要消除碎片，请考虑在收缩后重新生成文件的索引。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求具有 **sysadmin** 固定服务器角色或 **db_owner** 固定数据库角色的成员身份。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-shrink-a-data-or-log-file"></a>收缩数据或日志文件  
  
1.  在对象资源管理器中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例，然后展开该实例。  
  
2.  展开 **“数据库”** ，再右键单击要收缩的数据库。  
  
3.  依次指向 **“任务”** 和 **“收缩”** ，再单击 **“文件”** 。  
  
     **Database**  
     显示所选数据库的名称。  
  
     **文件类型**  
     选择文件的文件类型。 可用的选项包括 **“数据”** 和 **“日志”** 文件。 默认选项为 **“数据”** 。 选择不同的文件组类型，其他字段中的选项会相应地发生更改。  
  
     **文件组**  
     在与以上所选的 **“文件类型”** 相关联的文件组列表中选择文件组。 选择不同的文件组，其他字段中的选项会相应地发生更改。  
  
     **文件名**  
     从所选文件组和文件类型的可用文件列表中选择文件。  
  
     **位置**  
     显示当前所选文件的完整路径。 此路径无法编辑，但是可以复制到剪贴板。  
  
     **当前分配的空间**  
     对于数据文件，会显示当前分配的空间。 对于日志文件，会显示根据 DBCC SQLPERF (LOGSPACE) 的输出计算出的当前分配的空间。  
  
     **可用空间**  
     对于数据文件，会显示根据 SHOWFILESTATS (fileid) 的输出计算出的当前可用空间。 对于日志文件，会显示根据 DBCC SQLPERF (LOGSPACE) 的输出计算出的当前可用空间。  
  
     **释放未使用的空间**  
     将任何文件中未使用的空间释放给操作系统，并将文件收缩到最后分配的区，因此无需移动任何数据即可减小文件尺寸。 不会将行重新定位到未分配的页。  
  
     **在释放未使用的空间前重新组织页**  
     等效于执行用于指定目标文件大小的 DBCC SHRINKFILE。 选中此选项时，用户必须在 **“将文件收缩到”** 框中指定目标文件的大小。  
  
     **“将文件收缩到”**  
     为收缩操作指定目标文件的大小。 此大小值不得小于当前分配的空间或大于为文件分配的全部区的大小。 如果输入的值超出最小值或最大值，那么一旦焦点改变或单击工具栏上的按钮时，数值将恢复到最小值或最大值。  
  
     **通过将数据迁移到同一文件组中的其他文件来清空文件**  
     从指定文件迁移所有数据。 此选项允许使用 ALTER DATABASE 语句删除文件。 此选项等效于执行带有 EMPTYFILE 选项的 DBCC SHRINKFILE。  
  
4.  选择文件类型和文件名。  
  
5.  根据需要，选中 **“释放未使用的空间”** 复选框。  
  
     选中此选项后，将为操作系统释放文件中所有未使用的空间，并将文件收缩到上次分配的区。 这将减小文件的大小，但不移动任何数据。  
  
6.  根据需要，可以选中 **“在释放未使用的空间前重新组织文件”** 复选框。 如果选中此选项，则必须指定 **“将文件收缩到”** 值。 默认情况下，该选项为清除状态。  
  
     选中此选项后，将为操作系统释放文件中所有未使用的空间，并尝试将行重新定位到未分配页。  
  
7.  根据需要，输入在收缩数据库后数据库文件中要保留的最大可用空间百分比。 值可以介于 0 和 99 之间。 只有启用 **“在释放未使用的空间前重新组织文件”** 以后，此选项才可用。  
  
8.  根据需要，选中 **“通过将数据迁移到同一文件组中的其他文件来清空文件”** 复选框。  
  
     选中此选项后，将指定文件中的所有数据移至同一文件组中的其他文件中。 然后就可以删除空文件。 此选项与执行包含 EMPTYFILE 选项 DBCC SHRINKFILE 相同。  
  
9. 单击“确定”。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-shrink-a-data-or-log-file"></a>收缩数据或日志文件  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例使用 [DBCC SHRINKFILE](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md) 将 `UserDB` 数据库中名为 `DataFile1` 的数据文件的大小收缩到 7 MB。  
  
 [!code-sql[DBCC#DBCC_SHRINKFILE1](../../relational-databases/databases/codesnippet/tsql/shrink-a-file_1.sql)]  
  
## <a name="see-also"></a>另请参阅  
 [DBCC SHRINKDATABASE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md)   
 [收缩数据库](../../relational-databases/databases/shrink-a-database.md)   
 [删除数据库中的数据文件或日志文件](../../relational-databases/databases/delete-data-or-log-files-from-a-database.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
  
  

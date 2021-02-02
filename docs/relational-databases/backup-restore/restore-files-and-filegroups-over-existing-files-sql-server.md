---
title: 在现有文件上还原文件（和文件组）
description: 本文演示如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中在现有文件上还原文件和文件组。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- restoring files [SQL Server], how-to topics
- restoring files [SQL Server], steps
- file restores [SQL Server], how-to topics
- filegroups [SQL Server], restoring
- restoring filegroups [SQL Server]
- overwriting filegroups
- overwriting files
ms.assetid: 517e07eb-9685-4b06-90af-b1cc496700b7
author: cawrites
ms.author: chadam
ms.openlocfilehash: cf2a0e3637d4fb2f62d26072ed70015d030d2059
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076079"
---
# <a name="restore-files-and-filegroups-over-existing-files-sql-server"></a>在现有文件上还原文件和文件组 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中在现有文件上还原文件和文件组。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要在现有文件上还原文件和文件组，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   还原文件和文件组的系统管理员必须是当前使用要还原数据库的唯一人员。  
  
-   不允许在显式或隐式事务中使用 RESTORE。  
  
-   在完整恢复模式或大容量日志恢复模式下，必须先备份活动事务日志（称为日志尾部），然后才能还原文件。 有关详细信息，请参阅 [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)数据库还原到一个新位置并且可以选择重命名该数据库。  
  
-   若要还原已加密的数据库，您必须有权访问用于对数据库进行加密的证书或非对称密钥。 如果没有证书或非对称密钥，数据库将无法还原。 因此，只要需要该备份，就必须保留用于对数据库加密密钥进行加密的证书。 有关详细信息，请参阅 [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 如果不存在要还原的数据库，则用户必须有 CREATE DATABASE 权限才能执行 RESTORE。 如果数据库存在，则 RESTORE 权限默认授予 **sysadmin** 和 **dbcreator** 固定服务器角色成员以及数据库的所有者 (**dbo**)（对于 FROM DATABASE_SNAPSHOT 选项，数据库始终存在）。  
  
 RESTORE 权限被授予那些成员身份信息始终可由服务器使用的角色。 因为只有在固定数据库可以访问且没有损坏时（在执行 RESTORE 时并不会总是这样）才能检查固定数据库角色成员身份，所以 **db_owner** 固定数据库角色成员没有 RESTORE 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-restore-files-and-filegroups-over-existing-files"></a>在现有文件上还原文件和文件组  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的实例，再依次展开该实例、 **“数据库”** 。  
  
2.  右键单击所需数据库，指向“任务”  ，再指向“还原”  ，然后单击“文件和文件组”  。  
  
3.  在 **“常规”** 页上的 **目标数据库** 列表框中，输入要还原的数据库。 您可以输入新的数据库，也可以从下拉列表中选择现有的数据库。 该列表包含了服务器上除系统数据库 **master** 和 **tempdb** 之外的所有数据库。  
  
4.  若要指定要还原的备份集的源和位置，请单击以下选项之一：  
  
    -   **源数据库**  
  
         在列表框中输入数据库名称。 此列表仅包含根据 **msdb** 备份历史记录已进行过备份的数据库。  
  
    -   **源设备**  
  
         单击浏览按钮。 在 **“指定备份设备”** 对话框的 **“备份介质类型”** 列表框中，选择列出的设备类型之一。 若要为 **“备份介质”** 列表框选择一个或多个设备，请单击 **“添加”** 。  
  
         将所需设备添加到 **“备份介质”** 列表框后，单击 **“确定”** 返回到 **“常规”** 页。  
  
5.  在 **“选择用于还原的备份集”** 网格中，选择用于还原的备份。 此网格将显示对于指定位置可用的备份。 默认情况下，系统会推荐一个恢复计划。 若要覆盖建议的恢复计划，可以更改网格中的选择。 任何依赖于已取消选择备份的备份，也将被自动取消选择。  
  
    |列标题|值|  
    |-----------------|------------|  
    |**还原**|选中的复选框指示要还原的备份集。|  
    |**名称**|备份集的名称。|  
    |**文件类型**|指定备份中数据的类型：数据、日志，或 Filestream 数据    。 包含在表中的数据备份在 **“数据”** 文件中。 事务日志数据备份在 **“日志”** 文件中。 存储在文件系统上的二进制大型对象 (BLOB) 数据备份在 **Filestream 数据** 文件中。|  
    |类型 |执行的备份类型：“完整”、“差异”或“事务日志”    。|  
    |**Server**|执行备份操作的数据库引擎实例的名称。|  
    |**文件逻辑名称**|文件的逻辑名称。|  
    |**Database**|备份操作中涉及的数据库的名称。|  
    |**开始日期**|备份操作开始的日期和时间，按客户端的区域设置显示。|  
    |**完成日期**|备份操作完成的日期和时间，按客户端的区域设置显示。|  
    |**大小**|备份集的大小（字节）。|  
    |**用户名**|执行备份操作的用户的名称。|  
  
6.  在 **“选择页”** 窗格中，单击 **“选项”** 页。  
  
7.  在“还原选项”  面板中，选择“覆盖现有数据库 (WITH REPLACE)”  。 还原操作将会覆盖所有现有数据库及其相关文件，即使已存在同名的其他数据库或文件。  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-restore-files-and-filegroups-over-existing-files"></a>在现有文件上还原文件和文件组  
  
1.  执行 RESTORE DATABASE 语句以还原文件和文件组备份，同时指定下列内容：  
  
    -   要还原的数据库的名称。  
  
    -   从中还原完整数据库备份的备份设备。  
  
    -   每个要还原文件的 FILE 子句。  
  
    -   每个要还原文件组的 FILEGROUP 子句。  
  
    -   REPLACE 选项，用来指定可以在具有相同名称和位置的现有文件上还原每个文件。  
  
        > [!CAUTION]  
        >  请慎重使用 REPLACE 选项。 有关更多信息，请参见 。  
  
    -   NORECOVERY 选项。 如果在创建备份之后没有对文件进行修改，则指定 RECOVERY 子句。  
  
2.  如果在创建文件备份之后对文件进行了修改，则执行 RESTORE LOG 语句以应用事务日志备份，同时指定下列内容：  
  
    -   事务日志将应用到的数据库的名称。  
  
    -   要还原的事务日志备份的备份设备。  
  
    -   如果在应用当前事务日志备份之后还要应用其他事务日志备份，则指定 NORECOVERY 子句；否则指定 RECOVERY 子句。  
  
         事务日志备份（如果已应用）必须包含备份文件和文件组时的时间。  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 下面的示例将还原 `MyNwind` 数据库的文件和文件组，并替换任何具有相同名称的现有文件。 为了将数据库还原到当前时间，还将应用两个事务日志。  
  
```sql  
USE master;  
GO  
-- Restore the files and filesgroups for MyNwind.  
RESTORE DATABASE MyNwind  
   FILE = 'MyNwind_data_1',  
   FILEGROUP = 'new_customers',  
   FILE = 'MyNwind_data_2',  
   FILEGROUP = 'first_qtr_sales'  
   FROM MyNwind_1  
   WITH NORECOVERY,  
   REPLACE;  
GO  
-- Apply the first transaction log backup.  
RESTORE LOG MyNwind  
   FROM MyNwind_log1  
   WITH NORECOVERY;  
GO  
-- Apply the last transaction log backup.  
RESTORE LOG MyNwind  
   FROM MyNwind_log2  
   WITH RECOVERY;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [还原文件和文件组 (SQL Server)](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)   
 [通过备份和还原来复制数据库](../../relational-databases/databases/copy-databases-with-backup-and-restore.md)  
  
  

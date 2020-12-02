---
title: 查看逻辑备份设备的内容
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中查看逻辑备份设备的属性和内容。
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- displaying backup content
- viewing backup content
- database backups [SQL Server], viewing content
- backing up databases [SQL Server], viewing content
- backing up databases [SQL Server], properties
- displaying backup properties
- backup devices [SQL Server], viewing information
- viewing backup properties
- database backups [SQL Server], properties
ms.assetid: 3a309074-e816-454d-b6c3-fcfdde0cbf74
author: cawrites
ms.author: chadam
ms.openlocfilehash: dc251ab77129dd05e72129998fae3470ed691b5e
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96128910"
---
# <a name="view-the-properties-and-contents-of-a-logical-backup-device-sql-server"></a>查看逻辑备份设备的属性和内容 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题介绍如何通过使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 查看 [!INCLUDE[tsql](../../includes/tsql-md.md)]中逻辑备份设备的属性和内容。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要查看逻辑备份设备的属性和内容，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
 有关安全性的信息，请参阅 [RESTORE LABELONLY (Transact SQL)](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)。  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本中，获取有关备份集或备份设备的信息要求具有 CREATE DATABASE 权限。 有关详细信息，请参阅 [GRANT 数据库权限 (Transact-SQL)](../../t-sql/statements/grant-database-permissions-transact-sql.md)。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-view-the-properties-and-contents-of-a-logical-backup-device"></a>查看逻辑备份设备的属性和内容  
  
1.  连接到相应的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例之后，在“对象资源管理器”中，单击服务器名称以展开服务器树。  
  
2.  展开 **“服务器对象”** ，然后展开 **“备份设备”** 。  
  
3.  单击设备并右键单击“属性”  ，将打开“备份设备”  对话框。  
  
4.  **“常规”** 页将显示设备名称和目标，目标为磁带设备或者文件路径。  
  
5.  在 **“选择页”** 窗格中，单击 **“介质内容”** 。  
  
6.  以下属性面板中将显示右侧窗格：  
  
    -   **介质**  
  
         介质顺序信息（介质序列号、簇序列号和镜像标识符（如果存在））以及介质的创建日期和时间。  
  
    -   **介质集**  
  
         介质集信息：介质集名称和说明（如果有）以及介质集中的簇数。  
  
7.  **“备份集”** 网格显示了有关介质集内容的信息。  
  
> [!NOTE]  
>  有关详细信息，请参阅 [媒体内容页](../../relational-databases/backup-restore/backup-device-media-contents-page.md)。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-view-the-properties-and-contents-of-a-logical-backup-device"></a>查看逻辑备份设备的属性和内容  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  使用 [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) 语句。 此示例返回有关 `AdvWrks2008R2Backup` 逻辑备份设备的信息。  
  
```sql  
USE AdventureWorks2012 ;  
RESTORE LABELONLY  
   FROM AdvWrks2008R2Backup ;  
GO  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [backupfilegroup (Transact-SQL)](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupfile (Transact-SQL)](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupset (Transact-SQL)](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [backupmediaset (Transact-SQL)](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [backupmediafamily (Transact-SQL)](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [sp_addumpdevice (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)   
 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)  
  
  

---
title: 定义逻辑备份设备 - 磁盘
description: 本文演示如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中为磁带文件定义逻辑备份设备。
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backup devices [SQL Server], defining
- backup devices [SQL Server], disks
- disk backup devices [SQL Server]
- database backups [SQL Server], disks
- backing up databases [SQL Server], disks
ms.assetid: 86331d43-c738-4523-ae3d-7d6700348ed1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1f601cd1a2d6a69920bb818bcdccec92b46a2e43
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129272"
---
# <a name="define-a-logical-backup-device-for-a-disk-file-sql-server"></a>为磁盘文件定义逻辑备份设备 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中为磁盘文件定义逻辑备份设备。 逻辑设备是指向特定物理备份设备（磁盘文件或磁带机）的用户定义名称。  将备份写入备份设备后，便会初始化物理设备。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **若要为磁盘文件定义逻辑备份设备，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   逻辑设备名称在服务器实例上的所有逻辑备份设备中必须是唯一的。 若要查看现有逻辑设备名称，请查询 [sys.backup_devices](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md) 目录视图。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   我们建议备份磁盘应不同于数据库数据和日志的磁盘。 这是数据或日志磁盘出现故障时访问备份数据必不可少的。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求具有 **diskadmin** 固定服务器角色中的成员身份。  
  
 要求拥有写入磁盘的权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-define-a-logical-backup-device-for-a-disk-file"></a>为磁盘文件定义逻辑备份设备  
  
1.  连接到相应的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例之后，在“对象资源管理器”中，单击服务器名称以展开服务器树。  
  
2.  展开“服务器对象”  ，然后右键单击“备份设备”  。  
  
3.  单击 **“新建备份设备”** 。 将打开 **“备份设备”** 对话框。  
  
4.  输入设备名称。  
  
5.  若要确定目标位置，请单击 **“文件”** 并指定该文件的完整路径。  
  
6.  若要定义新设备，请单击 **“确定”** 。  
  
 若要备份至此新设备，请将该设备添加到“备份数据库”  （“常规”  ）对话框中的“备份到：”  字段。 有关详细信息，请参阅 [创建完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)中创建差异数据库备份。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-define-a-logical-backup-for-a-disk-file"></a>为磁盘文件定义逻辑备份  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。 此示例说明如何使用 [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md) 为磁盘文件定义逻辑备份设备。 下面的示例以物理名称 `mydiskdump`添加名为 `c:\dump\dump1.bak`的磁盘备份设备。  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_addumpdevice 'disk', 'mydiskdump', 'c:\dump\dump1.bak' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [sys.backup_devices (Transact-SQL)](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md)   
 [sp_addumpdevice (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)   
 [sp_dropdevice (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md)   
 [为磁带驱动器定义逻辑备份设备 (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)   
 [查看逻辑备份设备的属性和内容 (SQL Server)](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
  

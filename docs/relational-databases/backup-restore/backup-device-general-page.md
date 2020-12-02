---
title: 备份设备（“常规”页）| Microsoft Docs
description: 在 SQL Server 中，使用“常规”页可指定或查看逻辑备份设备的常规属性，包括指定设备名称。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.backupdevice.general.f1
ms.assetid: c557e37d-319e-4adb-a0c1-94189b15d2ac
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9ac10832856ca481d170cd4db79ff37eaecffaac
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130522"
---
# <a name="backup-device-general-page"></a>备份设备（“常规”页）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用 **“常规”** 页可指定或查看逻辑备份设备的常规属性。  
  
 **使用 SQL Server Management Studio 查看备份设备的内容**  
  
-   [查看备份磁带或文件的内容 (SQL Server)](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
-   [查看逻辑备份设备的属性和内容 (SQL Server)](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
## <a name="options"></a>选项  
 **设备名称**  
 查看现有逻辑备份设备的名称，或指定新逻辑备份设备的名称。  
  
 **磁带**  
 在“磁带”  列表中查看或选择目标磁带设备。 仅在运行 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例的计算机附连有磁带机时，此选项才可用。  
  
> [!NOTE]  
>  远程计算机中的磁带备份设备不是有效的备份目标。  
  
 **File**  
 查看现有逻辑备份设备的目标文件，或指定新逻辑备份设备的目标文件。  
  
-   对于现有的逻辑备份设备，将显示备份文件的路径。 **“文件”** 字段不可编辑，“浏览”按钮也不可用。  
  
-   对于新的逻辑备份设备，您必须提供为其定义逻辑备份设备的备份文件的路径。 此文件并不一定已经存在。  
  
     若要指定本地备份文件，可以单击 **“文件”** 文本框右侧的“浏览”按钮。 然后，在 **“定位数据库文件”** 对话框中，您可以导航到运行服务器实例的计算机中任意固定驱动器上的任意位置。 如果备份文件尚不存在，则必须在该对话框的 **“文件名”** 字段中输入要使用的文件名。  
  
     或者，也可以手动编辑 **“文件”** 字段，以覆盖默认路径、文件名和扩展名。 若要将远程文件指定为备份目标，请输入其完全限定的通用命名约定 (UNC) 名称。 有关详细信息，请参阅 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)。  
  
    > [!IMPORTANT]  
    >  通过网络备份数据时可能会出现网络错误；因此，建议您在完成备份后验证备份操作。 有关详细信息，请参阅 [RESTORE VERIFYONLY (Transact-SQL)](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)。  
  
## <a name="remarks"></a>备注  
 包含一个或多个备份设备的集合的备份构成一个介质集。 “介质集”  是“备份介质”（磁带或磁盘文件）的有序集合，使用固定类型和数量的备份设备向其写入一个或多个备份操作。 有关媒体集的信息，请参阅 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)实例的计算机附连有磁带机时，此选项才可用。  
  
 将介质集中的第一个备份写入逻辑备份设备时，将对与逻辑备份设备对应的物理备份设备进行初始化。 如果物理备份设备是尚不存在的文件，则此时将创建该文件。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [为磁盘文件定义逻辑备份设备 (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)  
  
-   [为磁带驱动器定义逻辑备份设备 (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)  
  
-   [将磁盘或磁带指定为备份目标 (SQL Server)](../../relational-databases/backup-restore/specify-a-disk-or-tape-as-a-backup-destination-sql-server.md)  
  
-   [删除备份设备 (SQL Server)](../../relational-databases/backup-restore/delete-a-backup-device-sql-server.md)  
  
-   [设置备份的过期日期 (SQL Server)](../../relational-databases/backup-restore/set-the-expiration-date-on-a-backup-sql-server.md)  
  
-   [查看备份磁带或文件的内容 (SQL Server)](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
-   [查看备份集中的数据文件和日志文件 (SQL Server)](../../relational-databases/backup-restore/view-the-data-and-log-files-in-a-backup-set-sql-server.md)  
  
-   [查看逻辑备份设备的属性和内容 (SQL Server)](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
-   [从设备还原备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-backup-from-a-device-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)  
  
  

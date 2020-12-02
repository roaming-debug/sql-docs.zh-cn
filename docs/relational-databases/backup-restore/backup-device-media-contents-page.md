---
title: 备份设备（“介质内容”页）| Microsoft Docs
description: 使用“备份设备”对话框可以查看备份信息。 此信息描述设备、介质、介质集以及备份集。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.backupdevice.contents.f1
ms.assetid: 5fc7bd22-b6d8-4af1-8a58-2e7d0b994d08
author: cawrites
ms.author: chadam
ms.openlocfilehash: b32df686813d69740f06bb711e2cd8bf6625fa6f
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129352"
---
# <a name="backup-device-media-contents-page"></a>备份设备（“介质内容”页）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用 **“备份设备”** 对话框可以查看备份信息。 此信息描述设备、介质、介质集以及备份集。  
  
 **使用 SQL Server Management Studio 查看备份设备的内容**  
  
-   [查看备份磁带或文件的内容 (SQL Server)](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
-   [查看逻辑备份设备的属性和内容 (SQL Server)](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
## <a name="options"></a>选项  
 查看有关各个介质、介质集和备份集的信息。  
  
 **介质**  
 存储备份信息的磁盘或磁带集。  
  
 **介质顺序**  
 列出介质序列号、簇序列号以及镜像标识符（如果有的话）。 每个物理备份介质都标记有介质序列号，表示介质的使用顺序。 第一个备份介质的标记为 1，第二个介质（第一个延续磁带）的标记为 2，依此类推。 在还原备份集时，还原备份的操作员根据介质序列号可以确保按正确的顺序装入介质。  
  
 **创建时间**  
 显示介质集的创建日期和时间。  
  
 **介质集**  
 介质集是通过使用一定数量的备份设备写入一个或多个备份操作的备份介质的有序集合。  
  
 **名称**  
 显示介质集的名称（如果有的话）。  
  
 **说明**  
 显示介质集的说明（如果有的话）。  
  
 **介质簇计数**  
 显示介质集中的簇数。 每个介质集都是一个或多个介质簇的集合。 对给定单个备份设备（或镜像备份设备组）的所有输出形成单个介质簇。 每个介质集对于一个单独设备（或镜像设备组）包含一个介质簇；例如，如果介质集使用两个非镜像备份设备，则介质集包含两个介质簇。  
  
 **备份集**  
 显示介质上包含的备份集的有关信息。 备份集是成功备份操作的结果，其内容分布于相应的一组备份设备上的介质中。  
  
|标头|值|  
|------------|------------|  
|**名称**|备份集的名称。|  
|类型|备份对象：数据库、文件或 \<blank>（对于事务日志）。|  
|组件|执行的备份类型：“完整”、“差异”或“事务日志”。|  
|**Server**|执行备份操作的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例名。|  
|**Database**|已备份数据库的名称。|  
|**位置**|备份集在卷中的位置。|  
|**Date**|备份操作完成的日期和时间，按客户端的区域设置显示。|  
|**大小**|备份集的大小（字节）。|  
|**用户名**|执行备份操作的用户的名称。|  
|**过期日期**|备份集的过期日期和时间。|  
  
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
  
  

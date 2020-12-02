---
title: 选择备份目标 | Microsoft Docs
description: 对于 SQL Server 还原，使用“选择备份目标”对话框可以选择磁盘或逻辑备份设备作为备份目标。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.selectbackupdest.f1
ms.assetid: f79e824b-1525-45de-8ede-513563af41b6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 387f799dc034a8c0db0d60cded709cb16fa0c2c7
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129044"
---
# <a name="select-backup-destination"></a>选择备份目标
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  使用 **“选择备份目标”** 对话框可以选择一个设备作为备份目标。 备份目标可以是磁盘，也可以是逻辑备份设备。  
  
 **使用 SQL Server Management Studio 备份数据库**  
  
-   [创建完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [创建差异数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
-   [备份文件和文件组 (SQL Server)](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)  
  
-   [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
## <a name="options"></a>选项  
 此对话框中的选项取决于所选目标是位于磁盘上还是位于磁带上。  
  
 **磁盘上的目标**  
 若要指定备份目标，请选择下列选项之一。  
  
|||  
|-|-|  
|**文件名**|选择此选项，以在文本框中输入作为备份目标的本地文件或远程文件：<br /><br /> 若要指定本地文件，请单击文本框右侧的“浏览”按钮，并导航到运行服务器的计算机上固定驱动器中的相应文件，或直接输入完整路径和文件名；例如， `C:\Program Files\Microsoft SQL Server\MSSQL\Backup\AdventureWorksBackup.bak`。<br /><br /> 若要将远程文件指定为备份目标，请输入其完全限定的通用命名约定 (UNC) 名称。 有关详细信息，请参阅 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)。<br /><br /> <br /><br /> **\*\* 重要说明 \*\*** 通过网络备份数据时可能会出现网络错误；因此，建议你在完成备份后验证备份操作。 有关详细信息，请参阅 [RESTORE VERIFYONLY (Transact-SQL)](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)。|  
|**备份设备**|选择此选项以选择逻辑备份设备。<br /><br /> 注意：有关如何创建磁盘备份设备的详细信息，请参阅[为磁盘文件定义逻辑备份设备 (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)。|  
  
 **磁带上的目标**  
 在与运行服务器（即 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例）的计算机建立了物理连接的磁带机上指定备份目标。 选择以下选项之一：  
  
|||  
|-|-|  
|**磁带机**|选择此选项，以从与运行服务器实例的计算机建立了物理连接的磁带机列表中选择磁带机作为备份目标。<br /><br /> 注意：远程计算机中的磁带备份设备不是有效的备份目标。|  
|**备份设备**|选择此选项以选择现有的逻辑备份设备。 这些逻辑备份设备对应于与运行服务器实例的计算机建立了物理连接的磁带机。<br /><br /> 注意：有关如何创建磁带备份设备的详细信息，请参阅[为磁带驱动器定义逻辑备份设备 &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)。|  
  
## <a name="see-also"></a>另请参阅  
 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)  
  
  

---
title: 备份数据库（“常规”页）| Microsoft Docs
description: 在 SQL Server 中，使用“备份数据库”对话框中的“常规”页可以查看或修改数据库备份操作的源和目标设置。
ms.custom: ''
ms.date: 07/01/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.backupdatabase.general.f1
ms.assetid: 5c344dfd-1ad3-41cc-98cd-732973b4a162
author: cawrites
ms.author: chadam
ms.openlocfilehash: dd1dd775aff0865b2ce0f2f833ef78b7c5eb4b5e
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130569"
---
# <a name="back-up-database-general-page"></a>备份数据库（“常规”页）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用 **“备份数据库”** 对话框中的 **“常规”** 页可以查看或修改数据库备份操作的设置。  
  
 有关基本备份概念的详细信息，请参阅 [备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)。  
  
> [!NOTE]  
>  使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 指定备份任务时，可以通过单击“脚本”按钮，再为脚本选择一个目标，生成对应的 [!INCLUDE[tsql](../../includes/tsql-md.md)] [BACKUP](../../t-sql/statements/backup-transact-sql.md) 脚本  。  
  
 **使用 SQL Server Management Studio 创建备份**  
  
-   [创建完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [创建差异数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
    > [!IMPORTANT]  
    >  可以定义用于创建数据库备份的数据库维护计划。 有关详细信息，请参阅 [联机丛书中的](../maintenance-plans/maintenance-plans.md) 数据库维护计划 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 。  
  
 **创建部分备份**  
  
-   对于部分备份，必须使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] [BACKUP](../../t-sql/statements/backup-transact-sql.md) 语句和 PARTIAL 选项。  
  
## <a name="options"></a>选项  
  
### <a name="source"></a>源  
 可通过 **“源”** 面板中的选项标识数据库并指定备份操作的备份类型和组件。  
  
 **Database**  
 选择要备份的数据库。  
  
 **恢复模式**  
 查看为所选数据库显示的恢复模式（SIMPLE、FULL 或 BULK_LOGGED）。  
  
 **备份类型**  
 选择要对指定数据库执行的备份的类型。  
  
|备份类型|适用于|限制|  
|-----------------|-------------------|------------------|  
|完全|数据库、文件和文件组|对于 **master** 数据库，只能执行完整备份。<br /><br /> 在简单恢复模式下，文件和文件组备份只适用于只读文件组。|  
|差异|数据库、文件和文件组|在简单恢复模式下，文件和文件组备份只适用于只读文件组。|  
|事务日志|事务日志|事务日志备份不适用于简单恢复模式。|  
  
 **仅复制备份**  
 选择创建仅复制备份。 *仅复制备份* 是[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]独立于常规备份序列[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的备份。 有关详细信息，请参阅[仅复制备份 (SQL Server)](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)。  
  
> [!NOTE]  
>  选择“差异”  选项时，无法创建仅复制备份。  
  
 **备份组件**  
 选择要备份的数据库组件。 如果在 **“备份类型”** 列表中选择了 **“事务日志”** ，则不会激活此选项。  
  
 选择以下选项按钮之一：  
  
|||  
|-|-|  
|**Database**|指定备份整个数据库。|  
|**文件和文件组**|指定要备份的文件和/或文件组。<br /><br /> 选择此选项，打开 **“选择文件和文件组”** 对话框。 在选择要备份的文件组或文件并单击“确定”  后，所选内容将显示在“文件组和文件”  框中。|  
  
### <a name="destination"></a>目标  
 “目标”  面板中的选项允许您为备份操作指定备份设备的类型，并查找现有的逻辑或物理备份设备。  
  
> [!NOTE]  
>  有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份设备的信息，请参阅[备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)。  
  
 **备份到**  
 选择以下类型介质之一，作为要备份到的目标。 选择的目标将显示在 **“备份到”** 列表中。  
  
|||  
|-|-|  
|**磁盘**|备份到磁盘。 这可能是一个为该数据库创建的系统文件或基于磁盘的逻辑备份设备。 当前所选的磁盘将显示在 **“备份到”** 列表中。 您最多可以为备份操作选择 64 个磁盘设备。|  
|**磁带**|备份到磁带。 这可能是一个为该数据库创建的本地磁带机或基于磁带的逻辑备份设备。 当前所选的磁带将显示在 **“备份到”** 列表中。 最大数目为 64。 如果服务器没有相连的磁带设备，此选项将停用。 您选择的磁带将列在 **“备份到”** 列表中。<br /><br /> 注意：在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的未来版本中将不再支持磁带备份设备。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。|  
|**URL**|备份到 Microsoft Azure Blob 存储。|  
  
 显示的下一组选项会取决于所选目标的类型。 如果您选择“磁盘”或“磁带”，则会显示以下选项。  
  
 **添加**  
 将文件或设备添加到“备份到”  列表中。 最多可以同时向本地磁盘或远程磁盘上的 64 个设备进行备份。 若要指定远程磁盘上的文件，请使用完全限定的通用命名约定 (UNC) 名称。 有关详细信息，请参阅 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)。  
 
 
  
 **删除**  
 从“备份到”  列表中删除一个或多个当前所选的设备。  
  
 **Contents**  
显示所选设备的媒体内容（如果存在）。  指定 **URL** 时，此按钮不会起作用。 
   
“选择备份目标”  对话框 选择“添加”  后将出现“选择备份目标”  对话框。   显示的选项集取决于所选目标的类型。 

如果你选择“磁盘”  或“磁带”  作为备份目标，则会显示以下选项。  

*
  **文件名**  
    指定备份文件的名称。

如果选择 **URL** 作为备份目标，则会显示以下选项：
*
  **Azure 存储容器**  
  用于存储备份文件的 Microsoft Azure 存储容器名称。 
   
*
  **共享访问策略：**  
  对手动输入的容器输入共享访问签名。  如果已选择现有容器，则此字段不可用。
  
*
  **备份文件：**  
  备份文件的名称。

*
  **新建容器：**  
用于注册没有共享访问签名的现有容器。  请参阅 [连接到 Microsoft Azure 订阅](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md)。
  
## <a name="see-also"></a>另请参阅  
 [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)   
 [备份文件和文件组 (SQL Server)](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)   
 [为磁盘文件定义逻辑备份设备 (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)   
 [为磁带驱动器定义逻辑备份设备 (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)   
 [恢复模式 (SQL Server)](../../relational-databases/backup-restore/recovery-models-sql-server.md)  
  
  

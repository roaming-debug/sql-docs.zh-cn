---
title: 还原数据库（“常规”页）| Microsoft Docs
description: 在 SQL Server 中还原数据库时，使用“常规”页可指定数据库还原操作的目标数据库和源数据库的有关信息。
ms.custom: ''
ms.date: 09/28/2020
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.restoredb.general.f1
ms.assetid: 160cf58c-b06a-475f-9a69-2b051e5767ab
author: cawrites
ms.author: chadam
ms.openlocfilehash: cda87fffca686f8e86c7b617e502952341bd459f
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125546"
---
# <a name="restore-database-general-page"></a>还原数据库（“常规”页）

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

使用“常规”页，可以指定数据库还原操作的目标数据库和源数据库的有关信息。  
    
-   [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [为磁带驱动器定义逻辑备份设备 (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)  
  
> [!NOTE]  
>  使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 指定还原任务时，可以通过单击“脚本”，再为脚本选择一个目标，生成相应的 [!INCLUDE[tsql](../../includes/tsql-md.md)] [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) 脚本。  
  
## <a name="permissions"></a>权限  
要对不存在的数据库执行 RESTORE 操作，用户必须具有 CREATE DATABASE 权限。 RESTORE 权限默认授予 sysadmin 和 dbcreator 固定服务器角色的成员以及现有数据库的所有者 (dbo)  。  

具有 RESTORE 权限的角色的成员身份信息在实例上始终可用。

在可访问且未损坏的数据库中，可以检查固定数据库角色的成员身份。 db_owner 固定数据库角色的成员没有 RESTORE 权限。  

只有在数据库可访问且未损坏时，才能检查固定数据库角色的成员身份。 执行 RESTORE 时，db_owner 固定数据库角色的成员并非总是没有 RESTORE 权限。  

RESTORE 权限被授予那些成员身份信息始终可由服务器使用的角色。 只有在数据库可访问且未损坏时，才能检查固定数据库角色的成员身份。 执行 RESTORE 时，db_owner 固定数据库角色的成员周期性地没有 RESTORE 权限。  

从加密的备份进行还原需要对在备份期间用于加密的证书或非对称密钥具有 **VIEW DEFINITION** 权限。  
  
## <a name="options"></a>选项  
  
### <a name="source"></a>源  

这些选项可标识数据库备份集的位置以及要还原的备份集。  
  
|术语|定义|  
|----------|----------------|  
|**Database**|从下拉列表中选择要还原的数据库。 此列表仅包含已根据 msdb 备份历史记录进行备份的数据库。|  
|**设备**|选择包含要还原的一个或多个备份的逻辑或物理备份设备：磁带、URL 或文件。 如果在另一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上执行数据库备份，则必须选择设备。<br /><br /> 要选择一个或多个逻辑或物理备份设备，请单击“浏览”按钮，这将打开“选择备份设备”对话框。 在此，最多可以选择属于一个介质集的 64 个设备。 磁带机必须与运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的计算机进行物理连接。 备份文件可以位于本地或远程磁带设备上。 有关详细信息，请参阅 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)。 还可以选择“URL”作为 Azure 存储中所存储备份文件的设备类型。<br /><br /> 退出“选择备份设备”对话框时，选择的设备将在“设备”列表中显示为只读值。|  
|**Database**|从下拉列表中选择要从其还原备份的数据库名称。<br /><br /> 注意：此列表仅在选择了 **“设备”** 时才可用。 只有已在所选设备上备份的数据库才可用。|  
  
### <a name="destination"></a>目标  
 **“还原到”** 面板中的选项可标识数据库和还原点。  
  
|术语|定义|  
|----------|----------------|  
|**Database**|在该列表中输入要还原的数据库。 您可以输入新的数据库，也可以从下拉列表中选择现有的数据库。 该列表包含了服务器上除系统数据库 **master** 和 **tempdb** 之外的所有数据库。<br /><br /> 注意：若要还原带有密码保护的备份，必须使用 [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) 语句。|  
|**“还原到”**|默认情况下， **“还原到”** 框将设置为“至最近一次进行的备份”。 你还可以选择“时间线”以便显示“备份时间线”对话框，该对话框将以时间线的形式显示数据库备份历史记录 。 选择“时间线”可以指定要将数据库还原到的特定日期/时间 。 数据库将还原到它在此指定时间点时的状态。 请参阅 [Backup Timeline](../../relational-databases/backup-restore/backup-timeline.md)。|  
  
### <a name="restore-plan"></a>还原计划  
  
|术语|定义|值|  
|----------|----------------|------------|  
|**备份集以进行还原**|显示可用于指定位置的备份集。 备份操作将创建一个备份集，该备份集分布在媒体集中的所有设备上。 默认情况下，会建议制定一个恢复计划，以实现基于所选必需备份集执行的还原操作目标。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 使用 msdb 中的备份历史记录。 该历史记录用于标识还原数据库所需的备份，并创建还原计划。 例如，为了进行数据库还原，还原计划将选择最近的完整数据库备份，然后选择最近的差异数据库备份（如果有）。 在完整恢复模式下，还原计划随后将选择所有日志备份。<br /><br /> 若要覆盖建议的恢复计划，可以更改网格中的选择。 任何依赖于已取消选择备份的备份，也将被自动取消选择。<br /><br /> 只有在选中了 **“手动选择”** 框后，这些复选框才启用。 你可以选择要还原的备份集。<br /><br /> 选中“手动选择”框后，每次修改还原计划时都会检查其精确性。 如果备份的顺序不正确，将显示一条错误消息。|**还原**： <br />                          选中的复选框指示要还原的备份集。<br /><br /> 名称： <br />                          备份集的名称。<br /><br /> **Component**：备份组件：  数据库、文件或 \<blank>（对于事务日志）。<br /><br /> **类型**：备份类型：“完整”、“差异”或“事务日志”  。<br /><br /> **服务器**：完成备份操作的[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例的名称。<br /><br /> **数据库**： <br />                          备份操作中涉及的数据库的名称。<br /><br /> **位置**：备份集在卷中的位置。<br /><br /> **第一个 LSN**： <br />                          备份集中第一个事务的日志序列号。 对于文件备份为空。<br /><br /> **最后一个 LSN**： <br />                          备份集中最后一个事务的日志序列号。 对于文件备份为空。<br /><br /> **检查点 LSN**： <br />                          创建备份时最新检查点的日志序列号 (LSN)。<br /><br /> **完整 LSN**： <br />                          最近的完整数据库备份的日志序列号。<br /><br /> **开始日期**： <br />                          备份操作开始的日期和时间，按客户端的区域设置显示。<br /><br /> **完成日期**： <br />                          备份操作完成的日期和时间，按客户端的区域设置显示。<br /><br /> **Size**： <br />                          备份集的大小（字节）。<br /><br /> **用户名**： <br />                          完成备份操作的用户的名称。<br /><br /> **过期日期**： <br />                          备份集的过期日期和时间。|  
|**验证备份介质**|对所选的备份集调用 RESTORE VERIFY_ONLY 语句。<br /><br /> 注意：验证操作耗时较长，你可以使用对话框框架上的进度监视器来跟踪其进度以及取消操作。<br /><br /> 此按钮可用于在还原所选备份文件之前检查其完整性。<br /><br /> 当检查备份集的完整性时，在该对话框左下角的进度状态将显示“正在验证”，而非“正在执行”。||  
  
## <a name="compatibility-support"></a>兼容性支持  
 在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]中，可以从使用 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 或更高版本创建的数据库备份来还原用户数据库。 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 无法还原使用 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 到 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 创建的 master、model 和 msdb 备份  。 此外，任何早期版本的 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 都无法还原在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中创建的备份。  
  
 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 使用与早期版本不同的默认路径。 要还原在早期 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本的默认位置创建的数据库，必须使用 MOVE 选项。  
  
 在您将早期版本数据库还原到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]后，将自动升级该数据库。 通常，该数据库将立即可用。 但是，如果 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库具有全文检索，则升级过程将导入、重置或重新生成它们，具体取决于“全文升级选项”服务器属性的设置。 如果将升级选项设置为“导入”或“重新生成”，在升级过程中将无法使用全文检索。 导入可能需要数小时，而重新生成所需的时间最多可能十倍于此，具体取决于要编制索引的数据量。 另请注意，当升级选项设置为“导入”时，如果全文目录不可用，将重新生成关联的全文检索。  
  
## <a name="restoring-from-an-encrypted-backup"></a>从加密备份还原  
 执行还原操作要求在要还原到的实例上具有最初用于创建备份的证书或非对称密钥。 执行还原的帐户应对证书或非对称密钥具有 **VIEW DEFINITIONS** 权限。 请勿续订或更新用于加密备份的证书。  
  
## <a name="restoring-from-microsoft-azure-storage"></a>从 Microsoft Azure 存储还原  
从“备份媒体类型”中选择 **URL**：“选择备份设备”对话框中的下拉列表。  接下来，选择“添加”，打开“选择备份文件位置” 。 选择现有的 SQL Server 凭据和 Azure 存储容器。 添加具有共享访问签名的新 Azure 存储容器。或为现有存储容器生成共享访问签名和 SQL Server 凭据。 连接到存储帐户后，将在“在 Microsoft Azure 上定位备份文件”对话框中显示备份文件，从中可选择用于还原的文件。  另请参阅，[连接到 Microsoft Azure 订阅](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md)。
  
## <a name="see-also"></a>另请参阅  
 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [从设备还原备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-backup-from-a-device-sql-server.md)   
 [将数据库还原到标记的事务 (SQL Server Management Studio)](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)   
 [还原事务日志备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)   
 [查看备份磁带或文件的内容 (SQL Server)](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)   
 [查看逻辑备份设备的属性和内容 (SQL Server)](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)   
 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [RESTORE 参数 (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md)   
 [应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)  
  
  

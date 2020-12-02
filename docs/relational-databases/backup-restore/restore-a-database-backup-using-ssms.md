---
title: 使用 SSMS 还原数据库备份 | Microsoft Docs
description: 本文说明如何使用 SQL Server Management Studio 还原完整 SQL Server 数据库备份。
ms.custom: ''
ms.date: 11/16/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.locatebackupfileazure.f1
- sql13.swb.specifybackup.f1
helpviewer_keywords:
- full backups [SQL Server]
- database restores [SQL Server], full backups
- backing up databases [SQL Server], full backups
- database backups [SQL Server], full backups
- restoring databases [SQL Server], full backups
ms.assetid: 24b3311d-5ce0-4581-9a05-5c7c726c7b21
author: cawrites
ms.author: chadam
ms.openlocfilehash: f93bb71a3f6dcbbd98e62cca67a877361c6766db
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125596"
---
# <a name="restore-a-database-backup-using-ssms"></a>使用 SSMS 还原数据库备份
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题阐释如何使用 SQL Server Management Studio 还原完整的数据库备份。    
       
### <a name="important"></a>重要说明！    
在完整恢复模式或大容量日志恢复模式下，可能需要先备份活动事务日志（称为 [日志尾部](tail-log-backups-sql-server.md)），然后才能还原数据库。 有关详细信息，请参阅 [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)数据库还原到一个新位置并且可以选择重命名该数据库。  

从其他实例还原数据库时，请考虑 [当数据库在其他服务器实例上可用时管理元数据 (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)中的信息。   
    
若要还原加密数据库，你需要有用于加密该数据库的证书或非对称密钥的访问权限。 如果没有证书或非对称密钥，数据库将无法还原。 如果需要保存备份，就必须保留用于加密数据库加密密钥的证书。 有关详细信息，请参阅 [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)。    
    
如果将较旧版本的数据库还原到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]，则该数据库将自动升级到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。 这将阻止数据库被旧版本的 [!INCLUDE[ssde_md](../../includes/ssde_md.md)] 使用。 但是，这与元数据升级相关，不会影响[数据库兼容性级别](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) 如果升级前用户数据库的兼容级别为 100 或更高，升级后将保持相应级别。 如果升级前兼容级别为 90，则在升级后的数据库中，兼容级别将设置为 100，该级别为 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]支持的最低兼容级别。 有关详细信息，请参阅 [ALTER DATABASE 兼容级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。  
  
通常，该数据库将立即可用。 但是，如果 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库具有全文检索，则升级过程将导入、重置或重新生成它们，具体取决于 **全文升级选项** 服务器属性的设置。 如果将升级选项设置为“导入”  或“重新生成”  ，在升级过程中将无法使用全文检索。 导入可能需要数小时，而重新生成所需的时间最多时可能十倍于此，具体取决于要编制索引的数据量。     
    
当升级选项设置为“导入”  时，如果全文目录不可用，将重新生成关联的全文检索。 有关查看或更改“全文升级选项”  属性设置的信息，请参阅[管理和监视服务器实例的全文搜索](../../relational-databases/search/manage-and-monitor-full-text-search-for-a-server-instance.md)。    

有关从 Microsoft Azure Blob 存储服务还原 SQL Server 的信息，请参阅[《SQL Server Backup and Restore with Microsoft Azure Blob Storage Service》](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)（使用 Microsoft Azure Blob 存储服务进行 SQL Server 备份和还原）。

## <a name="examples"></a>示例
    
### <a name="a-restore-a-full-database-backup"></a>A. 还原完整的数据库备份   
    
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例，然后展开该实例。  
    
2.  右键单击“数据库”  ，然后选择“还原数据库...”     
    
3.  在 **“常规”** 页上，使用 **“源”** 部分指定要还原的备份集的源和位置。 选择以下选项之一：    
    
    -   **Database**    
    
         从下拉列表中选择要还原的数据库。 此列表仅包含已根据 **msdb** 备份历史记录进行备份的数据库。    
    
        > [!NOTE]
        > 如果备份是从另一台服务器执行的，则目标服务器不具有指定数据库的备份历史记录信息。 这种情况下，请选择 **“设备”** 以手动指定要还原的文件或设备。 
    
    -   **设备**    
    
         单击“浏览”按钮 ( **...** ) 以打开“选择备份设备”  对话框。 
         
        -   **选择备份设备** 对话框  
        
            **备份介质类型**  
         从“备份介质类型”下拉列表中选择一个介质类型。   注意：只有在计算机上装有磁带机时，才会显示 **“磁带”** 选项，只有至少存在一个备份设备时，才会显示 **“备份设备”** 选项。

            **添加**  
            根据在“备份介质类型”下拉列表中选择的介质类型，单击“添加”将打开下列对话框之一。   （如果“备份介质”列表框中的列表已满，则“添加”按钮不可用。）  

            |介质类型|对话框|说明|    
            |----------------|----------------|-----------------|    
            |**File**|**定位备份文件**|在此对话框中，您可以从树中选择一个本地文件，或使用完全限定的通用命名约定 (UNC) 名称指定一个远程文件。 有关详细信息，请参阅 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)。|    
            |**设备**|**选择备份设备**|在此对话框中，您可以从服务器实例中定义的逻辑备份设备列表中进行选择。|    
            |**磁带**|**选择备份磁带**|在此对话框中，您可以从与运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的计算机物理连接的磁带机列表中进行选择。|    
            |**URL**|**选择备份文件位置**|在该对话框中可以选择现有 SQL Server 凭据/Azure 存储容器、添加具有共享访问签名的新 Azure 存储容器或为现有存储容器生成共享访问签名和 SQL Server 凭据。  另请参阅 [《Connect to a Microsoft Azure Subscription》](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md)（连接到 Microsoft Azure 订阅）。|  
         
             **删除**    
             删除一个或多个选定的文件、磁带或逻辑备份设备。    
                 
             **目录**    
            显示选定文件、磁带或逻辑备份设备的介质内容。  如果介质类型为“URL”，此按钮可能不起作用。   

             **备份介质**   
             列出所选介质。
    
             将所需设备添加到 **“备份介质”** 列表框后，单击 **“确定”** 返回到 **“常规”** 页。    
    
         在“源:设备:数据库”列表框中，选择应还原的数据库名称 **。**    
    
         > [!NOTE]
         > 此列表仅在选择了 **“设备”** 时才可用。 只有在所选设备上具有备份的数据库才可用。    
     
4.  在 **“目标”** 部分中， **“数据库”** 框自动填充要还原的数据库的名称。 若要更改数据库名称，请在 **“数据库”** 框中输入新名称。    
    
5.  在 **“还原到”** 框中，保留默认选项 **“至最近一次进行的备份”** ，或者单击 **“时间线”** 访问 **“备份时间线”** 对话框以手动选择要停止恢复操作的时间点。 有关指定特定时间点的详细信息，请参阅 [Backup Timeline](../../relational-databases/backup-restore/backup-timeline.md)。    
    
6.  在 **“要还原的备份集”** 网格中，选择要还原的备份。 此网格将显示对于指定位置可用的备份。 默认情况下，系统会推荐一个恢复计划。 若要覆盖建议的恢复计划，可以更改网格中的选择。 当取消选择某个早期备份时，将自动取消选择那些需要还原该早期备份才能进行的备份。 有关“用于还原的备份集”  网格中的列的信息，请参阅[还原数据库（“常规”页）](../../relational-databases/backup-restore/restore-database-general-page.md)。    
    
7.  或者单击 **“选择页”** 窗格中的 **“文件”** ，以便访问 **“文件”** 对话框。 在该对话框中，您可以通过在 **“将数据库文件还原为”** 网格中指定每个文件的新还原目标，将数据库还原到新的位置。 有关该网格的详细信息，请参阅[还原数据库（“文件”页）](../../relational-databases/backup-restore/restore-database-files-page.md)。    
    
8. 若要查看或选择高级选项，在 **“选项”** 页的 **“还原选项”** 面板中，可以根据您的实际情况选择下列任意选项：    

   1. **WITH** 选项（可选）：    
    
     - **覆盖现有数据库(WITH REPLACE)**    
    
     - **保留复制设置(WITH KEEP_REPLICATION)**    
    
     - **限制对还原数据库的访问(WITH RESTRICTED_USER)**    
    
   2. 为 **“恢复状态”** 框选择一个选项。 此框确定还原操作之后的数据库状态。    
    
     - **RESTORE WITH RECOVERY** 是默认行为，它通过回滚未提交的事务，使数据库处于可以使用的状态。 无法还原其他事务日志。 如果您要立即还原所有必要的备份，则选择此选项。    
    
     - **RESTORE WITH NORECOVERY** 不对数据库执行任何操作，不回滚未提交的事务。 可以还原其他事务日志。 除非恢复数据库，否则无法使用数据库。    
    
     - **RESTORE WITH STANDBY** 使数据库处于只读模式。 它撤消未提交的事务，但将撤消操作保存在备用文件中，以便能够还原恢复结果。    
    
   3. **还原前进行结尾日志备份。** 并非所有还原方案都要求执行结尾日志备份。  有关详细信息，请参阅 **《Tail-Log Backups (SQL Server)》** （结尾日志备份 (SQL Server)）中的 [Scenarios That Require a Tail-Log Backup](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)（需要结尾日志备份的场景）。
  
   4. 如果存在与数据库的活动连接，则还原操作可能会失败。 选中 **“关闭现有连接”** 以确保关闭 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 和数据库之间的所有活动连接。 此复选框可在执行还原操作之前将数据库设置为单用户模式，并在该操作完成后将数据库设置为多用户模式。    
  
   5. 如果要在每个还原操作之间进行提示，请选择 **“还原每个备份之前进行提示”** 。 除非数据库过大并且您要监视还原操作的状态，否则通常没有必要选中该选项。    
    
有关这些还原选项的详细信息，请参阅 [还原数据库（“选项”页）](../../relational-databases/backup-restore/restore-database-options-page.md)），然后才能还原数据库。    
    
9. [!INCLUDE[clickOK](../../includes/clickok-md.md)] 

### <a name="b-restore-an-earlier-disk-backup-over-an-existing-database"></a>B. 在现有数据库上还原以前的磁盘备份
下面的示例将还原 `Sales` 的以前的磁盘备份，并覆盖现有 `Sales` 数据库。

1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例，然后展开该实例。  
2.  右键单击“数据库”  ，然后选择“还原数据库...”   
3.  在“常规”  页上，在“源”  部分下选择“设备”  。
4.  单击“浏览”按钮 ( **...** ) 以打开“选择备份设备”  对话框。 单击“添加”  并导航到你的备份。 在选择你的磁盘备份文件之后单击“确定”  。
5.  单击“确定”以返回到“常规”页。  
6.  在“选择页”窗格中单击“选项”。  
7.  在“还原选项”部分中，选择“覆盖现有数据库 (WITH REPLACE)”。  

    > [!NOTE]
    > 未选中此选项可能会导致以下错误消息：“System.Data.SqlClient.SqlError:备份集中的数据库备份与现有的“`Sales`”数据库不同。 (Microsoft.SqlServer.SmoExtended)”

8.  在“结尾日志备份”  部分中，取消选中“还原前执行结尾日志备份”  。

    > [!NOTE]
    > 并非所有还原方案都要求执行结尾日志备份。 如果恢复点包含在较早的日志备份中，则无需结尾日志备份。 此外，如果您准备移动或替换（覆盖）数据库，并且在最新备份后不需要将该数据库还原到某一时间点，则不需要结尾日志备份。 有关详细信息，请参阅 [《Tail-Log Backups (SQL Server)》](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)（结尾日志备份 (SQL Server)）。

    此选项不可用于简单恢复模式下的数据库。

9.  在“服务器连接”  部分，选中“关闭目标数据库的现有连接”  。

    > [!NOTE]
    > 未选中此选项可能会导致以下错误消息：“System.Data.SqlClient.SqlError:因为数据库正在使用，所以无法获得对数据库的独占访问权。 (Microsoft.SqlServer.SmoExtended)”
    
10. [!INCLUDE[clickOK](../../includes/clickok-md.md)] 

### <a name="c--restore-an-earlier-disk-backup-with-a-new-database-name-where-the-original-database-still-exists"></a>C.  使用新的数据库名称还原以前的磁盘备份，并且原始数据库仍然存在
下面的示例将还原 `Sales` 的以前的磁盘备份，并创建名为 `SalesTest`的新数据库。  原始数据库 `Sales`仍存在于服务器上。

1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例，然后展开该实例。  
2.  右键单击“数据库”  ，然后选择“还原数据库...”   
3.  在“常规”  页上，在“源”  部分下选择“设备”  。
4.  单击“浏览”按钮 ( **...** ) 以打开“选择备份设备”  对话框。 单击“添加”  并导航到你的备份。 在选择你的磁盘备份文件之后单击“确定”  。
5.  单击“确定”以返回到“常规”页。  
6.  在 **“目标”** 部分中， **“数据库”** 框自动填充要还原的数据库的名称。 若要更改数据库名称，请在 **“数据库”** 框中输入新名称。
7.  在“选择页”窗格中单击“选项”。  
8.  在“结尾日志备份”  部分中，取消选中“还原前执行结尾日志备份”  。

    > [!IMPORTANT]
    > 如果不取消选中此选项，则会使现有的数据库中 `Sales`更改为还原状态。

9. [!INCLUDE[clickOK](../../includes/clickok-md.md)] 

    > [!NOTE]
    > 如果收到以下错误信息：      
    > “System.Data.SqlClient.SqlError:数据库“`Sales`”的日志结尾尚未备份。 如果该日志包含不希望丢失的工作，请使用 `BACKUP LOG WITH NORECOVERY` 备份该日志。 使用 `RESTORE` 语句的 `WITH REPLACE` 或 `WITH STOPAT` 子句覆盖该日志的内容。 (Microsoft.SqlServer.SmoExtended)”。      
    > 那么你可能未输入上面步骤 6 中的新数据库名称。 还原一般会防止意外使用一个数据库覆盖另一个数据库。 如果 `RESTORE` 语句中指定的数据库已存在于当前服务器上，并且指定的数据库系列 GUID 与备份集中记录的数据库系列 GUID 不同，则不还原该数据库。 这是一项重要的安全保护措施。

### <a name="d--restore-earlier-disk-backups-to-a-point-in-time"></a>D.  将以前的磁盘备份还原到某个时间点
下面的示例将数据库还原到它在 `1:23:17 PM` 的 `May 30, 2016` 的状态，并显示涉及多个日志备份的还原操作。 数据库当前不在服务器上。

1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例，然后展开该实例。  
2.  右键单击“数据库”  ，然后选择“还原数据库...”   
3.  在“常规”  页上，在“源”  部分下选择“设备”  。
4.  单击“浏览”按钮 ( **...** ) 以打开“选择备份设备”  对话框。 单击“添加”  并导航到你的完整备份和所有相关事务日志备份。  在选择你的磁盘备份文件之后单击“确定”  。
5.  单击“确定”以返回到“常规”页。  
6.  在“目标”部分中单击“时间线”访问“备份时间线”对话框，以手动选择要停止恢复操作的时间点。   
7.  选择“特定的日期和时间”。   
8.  在下拉列表框中将“时间线间隔”更改为“小时”（可选）。    
9.  将滑块移动到所需的时间。
10. 单击“确定”以返回到“常规”页。 
11. [!INCLUDE[clickOK](../../includes/clickok-md.md)] 

### <a name="e--restore-a-backup-from-the-microsoft-azure-storage-service"></a>E.  从 Microsoft Azure 存储服务还原备份

#### <a name="common-steps"></a>一般步骤
下面的两个示例执行从位于 Microsoft Azure 存储服务中的备份还原 `Sales` 。  存储帐户名称为 `mystorageaccount`。  容器名称为 `myfirstcontainer`。  出于简洁的目的，在此处一次列出前六个步骤，所有示例将从 **步骤 7** 开始。
1.  在“对象资源管理器”  中，连接到一个 SQL Server 数据库引擎实例，然后展开该实例。
2.  右键单击“数据库”  ，然后选择“还原数据库...”  。
3.  在“常规”  页上，在“源”  部分下选择“设备”  。
4.  单击“浏览 (...)”按钮以打开“选择备份设备”  对话框。    
5.  从“备份媒体类型:”  下拉列表中选择 **URL**。
6.  单击“添加”  可打开“选择备份文件位置”  对话框。

#### <a name="e1---restore-a-striped-backup-over-an-existing-database-and-a-shared-access-signature-exists"></a>E1.   在现有数据库上还原条带备份，并存在共享的访问签名。
已经创建具有读取、写入、删除和列表权限的存储访问策略。  已经为容器 `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`创建与存储访问策略相关联的共享访问签名。  如果已存在 SQL Server 凭据，则步骤几乎一样。  数据库 `Sales` 当前在服务器上。  备份文件为 `Sales_stripe1of2_20160601.bak` 和 `Sales_stripe2of2_20160601.bak`。  

1.  如果已存在 SQL Server 凭据，则从“Azure 存储容器:”下拉列表中选择 `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`，否则请手动输入容器 `https://mystorageaccount.blob.core.windows.net/myfirstcontainer` 的名称。  
1. 在“共享访问签名:”富文本框中输入共享访问签名。 
1. 单击“确定”  将打开“在 Microsoft Azure 上定位备份文件”  对话框。
1. 展开“容器”  并导航到 `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`。
1. 按住 ctrl 并选择文件 `Sales_stripe1of2_20160601.bak` 和 `Sales_stripe2of2_20160601.bak`。
1. 单击“确定”。 
1. 单击“确定”以返回到“常规”页。  
1. 在“选择页”窗格中单击“选项”。  
1. 在“还原选项”部分中，选择“覆盖现有数据库 (WITH REPLACE)”。  
1. 在“结尾日志备份”  部分中，取消选中“还原前执行结尾日志备份”  。
1. 在“服务器连接”  部分，选中“关闭目标数据库的现有连接”  。
1. 单击“确定”。 

#### <a name="e2---a-shared-access-signature-does-not-exist"></a>E2.   共享访问签名不存在
在本示例中数据库 `Sales` 当前不在服务器上。
1. 单击  “添加”将打开  “连接到 Microsoft 订阅”对话框。  
1. 完成  “连接到 Microsoft 订阅”对话框，然后单击  “确定”，返回到  “选择备份文件位置”对话框。  有关其他信息，请参阅[《Connect to a Microsoft Azure Subscription》](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md)（连接到 Microsoft Azure 订阅）。
1. 单击“选择备份文件位置”对话框中单击“确定”将打开“在 Microsoft Azure 上定位备份文件”对话框。   
1. 展开“容器”  并导航到 `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`。
1. 选择备份文件，再单击“确定”  。
1. 单击“确定”以返回到“常规”页。  
1. 单击“确定”。 

#### <a name="f-restore-local-backup-to-microsoft-azure-storage-url"></a>F. 将本地备份还原到 Microsoft Azure 存储 (URL)
将从位于 `Sales` 的备份中将 `https://mystorageaccount.blob.core.windows.net/myfirstcontainer` 数据库还原到 Microsoft Azure 存储容器 `E:\MSSQL\BAK`。  已创建 Azure 容器的 SQL Server 凭据。  必须已存在目标容器的 SQL Server 凭据，因为不能通过创建 **还原** 任务创建该凭据。  `Sales` 数据库当前不在服务器上。
1.  在“对象资源管理器”  中，连接到一个 SQL Server 数据库引擎实例，然后展开该实例。
2.  右键单击“数据库”  ，然后选择“还原数据库...”  。
3.  在“常规”  页上，在“源”  部分下选择“设备”  。
4.  单击“浏览 (...)”按钮以打开“选择备份设备”  对话框。  
5.  从“备份媒体类型:”  下拉列表中选择“文件”  。
6.  单击“添加”  将打开“定位备份文件”  对话框。
7.  导航到 `E:\MSSQL\BAK`，选择备份文件，再单击“确定”。 
8.  单击“确定”以返回到“常规”页。  
9.  在“选择页”窗格中，单击“文件”。  
10. 选中“将所有文件重新定位到文件夹”复选框。 
11. 在“数据文件文件夹:”和“日志文件文件夹:”的文本框中输入容器 `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`。  
12. 单击“确定”。 

## <a name="see-also"></a>另请参阅    
 [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)     
 [创建完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)     
 [将数据库还原到新位置 (SQL Server)](../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)     
 [还原事务日志备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)     
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)     
 [还原数据库（“选项”页）](../../relational-databases/backup-restore/restore-database-options-page.md)     
 [还原数据库（“常规”页）](../../relational-databases/backup-restore/restore-database-general-page.md)    
    
  

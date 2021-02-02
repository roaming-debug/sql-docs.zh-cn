---
title: 创建完整数据库备份 | Microsoft Docs
description: 本文演示如何使用 SQL Server Management Studio、Transact-SQL 或 PowerShell 在 SQL Server 中创建完整数据库备份。
ms.custom: sqlfreshmay19
ms.date: 09/12/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backing up databases [SQL Server], full backups
- backing up databases [SQL Server], SQL Server Management Studio
- backups [SQL Server], creating
- database backups [SQL Server], SQL Server Management Studio
ms.assetid: 586561fc-dfbb-4842-84f8-204a9100a534
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3eb0b33f2891c8f48d63b4446d31c1918fcb7aab
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076109"
---
# <a name="create-a-full-database-backup"></a>完整数据库备份

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]或 PowerShell 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中创建完整数据库备份。

若要了解如何将 SQL Server 备份到 Azure Blob 存储服务，请参阅[使用 Azure Blob 存储服务进行 SQL Server 备份和还原](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)和 [SQL Server 备份到 URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md)。

## <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限

- 不允许在显式或隐式事务中使用 `BACKUP` 语句。
- 无法在早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中还原较新版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]创建的备份。

有关备份概念和任务的概述和详细信息，请在继续操作前先参阅[备份概述 &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)。

## <a name="recommendations"></a><a name="Recommendations"></a> 建议

- 随着数据库不断增大，完整数据库备份的完成时间会延长，并且需要占用更多存储空间。 对于大型数据库，请考虑用一系列[差异数据库备份](../../relational-databases/backup-restore/differential-backups-sql-server.md)来补充完整数据库备份。
- 使用 [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md) 系统存储过程估计完整数据库备份的大小。
- 默认情况下，每个成功的备份操作都会在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志和系统事件日志中添加一个条目。 若频繁备份，这些成功消息会迅速累积，从而生成巨大的错误日志！ 这会使查找其他消息变得非常困难。 在这些情况下，如果脚本均不依赖于这些备份日志条目，则可使用跟踪标志 3226 取消这些条目。 有关详细信息，请参阅[跟踪标志 (Transact-SQL)](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。

## <a name="security"></a><a name="Security"></a> Security

针对数据库备份，TRUSTWORTHY 设置为 OFF  。 有关如何将 TRUSTWORTHY 设置为 ON 的详细信息，请参阅 [ALTER DATABASE SET 选项 &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)  。

从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始，PASSWORD 和 MEDIAPASSWORD 选项不再可用于创建备份   。 不过，您仍可以还原使用密码创建的备份。

## <a name="permissions"></a><a name="Permissions"></a> 权限

默认情况下，为 sysadmin 固定服务器角色以及 db_owner 和 db_backupoperator 固定数据库角色的成员授予 `BACKUP DATABASE` 和 `BACKUP LOG` 权限    。

 备份设备的物理文件的所有权和权限问题可能会妨碍备份操作。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务必须能够读取和写入设备，这意味着运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的帐户必须对备份设备具有写入权限。 但是，用于在系统表中为备份设备添加项目的 [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)不检查文件访问权限。 因此，除非你因尝试备份或还原而访问物理资源，否则可能看不到备份设备物理文件中的这些问题。

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio

> [!NOTE]
> 使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 指定备份任务时，可以通过单击“脚本”按钮并选择脚本目标生成相应的 [!INCLUDE[tsql](../../includes/tsql-md.md)] [BACKUP](../../t-sql/statements/backup-transact-sql.md) 脚本  。

1. 连接到相应的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例之后，在“对象资源管理器”中，展开服务器树  。

1. 展开“数据库”，选择用户数据库，或展开“系统数据库”，选择系统数据库。  

1. 右键单击要备份的数据库，指向“任务”，再单击“备份...”   。

1. 在“备份数据库”对话框中，所选的数据库显示在下拉列表中（可将其更改为服务器上的任何其他数据库）  。

1. 在“备份类型”下拉列表中，选择所需的备份类型，默认值为“完整”   。

   > [!IMPORTANT]
   > 必须先至少执行一个完整数据库备份，然后才能执行差异备份或事务日志备份。
   
1. 在“备份组件”下，选择“数据库”   。

1. 在“目标”部分中，查看备份文件的默认位置（位于 ../mssql/data 文件夹）  。

   要备份到其他设备，请使用“备份到”下拉列表更改选择  。 要跨多个文件条带化备份集来提高备份速度，请单击“添加”，添加其他备份对象和/或目标  。
 
   若要删除备份目标，请选择该备份目标并单击 **“删除”** 。 若要查看现有备份目标的内容，请选择该备份目标并单击“内容”。 

1. （可选）查看“介质选项”和“备份选项”页面下的其他可用设置   。

   要详细了解各种备份选项，请参阅[常规页](back-up-database-general-page.md)、[介质选项页](back-up-database-media-options-page.md)和[备份选项页](back-up-database-backup-options-page.md)。

1. 单击“确定”，启动备份  。

1. 备份成功完成后，单击“确定”，关闭“SQL Server Management Studio”对话框  。

### <a name="additional-information"></a>其他信息

- 创建完整数据库备份后，可以创建[差异数据库备份](create-a-differential-database-backup-sql-server.md)或[事务日志备份](back-up-a-transaction-log-sql-server.md)。

- 还可以选择“仅复制备份”复选框创建仅复制备份  。 *仅复制备份* 是[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]独立于常规备份序列[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的备份。 有关详细信息，请参阅[仅复制备份 (SQL Server)](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)。 仅复制备份不可用于 **差异** 备份类型。

- 如果要备份到 URL，请禁用“介质选项”页上的“覆盖介质”选项   。

### <a name="examples"></a>示例

对于以下示例，使用以下 Transact-SQL 代码创建测试数据库：

```sql
USE [master]
GO

CREATE DATABASE [SQLTestDB]
GO

USE [SQLTestDB]
GO
CREATE TABLE SQLTest
   (
      ID INT NOT NULL PRIMARY KEY,
      c1 VARCHAR(100) NOT NULL,
      dt1 DATETIME NOT NULL DEFAULT getdate()
   );
GO

USE [SQLTestDB]
GO

INSERT INTO SQLTest (ID, c1) VALUES (1, 'test1')
INSERT INTO SQLTest (ID, c1) VALUES (2, 'test2')
INSERT INTO SQLTest (ID, c1) VALUES (3, 'test3')
INSERT INTO SQLTest (ID, c1) VALUES (4, 'test4')
INSERT INTO SQLTest (ID, c1) VALUES (5, 'test5')
GO

SELECT * FROM SQLTest
GO
```

#### <a name="a-full-back-up-to-disk-to-default-location"></a>A. 完整备份到默认位置的磁盘

在此示例中，将 `SQLTestDB` 数据库备份到默认备份位置处的磁盘。

1. 连接到相应的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例之后，在“对象资源管理器”中，展开服务器树  。

1. 展开“数据库”  ，右键单击 `SQLTestDB`，然后指向“任务”  ，再单击“备份...”  。

1. 单击“确定”。 

1. 备份成功完成后，单击“确定”，关闭“SQL Server Management Studio”对话框  。

![执行 SQL 备份](media/quickstart-backup-restore-database/backup-db-ssms.png)

#### <a name="b-full-back-up-to-disk-to-non-default-location"></a>B. 完整备份到非默认位置的磁盘

在此示例中，将 `SQLTestDB` 数据库备份到所选位置处的磁盘。

1. 连接到相应的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例之后，在“对象资源管理器”中，展开服务器树  。

1. 展开“数据库”  ，右键单击 `SQLTestDB`，然后指向“任务”  ，再单击“备份...”  。

1. 在“常规”页的“目标”部分中，从“备份到:”下拉列表中选择“磁盘”。    

1. 选择“删除”，直到所有现有备份文件均已删除  。

1. 选择“添加”，这将打开“选择备份目标”对话框   。

1. 在“文件名”文本框中输入有效的路径和文件名，并使用 .bak 作为扩展名，以简化此文件的分类   。

1. 单击“确定”，然后再次单击“确定”，启动备份   。

1. 备份成功完成后，单击“确定”，关闭“SQL Server Management Studio”对话框  。

![更改 DB 位置](media/create-a-full-database-backup-sql-server/change-db-location.png)

#### <a name="c-create-an-encrypted-backup"></a>C. 创建加密备份

在此示例中，将已加密的 `SQLTestDB` 数据库备份到默认备份位置。

1. 连接到相应的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例之后，在“对象资源管理器”中，展开服务器树  。

1. 展开“数据库”，展开“系统数据库”，右键单击 `master`，然后单击“新建查询”，打开连接到 `SQLTestDB` 数据库的查询窗口    。

1. 执行以下命令，在 `master` 数据库中创建[数据库主密钥](../../relational-databases/security/encryption/create-a-database-master-key.md)和[证书](../../t-sql/statements/create-certificate-transact-sql.md)   。  

   ```sql
   -- Create the master key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = '23987hxJ#KL95234nl0zBe';  

   -- If the master key already exists, open it in the same session that you create the certificate (see next step)
   OPEN MASTER KEY DECRYPTION BY PASSWORD = '23987hxJ#KL95234nl0zBe'

   -- Create the certificate encrypted by the master key
   CREATE CERTIFICATE MyCertificate
   WITH SUBJECT = 'Backup Cert', EXPIRY_DATE = '20201031';  
   ```

1. 在对象资源管理器的“数据库”节点中，右键单击 `SQLTestDB`，然后指向“任务”，再单击“备份...”     。

1. 在“介质选项”页的“覆盖介质”部分中，选择“备份到新介质集并清除所有现有备份集”    。

1. 在“备份选项”页的“加密”部分中，选择“加密备份”复选框。   

1. 从“算法”下拉列表中选择 AES 256  。

1. 从“证书”或“非对称密钥”下拉列表中选择 `MyCertificate`。 

1. 选择“确定”  。

![加密备份](media/create-a-full-database-backup-sql-server/encrypted-backup.png)

#### <a name="d-back-up-to-the-azure-blob-storage-service"></a>D. 备份到 Azure Blob 存储服务

以下示例对 Azure Blob 存储服务执行完整的 `SQLTestDB` 数据库备份。 此示例假设你已有一个具有 blob 容器的存储帐户。 此示例会创建一个共享访问签名；如果容器具有现有的共享访问签名，此示例会失败。

如果在存储帐户中不具有 Azure blob 容器，请先创建一个，然后再继续操作。 有关详细信息，请参阅[创建常规用途存储帐户](/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal)和[创建容器](/azure/storage/blobs/storage-quickstart-blobs-portal#create-a-container)。

1. 连接到相应的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例之后，在“对象资源管理器”中，展开服务器树  。

1. 展开“数据库”  ，右键单击 `SQLTestDB`，然后指向“任务”  ，再单击“备份...”  。

1. 在“目标”  部分中的“常规”  页上，从“备份到：”  下拉列表中选择“URL” 

1. 单击“添加”  “选择备份目标”  对话框。

1. 如果以前已注册要与 SQL Server Management Studio 配合使用的 Azure 存储容器，请选择该容器。 否则，单击“新建容器”，注册新的容器  。

1. 在“连接到 Microsoft 订阅”对话框中，登录你的帐户  。

1. 在“选择存储帐户”下拉文本框中，选择你的存储帐户  。

1. 在“选择 Blob 容器”下拉文本框中，选择你的 Blob 容器  。

1. 在“共享访问策略过期”下拉日历框中，为此示例中创建的共享访问策略选择到期日期  。

1. 单击“创建凭据”，在 SQL Server Management Studio 中生成共享访问签名和凭据  。

1. 单击“确定”，关闭“连接到 Microsoft 订阅”对话框   。

1. 在“备份文件”文本框中，修改备份文件的名称（可选）  。

1. 单击“确定”，关闭“选择备份目标”对话框   。

1. 单击“确定”，启动备份  。

1. 备份成功完成后，单击“确定”，关闭“SQL Server Management Studio”对话框  。

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL

通过执行 `BACKUP DATABASE` 语句创建完整数据库备份，同时指定：

- 要备份的数据库的名称。
- 写入完整数据库备份的备份设备。

完整数据库备份的基本 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法如下：

 BACKUP DATABASE *database* TO *backup_device* [ **,** ...*n* ] [ WITH *with_options* [ **,** ...*o* ] ] ;

|选项|说明|
|------------|-----------------|
|*database*|要备份的数据库。|
|*backup_device* [ **,** ...*n* ]|指定一个列表，它包含 1 至 64 个用于备份操作的备份设备。 您可以指定物理备份设备，也可以指定对应的逻辑备份设备（如果已定义）。 若要指定物理备份设备，请使用 DISK 或 TAPE 选项：<br /><br /> { DISK &#124; TAPE } =  物理\_备份\_设备\_名称 <br /><br /> 有关详细信息，请参阅 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)。|
|WITH with_options  [ **,** ...*o* ]|您也可以指定一个或多个附加选项 *o*。 有关某些基本 WITH 选项的信息，请参阅步骤 2。|
|||

（可选）指定一个或多个 WITH 选项  。 下面描述了几个基本 WITH 选项  。 有关所有 WITH 选项的详细信息，请参阅 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)  。

基本备份集 WITH 选项  ：

- { COMPRESSION | NO_COMPRESSION }  ：在 [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] 及更高版本中，指定是否为此备份执行 [备份压缩](../../relational-databases/backup-restore/backup-compression-sql-server.md) ，该设置将替代服务器级默认设置。
- ENCRYPTION (ALGORITHM, SERVER CERTIFICATE |ASYMMETRIC KEY)  ：仅在 SQL Server 2014 或更高版本中，指定要使用的加密算法以及要用于保护加密的证书或非对称密钥。
- **DESCRIPTION** **=** { **'** _text_ **'**  |  **@** _text\_variable_ }:指定说明备份集的自由格式文本。 该字符串最长可达 255 个字符。
- NAME = { backup_set_name | @_backup\_set\_name\_var_ }   ：指定备份集的名称。 名称最长可达 128 个字符。 如果未指定 NAME，它将为空。

默认情况下，`BACKUP` 将备份追加到现有介质集中，并保留现有备份集。 若要显式指定此项，请使用 `NOINIT` 选项。 有关附加到现有备份集的信息，请参阅 [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)。

或者，若要将备份介质格式化，可以使用 FORMAT 选项  ：

 FORMAT [ ,  MEDIANAME=  { media_name   | @  media\_name\_variable  } ] [ ,  MEDIADESCRIPTION =  { text   | @  text\_variable  } ]

 当您第一次使用介质或者希望覆盖所有现有数据时可以使用 FORMAT 子句  。 根据需要，可以为新介质指定介质名称和说明。

 > [!IMPORTANT]
 > 当使用 `BACKUP` 语句的 FORMAT 子句时要十分小心，因为它会破坏以前存储在备份介质中的所有备份  。

### <a name="examples"></a><a name="TsqlExample"></a> 示例

对于以下示例，使用以下 Transact-SQL 代码创建测试数据库：

```sql
USE [master]
GO

CREATE DATABASE [SQLTestDB]
GO

USE [SQLTestDB]
GO
CREATE TABLE SQLTest (
   ID INT NOT NULL PRIMARY KEY,
   c1 VARCHAR(100) NOT NULL,
   dt1 DATETIME NOT NULL DEFAULT GETDATE()
)
GO

USE [SQLTestDB]
GO

INSERT INTO SQLTest (ID, c1) VALUES (1, 'test1')
INSERT INTO SQLTest (ID, c1) VALUES (2, 'test2')
INSERT INTO SQLTest (ID, c1) VALUES (3, 'test3')
INSERT INTO SQLTest (ID, c1) VALUES (4, 'test4')
INSERT INTO SQLTest (ID, c1) VALUES (5, 'test5')
GO

SELECT * FROM SQLTest
GO
```

#### <a name="a-back-up-to-a-disk-device"></a>A. 备份到磁盘设备

下面的示例通过使用 `SQLTestDB` 创建新的介质集，将整个 `FORMAT` 数据库备份到磁盘。

```sql
USE SQLTestDB;
GO
BACKUP DATABASE SQLTestDB
TO DISK = 'c:\tmp\SQLTestDB.bak'
   WITH FORMAT,
      MEDIANAME = 'SQLServerBackups',
      NAME = 'Full Backup of SQLTestDB';
GO
```

#### <a name="b-back-up-to-a-tape-device"></a>B. 备份到磁带设备

 下面的示例将完整的 `SQLTestDB` 数据库备份到磁带上，并将该备份追加到以前的备份中。

```sql
USE SQLTestDB;
GO
BACKUP DATABASE SQLTestDB
   TO TAPE = '\\.\Tape0'
   WITH NOINIT,
      NAME = 'Full Backup of SQLTestDB';
GO
```

#### <a name="c-back-up-to-a-logical-tape-device"></a>C. 备份到逻辑磁带设备

下例为某个磁带驱动器创建一个逻辑备份设备， 然后，将完整的 SQLTestDB 数据库备份到该设备上。

```sql
-- Create a logical backup device,
-- SQLTestDB_Bak_Tape, for tape device \\.\tape0.
USE master;
GO
EXEC sp_addumpdevice 'tape', 'SQLTestDB_Bak_Tape', '\\.\tape0'; USE SQLTestDB;
GO
BACKUP DATABASE SQLTestDB
   TO SQLTestDB_Bak_Tape
   WITH FORMAT,
      MEDIANAME = 'SQLTestDB_Bak_Tape',
      MEDIADESCRIPTION = '\\.\tape0',
      NAME = 'Full Backup of SQLTestDB';
GO
```

## <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell

使用 **Backup-SqlDatabase** cmdlet。 若要显式指示这是完整数据库备份，请使用其默认值 Database 指定 -BackupAction 参数   。 对于完整数据库备份而言，此参数是可选的。

> [!NOTE]
> 这些示例需要安装 SqlServer 模块。 要确定是否已安装该模块，请运行 `Get-Module -Name SqlServer`。 要安装此模块，请在 PowerShell 的管理员会话中运行 `Install-Module -Name SqlServer`。
>
> 有关详细信息，请参阅 [SQL Server PowerShell Provider](../../powershell/sql-server-powershell-provider.md)。

> [!IMPORTANT]
> 如果从 SQL Server Management Studio 中打开 PowerShell 窗口来连接到 SQL Server 的安装，则可省略此示例的凭据部分，因为 SSMS 中的凭据会自动用于建立 PowerShell 和 SQL Server 实例之间的连接。

### <a name="examples"></a>示例

#### <a name="a-full-backup-local"></a>A. 完整备份（本地）

下面的示例在服务器实例 `<myDatabase>` 的默认备份位置创建数据库 `Computer\Instance`的完整数据库备份。 此示例也可以指定 **-BackupAction Database**。

有关完整语法和其他示例，请参阅 [Backup-SqlDatabase](/powershell/module/sqlserver/backup-sqldatabase)。

```powershell
$credential = Get-Credential

Backup-SqlDatabase -ServerInstance Computer[\Instance] -Database <myDatabase> -BackupAction Database -Credential $credential
```

#### <a name="b-full-backup-to-azure"></a>B. 完整备份到 Azure

下面的示例在 `<myDatabase>` 实例上向 Azure Blob 存储服务创建 `<myServer>` 数据库的完整备份。 已经创建具有读取、写入和表权限的存储访问策略。 已使用与存储访问策略相关联的共享访问签名创建 SQL Server 凭据 `https://<myStorageAccount>.blob.core.windows.net/<myContainer>` 。 PowerShell 命令使用 **BackupFile** 参数指定位置 (URL) 和备份文件名。

```powershell
$credential = Get-Credential
$container = 'https://<myStorageAccount>blob.core.windows.net/<myContainer>'
$fileName = '<myDatabase>.bak'
$server = '<myServer>'
$database = '<myDatabase>'
$backupFile = $container + '/' + $fileName

Backup-SqlDatabase -ServerInstance $server -Database $database -BackupFile $backupFile -Credential $credential
```

## <a name="related-tasks"></a><a name="RelatedTasks"></a> Related tasks

- [备份数据库 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)
- [创建差异数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)
- [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)
- [在简单恢复模式下还原数据库备份 (Transact-SQL)](../../relational-databases/backup-restore/restore-a-database-backup-under-the-simple-recovery-model-transact-sql.md)
- [在完整恢复模式下将数据库还原到故障点 (Transact-SQL)](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)
- [将数据库还原到新位置 (SQL Server)](../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)
- [使用维护计划向导](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)

## <a name="see-also"></a>另请参阅

- [SQL Server 备份和还原操作疑难解答](https://support.microsoft.com/kb/224071)
- [备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)
- [事务日志备份 (SQL Server)](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)
- [媒体集、媒体簇和备份集 (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)
- [sp_addumpdevice (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)
- [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)
- [备份数据库（“常规”页）](../../relational-databases/backup-restore/back-up-database-general-page.md)
- [备份数据库（“备份选项”页）](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)
- [差异备份 &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md)
- [完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/full-database-backups-sql-server.md)
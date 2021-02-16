---
description: 教程：将 Azure Blob 存储服务用于 SQL Server 2016
title: 教程：将 Azure Blob 存储服务用于 SQL Server 2016
ms.custom: seo-dt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.technology: ''
ms.prod_service: database-engine
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server 2016
ms.assetid: e69be67d-da1c-41ae-8c9a-6b12c8c2fb61
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4a87a9842ac5fdf05de192d1fc562fe241441c75
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340046"
---
# <a name="tutorial-use-azure-blob-storage-service-with-sql-server-2016"></a>教程：将 Azure Blob 存储服务用于 SQL Server 2016

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
欢迎使用“Microsoft Azure Blob 存储服务中使用 SQL Server 2016”教程。 本教程有助于学习如何将 Microsoft Azure Blob 存储服务用于 SQL Server 数据文件和 SQL Server 备份。  
  
Microsoft Azure Blob 存储服务的 SQL Server 集成支持最初是 SQL Server 2012 Service Pack 1 CU2 的一项增强功能，现在已随 SQL Server 2014 和 SQL Server 2016 进一步增强。 有关此功能的概述以及使用此功能的好处，请参阅 [Microsoft Azure 中的 SQL Server 数据文件](../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md)。 有关实时演示，请参阅 [时间点还原的演示](https://channel9.msdn.com/Blogs/Windows-Azure/File-Snapshot-Backups-Demo)。  

此教程通过多个部分介绍如何在 Microsoft Azure Blob 存储服务中处理 SQL Server 数据文件。 每个部分专注于某个特定任务，应按顺序完成各部分的内容。 首先，将学习如何使用存储访问策略和共享访问签名在 Blob 存储中新建容器。 然后，学习如何创建 SQL Server 凭据以将 SQL Server 与 Azure Blob 存储相集成。 接下来，将数据库备份到 Blob 存储，并将其还原到 Azure 虚拟机。 然后，使用 SQL Server 2016 文件快照事务日志备份还原到某个时间点和新的数据库。 最后，本教程会演示元数据系统存储过程和函数的使用方法，帮助了解和使用文件快照备份。
  
## <a name="prerequisites"></a>先决条件

若要完成本教程，你必须熟悉 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 备份和还原概念以及 T-SQL 语法。 若要使用本教程，你需要一个 Azure 存储帐户、SQL Server Management Studio (SSMS)、本地 SQL Server 实例的访问权限、运行 SQL Server 2016 的 Azure 虚拟机 (VM) 的访问权限和一个 AdventureWorks2016 数据库。 此外，用于发出 BACKUP 和 RESTORE 命令的帐户应属于具有“更改任意凭据”权限的 db_backupoperator数据库角色。 

- 获取免费的 [Azure 帐户](https://azure.microsoft.com/offers/ms-azr-0044p/)。
- 创建 [Azure 存储帐户](/azure/storage/common/storage-quickstart-create-account?tabs=portal)。
- 安装 [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads)。
- 预配[运行 SQL Server 的 Azure VM](https://azure.microsoft.com/services/virtual-machines/sql-server/)
- 安装 [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)。
- 下载 [AdventureWorks2016 示例数据库](../samples/adventureworks-install-configure.md)。
- 将用户帐户分配到 [db_backupoperator](./security/authentication-access/database-level-roles.md) 角色，并授予[更改任意凭据](../t-sql/statements/alter-credential-transact-sql.md)权限。 

## <a name="1---create-stored-access-policy-and-shared-access-storage"></a>1 - 创建存储访问策略和共享访问存储

本部分将介绍如何通过使用存储访问策略使用 [Azure PowerShell](/powershell/azure/) 脚本在 Azure blob 容器上创建共享访问签名。  
  
> [!NOTE]  
> 该脚本是使用 Azure PowerShell 5.0.10586 编写的。  
  
共享访问签名是一个 URI，它授予对容器、Blob、队列或表的受限访问权限。 存储访问策略在服务器端的共享访问签名之外又增加了一级控制，包括撤消、终止或扩展访问。 使用此项新增强时，需要在容器上创建至少具有读取、写入和列表权限的策略。  
  
可以使用 Azure PowerShell、Azure 存储 SDK、Azure REST API 或第三方实用程序创建存储访问策略和共享访问签名。 本教程演示了如何使用 Azure PowerShell 脚本来完成此任务。 该脚本使用资源管理器部署模型并创建以下新资源  
  
-   资源组   
-   存储帐户  
-   Azure blob 容器   
-   SAS 策略    

此脚本首先通过声明一些变量以指定以上资源的名称和以下必需的输入值的名称：  
  
-   用于命名其他资源对象的前缀名称    
-   订阅名称    
-   数据中心位置  

  
此脚本将通过生成适当的 CREATE CREDENTIAL 语句完成，此语句是将在 [2 - 使用共享访问签名创建 SQL Server 凭据](#2---create-a-sql-server-credential-using-a-shared-access-signature)中使用的。 此语句已被复制到剪贴板，并将输出到控制台供你使用。  
  
若要在容器上创建策略并生成共享访问签名 (SAS) 密钥，请按以下步骤进行操作：  
  
1.  打开 Window PowerShell 或 Windows PowerShell ISE（请参阅以上版本要求）。  
  
2.  编辑，然后执行以下脚本：  
  
    ```powershell
    # Define global variables for the script  
    $prefixName = '<a prefix name>'  # used as the prefix for the name for various objects  
    $subscriptionID = '<your subscription ID>'   # the ID  of subscription name you will use  
    $locationName = '<a data center location>'  # the data center region you will use  
    $storageAccountName= $prefixName + 'storage' # the storage account name you will create or use  
    $containerName= $prefixName + 'container'  # the storage container name to which you will attach the SAS policy with its SAS token  
    $policyName = $prefixName + 'policy' # the name of the SAS policy 

    # Set a variable for the name of the resource group you will create or use  
    $resourceGroupName=$prefixName + 'rg'   
  
    # Add an authenticated Azure account for use in the session   
    Connect-AzAccount    
  
    # Set the tenant, subscription and environment for use in the rest of   
    Set-AzContext -SubscriptionId $subscriptionID   
  
    # Create a new resource group - comment out this line to use an existing resource group  
    New-AzResourceGroup -Name $resourceGroupName -Location $locationName   
  
    # Create a new Azure Resource Manager storage account - comment out this line to use an existing Azure Resource Manager storage account  
    New-AzStorageAccount -Name $storageAccountName -ResourceGroupName $resourceGroupName -Type Standard_RAGRS -Location $locationName   
  
    # Get the access keys for the Azure Resource Manager storage account  
    $accountKeys = Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName  
  
    # Create a new storage account context using an Azure Resource Manager storage account  
    $storageContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $accountKeys[0].Value

    # Creates a new container in blob storage  
    $container = New-AzStorageContainer -Context $storageContext -Name $containerName  
  
    # Sets up a Stored Access Policy and a Shared Access Signature for the new container  
    $policy = New-AzStorageContainerStoredAccessPolicy -Container $containerName -Policy $policyName -Context $storageContext -StartTime $(Get-Date).ToUniversalTime().AddMinutes(-5) -ExpiryTime $(Get-Date).ToUniversalTime().AddYears(10) -Permission rwld

    # Gets the Shared Access Signature for the policy  
    $sas = New-AzStorageContainerSASToken -name $containerName -Policy $policyName -Context $storageContext
    Write-Host 'Shared Access Signature= '$($sas.Substring(1))''  

    # Sets the variables for the new container you just created
    $container = Get-AzStorageContainer -Context $storageContext -Name $containerName
    $cbc = $container.CloudBlobContainer 
  
    # Outputs the Transact SQL to the clipboard and to the screen to create the credential using the Shared Access Signature  
    Write-Host 'Credential T-SQL'  
    $tSql = "CREATE CREDENTIAL [{0}] WITH IDENTITY='Shared Access Signature', SECRET='{1}'" -f $cbc.Uri,$sas.Substring(1)   
    $tSql | clip  
    Write-Host $tSql 

    # Once you're done with the tutorial, remove the resource group to clean up the resources. 
    # Remove-AzResourceGroup -Name $resourceGroupName  
    ```  

3.  脚本完成后，CREATE CREDENTIAL 语句将位于剪贴板中，以便在下一部分中使用。  


## <a name="2---create-a-sql-server-credential-using-a-shared-access-signature"></a>2 - 使用共享访问签名创建 SQL Server 凭据

本部分将介绍如何创建凭据以存储 SQL Server 将用于写入上一步中所创建的 Azure 容器或从中读取的安全信息。  
  
SQL Server 凭据是一个对象，用于存储连接到 SQL Server 以外资源所需的身份验证信息。 凭据中存储了存储容器的 URI 路径以及此容器的共享访问签名。  

若要创建 SQL Server 凭据，请执行以下步骤：  
  
1.  连接到 SQL Server Management Studio。  
2.  打开一个新查询窗口，然后连接到本地环境中数据引擎的 SQL Server 实例。  
  
3.  在新查询窗口中，粘贴 CREATE CREDENTIAL 语句以及第 1 部分中的共享访问签名，然后执行该脚本。  
  
    该脚本将类似如下代码。  
  
    ```sql   
    /* Example:
    USE master  
    CREATE CREDENTIAL [https://msfttutorial.blob.core.windows.net/containername] 
    WITH IDENTITY='SHARED ACCESS SIGNATURE'   
    , SECRET = 'sharedaccesssignature' 
    GO */
    
    USE master  
    CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>] 
      -- this name must match the container path, start with https and must not contain a forward slash at the end
    WITH IDENTITY='SHARED ACCESS SIGNATURE' 
      -- this is a mandatory string and should not be changed   
     , SECRET = 'sharedaccesssignature' 
       -- this is the shared access signature key that you obtained in section 1.   
    GO    
    ```  
  
4.  若要查看所有可用的凭据，可在连接到实例的查询窗口中运行以下语句：  
  
    ```sql  
    SELECT * from sys.credentials  
    ```  
  
5.  打开一个新查询窗口，连接到 Azure 虚拟机中数据库引擎的 SQL Server 实例。  
  
6.  在新查询窗口中，粘贴 CREATE CREDENTIAL 语句以及第 1 部分中的共享访问签名，然后执行该脚本。  
  
7.  对想要为其提供 Azure 容器访问权限的任何其他 SQL Server 实例重复执行步骤 5 和 6。  

## <a name="3---database-backup-to-url"></a>3 - 将数据库备份到 URL

本部分将介绍如何将本地 SQL Server 2016 实例中的 AdventureWorks2016 数据库备份到在[第 1 部分](#1---create-stored-access-policy-and-shared-access-storage)中所创建的 Azure 容器。
  
> [!NOTE]  
> 如果要将 SQL Server 2012 SP1 CU2 或更高版本数据库或 SQL Server 2014 数据库备份到此 Azure 容器，则可以通过 [此处](/previous-versions/sql/2014/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-2014&preserve-view=true) 记录的已弃用语法，使用 WITH CREDENTIAL 语法备份到 URL。  
  
若要将数据库备份到 Blob 存储，请执行以下步骤：  
  
1.  连接到 SQL Server Management Studio。  
2.  打开一个新查询窗口，连接到 Azure 虚拟机中数据库引擎的 SQL Server 2016 实例。  
3.  复制以下 Transact-SQL 脚本，然后将其粘贴到查询窗口中。 针对在第 1 部分中指定的存储帐户名称以及容器适当修改 URL，然后执行此脚本。  
  
    ```sql  
  
    -- To permit log backups, before the full database backup, modify the database to use the full recovery model.  
    USE master;  
    ALTER DATABASE AdventureWorks2016  
       SET RECOVERY FULL;  
  
    -- Back up the full AdventureWorks2016 database to the container that you created in section 1  
    BACKUP DATABASE AdventureWorks2016   
       TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_onprem.bak'  
  
    ```  
  
4.  打开对象资源管理器并使用存储帐户和帐户密钥连接到 Azure 存储。 
    1.  展开“容器”，展开第 1 部分中创建的容器，并验证上面步骤 3 中的备份是否显示在此容器中。  
  
   ![连接到 Azure 存储帐户](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/connect-to-azure-storage.png)


## <a name="4----restore-database-to-virtual-machine-from-url"></a>4 - 从 URL 将数据库还原到虚拟机

本部分将介绍如何将 AdventureWorks2016 数据库还原到 Azure 虚拟机中的 SQL Server 2016 实例。
  
> [!NOTE]  
> 在本教程中为了简单起见，我们对用于数据库备份的数据和日志文件使用相同的容器。 在生产环境中可能使用多个容器，以及经常使用多个数据文件。 在 SQL Server 2016 中，在备份大型数据库时也可以考虑将备份在多个 blob 上条带化，以便提高备份性能。  
  
若要从 Azure Blob 存储将 AdventureWorks2016 数据库还原到 Azure 虚拟机中的 SQL Server 2016 实例，请按以下步骤进行操作：  
  
1.  连接到 SQL Server Management Studio。   
2.  打开一个新查询窗口，连接到 Azure 虚拟机中数据库引擎的 SQL Server 2016 实例。   
3.  复制以下 Transact-SQL 脚本，然后将其粘贴到查询窗口中。 针对在第 1 部分中指定的存储帐户名称以及容器适当修改 URL，然后执行此脚本。  
  
    ```sql  
    -- Restore AdventureWorks2016 from URL to SQL Server instance using Azure blob storage for database files  
    RESTORE DATABASE AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_onprem.bak'   
       WITH  
          MOVE 'AdventureWorks2016_data' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Data.mdf'  
         ,MOVE 'AdventureWorks2016_log' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Log.ldf'  
    --, REPLACE  
  
    ```  
  
4.  打开对象资源管理器，连接到 Azure SQL Server 2016 实例。   
5.  在对象资源管理器中，展开“数据库”节点，并确认 AdventureWorks2016 数据库已还原（必要时请刷新节点）    
    1. 右键单击 AdventureWorks2016，然后选择“属性”。
    1. 选择“文件”，并确认两个数据库文件的路径为指向 Azure Blob 容器中的 Blob 的 URL（完成后选择“取消”）。
    
     ![Azure VM 上的 AdventureWorks 数据库](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/adventureworks-on-azure-vm.png)

    
8.  在对象资源管理器中，连接到 Azure 存储。
    1. 展开“容器”，展开在第 1 部分中创建的容器，并确认此容器中显示上面步骤 3 中的 AdventureWorks2016_Data.mdf 和 AdventureWorks2016_Log.ldf，以及第 3 部分中的备份文件（必要时请刷新节点）。  
  
   ![Azure 上容器内的数据文件](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/data-files-in-container.png)

## <a name="5---backup-database-using-file-snapshot-backup"></a>5 - 使用文件快照备份来备份数据库

在本部分中，将使用文件快照备份来备份 Azure 虚拟机中的 AdventureWorks2016 数据库，以便使用 Azure 快照执行近乎即时的备份。 有关文件快照备份的详细信息，请参阅 [Azure 中数据库文件的文件快照备份](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)。  
  
若要使用文件快照备份来备份 AdventureWorks2016 数据库，请执行以下步骤：  
  
1.  连接到 SQL Server Management Studio。   
2.  打开一个新查询窗口，连接到 Azure 虚拟机中数据库引擎的 SQL Server 2016 实例。  
3.  复制以下 Transact-SQL 脚本，将其粘贴到此查询窗口中并执行（请勿关闭此查询窗口 - 在步骤 5 中将再次执行此脚本。） 通过此系统存储过程可查看包含指定数据库的每个文件的现有的文件快照备份。 你会注意到此数据库没有文件快照备份。  
  
    ```sql  
    -- Verify that no file snapshot backups exist  
    SELECT * FROM sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
    ```  
  
4.  复制以下 Transact-SQL 脚本，然后将其粘贴到查询窗口中。 针对在第 1 部分中指定的存储帐户名称以及容器适当修改 URL，然后执行此脚本。 请注意此备份的速度。  
  
    ```sql  
    -- Backup the AdventureWorks2016 database with FILE_SNAPSHOT  
    BACKUP DATABASE AdventureWorks2016   
       TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Azure.bak'   
       WITH FILE_SNAPSHOT;    
    ```  
  
5.  在确认步骤 4 中的脚本已成功执行之后，再次执行下面的脚本。 请注意，步骤 4 中的文件快照备份操作生成了数据文件和日志文件的文件快照。  
  
    ```sql  
    -- Verify that two file-snapshot backups exist  
    SELECT * FROM sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
  
    ```  
  
    ![显示快照的 fn_db_backup_file_snapshots 结果](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/results-showing-snapshot.png) 
  
6.  在对象资源管理器中，在 Azure 虚拟机的 SQL Server 2016 实例中展开“数据库”节点，并确认 AdventureWorks2016 数据库已还原成此实例（必要时请刷新节点）。  
7.  在对象资源管理器中，连接到 Azure 存储。   
8.  展开“容器”，展开在第 1 部分中创建的容器，并确认此容器中显示上面步骤 4 中的 AdventureWorks2016_Azure.bak，以及第 3 部分中的备份文件和第 4 部分中的数据库文件（必要时请刷新节点）。  
  
    ![Azure 上的快照备份](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/snapshot-backup-on-azure.PNG)

## <a name="6----generate-activity-and-backup-log-using-file-snapshot-backup"></a>6 - 使用文件快照备份生成活动和备份日志

本部分将介绍如何在 AdventureWorks2016 数据库中生成活动，以及如何使用文件快照备份定期创建事务日志备份。 有关使用文件快照备份的详细信息，请参阅 [Azure 中数据库文件的文件快照备份](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)。  
  
若要在 AdventureWorks2016 数据库中生成活动并使用文件快照备份定期创建事务日志备份，请执行以下步骤：  
  
1.  连接到 SQL Server Management Studio。   
2.  打开两个新查询窗口，将每个连接到 Azure 虚拟机中的数据库引擎的 SQL Server 2016 实例。   
3.  复制以下 Transact-SQL 脚本，将其粘贴到其中一个查询窗口中，并执行。 请注意，在步骤 4 中添加新行之前，Production.Location 表有 14 行。  
  
    ```sql 
    -- Verify row count at start  
    SELECT COUNT (*) from AdventureWorks2016.Production.Location;    
    ```  
  
4.  复制以下两个 Transact-SQL 脚本，将它们分别粘贴到这两个不同的查询窗口中。 针对在第 1 部分中指定的存储帐户名称以及容器适当修改 URL，然后同时执行这两个查询窗口中的这些脚本。 这些脚本完成时间大约需要 7 分钟。  
  
    ```sql  
    -- Insert 30,000 new rows into the Production.Location table in the AdventureWorks2014 database in batches of 75  
    DECLARE @count INT=1, @inner INT;  
    WHILE @count < 400  
       BEGIN  
          BEGIN TRAN;  
             SET @inner =1;  
                WHILE @inner <= 75  
                   BEGIN;  
                      INSERT INTO AdventureWorks2016.Production.Location    
                         (Name, CostRate, Availability, ModifiedDate)   
                            VALUES (NEWID(), .5, 5.2, GETDATE());  
                      SET @inner = @inner + 1;  
                   END;  
          COMMIT;  
       WAITFOR DELAY '00:00:01';   
       SET @count = @count + 1;  
       END;  
    SELECT COUNT (*) from AdventureWorks2014.Production.Location;    
    ```  
  
    ```sql  
    --take 7 transaction log backups with FILE_SNAPSHOT, one per minute, and include the row count and the execution time in the backup file name   
    DECLARE @count INT=1, @device NVARCHAR(120), @numrows INT;  
    WHILE @count <= 7  
       BEGIN  
             SET @numrows = (SELECT COUNT (*) FROM AdventureWorks2016.Production.Location);  
             SET @device = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/tutorial-' + CONVERT (varchar(10),@numrows) + '-' + FORMAT(GETDATE(), 'yyyyMMddHHmmss') + '.bak';  
             BACKUP LOG AdventureWorks2016 TO URL = @device WITH FILE_SNAPSHOT;  
             SELECT * from sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
          WAITFOR DELAY '00:1:00';   
             SET @count = @count + 1;  
       END;  
    ```  
  
5.  查看第一个脚本的输出，并注意最后的行数现在为 29,939。  
  
    ![行数显示为 29,939](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/29-thousand-rows.png)
  
6.  查看第二个脚本的输出，并注意每执行一次 BACKUP LOG 语句就会创建两个新文件快照，一个是日志文件的文件快照，另一个是数据文件的文件快照 — 每个数据库文件共有两个文件快照。 第二个脚本完成后，请注意现在共有 16 个文件快照，每个数据库文件有 8 个 — 一个来自 BACKUP DATABASE 语句，另一个是 BACKUP LOG 语句每次执行的结果。  
  
   ![备份快照结果](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/snapshot-back-results.png)
  
7.  在对象资源管理器中，连接到 Azure 存储。  
  
8.  展开“容器”，展开在第 1 部分中创建的容器，并确认此容器中显示 7 个新的备份文件以及之前部分中的数据文件（必要时刷新节点）。  
  
    ![Azure 容器中的多个快照](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/tutorial-snapshots-in-container.png)

## <a name="7---restore-a-database-to-a-point-in-time"></a>7 - 将数据库还原到一个时间点

在本部分中，需要将 AdventureWorks2016 数据库还原到两个事务日志备份之间的一个时间点。  
  
借助传统备份来完成时间点还原，需要使用完整数据库备份（可能是差异备份）以及所有达到以及刚刚超过想要还原到的时间点的事务日志文件。 借助文件快照备份，则只需要两个相邻的日志备份文件，这些文件提供想要还原到的时间的目标范围。 由于每个日志备份会创建每个数据库文件（每个数据文件和日志文件）的文件快照，因此只需要两个日志文件快照备份集。  
  
要将数据库从文件快照备份集还原到指定的时间点，请执行下列步骤：  
  
1.  连接到 SQL Server Management Studio。  
2.  打开一个新查询窗口，连接到 Azure 虚拟机中数据库引擎的 SQL Server 2016 实例。   
3.  复制以下 Transact-SQL 脚本，将其粘贴到查询窗口中，并执行。 在将 Production.Location 表还原到步骤 5 中它所有的行更少时的某个时间点之前，验证该表是否有 29,939 行。  
  
    ```sql
    -- Verify row count at start  
    SELECT COUNT (*) from AdventureWorks2016.Production.Location   
    ```  
  
    ![行数显示为 29,939](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/29-thousand-rows.png) 
  
4.  复制以下 Transact-SQL 脚本，然后将其粘贴到查询窗口中。 选择两个相邻的日志备份文件并将文件名转换为此脚本所需的日期和时间。 针对在第 1 部分中指定的存储帐户名称以及容器适当修改 URL，提供第一个和第二个备份文件名称，提供“June 26, 2018 01:48 PM”格式的 STOPAT 时间，然后执行此脚本。 此脚本将需要几分钟完成  
  
    ```sql  
    -- restore and recover to a point in time between the times of two transaction log backups, and then verify the row count  
    ALTER DATABASE AdventureWorks2016 SET SINGLE_USER WITH ROLLBACK IMMEDIATE;  
    RESTORE DATABASE AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<firstbackupfile>.bak'   
       WITH NORECOVERY,REPLACE;  
    RESTORE LOG AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<secondbackupfile>.bak'    
       WITH RECOVERY, STOPAT = 'June 26, 2018 01:48 PM';  
    ALTER DATABASE AdventureWorks2016 set multi_user;  
    -- get new count  
    SELECT COUNT (*) FROM AdventureWorks2016.Production.Location ;
    ```  
  
5.  查看输出。 请注意，还原后，行计数为 18,389，介于日志备份 5 和 6 的行计数之间（你的行计数会有所变化）。  
  
    ![18-thousand-rows.JPG](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/18-thousand-rows.png)

## <a name="8----restore-as-new-database-from-log-backup"></a>8 - 从日志备份还原为新数据库

在本部分中，需要将 AdventureWorks2016 数据库作为新数据库从文件快照事务日志备份进行还原。  
  
在此方案中，是要还原到不同虚拟机上的 SQL Server 实例，以便进行业务分析和报告。 还原到不同虚拟机上的不同实例可将工作负荷卸载到针对此用途调整了大小的专用虚拟机，从而从事务系统中消除资源要求。  
  
使用文件快照备份从事务日志备份进行的还原非常快速，比传统流式备份要快得多。 对于传统流式备份，需要使用完整数据库备份（可能要使用差异备份）以及部分或所有事务日志备份（或新的完整数据库备份）。 但是，对于文件快照日志备份，只需要最新日志备份（或是任何其他日志备份或任何两个相邻日志备份以实现到两个日志备份时间之间的某个点的时间点还原）。 为清楚起见，只需要一个日志文件快照备份集，因为每个文件快照日志备份会创建每个数据库文件（每个数据文件和日志文件）的文件快照。  
  
若要使用文件快照备份从事务日志备份将数据库还原到新的数据库，请执行以下步骤：  
  
1.  连接到 SQL Server Management Studio。   
2.  打开一个新查询窗口，连接到 Azure 虚拟机中数据库引擎的 SQL Server 2016 实例。  
  
    > [!NOTE]  
    > 如果此 Azure 虚拟机与你用于之前部分的虚拟机不同，请确保已执行了 [2 -使用共享访问签名创建 SQL Server 凭据](#2---create-a-sql-server-credential-using-a-shared-access-signature)中的步骤。 如果想要还原到其他容器，请按照 [1 - 创建存储访问策略和共享访问存储](#1---create-stored-access-policy-and-shared-access-storage)中的步骤创建新容器。  
  
3.  复制以下 Transact-SQL 脚本，然后将其粘贴到查询窗口中。 选择要使用的日志备份文件。 针对在第 1 部分中指定的存储帐户名称以及容器适当修改 URL，提供日志备份文件名称，然后执行此脚本。  
  
    ```sql  
    -- restore as a new database from a transaction log backup file  
    RESTORE DATABASE AdventureWorks2016_EOM   
        FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<logbackupfile.bak>'    
        WITH MOVE 'AdventureWorks2016_data' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_EOM_Data.mdf'  
       , MOVE 'AdventureWorks2016_log' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_EOM_Log.ldf'  
       , RECOVERY  
    --, REPLACE   
    ```  
  
4.  检查输出以验证还原是否成功。   
5.  在对象资源管理器中，连接到 Azure 存储。    
6.  展开“容器”，展开在第 1 部分中创建的容器（必要时进行刷新），验证新数据和日志文件是否随之前部分中的 Blob 一起出现在容器中。  
  
    ![显示新数据库的数据和日志文件的 Azure 容器](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/new-db-in-azure-container.png)

## <a name="9---manage-backup-sets-and-file-snapshot-backups"></a>9 - 管理备份集和文件快照备份

本部分中，将使用 [sp_delete_backup &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md) 系统存储过程删除备份集。 此系统存储过程会删除每个与此备份集关联的数据库文件中的备份文件和文件快照。  
  
> [!NOTE]  
> 如果试图通过仅从 Azure Blob 容器中删除备份文件来删除备份集，将只删除备份文件本身 - 相关的文件快照将保留。 如果发现自己处于这种情况下，请使用 [sys.fn_db_backup_file_snapshots (Transact-SQL)](../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md) 系统函数来标识孤立文件快照的 URL，并使用 [sp_delete_backup_file_snapshot (Transact-SQL)](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) 系统存储过程来删除每个孤立文件快照。 有关详细信息，请参阅  [Azure 中数据库文件的文件快照备份](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)。  
  
若要删除文件快照备份集，请执行下列步骤：  
  
1.  连接到 SQL Server Management Studio。  
2.  打开一个新查询窗口并连接到 Azure 虚拟机中数据库引擎的 SQL Server 2016 实例（或连接到任何在此容器上具有读写权限的 SQL Server 2016 实例）。   
3.  复制以下 Transact-SQL 脚本，然后将其粘贴到查询窗口中。 选择想要将其及其关联的文件快照删除的日志备份。 针对在第 1 部分中指定的存储帐户名称以及容器适当修改 URL，提供日志备份文件名称，然后执行此脚本。  
  
  ```sql
  sys.sp_delete_backup 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/tutorial-21764-20181003205236.bak';  
  ```  
  
4.  在对象资源管理器中，连接到 Azure 存储。  
5.  展开“容器”，展开第 1 部分中创建的容器，并验证在步骤 3 中使用的备份文件不再出现在此容器中（必要时请刷新节点）。  
  
    ![显示删除日志备份 Blob 的 Azure 容器](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/deleted-backup-snapshot.png)
  
6.  复制以下 Transact-SQL 脚本，将其粘贴到查询窗口中，然后执行以验证是否已删除了这两个文件快照。  
  
    ```sql  
    -- verify that two file snapshots have been removed  
    SELECT * from sys.fn_db_backup_file_snapshots ('AdventureWorks2016');   
    ```  
    ![显示已删除 2 个文件快照的“结果”窗格](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/results-of-two-deleted-snapshot-files.png)

## <a name="10---remove-resources"></a>10 - 删除资源

完成本教程的学习后，为了节省资源，请务必删除本教程中创建的资源组。 

若要删除资源组，请运行以下 powershell 代码：

  ```powershell
  # Define global variables for the script  
  $prefixName = '<prefix name>'  # should be the same as the beginning of the tutorial
  
  # Set a variable for the name of the resource group you will create or use  
  $resourceGroupName=$prefixName + 'rg'   
  
  # Adds an authenticated Azure account for use in the session   
  Connect-AzAccount    
  
  # Set the tenant, subscription and environment for use in the rest of   
  Set-AzContext -SubscriptionId $subscriptionID    
    
  # Remove the resource group
  Remove-AzResourceGroup -Name $resourceGroupName   
  ```


  
## <a name="see-also"></a>另请参阅

[Microsoft Azure 中的 SQL Server 数据文件](../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md)  
[Azure 中数据库文件的文件快照备份](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)  
[将 SQL Server 备份到 URL](../relational-databases/backup-restore/sql-server-backup-to-url.md) 
[共享访问签名，第 1 部分：了解 SAS 模型](/azure/storage/common/storage-sas-overview)  
[创建容器](/rest/api/storageservices/Create-Container)  
[Set Container ACL](/rest/api/storageservices/Set-Container-ACL)  
[Get Container ACL](/rest/api/storageservices/Get-Container-ACL)
[凭据（数据库引擎）](../relational-databases/security/authentication-access/credentials-database-engine.md)  
[CREATE CREDENTIAL &#40;Transact-SQL&#41;](../t-sql/statements/create-credential-transact-sql.md)  
[sys.credentials (Transact-SQL)](../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)  
[sp_delete_backup (Transact-SQL)](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md)  
[sys.fn_db_backup_file_snapshots (Transact-SQL)](../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md)  
[sp_delete_backup_file_snapshot (Transact-SQL)](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) [Azure 中数据库文件的文件快照备份](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)
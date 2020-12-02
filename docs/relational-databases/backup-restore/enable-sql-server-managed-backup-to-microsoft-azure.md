---
title: 使用目标为 Azure 的托管备份
description: 了解如何在数据库和实例级别启用目标为 Microsoft Azure 的 SQL Server 托管备份，以及如何启用通知和监视备份活动。
ms.custom: seo-lt-2019, devx-track-azurecli
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 68ebb53e-d5ad-4622-af68-1e150b94516e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 554bb3164902afa909556ea6836335fdd76dd79a
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96126976"
---
# <a name="enable-sql-server-managed-backup-to-azure"></a>启用目标为 Azure 的 SQL Server 托管备份

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍了如何在数据库级别和实例级别使用默认设置启用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 。 还介绍了如何启用电子邮件通知以及如何监视备份活动。  
  
 本教程使用 Azure PowerShell。 教程开始前， [请下载并安装 Azure PowerShell](/powershell/azure/)。  
  
> [!IMPORTANT]  
>  如果想启用高级选项或使用自定义计划，请在启用 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]之前先配置这些设置。 有关详细信息，请参阅[为目标为 Microsoft Azure 的 SQL Server 托管备份配置高级选项](../../relational-databases/backup-restore/configure-advanced-options-for-sql-server-managed-backup-to-microsoft-azure.md)。  
  
## <a name="create-the-azure-blob-container"></a>创建 Azure Blob 容器

此过程需要一个 Azure 帐户。 如果已有帐户，请转到下一步。 如果没有，可以使用 [免费试用版](https://azure.microsoft.com/pricing/free-trial/) 开始，或者浏览 [购买选项](https://azure.microsoft.com/pricing/purchase-options/)。

有关存储帐户的详细信息，请参阅 [关于 Azure 存储帐户](/azure/storage/common/storage-account-create)。 

#### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. 登录 Azure 帐户。

    ```azurecli-interactive
    az login
    ```
  
1. 创建 Azure 存储帐户。 如果已经有存储帐户，请转到下一步。 以下命令在美国东部地区创建一个名为 `<backupStorage>` 的存储帐户。  
  
    ```azurecli-interactive
    az storage account create -n <backupStorage> -l "eastus" --resource-group <resourceGroup>
    ```  
    
1. 为备份文件创建名为 `<backupContainer>` 的 Blob 容器。
  
    ```azurecli-interactive
    $keys = az storage account keys list --account-name <backupStorage> --resource-group <resourceGroup> | ConvertFrom-Json
    az storage container create --name <backupContainer> --account-name <backupStorage> --account-key $keys[0].value 
    ```  
  
1. 生成共享访问签名 (SAS) 以访问容器。 以下命令为一年后过期的 `<backupContainer>` Blob 容器创建 SAS 令牌。  
  
    ```azurecli-interactive 
    az storage container generate-sas --name <backupContainer> --account-name <backupStorage> --account-key $keys[0].value
    ```

#### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. 以下命令登录到你的 Azure 帐户。

    ```azurepowershell-interactive
    Connect-AzAccount
    Set-AzContext -SubscriptionId "<subscriptionId>"
    ```
  
1. 创建 Azure 存储帐户。 如果已经有存储帐户，请转到下一步。 以下命令在美国东部地区创建一个名为 `<backupStorage>` 的存储帐户。  
  
    ```azurepowershell-interactive
    New-AzStorageAccount -StorageAccountName <backupStorage> -Location "EAST US" -ResourceGroupName <resourceGroup> -SkuName Standard_GRS
    ```   
  
1. 为备份文件创建名为 `<backupContainer>` 的 Blob 容器。  
  
    ```azurepowershell-interactive
    $context = New-AzStorageContext -StorageAccountName <backupStorage> -StorageAccountKey (Get-AzStorageAccountKey -Name <backupStorage> -ResourceGroupName <resourceGroup>).Value[0]  
    New-AzStorageContainer -Name <backupContainer> -Context $context  
    ```  
  
1. 生成共享访问签名 (SAS) 以访问容器。 以下命令为一年后过期的 `<backupContainer>` Blob 容器创建 SAS 令牌。
  
    ```azurepowershell-interactive 
    New-AzStorageContainerSASToken -Name <backupContainer> -Permission rwdl -ExpiryTime (Get-Date).AddYears(1) -FullUri -Context $context 
    ```

* * *

> [!NOTE]
> 也可使用 [Azure 门户](https://portal.azure.com/)完成这些步骤。

输出将包含容器的 URL 和/或 SAS 令牌。 以下是一个示例：  
  
  `https://managedbackupstorage.blob.core.windows.net/backupcontainer?sv=2014-02-14&sr=c&sig=xM2LXVo1Erqp7LxQ%9BxqK9QC6%5Qabcd%9LKjHGnnmQWEsDf%5Q%se=2015-05-14T14%3B93%4V20X&sp=rwdl`
  
如果包含 URL，请将其与问号处的 SAS 令牌分开（不包括问号）。 例如，前面的输出会得出以下两个值。  
  
|类型|输出|  
|-|-|  
|**容器 URL**|https://managedbackupstorage.blob.core.windows.net/backupcontainer|  
|**SAS 令牌**|sv=2014-02-14&sr=c&sig=xM2LXVo1Erqp7LxQ%9BxqK9QC6%5Qabcd%9LKjHGnnmQWEsDf%5Q%se=2015-05-14T14%3B93%4V20X&sp=rwdl|  
|||
  
将容器 URL 和 SAS 记录下来，以便在创建 SQL 凭据时使用。 若要详细了解 SAS，请参阅[共享访问签名，第 1 部分：了解 SAS 模型](/azure/storage/common/storage-sas-overview)。  
  
## <a name="enable-managed-backup-to-azure"></a>启用目标为 Azure 的托管备份
  
1.  **创建 SAS URL 的 SQL 凭据：** 使用 SAS 令牌来创建 Blob 容器 URL 的 SQL 凭据。 在 SQL Server Management Studio 中，使用下列 Transact-SQL 查询来创建 Blob 容器 URL 的凭据，示例如下：  
  
    ```sql  
    CREATE CREDENTIAL [https://managedbackupstorage.blob.core.windows.net/backupcontainer]   
    WITH IDENTITY = 'Shared Access Signature',  
    SECRET = 'sv=2014-02-14&sr=c&sig=xM2LXVo1Erqp7LxQ%9BxqK9QC6%5Qabcd%9LKjHGnnmQWEsDf%5Q%se=2015-05-14T14%3B93%4V20X&sp=rwdl'  
    ```  
  
2.  **确保 SQL Server 代理服务已启动且正在运行：** 如果 SQL Server 代理当前未运行，请启动它。  [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 需要实例上运行有 SQL Server 代理才能执行备份操作。  可能要将 SQL Server 代理设置为自动运行，以确保可定期进行备份操作。  
  
3.  **确定保持期：** 确定备份文件的保持期。 以天为单位指定保持期，范围可为 1 到 30。  
  
4.  **启用和配置 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]：** 启动 SQL Server Management Studio 并连接到目标 SQL Server 实例。 在根据要求修改数据库名称、容器 URL 和保持期的值后，从查询窗口中运行以下语句：  
  
    > [!IMPORTANT]  
    >  若要在实例级别启用托管备份，请为 `NULL` 参数指定 `database_name` 。  
  
    ```sql  
    USE msdb;  
    GO  
    EXEC msdb.managed_backup.sp_backup_config_basic   
     @enable_backup = 1,   
     @database_name = 'yourdatabasename',  
     @container_url = 'https://managedbackupstorage.blob.core.windows.net/backupcontainer',   
     @retention_days = 30  
    GO  
    ```  
  
     [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 。 数据库上的备份操作最多可能需要 15 分钟才能运行。  
  
5.  **查看扩展事件默认配置：** 通过运行以下 transact-SQL 语句，检查扩展事件设置。  
  
    ```sql
    SELECT * FROM msdb.managed_backup.fn_get_current_xevent_settings()  
    ```  
  
     应看到默认情况下启用管理、操作和分析通道事件，且无法禁用这些事件。 这对于需要手动干预的事件来说应当已经足够。  您可以启用调试事件，但是调试通道包含 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 用来检测问题和解决问题的信息性事件和调试事件。  
  
6.  **启用和配置运行状况通知：** [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 有一个存储过程，它创建代理作业以发出可能需要注意的错误或警告的电子邮件通知。 以下步骤说明了启用和配置电子邮件通知的过程：  
  
    1.  如果尚未在此实例上启用数据库邮件，请进行设置。 有关详细信息，请参阅 [Configure Database Mail](../../relational-databases/database-mail/configure-database-mail.md)。  
  
    2.  将 SQL Server 代理通知配置为使用数据库邮件。 有关详细信息，请参阅 [Configure SQL Server Agent Mail to Use Database Mail](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)。  
  
    3.  **启用电子邮件通知以接收备份错误和警告：** 在查询窗口中，运行以下 Transact-SQL 语句：  
  
        ```sql
        EXEC msdb.managed_backup.sp_set_parameter  
        @parameter_name = 'SSMBackup2WANotificationEmailIds',  
        @parameter_value = '<email1;email2>'  
        ```  
  
7.  **查看 Azure 存储帐户中的备份文件：** 从 SQL Server Management Studio 或 Azure 门户连接到存储帐户。 你将在指定容器中看到任何备份文件。 请注意，你会在启用数据库的 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 的 5 分钟内看到数据库和日志备份。  
  
8.  **监视运行状态：** 可以通过以前配置的电子邮件通知进行监视，也可以主动监控记录的事件。 以下是一些用于查看事件的示例 Transact-SQL 语句：  
  
    ```sql  
    --  view all admin events  
    USE msdb;  
    GO  
    DECLARE @startofweek datetime  
    DECLARE @endofweek datetime  
    SET @startofweek = DATEADD(Day, 1-DATEPART(WEEKDAY, CURRENT_TIMESTAMP), CURRENT_TIMESTAMP)   
    SET @endofweek = DATEADD(Day, 7-DATEPART(WEEKDAY, CURRENT_TIMESTAMP), CURRENT_TIMESTAMP)  
  
    DECLARE @eventresult TABLE  
    (event_type nvarchar(512),  
    event nvarchar (512),  
    timestamp datetime  
    )  
  
    INSERT INTO @eventresult  
  
    EXEC managed_backup.sp_get_backup_diagnostics @begin_time = @startofweek, @end_time = @endofweek  
  
    SELECT * from @eventresult  
    WHERE event_type LIKE '%admin%'  
    ```  
  
    ```sql  
    -- to enable debug events  
    USE msdb;  
    GO  
    EXEC managed_backup.sp_set_parameter 'FileRetentionDebugXevent', 'True'  
    ```  
  
    ```sql  
    --  View all events in the current week  
    USE msdb;  
    GO  
    DECLARE @startofweek datetime  
    DECLARE @endofweek datetime  
    SET @startofweek = DATEADD(Day, 1-DATEPART(WEEKDAY, CURRENT_TIMESTAMP), CURRENT_TIMESTAMP)   
    SET @endofweek = DATEADD(Day, 7-DATEPART(WEEKDAY, CURRENT_TIMESTAMP), CURRENT_TIMESTAMP)  
  
    EXEC managed_backup.sp_get_backup_diagnostics @begin_time = @startofweek, @end_time = @endofweek;  
    ```
  
本节中描述的步骤是专门用于在数据库上首次配置 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 。 你可以使用相同的系统存储过程来修改现有配置，并提供新值。  
  
## <a name="see-also"></a>另请参阅  
 [目标为 Azure 的 SQL Server 托管备份](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md)

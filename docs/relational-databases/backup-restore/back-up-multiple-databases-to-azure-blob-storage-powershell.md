---
title: 备份多个数据库：Azure Blob 存储
description: 本主题提供一些示例脚本，可用于通过 PowerShell cmdlet 在 SQL Server 中自动备份到 Azure Blob 存储服务。
titleSuffix: PowerShell
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: f7008339-e69d-4e20-9265-d649da670460
author: cawrites
ms.author: chadam
ms.openlocfilehash: ef69119a6e9b18d2bbe8008cf4113a0181ce74eb
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129362"
---
# <a name="back-up-multiple-databases-to-azure-blob-storage---powershell"></a>将多个数据库备份到 Azure Blob 存储 - PowerShell

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题提供一些示例脚本，可用于通过 PowerShell cmdlet 自动备份到 Azure Blob 存储服务。  
  
## <a name="overview-of-powershell-cmdlets-for-backup-and-restore"></a>用于备份和还原的 PowerShell cmdlet 的概述

**Backup-SqlDatabase** 和 **Restore-SqlDatabase** 是两个用于进行备份和还原操作的主要 cmdlet。

此外，可能需要其他 cmdlet 才能自动备份到 Microsoft Azure Blob 存储，如 SqlCredential cmdlet 集  。

有关所有可用 cmdlet 的列表，请参阅 [SqlServer cmdlet](/powershell/module/sqlserver)。
  
> [!TIP]  
> SqlCredential cmdlet 用于备份和还原到 Azure Blob 存储的场景中  。 有关凭据及其使用情况的详细信息，请参阅 [使用 Microsoft Azure Blob 存储服务进行 SQL Server 备份和还原](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)。
  
### <a name="powershell-for-multi-database-multi-instance-backup-operations"></a>用于多数据库、多实例备份操作的 PowerShell

以下几个部分包含用于多种操作的脚本，如在多个 SQL Server 实例上创建 SQL 凭据、备份 SQL Server 实例中的所有用户数据库等。 可使用这些脚本根据所处环境的要求自动进行备份操作或安排备份操作。 此处提供的脚本仅为示例，可针对所处环境修改或扩展这些脚本。  
  
以下是这些示例脚本的注意事项：  
  
- SQL Server PowerShell 实现了一些 cmdlet，可以导航表示 PowerShell 提供程序支持的对象层次结构的路径结构。 在您导航到该路径中的节点时，可以使用其他 cmdlet 以便针对当前对象执行基本操作。

  有关详细信息，请参阅 [Navigate SQL Server PowerShell Paths](../../powershell/navigate-sql-server-powershell-paths.md)。

- Get-ChildItem  cmdlet：Get-ChildItem  返回的信息取决于 SQL Server PowerShell 路径中的位置。 例如，如果该位置在计算机级别，则此 cmdlet 返回计算机上安装的所有 SQL Server 数据库引擎实例。 或者，如果该位置在数据库等对象级别，则此 cmdlet 返回数据库对象的列表。 默认情况下， **Get-ChildItem** cmdlet 不返回系统对象。 使用 `–Force` 参数查看系统对象。

- Azure 存储帐户和 SQL 凭据是必需的先决条件，适用于 Azure Blob 存储服务的所有备份和还原操作。
  
### <a name="create-a-sql-credential-on-all-instances-of-sql-server"></a>在所有 SQL Server 实例上创建 SQL 凭据

以下脚本可用于在计算机上的所有 SQL Server 实例上创建通用 SQL 凭据。 如果计算机的某个实例上已有同名的现有凭据，则该脚本显示错误并继续运行。  
  
```powershell
# load the sqlps module
import-module sqlps  
  
# set parameters
$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$storageAccount = "<myStorageAccount>"  
$storageKey = "<myStorageAccessKey>"  
$secureString = ConvertTo-SecureString $storageKey -AsPlainText -Force  
$credentialName = "myCredential-$(Get-Random)"

Write-Host "Generate credential: " $credentialName
  
#cd to sql server and get instances  
cd $sqlPath
$instances = Get-ChildItem

#loop through instances and create a SQL credential, output any errors
foreach ($instance in $instances)  {
    try {
        $path = "$($sqlPath)\$($instance.DisplayName)\credentials"
        New-SqlCredential -Name $credentialName -Identity $storageAccount -Secret $secureString -Path $path -ea Stop | Out-Null
        Write-Host "...generated credential $($path)\$($credentialName)."  }
    catch { Write-Host $_.Exception.Message } }
```

> [!NOTE]
> `-ea Stop | Out-Null` 语句用于用户定义的异常输出。 如果首选默认的 PowerShell 错误消息，则可删除此语句。 

### <a name="remove-a-sql-credential-from-all-instances-of-sql-server"></a>从所有 SQL Server 实例中删除 SQL 凭据

以下脚本可用于从计算机上安装的所有 SQL Server 实例中删除某个特定凭据。 如果某个特定实例上不存在凭据对象，则显示一条错误消息，而该脚本继续运行，直到检查完所有实例。  
  
```powershell
import-module sqlps

$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$credentialName = "<myCredential>"

Write-Host "Delete credential: " $credentialName

cd $sqlPath
$instances = Get-ChildItem

#loop through instances and delete a SQL credential
foreach ($instance in $instances)  {
    try {
        $path = "$($sqlPath)\$($instance.DisplayName)\credentials\$($credentialName)"
        Remove-SqlCredential -Path $path -ea Stop | Out-Null
        Write-Host "...deleted credential $($path)."  }
    catch { Write-Host $_.Exception.Message } }
```  
  
### <a name="full-backup-for-all-databases"></a>所有数据库的完整备份

以下脚本创建计算机上所有数据库的备份。 其中包括用户数据库和 **msdb** 系统数据库。 该脚本筛选出 **tempdb** 和 **model** 系统数据库。  
  
```powershell
import-module sqlps  

$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$storageAccount = "<myStorageAccount>"  
$blobContainer = "<myBlobContainer>"  
$backupUrlContainer = "https://$storageAccount.blob.core.windows.net/$blobContainer/"  
$credentialName = "<myCredential>"

Write-Host "Backup database: " $backupUrlContainer
  
cd $sqlPath
$instances = Get-ChildItem

#loop through instances and backup all databases (excluding tempdb and model)
foreach ($instance in $instances)  {
    $path = "$($sqlPath)\$($instance.DisplayName)\databases"
    $databases = Get-ChildItem -Force -Path $path | Where-object {$_.name -ne "tempdb" -and $_.name -ne "model"}

    foreach ($database in $databases) {
        try {
            $databasePath = "$($path)\$($database.Name)"
            Write-Host "...starting backup: " $databasePath
            Backup-SqlDatabase -Database $database.Name -Path $path -BackupContainer $backupUrlContainer -SqlCredential $credentialName -Compression On
            Write-Host "...backup complete."  }
        catch { Write-Host $_.Exception.Message } } }
```  
  
### <a name="full-backup-for-system-databases-only-on-a-specific-instance-of-sql-server"></a>仅位于 SQL Server 的某个特定实例上的系统数据库的完整备份

完整脚本可用于备份 SQL Server 的命名实例上的 **master** 和 **msdb** 数据库。 可以通过更改实例参数值将同一脚本用于任何实例。 SQL Server 的默认实例名为 `DEFAULT`。
  
```powershell
import-module sqlps  

$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$instanceName = "DEFAULT"
$storageAccount = "<myStorageAccount>"  
$blobContainer = "<myBlobContainer>"  
$backupUrlContainer = "https://$storageAccount.blob.core.windows.net/$blobContainer/"  
$credentialName = "<myCredential>"

Write-Host "Backup database: " $instanceName " to " $backupUrlContainer
  
cd "$($sqlPath)\$($instanceName)"

#loop through instance and backup specific databases
$databases = "master", "msdb"  
foreach ($database in $databases) {
    try {
        Write-Host "...starting backup: " $database
        Backup-SqlDatabase -Database $database -BackupContainer $backupUrlContainer -SqlCredential $credentialName -Compression On
        Write-Host "...backup complete." }
    catch { Write-Host $_.Exception.Message } }
```  
  
## <a name="see-also"></a>另请参阅

[使用 Azure Blob 存储服务进行 SQL Server 备份和还原](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)

[从 SQL Server 备份到 URL 的最佳做法和故障排除](../../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)
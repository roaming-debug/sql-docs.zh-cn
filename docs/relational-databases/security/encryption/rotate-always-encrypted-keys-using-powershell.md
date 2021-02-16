---
description: 使用 PowerShell 轮换 Always Encrypted 密钥
title: 使用 PowerShell 轮换 Always Encrypted 密钥 | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: security, sql-database"
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 5117b4fd-c8d3-48d5-87c9-756800769f31
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5d12d2e93c7580e3a70f7f63af085af335bbc5d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340153"
---
# <a name="rotate-always-encrypted-keys-using-powershell"></a>使用 PowerShell 轮换 Always Encrypted 密钥
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

本文提供使用 SqlServer PowerShell 模块来轮换“Always Encrypted”密钥的步骤。 有关如何开始将 SqlServer PowerShell 模块用于 Always Encrypted 的信息，请参阅 [使用 PowerShell 配置 Always Encrypted](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)。

轮换“始终加密”密钥是使用新密钥替换现有密钥的过程。 如果密钥已泄漏，或者为了遵守强制规定必须定期轮换加密密钥的组织的策略或符合性规则，则可能需要轮换密钥。 

“始终加密”使用两种类型的密钥，因此有两个高级密钥轮换工作流：轮换列主密钥和轮换列加密密钥。

* **列加密密钥轮换** - 包括解密使用当前密钥加密的数据，和使用新的列加密密钥对数据进行加密。 由于轮换列加密密钥需要访问密钥和数据库，因此在不分离角色的情况下，只能执行列加密密钥轮换。
* **列主密钥转换** - 包括解密使用当前列主密钥保护的列加密密钥，使用新的列主密钥重新加密列加密密钥，以及更新这两种类型的密钥的元数据。 可以使用或不使用角色分离来完成列主密钥轮换（当使用 SqlServer PowerShell 模块时）。

## <a name="column-master-key-rotation-without-role-separation"></a>不使用角色分离的列主密钥轮换

此部分介绍的列主密钥轮换方法不支持角色分隔安全管理员和 DBA。 下面的一些步骤将针对物理密钥的操作和针对密钥元数据的操作结合在一起，因此，如果组织使用的是 DevOps 模型，或者数据库托管在云中且主要目的是限制云管理员（而不是本地 DBA）访问敏感数据，则推荐使用该工作流。 如果潜在对手包括 DBA，或 DBA 不得有权访问敏感数据，不建议这样做。  


| 任务 | 项目 | 访问纯文本密钥/密钥存储| 访问数据库
|:---|:---|:---|:---
|步骤 1。 在密钥存储中创建新的列主密钥。<br><br>**注意：** SqlServer PowerShell 模块不支持这一步。 若要从命令行完成此任务，需要使用特定于你的密钥存储的工具。 | [创建并存储 Always Encrypted 的列主密钥](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| 是 | 否
|步骤 2. 启动 PowerShell 环境并导入 SqlServer 模块 | [导入 SqlServer 模块](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | 否 | 否
|步骤 3. 连接到服务器和数据库。 | [连接到数据库](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | 否 | 是
|步骤 4. 创建包含新的列主密钥的位置信息的 SqlColumnMasterKeySettings 对象。 SqlColumnMasterKeySettings 是存在于内存中的对象（在 PowerShell 中）。 若要创建该对象，需要使用特定于你的密钥存储的 cmdlet。 |[New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)<br> | 否 | 否
|步骤 5。 在数据库中创建有关新的列主密钥的元数据。 | [New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br><br>**注意：** 实际上，该 cmdlet 发布 [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) 语句来创建密钥元数据。 | 否 | 是
|步骤 6. 如果当前的列主密钥或新的列主密钥存储在 Azure 密钥保管库中，请对 Azure 进行身份验证 | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | 是 | 否
|步骤 7. 开始进行轮换，方法是使用新的列主密钥加密当前使用旧列主密钥保护的每个列加密密钥。 该步骤完成之后，每个受影响的列加密密钥（要轮换的与旧列主密钥关联的密钥）均使用旧和新的列主密钥进行加密，并在数据库元数据中有两个加密的值。| [Invoke-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/invoke-sqlcolumnmasterkeyrotation) | 是 | 是
|步骤 8。 与查询数据库中的加密列（使用旧的列主密钥保护）的所有应用程序的管理员协调，以便他们可以确保应用程序可以访问新的列主密钥。| [创建并存储列主密钥 (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md) | 是 | 否
|步骤 9. 完成轮换<br><br>**注意：** 执行此步骤之前，请确保查询使用旧的列主密钥保护的加密列的所有应用程序已配置为使用新的列主密钥。 如果过早地执行此步骤，一些应用程序可能无法对数据进行解密。 通过从数据库中删除使用旧的列主密钥创建的加密值来完成轮换。 该操作将删除旧的列主密钥和它所保护的列加密密钥之间的关联。 |[Complete-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/complete-sqlcolumnmasterkeyrotation)| 否 | 是
|步骤 10. 从旧的列主密钥中删除元数据。 |[Remove-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnmasterkey)| 否 | 是

> [!NOTE]
> 强烈建议你不要在轮换后永久地删除旧的列主密钥。 相反，应该在其当前密钥存储中保留旧的列主密钥，或将其存档在另一个安全位置。 如果在配置新的列主密钥之前从备份文件中将数据库还原到某个时间点，则需要此旧密钥来访问数据。 

### <a name="rotating-a-column-master-key-without-role-separation-windows-certificate-example"></a>不使用角色分离轮换列主密钥（Windows 证书示例）

以下脚本是一个端到端示例，用于演示使用新的列主密钥 (CMK2) 替换现有的列主密钥 (CMK1)。

```powershell
# Create a new column master key in Windows Certificate Store.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:CurrentUser\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage KeyEncipherment -KeySpec KeyExchange -KeyLength 2048

# Import the SqlServer module
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint

# Create metadata for your new column master key in the database.
$newCmkName = "CMK2"
New-SqlColumnMasterKey -Name $newCmkName -InputObject $database -ColumnMasterKeySettings $newCmkSettings

# Initiate the rotation from the current column master key to the new column master key.
$oldCmkName = "CMK1"
Invoke-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName -TargetColumnMasterKeyName $newCmkName -InputObject $database

# Complete the rotation of the old column master key.
Complete-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName  -InputObject $database

# Remove the old column master key metadata.
Remove-SqlColumnMasterKey -Name $oldCmkName -InputObject $database
```

## <a name="column-master-key-rotation-with-role-separation"></a>使用角色分离的列主密钥轮换

本节中所述的列主密钥轮换工作流可以确保安全管理员和 DBA 之间的角色分离。

> [!IMPORTANT]
> 在执行以下表格中的“访问纯文本密钥/密钥存储” =**是** 的任意步骤之前（访问纯文本密钥或密钥存储的步骤），请确保在另一台安全的非托管数据库的计算机上运行 PowerShell 环境。 有关详细信息，请参阅 [密钥管理的安全注意事项](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management)。

### <a name="part-1-dba"></a>第 1 部分：DBA

DBA 将检索要轮换的列主密钥的元数据，以及与当前列主密钥关联的受影响的列加密密钥的元数据。 DBA 与安全管理员共享所有此信息。


| 任务 | 项目 | 访问纯文本密钥/密钥存储| 访问数据库
|:---|:---|:---|:---
|步骤 1。 启动 PowerShell 环境并导入 SqlServer 模块。 | [导入 SqlServer 模块](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | 否 | 无
|步骤 2. 连接到服务器和数据库。 | [连接到数据库](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | 否 | 是
|步骤 3. 检索旧的列主密钥的元数据。| [Get-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnmasterkey) | 否 | 是
|步骤 4. 检索旧的列主密钥保护的列加密密钥的元数据，包括其加密的值。 | [Get-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnencryptionkey) | 否 | 是
|步骤 5。 共享列主密钥的位置（提供程序名称和列主密钥的密钥路径）和相应的使用旧的列主密钥保护的列加密密钥的加密值。| 请参阅下面的示例。 | 否 | 否

### <a name="part-2-security-administrator"></a>第 2 部分：安全管理员

安全管理员生成新的列主密钥，使用该新的列主密钥重新加密受影响的列加密密钥，与 DBA 共享有关新的列主密钥以及受影响的列加密密钥的新加密值集的信息。

| 任务 | 项目 | 访问纯文本密钥/密钥存储| 访问数据库
|:---|:---|:---|:---
|步骤 1。 从 DBA 处获取旧的列主密钥的位置和相应的使用旧的列主密钥保护的列加密密钥的加密值。|空值<br>请参阅下面的示例。|否| 否
|步骤 2. 在密钥存储中创建新的列主密钥。<br><br>**注意：** SqlServer 模块不支持这一步。 若要从命令行完成此任务，你需要使用特定于密钥存储类型的工具。|[创建并存储 Always Encrypted 的列主密钥](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| 是 | 否
|步骤 3. 启动 PowerShell 环境并导入 SqlServer 模块。 | [导入 SqlServer 模块](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | 否 | 否
|步骤 4. 创建包含 **旧的** 列主密钥的位置信息的 SqlColumnMasterKeySettings 对象。 SqlColumnMasterKeySettings 是存在于内存中的对象（在 PowerShell 中）。 |New-SqlColumnMasterKeySettings| 否 | 否
|步骤 5。 创建包含 **新的** 列主密钥的位置信息的 SqlColumnMasterKeySettings 对象。 SqlColumnMasterKeySettings 是存在于内存中的对象（在 PowerShell 中）。 若要创建该对象，需要使用特定于你的密钥存储的 cmdlet。 | [New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)| 否 | 否
|步骤 6. 如果旧的（当前）列主密钥或新的列主密钥存储在 Azure 密钥保管库中，请对 Azure 进行身份验证。 | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | 是 | 否
|步骤 7. 使用新的列主密钥重新加密当前使用旧列主密钥保护的每个列加密密钥的值。 | [New-SqlColumnEncryptionKeyEncryptedValue](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkeyencryptedvalue)<br><br>**注意：** 调用此 cmdlet 时，将传递旧的和新的列主密钥的 SqlColumnMasterKeySettings 对象，以及要重新加密的列加密密钥的值。|是|否
|步骤 8。 与 DBA 共享新的列主密钥的位置（提供程序名称和列主密钥的密钥路径）以及列加密密钥的新加密值组。| 请参阅下面的示例。 | 否 | 否

> [!NOTE]
> 强烈建议你不要在轮换后永久地删除旧的列主密钥。 相反，应该在其当前密钥存储中保留旧的列主密钥，或将其存档在另一个安全位置。 如果在配置新的列主密钥之前从备份文件中将数据库还原到某个时间点，则需要此旧密钥来访问数据。 

### <a name="part-3-dba"></a>第 3 部分：DBA

DBA 创建新的列主密钥的元数据，并更新受影响的列加密密钥的元数据，以便添加新的加密值集合。 在此步骤中，DBA 还与查询加密列的应用程序的管理员协调，该管理员将确保应用程序可以访问新的列主密钥。 一旦所有应用程序设置为使用新的列主密钥，DBA 将删除旧的加密值集和旧的列主密钥元数据。

| 任务 | 项目 | 访问纯文本密钥/密钥存储| 访问数据库
|:---|:---|:---|:---
|步骤 1。 从安全管理员处获取新的列主密钥的位置和相应的使用旧的列主密钥保护的列加密密钥的新的加密值集。| 请参阅下面的示例。 | 否 | 否
|步骤 2. 启动 PowerShell 环境并导入 SqlServer 模块。 | [导入 SqlServer 模块](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | 否 | 否
|步骤 3. 连接到服务器和数据库。 | [连接到数据库](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | 否 | 是
|步骤 4. 创建包含新的列主密钥的位置信息的 SqlColumnMasterKeySettings 对象。 SqlColumnMasterKeySettings 是存在于内存中的对象（在 PowerShell 中）。 |New-SqlColumnMasterKeySettings| 否| 否
|步骤 5。 在数据库中创建有关新的列主密钥的元数据。|[New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br><br>**注意：** 实际上，该 cmdlet 发布 [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) 语句来创建密钥元数据。 | 否 | 是
|步骤 6. 检索旧的列主密钥保护的列加密密钥的元数据。| [Get-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnencryptionkey)| 否 | 是
|步骤 7. 将新的加密值（使用新的列主密钥生成）添加到每个受影响的列加密密钥的元数据。|[Add-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/add-sqlcolumnencryptionkeyvalue)|否|是
|步骤 8。 与查询数据库中的加密列（使用旧的列主密钥保护）的所有应用程序的管理员协调，以便他们可以确保应用程序可以访问新的列主密钥。|[创建并存储列主密钥 (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| 否|否
|步骤 9. 通过从数据库中删除与旧的列主密钥关联的加密值来完成轮换。<br><br>**注意：** 执行此步骤之前，请确保查询使用旧的列主密钥保护的加密列的所有应用程序已配置为使用新的列主密钥。 如果过早地执行此步骤，一些应用程序可能无法对数据进行解密。<br><br>该步骤将删除旧的列主密钥和它所保护的列加密密钥之间的关联。 | [Complete-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/complete-sqlcolumnmasterkeyrotation)<br><br>或者，可以使用 [Remove-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkeyvalue) | 否|是
|步骤 10. 从数据库中删除旧的列主密钥的元数据| [Remove-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnmasterkey)| 否|是

### <a name="rotating-a-column-master-key-with-role-separation-windows-certificate-example"></a>使用角色分离轮换列主密钥（Windows 证书示例）

以下脚本是端到端示例，用于生成作为 Windows 证书存储中的证书的新列主密钥，轮换现有（当前）的列主密钥，以将其替换为新的列主密钥。 该脚本假设目标数据库包含对某些列加密密钥进行加密的列主密钥，其名称为 CMK1（要轮换的密钥）。

第 1 部分：DBA

```powershell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Retrieve the data about the old column master key, which needs to be rotated.
$oldCmkName = "CMK1"
$oldCmk = Get-SqlColumnMasterKey -Name $oldCmkName -InputObject $database


# Share the location of the old column master key with a Security Administrator, via a CSV file on a share drive.
$oldCmkDataFile = "Z:\oldcmkdata.txt"
"KeyStoreProviderName, KeyPath" > $oldCmkDataFile
$oldCmk.KeyStoreProviderName +", " + $oldCmk.KeyPath >> $oldCmkDataFile


# Find column encryption keys associated with the old column master key and provide the encrypted values of column encryption keys to the Security Administrator, via a CSV file on a share drive.
$ceks = Get-SqlColumnEncryptionKey -InputObject $database
$oldCekValuesFile = "Z:\oldcekvalues.txt"
"CEKName, CEKEncryptedValue" > $oldCekValuesFile 
for($i=0; $i -lt $ceks.Length; $i++){
    if($ceks[$i].ColumnEncryptionKeyValues.Length -eq 2) {
        # This column encryption has 2 encrypted values - let's check, if it is associated with the old column master key.
        if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName -or $ceks[$i].ColumnEncryptionKeyValues[1].ColumnMasterKeyName -eq $oldCmkName) {
            Write-Host $ceks[$i].Name "already has 2 encrypted values and therefore" $oldCmkName + "cannot be rotated"
            exit 1
        }
    }
    if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName) {# This column encryption key has 1 encrypted value that was produced using the old column master key
        # Save the name and the encrypted value of the column encryption key in the file.
        $encryptedValue =  "0x" + -join ($ceks[$i].ColumnEncryptionKeyValues[0].EncryptedValue |  foreach {$_.ToString("X2") } )
        $ceks[$i].Name + "," + $encryptedValue >> $oldCekValuesFile
    }
} 
```


第 2 部分：安全管理员

```powershell
# Obtain the location of the old column master key and the encrypted values of the corresponding column encryption keys, from your DBA, via a CSV file on a share drive.
$oldCmkDataFile = "Z:\oldcmkdata.txt"
$oldCmkData = Import-Csv $oldCmkDataFile
$oldCekValuesFile = "Z:\oldcekvalues.txt"
$oldCekValues = @(Import-Csv $oldCekValuesFile)

# Create a new column master key in Windows Certificate Store.
$storeLocation = "CurrentUser"
$certPath = "Cert:\" + $storeLocation + "\My"
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation $certPath -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module.
Import-Module "SqlServer"

# Create a SqlColumnMasterKeySettings object for your old column master key. 
$oldCmkSettings = New-SqlColumnMasterKeySettings -KeyStoreProviderName $oldCmkData.KeyStoreProviderName -KeyPath $oldCmkData.KeyPath

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation $storeLocation -Thumbprint $cert.Thumbprint


# Prepare a CSV file, you will use to share the encrypted values of column encryption keys, produced using the new column master key.
$newCekValuesFile = "Z:\newcekvalues.txt"
"CEKName, CEKEncryptedValue" > $newCekValuesFile

# Re-encrypt each value with the new column master key and save the new encrypted value in the file.
for($i=0; $i -lt $oldCekValues.Count; $i++){
    # Re-encrypt each value with the new CMK
    $newValue = New-SqlColumnEncryptionKeyEncryptedValue -TargetColumnMasterKeySettings $newCmkSettings -ColumnMasterKeySettings $oldCmkSettings -EncryptedValue $oldCekValues[$i].CEKEncryptedValue
    $oldCekValues[$i].CEKName + ", " + $newValue >> $newCekValuesFile
}

# Share the new column master key data with your DBA, via a CSV file.
$newCmkDataFile = $home + "\newcmkdata.txt"
"KeyStoreProviderName, KeyPath" > $newCmkDataFile
$newCmkSettings.KeyStoreProviderName +", " + $newCmkSettings.KeyPath >> $newCmkDataFile
```


第 3 部分：DBA

```powershell
# Obtain the location of the new column master key and the new encrypted values of the corresponding column encryption keys, from your Security Administrator, via a CSV file on a share drive.
$newCmkDataFile = "Z:\newcmkdata.txt"
$newCmkData = Import-Csv $newCmkDataFile
$newCekValuesFile = "Z:\newcekvalues.txt"
$newCekValues = @(Import-Csv $newCekValuesFile)

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlColumnMasterKeySettings -KeyStoreProviderName $newCmkData.KeyStoreProviderName -KeyPath $newCmkData.KeyPath
# Create metadata for the new column master key in the database.
$newCmkName = "CMK2"
New-SqlColumnMasterKey -Name $newCmkName -InputObject $database -ColumnMasterKeySettings $newCmkSettings


# Get all CEK objects
$oldCmkName = "CMK1"
$ceks = Get-SqlColumnEncryptionKey -InputObject $database
for($i=0; $i -lt $ceks.Length; $i++){
    if($ceks[$i].ColumnEncryptionKeyValues.Length -eq 2) {# This column encryption key has 2 encrypted values. Let's check, if it is associated with the old CMK.
        if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName -or $ceks[$i].ColumnEncryptionKeyValues[1].ColumnMasterKeyName -eq $oldCmkName) {
            Write-Host $ceks[$i].Name "already has 2 encrypted values and therefore" $oldCmkName + "cannot be rotated"
            exit 1
        }
    }
    if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName) {
        # Find the corresponding new encrypted value, received from the Security Administrator.
        $newValueRow = ($newCekValues| Where-Object {$_.CEKName -eq $ceks[$i].Name }[0])
        # Update the column encryption key metadata object by adding the new encrypted value
        Add-SqlColumnEncryptionKeyValue -ColumnMasterKeyName $newCmkName -Name $ceks[$i].Name -EncryptedValue $newValueRow.CEKEncryptedValue -InputObject $database 
    }
}

# Complete the rotation of the current column master key.
Complete-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName  -InputObject $database

# Remove the old column master key.
Remove-SqlColumnMasterKey -Name $oldCmkName -InputObject $database
```

## <a name="rotating-a-column-encryption-key"></a>轮换列加密密钥

轮换列加密密钥包括解密使用要轮换的密钥加密的所有列中的数据，和使用新的列加密密钥对数据进行重新加密。 因为此轮换工作流需要访问密钥和数据库，所以无法在使用角色分隔的情况下执行。 如果表包含使用要轮换的密钥进行加密的列且很大，那么轮换列加密密钥可能需要很长时间才能完成。 因此，组织需要谨慎地计划列加密密钥轮换。

可使用脱机或联机方法轮换列加密密钥。 前一种方法可能速度更快，但应用程序无法向受影响的表写入数据。 后一种方法可能耗时更长，但可限制受影响的表不可用于应用程序的时间间隔。 有关详细信息，请参阅[通过 PowerShell 配置使用 Always Encrypted 的列加密](configure-column-encryption-using-powershell.md)和 [Set-SqlColumnEncryption](/powershell/module/sqlserver/set-sqlcolumnencryption/)。

| 任务 | 项目 | 访问纯文本密钥/密钥存储| 访问数据库
|:---|:---|:---|:---
|步骤 1。 启动 PowerShell 环境并导入 SqlServer 模块。 | [导入 SqlServer 模块](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | 否 | 否
|步骤 2. 连接到服务器和数据库。 | [连接到数据库](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | 否 | 是
|步骤 3. 如果列主密钥（保护要轮换的列加密密钥）存储在 Azure 密钥保管库中，请对 Azure 进行身份验证。 | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | 是 | 否
|步骤 4. 生成新的列加密密钥，使用列主密钥对其加密，并在数据库中创建列加密密钥元数据。  | [New-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkey)<br><br>**注意：** 使用内部生成并加密列加密密钥的 cmdlet 的变体。<br>实际上，该 cmdlet 发布 [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md) 语句来创建密钥元数据。 | 是 | 是
|步骤 5。 查找使用旧的列加密密钥加密的所有列。 | [SQL Server 管理对象 (SMO) 编程指南](../../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md) | 否 | 是
|步骤 6. 为每个受影响的列创建 *SqlColumnEncryptionSettings* 对象。  SqlColumnMasterKeySettings 是存在于内存中的对象（在 PowerShell 中）。 它用于指定列的目标加密方案。 在此情况下，该对象应指定使用新的列加密密钥加密受影响的列。 | [New-SqlColumnEncryptionSettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionsettings) | 否 | 否
|步骤 7. 使用新的列加密密钥重新加密步骤 5 中标识的列。 | [Set-SqlColumnEncryption](/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption)<br><br>**注意：** 此步骤可能需要较长时间。 应用程序无法通过整个操作或部分操作访问表，具体视你选择的方法（联机与脱机）而定。 | 是 | 是
|步骤 8。 删除旧的列加密密钥的元数据。 | [Remove-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkey) | 否 | 是

### <a name="example---rotating-a-column-encryption-key"></a>示例 - 轮换列加密密钥

下面的脚本演示了如何轮换列加密密钥。  此脚本假定，目标数据库中包含一些使用列加密密钥（即要轮换的密钥 CEK1）进行加密的列，但使用列主密钥 CMK1（列主密钥未存储在 Azure Key Vault 中）获得保护。 


```powershell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Generate a new column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cmkName = "CMK1"
$newCekName = "CEK2"
New-SqlColumnEncryptionKey -Name $newCekName -InputObject $database -ColumnMasterKey $cmkName 


# Find all columns encrypted with the old column encryption key, and create a SqlColumnEncryptionSetting object for each column.
$ces = @()
$oldCekName = "CEK1"
$tables = $database.Tables
for($i=0; $i -lt $tables.Count; $i++){
    $columns = $tables[$i].Columns
    for($j=0; $j -lt $columns.Count; $j++) {
        if($columns[$j].isEncrypted -and $columns[$j].ColumnEncryptionKeyName -eq $oldCekName) {
            $threeColPartName = $tables[$i].Schema + "." + $tables[$i].Name + "." + $columns[$j].Name 
            $ces += New-SqlColumnEncryptionSettings -ColumnName $threeColPartName -EncryptionType $columns[$j].EncryptionType -EncryptionKey $newCekName
        }
     }
}

# Re-encrypt all columns, currently encrypted with the old column encryption key, using the new column encryption key.
Set-SqlColumnEncryption -ColumnEncryptionSettings $ces -InputObject $database -UseOnlineApproach -MaxDowntimeInSeconds 120 -LogFileDirectory .

# Remove the old column encryption key metadata.
Remove-SqlColumnEncryptionKey -Name $oldCekName -InputObject $database
```

## <a name="next-steps"></a>后续步骤
- [通过 SQL Server Management Studio 查询使用 Always Encrypted 的列](always-encrypted-query-columns-ssms.md)
- [使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md)
  
## <a name="see-also"></a>另请参阅
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Always Encrypted 密钥管理概述](overview-of-key-management-for-always-encrypted.md) 
- [使用 PowerShell 配置“始终加密”功能](configure-always-encrypted-using-powershell.md)
- [使用 SQL Server Management Studio 轮换 Always Encrypted 密钥](rotate-always-encrypted-keys-using-ssms.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)
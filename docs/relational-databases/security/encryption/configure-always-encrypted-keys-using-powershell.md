---
title: 使用 PowerShell 预配 Always Encrypted 密钥 | Microsoft Docs
description: 了解如何使用 SqlServer PowerShell 模块为 Always Encrypted 设置密钥，以提供对加密密钥和数据库的控制访问权限。
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 3bdf8629-738c-489f-959b-2f5afdaf7d61
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b9b9cbe8eb0a443471f261a59aadcaa5b48fc835
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97405763"
---
# <a name="provision-always-encrypted-keys-using-powershell"></a>使用 PowerShell 预配 Always Encrypted 密钥
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

    
本文提供使用 [SqlServer PowerShell 模块](../../../powershell/sql-server-powershell-provider.md)来预配 Always Encrypted 密钥的步骤。 你可在 [使用或不使用角色分隔的情况下](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md#KeyManagementRoles)使用 PowerShell 预配 Always Encrypted 密钥，控制可访问密钥存储中实际加密密钥的人员和可访问该数据库的人员。 

有关 Always Encrypted 密钥管理的概述（包括一些高级最佳做法建议），请参阅 [Always Encrypted 密钥管理概述](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)。
有关如何开始将 SqlServer PowerShell 模块用于 Always Encrypted 的信息，请参阅 [使用 PowerShell 配置 Always Encrypted](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)。


## <a name="key-provisioning-without-role-separation"></a><a name="KeyProvisionWithoutRoles"></a> 不使用角色分隔的密钥预配

本部分描述的密钥预配方法不支持安全管理员和 DBA 间的角色分隔。 下面的一些步骤将对物理密钥的操作与对密钥元数据的操作组合在一起。 因此，如果组织使用的是 DevOps 模型，或者数据库托管在云中且主要目的是限制云管理员（而不是本地 DBA）访问敏感数据，则推荐使用此密钥预配方法。 如果潜在对手包括 DBA，或 DBA 不得有权访问敏感数据，不推荐使用这种方法。

在运行任何涉及访问纯文本密钥或密钥存储（在下表的 **访问纯文本密钥/密钥存储** 列中标识）的步骤之前，请确保 PowerShell 不是在托管数据库的计算机上运行，而是在另一台安全计算机上运行。 有关详细信息，请参阅 [密钥管理的安全注意事项](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management)。


任务  |项目  |访问纯文本密钥/密钥存储  |访问数据库   
---------|---------|---------|---------
步骤 1。 在密钥存储中创建列主密钥。<br><br>**注意：** SqlServer PowerShell 模块不支持这一步。 若要从命令行完成此任务，请使用特定于所选密钥存储的工具。 |[创建并存储 Always Encrypted 的列主密钥](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md) | 是 | 否     
步骤 2.  启动 PowerShell 环境并导入 SqlServer PowerShell 模块。  |   [使用 PowerShell 配置“始终加密”功能](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)   |    否    | 否         
步骤 3.  连接到服务器和数据库。     |     [连接到数据库](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase)    |    否     | 是         
步骤 4.  创建 *SqlColumnMasterKeySettings* 对象，该对象中包含列主密钥的位置信息。 SqlColumnMasterKeySettings 是存在于内存中的对象（在 PowerShell 中）。 使用特定于密钥存储的 cmdlet。   |     [New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)        |   否      | 否         
步骤 5。  在数据库中创建有关列主密钥的元数据。      |    [New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br><br>**注意：** 实际上，该 cmdlet 发布 [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) 语句来创建密钥元数据。|    否     |    是
步骤 6.  如果列主密钥存储在 Azure 密钥保管库中，请对 Azure 进行身份验证。 | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext)    |  是   | 否         
步骤 7.  生成新的列加密密钥，使用列主密钥对其加密，并在数据库中创建列加密密钥元数据。     |    [New-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkey)<br><br>**注意：** 使用内部生成并加密列加密密钥的 cmdlet 的变体。<br><br>**注意：** 实际上，该 cmdlet 发布 [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md) 语句来创建密钥元数据。  | 是 | 是
  

## <a name="windows-certificate-store-without-role-separation-example"></a>不使用角色分隔的 Windows 证书存储（示例）

此脚本是一个端到端示例，用于生成作为 Windows 证书存储中的证书的列主密钥、生成并加密列加密密钥，并在 SQL Server 数据库中创建密钥元数据。 


```powershell
# Create a column master key in Windows Certificate Store.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:CurrentUser\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings


# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName  -InputObject $database -ColumnMasterKey $cmkName
```

## <a name="azure-key-vault-without-role-separation-example"></a>不使用角色分隔的 Azure 密钥保管库（示例）

此脚本是一个端到端示例，用于预配和配置 Azure 密钥保管库、在该保管库中生成列主密钥、生成并加密列加密密钥，并在 Azure SQL 数据库中创建密钥元数据。


```powershell
# Create a column master key in Azure Key Vault.
Import-Module Az
Connect-AzAccount
$SubscriptionId = "<Azure SubscriptionId>"
$resourceGroup = "<resource group name>"
$azureLocation = "<datacenter location>"
$akvName = "<key vault name>"
$akvKeyName = "<key name>"
$azureCtx = Set-AzConteXt -SubscriptionId $SubscriptionId # Sets the context for the below cmdlets to the specified subscription.
New-AzResourceGroup -Name $resourceGroup -Location $azureLocation # Creates a new resource group - skip, if your desired group already exists.
New-AzKeyVault -VaultName $akvName -ResourceGroupName $resourceGroup -Location $azureLocation # Creates a new key vault - skip if your vault already exists.
Set-AzKeyVaultAccessPolicy -VaultName $akvName -ResourceGroupName $resourceGroup -PermissionsToKeys get, create, delete, list, wrapKey,unwrapKey, sign, verify -UserPrincipalName $azureCtx.Account
$akvKey = Add-AzureKeyVaultKey -VaultName $akvName -Name $akvKeyName -Destination "Software"

# Connect to your database (Azure SQL database).
Import-Module "SqlServer"
$serverName = "<Azure SQL server name>.database.windows.net"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Authentication = Active Directory Integrated"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlAzureKeyVaultColumnMasterKeySettings -KeyURL $akvKey.ID

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Authenticate to Azure
Add-SqlAzureAuthenticationContext -Interactive

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName
```

## <a name="cngksp-without-role-separation-example"></a>不使用角色分隔的 CNG/KSP（示例）

下面的脚本是一个端到端示例，用于在实现下一代加密技术 API (CNG) 的密钥存储中生成列主密钥、生成并加密列加密密钥，并在 SQL Server 数据库中创建密钥元数据。

该示例利用的密钥存储使用 Microsoft 软件密钥存储提供程序。 你可选择将该示例修改为使用另一存储（例如你的硬件安全模块）。 为此，必须确保在计算机上正确安装为设备实现 CNG 的密钥存储提供程序 (KSP)。 需要将 `Microsoft Software Key Storage Provider` 替换为设备的 KSP 名称。


```powershell
# Create a column master key in a key store that has a CNG provider, a.k.a key store provider (KSP).
$cngProviderName = "Microsoft Software Key Storage Provider" # If you have an HSM, you can use a KSP for your HSM instead of a Microsoft KSP
$cngAlgorithmName = "RSA"
$cngKeySize = 2048 # Recommended key size for Always Encrypted column master keys
$cngKeyName = "AlwaysEncryptedKey" # Name identifying your new key in the KSP
$cngProvider = New-Object System.Security.Cryptography.CngProvider($cngProviderName)
$cngKeyParams = New-Object System.Security.Cryptography.CngKeyCreationParameters
$cngKeyParams.provider = $cngProvider
$cngKeyParams.KeyCreationOptions = [System.Security.Cryptography.CngKeyCreationOptions]::OverwriteExistingKey
$keySizeProperty = New-Object System.Security.Cryptography.CngProperty("Length", [System.BitConverter]::GetBytes($cngKeySize), [System.Security.Cryptography.CngPropertyOptions]::None);
$cngKeyParams.Parameters.Add($keySizeProperty)
$cngAlgorithm = New-Object System.Security.Cryptography.CngAlgorithm($cngAlgorithmName)
$cngKey = [System.Security.Cryptography.CngKey]::Create($cngAlgorithm, $cngKeyName, $cngKeyParams)

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlCngColumnMasterKeySettings -CngProviderName $cngProviderName -KeyName $cngKeyName

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName
```

## <a name="key-provisioning-with-role-separation"></a><a name="KeyProvisionWithRoles"></a> 不使用角色分隔的密钥设置

此部分分步介绍了如何配置加密，其中安全管理员无权访问数据库，且数据库管理员无权访问密钥存储或纯文本密钥。


### <a name="security-administrator"></a>安全管理员

在运行任何涉及访问纯文本密钥或密钥存储（在下表的 **访问纯文本密钥/密钥存储** 列中标识）的步骤之前，请确保：
1.  运行 PowerShell 环境的安全计算机并非托管数据库的计算机。
2.  你组织中的 DBA 没有对计算机的访问权限（这会违背角色分隔的目的）。

有关详细信息，请参阅 [密钥管理的安全注意事项](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management)。


任务  |项目  |访问纯文本密钥/密钥存储  |访问数据库  
---------|---------|---------|---------
步骤 1。 在密钥存储中创建列主密钥。<br><br>**注意：** SqlServer 模块不支持这一步。 若要从命令行完成此任务，你需要使用特定于密钥存储类型的工具。     | [创建并存储 Always Encrypted 的列主密钥](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)  |    是    | 否 
步骤 2.  启动 PowerShell 会话并导入 SqlServer 模块。      |     [导入 SqlServer 模块](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule)     | 否 | 否         
步骤 3.  创建 *SqlColumnMasterKeySettings* 对象，该对象中包含列主密钥的位置信息。 *SqlColumnMasterKeySettings* 是存在于内存中的对象（在 PowerShell 中）。 使用特定于密钥存储的 cmdlet。 |      [New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)   | 否         | 否         
步骤 4.  如果列主密钥存储在 Azure 密钥保管库中，请对 Azure 进行身份验证 |    [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext)    |是|否         
步骤 5。  生成列加密密钥，并使用列主密钥对其进行加密，以生成列加密密钥的加密值。     |   [New-SqlColumnEncryptionKeyEncryptedValue](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkeyencryptedvalue)     |    是    | 否        
步骤 6.  向 DBA 提供列主密钥的位置（提供程序名称和列主密钥的密钥路径）以及列加密密钥的加密值。  | 请参阅下面的示例。        |   否      | 否         

### <a name="dba"></a>DBA 

DBA 使用其从安全管理员（上面的步骤 6）接收的信息在数据库中创建和管理 Always Encrypted 密钥元数据。

任务  |项目  |访问纯文本密钥  |访问数据库   
---------|---------|---------|---------
步骤 1。  从安全管理员处获取列主密钥的位置和列加密密钥的加密值。 |请参阅下面的示例。 | 否 | 否
步骤 2.  启动 PowerShell 环境并导入 SqlServer 模块。  | [使用 PowerShell 配置“始终加密”功能](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)  | 否 | 否
步骤 3.  连接到服务器和数据库。 | [连接到数据库](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | 否 | 是
步骤 4.  创建 SqlColumnMasterKeySettings 对象，该对象中包含列主密钥的位置信息。 SqlColumnMasterKeySettings 是存在于内存中的对象。 | New-SqlColumnMasterKeySettings | 否 | 否
步骤 5。 在数据库中创建有关列主密钥的元数据 | [New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br>**注意：** 实际上，该 cmdlet 发布 [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) 语句来创建列主密钥元数据。 | 否 | 是
步骤 6. 在数据库中创建列加密密钥元数据。 | New-SqlColumnEncryptionKey<br>**注意：** DBA 使用仅创建列加密密钥元数据的 cmdlet 的变体。<br>实际上，该 cmdlet 发布 [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md) 语句来创建列加密密钥元数据。 | 否 | 是
  
## <a name="windows-certificate-store-with-role-separation-example"></a>使用角色分隔的 Windows 证书存储（示例）

### <a name="security-administrator"></a>安全管理员


```powershell
# Create a column master key in Windows Certificate Store.
$storeLocation = "CurrentUser"
$certPath = "Cert:" + $storeLocation + "\My"
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation $certPath -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module
Import-Module "SqlServer"

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint

# Generate a column encryption key, encrypt it with the column master key to produce an encrypted value of the column encryption key.
$encryptedValue = New-SqlColumnEncryptionKeyEncryptedValue -TargetColumnMasterKeySettings $cmkSettings

# Share the location of the column master key and an encrypted value of the column encryption key with a DBA, via a CSV file on a share drive
$keyDataFile = "Z:\keydata.txt"
"KeyStoreProviderName, KeyPath, EncryptedValue" > $keyDataFile
$cmkSettings.KeyStoreProviderName + ", " + $cmkSettings.KeyPath + ", " + $encryptedValue >> $keyDataFile

# Read the key data back to verify
$keyData = Import-Csv $keyDataFile
$keyData.KeyStoreProviderName
$keyData.KeyPath
$keyData.EncryptedValue 
```

### <a name="dba"></a>DBA

```powershell
# Obtain the location of the column master key and the encrypted value of the column encryption key from your Security Administrator, via a CSV file on a share drive.
$keyDataFile = "Z:\keydata.txt"
$keyData = Import-Csv $keyDataFile

# Import the SqlServer module
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlColumnMasterKeySettings -KeyStoreProviderName $keyData.KeyStoreProviderName -KeyPath $keyData.KeyPath

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a  column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName -EncryptedValue $keyData.EncryptedValue
```  
 
## <a name="next-steps"></a>后续步骤    
- [通过 PowerShell 配置使用 Always Encrypted 的列加密](configure-column-encryption-using-powershell.md)  
- [使用 PowerShell 轮换 Always Encrypted 密钥](../../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell.md)
- [使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md)

## <a name="see-also"></a>另请参阅    
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)    
- [Always Encrypted 密钥管理概述](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [创建并存储 Always Encrypted 的列主密钥](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)
- [使用 PowerShell 配置“始终加密”功能](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)
- [使用 SQL Server Management Studio 预配 Always Encrypted 密钥](configure-always-encrypted-keys-using-ssms.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)
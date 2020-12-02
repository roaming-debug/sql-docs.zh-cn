---
description: 预配已启用 enclave 的密钥
title: 预配已启用 enclave 的密钥 | Microsoft Docs
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.reviewer: vanto
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: 77a438ee4f495429bbe0eb9c1e98728ecb324009
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91867663"
---
# <a name="provision-enclave-enabled-keys"></a>预配已启用 enclave 的密钥
[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

本文介绍如何预配已启用 enclave 的密钥，这些密钥支持用于[具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves.md) 的服务器端安全 enclave 中的计算。 

[管理 Always Encrypted 密钥](overview-of-key-management-for-always-encrypted.md)的一般准则和流程在预配已启用 enclave 的密钥时适用。 本文介绍特定于具有安全 enclave 的 Always Encrypted 的详细信息。

若要使用 SQL Server Management Studio 或 PowerShell 预配已启用 enclave 的列主密钥，请确保新密钥支持 enclave 计算。 这会导致工具（SSMS 或 PowerShell）生成 `CREATE COLUMN MASTER KEY` 语句，该语句在数据库的列主密钥元数据中设置 `ENCLAVE_COMPUTATIONS`。 有关详细信息，请参阅 [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)。 

该工具还会使用列主密钥对列主属性进行数字签名，并将签名存储在数据库元数据中。 此签名可防止恶意篡改 `ENCLAVE_COMPUTATIONS` 设置。 SQL 客户端驱动程序验证签名之后才会允许使用 enclave。 这使安全管理员能够控制可以在 enclave 内计算哪些列数据。

`ENCLAVE_COMPUTATIONS` 是不可变的，也就是说，在元数据中定义列主密钥后无法更改它。 若要启用使用列加密密钥（使用给定列主密钥进行加密）进行 enclave 计算，需要轮换列主密钥并将它替换为已启用 enclave 的列主密钥。 请参阅[轮换已启用 enclave 的密钥](always-encrypted-enclaves-rotate-keys.md)。

> [!NOTE]
> 当前，SSMS 和 PowerShell 都支持 Azure Key Vault 或 Windows 证书存储中存储的已启用 enclave 的列主密钥。 不支持硬件安全模块（使用 CNG 或 CAPI）。

若要创建已启用 enclave 的列加密密钥，需要确保选择已启用 enclave 的列主密钥来加密新密钥。 

以下部分提供有关如何使用 SSMS 和 PowerShell 预配已启用 enclave 的密钥的更多详细信息。

## <a name="provision-enclave-enabled-keys-using-sql-server-management-studio"></a>使用 SQL Server Management Studio 预配已启用 enclave 的密钥
在 SQL Server Management Studio 18.3 或更高版本中，可以预配：
- 已启用 enclave 的列主密钥（使用“新建列主密钥”对话框）。
- 已启用 enclave 的列加密密钥（使用“新建列加密密钥”对话框）。

> [!NOTE]
> [Always Encrypted 向导](always-encrypted-wizard.md)当前不支持生成已启用 enclave 的密钥。 不过，可以先使用以上对话框创建已启用 enclave 的密钥，然后在运行该向导时，为要加密的列选择现有已启用 enclave 的列加密。

### <a name="provision-enclave-enabled-column-master-keys-with-the-new-column-master-key-dialog"></a>使用“新建列主密钥”对话框预配已启用 enclave 的列主密钥
若要预配已启用 enclave 的列主密钥，请按照[使用“新建列主密钥”对话框预配列主密钥](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog)中的步骤进行操作。 确保选择“允许 enclave 计算”。 请参阅以下屏幕截图：

![允许 enclave 计算](./media/always-encrypted-enclaves/allow-enclave-computations.png)

> [!NOTE]
> 仅当 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例包含正确初始化的安全 enclave 时，“允许 enclave 计算”复选框才会出现。 有关详细信息，请参阅[为 Always Encrypted 配置 enclave 类型](../../../database-engine/configure-windows/configure-column-encryption-enclave-type.md)。

> [!TIP]
> 若要检查列主密钥是否已启用 enclave，请在对象资源管理器中右键单击它，然后选择“属性”。 如果密钥已启用 enclave，则“Enclave 计算:允许”会出现在显示密钥属性的窗口中。 或者，可以使用 [sys.column_master_keys (Transact-SQL)](../../system-catalog-views/sys-column-master-keys-transact-sql.md) 视图。

### <a name="provision-enclave-enabled-column-encryption-keys-with-the-new-column-encryption-key-dialog"></a>使用“新建列加密密钥”对话框预配已启用 enclave 的列加密密钥
若要预配已启用 enclave 的列加密密钥，请按照[使用“新建列加密密钥”对话框预配列加密密钥](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog)中的步骤进行操作。 选择列主密钥时，请确保它已启用 enclave。

> [!TIP]
> 若要检查列加密密钥是否已启用 enclave，请在对象资源管理器中右键单击它，然后选择“属性”。 如果密钥已启用 enclave，则“Enclave 计算:允许”会出现在显示密钥属性的窗口中。

## <a name="provision-enclave-enabled-keys-using-powershell"></a>使用 PowerShell 预配已启用 enclave 的密钥
若要使用 PowerShell 预配已启用 enclave 的密钥，需要 SqlServer Powershell 模块版本 21.1.18179 或更高版本。

一般而言，适用于 Always Encrypted 的 PowerShell 密钥预配工作流（在[使用 PowerShell 预配 Always Encrypted 密钥](configure-always-encrypted-keys-using-powershell.md)中进行了介绍）也适用于已启用 enclave 的密钥。 此部分介绍特定于已启用 enclave 的密钥的详细信息。

SqlServer PowerShell 模块使用 `-AllowEnclaveComputations` 参数扩展了 [New-SqlCertificateStoreColumnMasterKeySettings](/powershell/module/sqlserver/new-sqlcertificatestorecolumnmasterkeysettings) 和 [New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/module/sqlserver/new-sqlazurekeyvaultcolumnmasterkeysettings) cmdlet，以便使你可以在预配过程中指定已启用 enclave 的列主密钥。 任一 cmdlet 都会创建包含列主密钥（存储在 Azure Key Vault 或 Windows 证书存储中）的属性的本地对象。 如果指定，则 `-AllowEnclaveComputations` 属性会在本地对象中将密钥标记为已启用 enclave。 它还会使 cmdlet 访问所引用的列主密钥（在 Azure Key Vault 或 Windows 证书存储中）以对密钥的属性进行数字签名。 为新的已启用 enclave 的列主密钥创建设置对象后，便可以在 [New-SqlColumnMasterKey](/powershell/module/sqlserver/new-sqlcolumnmasterkey) cmdlet 的后续调用中使用它，以在数据库中创建描述新密钥的元数据对象。

预配已启用 enclave 的列加密密钥与预配未启用 enclave 的列加密密钥不同。 只需确保用于加密新的列加密密钥的列主密钥已启用 enclave。

> [!NOTE]
> SqlServer PowerShell 模块当前不支持预配存储在硬件安全模块（使用 CNG 或 CAPI）中的已启用 enclave 的密钥。

### <a name="example---provision-enclave-enabled-keys-using-windows-certificate-store"></a>示例 - 使用 Windows 证书存储预配已启用 enclave 的密钥
下面的端到端示例演示如何预配已启用 enclave 的密钥（将列主密钥存储在 Windows 证书存储中）。 该脚本基于[不使用角色分隔的 Windows 证书存储（示例）](configure-always-encrypted-keys-using-powershell.md#windows-certificate-store-without-role-separation-example)中的示例。 请务必注意 `-AllowEnclaveComputations` 参数在 [New-SqlCertificateStoreColumnMasterKeySettings](/powershell/module/sqlserver/new-sqlcertificatestorecolumnmasterkeysettings) cmdlet 中的使用，这是两个示例中的工作流之间的唯一区别。

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

# Create a SqlColumnMasterKeySettings object for your column master key
# using the -AllowEnclaveComputations parameter.
$cmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint -AllowEnclaveComputations

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName  -InputObject $database -ColumnMasterKey $cmkName
```

### <a name="example---provision-enclave-enabled-keys-using-azure-key-vault"></a>示例 - 使用 Azure Key Vault 预配已启用 enclave 的密钥
下面的端到端示例演示如何预配已启用 enclave 的密钥（将列主密钥存储在 Azure Key Vault 中）。 该脚本基于[不使用角色分隔的 Azure Key Vault（示例）](configure-always-encrypted-keys-using-powershell.md#azure-key-vault-without-role-separation-example)中的示例。 请务必注意已启用 enclave 的密钥与未启用 enclave 的密钥相比，工作流之间的两个差异。 
- 在下面的脚本中，[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/module/sqlserver/new-sqlcertificatestorecolumnmasterkeysettings) 使用 `-AllowEnclaveComputations` 参数使新的列主密钥启用 enclave。 
- 下面的脚本在调用 [New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/module/sqlserver/new-sqlazurekeyvaultcolumnmasterkeysettings) cmdlet 之前调用 [Add-SqlAzureAuthenticationContext](/powershell/module/sqlserver/add-sqlazureauthenticationcontext) cmdlet，以登录 Azure。 先登录 Azure 是必需的，因为 `-AllowEnclaveComputations` 参数会使 New-SqlAzureKeyVaultColumnMasterKeySettings 调用 Azure Key Vault 以对列主密钥的属性进行签名。

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

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Authenticate to Azure - it is required before calling the next cmdlet.
Add-SqlAzureAuthenticationContext -Interactive

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlAzureKeyVaultColumnMasterKeySettings -KeyURL $akvKey.ID -AllowEnclaveComputations

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName
```

## <a name="next-steps"></a>后续步骤
- [查询使用具有安全 enclave 的 Always Encrypted 的列](always-encrypted-enclaves-query-columns.md)
- [使用具有安全 Enclave 的 Always Encrypted 就地配置列加密](always-encrypted-enclaves-configure-encryption.md)
- [为现有加密列启用具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves-enable-for-encrypted-columns.md)
- [使用具有安全 enclave 的 Always Encrypted 开发应用程序](always-encrypted-enclaves-client-development.md) 

## <a name="see-also"></a>另请参阅  
- [教程：通过 SSMS 开始使用具有安全 enclave 的 Always Encrypted](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [管理具有安全 enclave 的 Always Encrypted 的密钥](always-encrypted-enclaves-manage-keys.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
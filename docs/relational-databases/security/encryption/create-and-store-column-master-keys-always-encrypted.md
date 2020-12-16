---
title: 创建并存储 Always Encrypted 的列主密钥
description: 了解如何选择密钥存储并创建 SQL Server Always Encrypted 的列主密钥。
ms.custom: seo-lt-2019
ms.date: 10/31/2019
ms.prod: sql
ms.prod_service: security, sql-database"
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 856e8061-c604-4ce4-b89f-a11876dd6c88
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c9a0dfad97e37325c0990bb8c1786a63a5bf897a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479358"
---
# <a name="create-and-store-column-master-keys-for-always-encrypted"></a>创建并存储 Always Encrypted 的列主密钥
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

列主密钥是“始终加密”功能中使用的保护密钥的密钥，用于对列加密密钥进行加密。  列主密钥必须存储在受信任的密钥存储，并且这些密钥可供需要加密或解密数据的应用程序以及用于配置“始终加密”和管理“始终加密”密钥的工具访问。

本文详细介绍了选择密钥存储和为“始终加密”创建列主密钥。 有关详细概述，请参阅 [Always Encrypted 密钥管理概述](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)。

## <a name="selecting-a-key-store-for-your-column-master-key"></a>为列主密钥选择密钥存储

“始终加密”功能支持使用多个密钥存储来存储“始终加密”列主密钥。 受支持的密钥存储因你使用的驱动程序和版本而异。

将考虑两种高级别密钥存储 - 本地密钥存储和集中式密钥存储。  

###  <a name="local-or-centralized-key-store"></a>本地密钥存储还是集中式密钥存储？

* **本地密钥存储** - 仅供包含本地密钥存储的计算机上的应用程序使用。 换而言之，需要将密钥存储和密钥复制到运行你的应用程序的每台计算机。 本地密钥存储的一个示例为 Windows 证书存储。 使用本地密钥存储时，需要确保密钥存储存在于托管应用程序的每台计算机上，并且计算机中包含应用程序访问使用“始终加密”功能保护的数据所需的列主密钥。 当你第一次预配列主密钥，或者当你更改（轮换）密钥时，需要确保密钥部署到托管你的应用程序的所有计算机。

* **集中式密钥存储** - 为多台计算机上的应用程序服务。 集中式密钥存储的一个示例是 [Azure 密钥保管库](https://azure.microsoft.com/services/key-vault/)。 集中式密钥存储通常使密钥管理更简单，因为你无需维护多台计算机上的列主密钥的多个副本。 确保将应用程序配置为连接到集中式密钥存储。

### <a name="which-key-stores-are-supported-in-always-encrypted-enabled-client-drivers"></a>在启用“始终加密”功能的客户端驱动程序中支持哪种密钥存储？

启用“始终加密”功能的客户端驱动程序为内置了对将“始终加密”功能集成到客户端应用程序的支持的 SQL Server 客户端驱动程序。 启用“始终加密”的驱动程序包括几个内置的常用的密钥存储提供者。 通过某些驱动程序，你还可以实施和注册自定义的列主密钥存储提供程序，这样即使没有内置的提供程序，你也可以使用任何密钥存储。 在决定使用内置提供者还是自定义提供者时，考虑如果使用内置提供者则通常意味着对应用程序的更改很少（在某些情况下，只需要更改数据库连接字符串）。

可用的内置提供者取决于选择哪些驱动程序、驱动程序版本和操作系统。  请参阅特定驱动程序的 Always Encrypted 文档来确定哪些现成的密钥存储受支持，以及驱动程序是否支持自定义的密钥存储提供程序 - [使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md)。

### <a name="which-key-stores-are-supported-in-sql-tools"></a>SQL 工具中支持哪些密钥存储？
SQL Server Management Studio 和 SqlServer PowerShell 模块仅支持位于 Azure Key Vault、Windows 证书存储以及提供下一代加密技术 (CNG) API 或加密 API (CAPI) 的密钥存储中的列主密钥。 

## <a name="creating-column-master-keys-in-windows-certificate-store"></a>创建 Windows 证书存储中的列主密钥    

列主密钥可以是存储在 Windows 证书存储中的证书。 启用 Always Encrypted 的驱动程序不会验证到期日期或证书颁发机构链。 证书仅用作包含公钥和私钥的密钥对。

若要成为有效的列主密钥，证书必须：
* 是 X.509 证书。
* 存储在以下两个证书存储位置之一：本地计算机  或当前用户  。 （若要在本地计算机证书存储位置中创建证书，必须是目标计算机上的管理员。）
* 包含私钥（证书中密钥的推荐长度为 2048 位或更高）。
* 针对密钥交换而创建。


可以有多种方法来创建作为有效的列主密钥的证书，但最简单方法是创建自签名证书。


### <a name="create-a-self-signed-certificate-using-powershell"></a>使用 PowerShell 创建自签名证书

使用 [New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate) cmdlet 创建自签名证书。 以下示例演示如何生成可以用作“始终加密”功能的列主密钥的证书。

```
# New-SelfSignedCertificate is a Windows PowerShell cmdlet that creates a self-signed certificate. The below examples show how to generate a certificate that can be used as a column master key for Always Encrypted.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:CurrentUser\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage KeyEncipherment -KeySpec KeyExchange -KeyLength 2048 

# To create a certificate in the local machine certificate store location you need to run the cmdlet as an administrator.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:LocalMachine\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage KeyEncipherment -KeySpec KeyExchange -KeyLength 2048
```

### <a name="create-a-self-signed-certificate-using-sql-server-management-studio-ssms"></a>使用 SQL Server Management Studio (SSMS) 创建自签名证书

有关详细信息，请参阅[使用 SQL Server Management Studio 预配 Always Encrypted 密钥](configure-always-encrypted-keys-using-ssms.md)。
有关使用 SSMS 并在 Windows 证书存储中存储“始终加密”密钥的分布式教程，请参阅 [《Always Encrypted Wizard tutorial (Windows Certificate Store)》](/azure/azure-sql/database/always-encrypted-certificate-store-configure)（始终加密向导教程（Windows 证书存储））。


### <a name="making-certificates-available-to-applications-and-users"></a>使证书可用于应用程序和用户

如果列主密钥是存储在本地计算机  证书存储位置的一个证书，你需要导出包含私钥的证书并将其导入到托管应用程序（预期要加密或解密存储在加密列中的数据）的所有计算机，或用于配置“始终加密”功能和管理“始终加密”密钥的工具。 此外，每个用户必须被授予本地计算机证书存储位置中存储的证书的读取权限，以便能够使用该证书作为列主密钥。

如果列主密钥是存储在当前用户  证书存储位置的一个证书，你需要导出包含私钥的证书并将其导入到运行应用程序（预期要加密或解密存储在加密列中的数据）的所有用户帐户的当前用户证书存储位置，或用于配置“始终加密”功能和管理“始终加密”密钥（包含这些应用程序/工具的所有计算机上的密钥）的工具。 无需任何权限配置 - 登录计算机后，用户可以访问其当前用户证书存储位置的所有证书。

#### <a name="using-powershell"></a>使用 PowerShell
使用 [Import-PfxCertificate](/powershell/module/pkiclient/import-pfxcertificate) 和 [Export-PfxCertificate](/powershell/module/pkiclient/export-pfxcertificate) cmdlet 导入和导出证书。

#### <a name="using-microsoft-management-console"></a>使用 Microsoft 管理控制台 

若要向用户授予存储在本地计算机证书存储位置的证书的读取权限  ，请执行以下步骤：

1.  打开命令提示符，并键入 **mmc**。
2.  在 MMC 控制台的“文件”  菜单上，单击“添加/删除管理单元”  。
3.  在“添加/删除管理单元”  对话框中，单击“添加”  。
4.  在“添加独立管理单元”  对话框中，单击“证书”  ，再单击“添加”  。
5.  在“证书”管理单元对话框中，单击“计算机帐户”，再单击“完成”。   
6.  在“添加独立管理单元”  对话框中，单击“关闭”  。
7.  在“添加/删除管理单元”  对话框中，单击“确定”  。
8.  在“证书”管理单元中的“证书”>“个人”文件夹中找到证书，右键单击证书，指向“所有任务”，然后单击“管理私钥”。    
9.  在“安全”  对话框中，根据需要添加用户帐户的读取权限。

## <a name="creating-column-master-keys-in-azure-key-vault"></a>创建 Azure 密钥保管库中的列主密钥

Azure 密钥保管库有助于保护加密密钥和机密，是用于存储“始终加密”的列主密钥的便利选项，尤其是当你的应用程序在 Azure 中托管时。 若要在 [Azure 密钥保管库](/azure/key-vault/general/overview)中创建密钥，需要有 [Azure 订阅](https://azure.microsoft.com/free/) 和 Azure 密钥保管库。

### <a name="using-powershell"></a>使用 PowerShell

以下示例将创建一个新的 Azure 密钥保管库和密钥，然后向所需用户授予权限。

```
# Create a column master key in Azure Key Vault.
Connect-AzAccount
$SubscriptionId = "<Azure subscription ID>"
$resourceGroup = "<resource group name>"
$azureLocation = "<key vault location>"
$akvName = "<key vault name>"
$akvKeyName = "<column master key name>"
$azureCtx = Set-AzContext -SubscriptionId $SubscriptionId # Sets the context for the below cmdlets to the specified subscription.
New-AzResourceGroup -Name $resourceGroup -Location $azureLocation # Creates a new resource group - skip, if you desire group already exists.
New-AzKeyVault -VaultName $akvName -ResourceGroupName $resourceGroup -Location $azureLocation -SKU premium # Creates a new key vault - skip if your vault already exists.
Set-AzKeyVaultAccessPolicy -VaultName $akvName -ResourceGroupName $resourceGroup -PermissionsToKeys get, create, delete, list, update, import, backup, restore, wrapKey, unwrapKey, sign, verify -UserPrincipalName $azureCtx.Account
$akvKey = Add-AzKeyVaultKey -VaultName $akvName -Name $akvKeyName -Destination HSM
```

### <a name="using-sql-server-management-studio-ssms"></a>使用 SQL Server Management Studio (SSMS)

有关如何使用 SSMS 在 Azure Key Vault 中创建列主密钥的详细信息，请参阅[使用 SQL Server Management Studio 预配 Always Encrypted 密钥](configure-always-encrypted-keys-using-ssms.md)。
有关使用 SSMS 并在 Azure 密钥保管库中存储“始终加密”密钥的分布式教程，请参阅 [《Always Encrypted Wizard tutorial (Azure Key Vault)》](/azure/azure-sql/database/always-encrypted-azure-key-vault-configure)（始终加密向导教程（Azure 密钥保管库））。

### <a name="making-azure-key-vault-keys-available-to-applications-and-users"></a>使 Azure 密钥保管库密钥可用于应用程序和用户

将 Azure 密钥保管库密钥用作列主密钥时，应用程序需要进行 Azure 身份验证，并且应用程序的身份需要具有密钥保管库的以下权限：get、unwrapKey 和 verify    。 

若要预配使用存储在 Azure 密钥保管库中的列主密钥保护的列加密密钥，你需要 get  、unwrapKey  、wrapKey  、sign  和 verify  权限。 此外，若要在 Azure 密钥保管库中创建新的密钥，你需要 create  权限；若要列出密钥保管库内容，你需要 list  权限。

#### <a name="using-powershell"></a>使用 PowerShell

若要使用户和应用程序能够访问 Azure Key Vault 中的实际密钥，必须设置保管库访问策略 ([Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy))：

```
$vaultName = "<vault name>"
$resourceGroupName = "<resource group name>"
$userPrincipalName = "<user to grant access to>"
$clientId = "<client Id>"

# grant users permissions to the keys:
Set-AzKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
# grant applications permissions to the keys:
Set-AzKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $clientId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list
```

## <a name="creating-column-master-keys-in-hardware-security-modules-using-cng"></a>使用 CNG 创建硬件安全模块中的列主密钥

“始终加密”功能的列主密钥可以存储在实现下一代加密技术 (CNG) API 的密钥存储。 通常，这种类型的存储是一种硬件安全模块 (HSM)。 HSM 是一种保护并管理数字密钥，以及提供加密处理的物理设备。 HSM 通常以插件卡或外部设备的形式直接连接到一台计算机（本地 HSM）或网络服务器。

若要使 HSM 可用于指定计算机上的应用程序，必须在该计算机上安装并配置实现 CNG 的密钥存储提供者 (KSP)。 始终加密客户端驱动程序（驱动程序内的列主密钥存储提供者）使用 KSP 加密和解密使用存储在密钥存储中的列主密钥保护的列加密密钥。

Windows 包括 Microsoft 软件密钥存储提供者 - 可用于测试目的的基于软件的 KSP。 请参阅 [《CNG Key Storage Providers》](/windows/desktop/SecCertEnroll/cng-key-storage-providers)（CNG 密钥存储提供者）。

### <a name="creating-column-master-keys-in-a-key-store-using-cngksp"></a>使用 CNG/KSP 在密钥存储中创建列主密钥

列主密钥应为使用 RSA 算法的非对称密钥（公钥/私钥对）。 建议的密钥长度为 2048 或更高。

#### <a name="using-hsm-specific-tools"></a>使用特定于 HSM 的工具
请查阅 HSM 的文档。

#### <a name="using-powershell"></a>使用 PowerShell

可以通过 .NET API 使用 PowerShell 中的 CNG 在密钥存储中创建密钥。


```
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
```

#### <a name="using-sql-server-management-studio"></a>使用 SQL Server Management Studio

请参阅[使用 SQL Server Management Studio 预配 Always Encrypted 密钥](configure-always-encrypted-keys-using-ssms.md)。

### <a name="making-cng-keys-available-to-applications-and-users"></a>使 CNG 密钥可用于应用程序和用户

请参阅 HSM 和 KSP 文档，了解如何在计算机上配置 KSP，以及如何向应用程序和用户授予访问 HSM 的权限。

## <a name="creating-column-master-keys-in-hardware-security-modules-using-capi"></a>使用 CAPI 创建硬件安全模块中的列主密钥

“始终加密”功能的列主密钥可以存储在实现加密 API (CAPI) 的密钥存储中。 通常，此类存储是一种硬件安全模块 (HSM) - 一种保护并管理数字密钥，以及提供加密处理的物理设备。 HSM 通常以插件卡或外部设备的形式直接连接到一台计算机（本地 HSM）或网络服务器。

若要使 HSM 可用于指定计算机上的应用程序，必须在该计算机上安装并配置实现 CAPI 的加密服务提供程序 (CSP)。 始终加密客户端驱动程序（驱动程序内的列主密钥存储提供者）使用 CSP 加密和解密使用存储在密钥存储中的列主密钥保护的列加密密钥。 

> [!NOTE]
> CAPI 是一个旧的、已弃用的 API。 如果 HSM 可以使用 KSP，则应使用它，而不是 CSP/CAPI。

CSP 必须支持要用于“始终加密”功能的 RSA 算法。

Windows 包含以下基于软件的（不受 HSM 支持）支持 RSA 且可用于测试的 CSP：Microsoft 增强 RSA 和 AES 加密提供程序。

### <a name="creating-column-master-keys-in-a-key-store-using-capicsp"></a>使用 CAPI/CSP 在密钥存储中创建列主密钥

列主密钥应为使用 RSA 算法的非对称密钥（公钥/私钥对）。 建议的密钥长度为 2048 或更高。

#### <a name="using-hsm-specific-tools"></a>使用特定于 HSM 的工具
请查阅 HSM 的文档。

#### <a name="using-sql-server-management-studio-ssms"></a>使用 SQL Server Management Studio (SSMS)
请参阅[使用 SQL Server Management Studio 预配 Always Encrypted 密钥](configure-always-encrypted-keys-using-ssms.md)。

### <a name="making-cng-keys-available-to-applications-and-users"></a>使 CNG 密钥可用于应用程序和用户
请参阅 HSM 和 CSP 的相关文档，了解如何在计算机上配置 CSP，以及如何向应用程序和用户授予访问 HSM 的权限。
 
## <a name="next-steps"></a>后续步骤  
- [使用 SQL Server Management Studio 预配 Always Encrypted 密钥](configure-always-encrypted-keys-using-ssms.md)
- [使用 PowerShell 预配 Always Encrypted 密钥](configure-always-encrypted-keys-using-powershell.md)
  
## <a name="see-also"></a>另请参阅 
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Always Encrypted 密钥管理概述](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
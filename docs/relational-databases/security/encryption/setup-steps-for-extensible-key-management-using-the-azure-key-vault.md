---
title: 使用 Azure 密钥保管库设置透明数据加密 (TDE) 可扩展密钥管理
description: 为 Azure 密钥保管库安装和配置 SQL Server 连接器。
ms.custom: seo-lt-2019
ms.date: 11/25/2020
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Extensible Key Management
- EKM, with key vault setup
- SQL Server Connector, setup
- SQL Server Connector
- TDE, AKV, EKM
ms.assetid: c1f29c27-5168-48cb-b649-7029e4816906
author: Rupp29
ms.author: arupp
ms.openlocfilehash: 83a1b1c7e923362678a013de24b211a9da70be0e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345386"
---
# <a name="set-up-sql-server-tde-extensible-key-management-by-using-azure-key-vault"></a>使用 Azure 密钥保管库设置 SQL Server TDE 可扩展密钥管理

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

本文介绍如何为 Azure 密钥保管库安装和配置 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 连接器。  
  
## <a name="prerequisites"></a>先决条件

在开始将 Azure 密钥保管库与 SQL Server 实例配合使用之前，请确保已满足以下先决条件：  
  
- 必须拥有 Azure 订阅。
  
- 安装 [Azure PowerShell 5.2.0 版或更高版本](/powershell/azure/)。  

- 创建 Azure Active Directory (Azure AD) 实例。

- 查看[使用 Azure 密钥保管库的 EKM (SQL Server)](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)，以熟悉使用 Azure 密钥保管库的可扩展密钥管理 (EKM) 存储的主体。  

- 安装基于当前运行的 SQL Server 版本的 Visual Studio C++ Redistributable 版本：
  
  SQL Server 版本  | Visual Studio C++ Redistributable 版本
  ---------|---------
  2008、2008 R2、2012、2014 | [Visual C++ Redistributable Packages for Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=40784)
  2016 | [Visual C++ Redistributable for Visual Studio 2015](https://www.microsoft.com/download/details.aspx?id=48145)

## <a name="step-1-set-up-an-azure-ad-service-principal"></a>步骤 1：设置 Azure AD 服务主体

若要向 Azure 密钥保管库授予 SQL Server 实例访问权限，你需要在 Azure AD 中具有服务主体帐户。  
  
1. 登录 [Azure 门户](https://ms.portal.azure.com/)，然后执行以下任一操作：

    - 选择“Azure Active Directory”按钮。

      ![“Azure 服务”窗格的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part1-login-portal.png)

    - 选择“更多服务”，然后在“所有服务”框中键入 Azure Active Directory。

      ![“所有 Azure 服务”窗格的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part1-select-aad.png)  

1. 通过执行以下操作，将应用程序注册到 Azure Active Directory。 （有关详细的分步说明，请参阅 [Azure 密钥保管库博客文章](/archive/blogs/kv/azure-key-vault-step-by-step)中的“获取应用程序的标识”部分。）

    a. 在“Azure Active Directory 概述”窗格中，选择“应用注册”。

    ![“Azure Active Directory 概述”窗格的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-app-register.png)

    b. 在“应用注册”窗格中，选择“新建注册” 。

    ![“应用注册”窗格的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-new-registration.png)  

    c. 在“注册应用程序”窗格中，输入应用面向用户的名称，然后选择“注册”。

    ![“注册应用程序”窗格的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-register.png)

    d. 在左侧窗格中，选择“证书和密码”，然后选择“新建客户端密码” 。

    ![“证书和密码”窗格的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-certs-secrets.png)  

    e. 在“添加客户端密码”下，输入说明和适当的到期时间，然后选择“添加”。

    ![“添加客户端密码”部分的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-add-secret.png)  

    f. 在“证书和密码”窗格的“值”下，选择要用于在 SQL Server 中创建非对称密钥的客户端密码值旁边的“复制”按钮。

    ![密码值的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-new-secret.png)  

    g. 在左侧窗格中，选择“概述”，然后在“应用程序(客户端) ID”框中，复制要用于在 SQL Server 中创建非对称密钥的值。

    ![“概述”窗格中的“应用程序(客户端) ID”值的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-appid.png)  

## <a name="step-2-create-a-key-vault"></a>步骤 2：创建 key vault

选择要用于创建密钥保管库的方法。

## <a name="azure-portal"></a>[Azure 门户](#tab/portal)

### <a name="create-a-key-vault-by-using-the-azure-portal"></a>使用 Azure 门户创建密钥保管库

可以使用 Azure 门户创建密钥保管库，然后向其中添加 Azure AD 主体。

1. 创建资源组。

   通过 Azure 门户创建的所有 Azure 资源都必须包含在创建的资源组中，该资源组用于承载密钥保管库。 在本示例中，资源名称为 ContosoDevRG。 选择你自己的资源组和密钥保管库名称，因为所有密钥保管库名称都必须是全局唯一的。

   在“创建资源组”窗格的“项目详细信息”下，输入值，然后选择“查看 + 创建”。

      ![“创建资源组”窗格的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part2-create-resource-group.png)  

1. 创建密钥保管库。

    在“创建密钥保管库”窗格中，选择“基本信息”选项卡，输入适当的值，然后选择“查看 + 创建”。

    ![“创建密钥保管库”窗格的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part2-create-key-vault.png)  

1. 在“访问策略”窗格中，选择“添加访问策略”。

    ![“访问策略”窗格中的“添加访问策略”链接的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part2-add-access-policy.png)  

1. 在“添加访问策略”窗格中，执行以下操作：
  
    a. 在“从模板配置(可选)”下拉列表中，选择“密钥管理”。

    b. 在左侧窗格中，选择“密钥权限”选项卡，然后验证是否选中了“获取”、“列出”、“解包密钥”和“包装密钥”复选框。

    c. 选择 **添加** 。

    ![“添加访问策略”窗格的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part2-access-policy-permission.png)

1. 在左侧窗格中，选择“选择主体”选项卡，然后执行以下操作：

    a. 在“主体”窗格的“选择”下，开始键入 Azure AD 应用程序的名称，然后在结果列表中选择要添加的应用程序。

    ![“主体”窗格中的应用程序搜索框的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part2-select-principal.png)  

    b. 选择“选择”按钮，将主体添加到密钥保管库中。

    ![“主体”窗格中的“选择”按钮的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part2-save-principal.png)

    c. 在左下方，选择“添加”以保存更改。

    ![“添加访问策略”窗格中的“添加”按钮的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part2-select-principal-new.png)

1. 在“Key Vault”窗格上，选择“密钥”并输入密钥保管库名称。 使用密钥类型 RSA 和 RSA 密钥大小 2048。 根据需要设置激活日期和到期日期并将“是否启用?”设置为”是”。

   ![“创建密钥”窗格的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part2-add-key-vault-key.png)  

1. 在“访问策略”窗格中，选择“保存”。
  
   ![“添加访问策略”窗格中的“保存”按钮的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-part2-save-access-policy.png)  

## <a name="powershell"></a>[PowerShell](#tab/powershell)

### <a name="create-a-key-vault-and-key-by-using-powershell"></a>使用 PowerShell 创建密钥保管库和密钥

此处创建的密钥保管库和密钥将供 SQL Server 数据库引擎使用，以实现加密密钥保护。  
  
> [!IMPORTANT]
> 创建密钥保管库的订阅必须在创建 Azure AD 服务主体的同一默认 Azure AD 实例中。 如果想要使用默认实例以外的 Azure AD 实例来创建 SQL Server 连接器的服务主体，则必须在创建密钥保管库之前更改 Azure 帐户中的默认 Azure AD 实例。 若要了解如何将默认 Azure AD 实例更改为要使用的实例，请参阅 [SQL Server 连接器维护与故障排除](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixB)的“常见问题”部分。  
  
1. 使用以下命令安装并登录到 [Azure PowerShell 5.2.0 或更高版本](/powershell/azure/)：  
  
    ```powershell  
    Connect-AzAccount  
    ```  
  
    该语句返回：  
  
    ```console  
    Environment           : AzureCloud  
    Account               : <account_name>  
    TenantId              : <tenant_id>  
    SubscriptionId        : <subscription_id>  
    CurrentStorageAccount :  
    ```  
  
    > [!NOTE]  
    > 如果你有多个订阅并想要指定一个订阅用于保管库，请运行 `Get-AzSubscription` 以查看订阅，然后运行 `Select-AzSubscription` 以选择正确的订阅。 否则，PowerShell 将默认为你选择一个订阅。  
  
1. 创建资源组。

    通过 PowerShell 创建的所有 Azure 资源都必须包含在创建的资源组中，该资源组用于承载密钥保管库。 在本示例中，资源名称为 ContosoDevRG。 选择你自己的资源组和密钥保管库名称，因为所有密钥保管库名称都必须是全局唯一的。  
  
    ```powershell  
    New-AzResourceGroup -Name ContosoDevRG -Location 'East Asia'  
    ```  
  
    该语句返回：  
  
    ```console
    ResourceGroupName: ContosoDevRG  
    Location         : eastasia  
    ProvisioningState: Succeeded  
    Tags             :
    ResourceId       : /subscriptions/<subscription_id>/  
                        resourceGroups/ContosoDevRG  
    ```  
  
    > [!NOTE]
    > 对于 `-Location` 参数，请使用命令 `Get-AzureLocation` 来确定如何指定一个与本示例中的位置不同的位置。 如需更多信息，请键入 Get-Help Get-AzureLocation。  
  
1. 创建密钥保管库。

    `New-AzKeyVault` cmdlet 需要一个资源组名称、一个密钥保管库名称和一个地理位置。 例如，对于名为 `ContosoEKMKeyVault` 的密钥保管库，请运行：  
  
    ```powershell  
    New-AzKeyVault -VaultName 'ContosoEKMKeyVault' `  
       -ResourceGroupName 'ContosoDevRG' -Location 'East Asia'  
    ```  
  
    记录密钥保管库的名称。  
  
    该语句返回：

    ```console
    Vault Name                       : ContosoEKMKeyVault  
    Resource Group Name              : ContosoDevRG  
    Location                         : East Asia  
    ResourceId                       : /subscriptions/<subscription_id>/  
                                        resourceGroups/ContosoDevRG/providers/  
                                        Microsoft/KeyVault/vaults/ContosoEKMKeyVault  
    Vault URI: https://ContosoEKMKeyVault.vault.azure.net  
    Tenant ID                        : <tenant_id>  
    SKU                              : Standard  
    Enabled For Deployment?          : False  
    Enabled For Template Deployment? : False  
    Enabled For Disk Encryption?     : False  
    Access Policies                  :  
             Tenant ID              : <tenant_id>  
             Object ID              : <object_id>  
             Application ID         :
             Display Name           : <display_name>  
             Permissions to Keys    : get, create, delete, list, update, import,
                                      backup, restore  
             Permissions to Secrets : all  
    Tags                             :  
    ```  
  
1. 为 Azure AD 服务主体授予权限以访问 Azure 密钥保管库。  
  
    你可以授权其他用户和应用程序使用密钥保管库。  对于我们的示例，让我们使用你在[步骤 1：设置 Azure AD 服务主体](#step-1-set-up-an-azure-ad-service-principal)中创建的服务主体来对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例授权。  
  
    > [!IMPORTANT]
    > Azure AD 服务主体对密钥保管库必须至少具有“get”、“list”、“wrapKey”和“unwrapKey”权限。  
  
    如以下命令所示，将[步骤 1：设置 Azure AD 服务主体](#step-1-set-up-an-azure-ad-service-principal)中的“应用(客户端) ID”用于 `ServicePrincipalName` 参数。 如果 `Set-AzKeyVaultAccessPolicy` 成功运行，则以无提示方式运行，并且无任何输出。  
  
    ```powershell  
    Set-AzKeyVaultAccessPolicy -VaultName 'ContosoEKMKeyVault' `  
      -ServicePrincipalName 9A57CBC5-4C4C-40E2-B517-EA677 `  
      -PermissionsToKeys get, list, wrapKey, unwrapKey  
    ```  
  
    调用 `Get-AzKeyVault` cmdlet 以确认权限。 在`Access Policies`下方的语句输出中，应看到作为有权访问此密钥保管库的另一个租户列出的 Azure AD 应用程序名称。  
  
1. 在密钥保管库中生成非对称密钥。 可以通过以下两种方式之一实现此目的：导入现有密钥或创建新密钥。  

     > [!NOTE]
     > SQL Server 仅支持 2048 位和 3072 位 RSA 密钥，以及 2048 位和 3072 位 RSA-HSM 密钥。

### <a name="best-practices"></a>最佳做法

为了确保快速恢复密钥并能够访问 Azure 外的数据，建议采用以下最佳做法：

- 在本地硬件安全模块 (HSM) 设备上创建加密密钥。 确保使用非对称 RSA 2048 或 3072 密钥，以便得到 SQL Server 的支持。
- 将加密密钥导入到 Azure 密钥保管库。 此过程在后续部分中介绍。
- 在首次使用 Azure 密钥保管库中的密钥之前，先进行 Azure 密钥保管库密钥备份。 有关详细信息，请参阅 [Backup-AzureKeyVaultKey]() 命令。
- 每次更改密钥（例如添加 ACL、标记或密钥属性）时，务必再进行一次 Azure 密钥保管库密钥备份。

  > [!NOTE]
  > 备份密钥是一项 Azure Key Vault 密钥操作，将返回一个可在任意位置保存的文件。

### <a name="types-of-keys"></a>密钥类型

可以在兼容 SQL Server 的 Azure 密钥保管库中生成四种类型的密钥。 非对称 2048 位和 3072 位 RSA 密钥，以及 2048 位和 3072 位 RSA-HSM 密钥。
  
- **受软件保护的密钥**：在软件中处理，在静止时加密。 对受软件保护的密钥的操作在 Azure 虚拟机上进行。 对于不在生产部署中使用的密钥，建议使用此类型。  

- **受 HSM 保护的密钥**：由硬件安全模块创建并保护，以增加安全性。 每个密钥版本的费用大约为 1 美元。  
  
    > [!IMPORTANT]
    > 对于 SQL Server 连接器，只能使用字符 a-z、A-Z、0-9 和连字符 (-)，且长度限制为 26 个字符。
    > Azure 密钥保管库中具有同一密钥名称的不同密钥版本不会使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 连接器。 若要轮换 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 使用的 Azure 密钥保管库密钥，请参阅 [SQL Server 连接器维护与故障排除](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)的“A. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 连接器的维护说明”部分中的密钥滚动更新步骤。  

### <a name="import-an-existing-key"></a>导入现有密钥
  
如果已有 2048 位 RSA 软件保护密钥，则可以将该密钥上传到 Azure 密钥保管库。 例如，如果 `C:\\` 驱动器中保存了你要上传到 Azure 密钥保管库的、名为 softkey.pfx 的 PFX 文件，请运行以下命令来设置变量 `securepfxpwd`，以将 PFX 文件的密码设为 `12987553`：  
  
``` powershell  
$securepfxpwd = ConvertTo-SecureString -String '12987553' `  
  -AsPlainText -Force  
```  

然后可以运行以下命令从 PFX 文件导入密钥，以便硬件（建议）在密钥保管库服务中保护密钥：  
  
``` powershell  
    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' `  
      -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' `  
      -KeyFilePassword $securepfxpwd $securepfxpwd  -Destination 'HSM'  
```  

> [!CAUTION]
> 强烈建议对生产情况导入非对称密钥，因为这样管理员就可以在密钥托管系统中托管密钥。 如果非对称密钥是在保管库中创建的，则无法托管，因为私钥不能离开保管库。 应托管用于保护关键数据的密钥。 丢失非对称密钥将导致永久性数据丢失。  

### <a name="create-a-new-key"></a>新建密钥

或者，可直接在 Azure 密钥保管库中创建新的加密密钥，并使其受软件保护或受 HSM 保护。  在本示例中，让我们使用 `Add-AzureKeyVaultKey` cmdlet 创建受软件保护的密钥：  

``` powershell  
Add-AzureKeyVaultKey -VaultName 'ContosoEKMKeyVault' `  
  -Name 'ContosoRSAKey0' -Destination 'Software'  
```  
  
该语句返回：  
  
```console
Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes  
Key        :  {"kid":"https:contosoekmkeyvault.azure.net/keys/  
                ContosoRSAKey0/<guid>","dty":"RSA:,"key_ops": ...  
VaultName  : contosodevkeyvault  
Name       : contosoRSAKey0  
Version    : <guid>  
Id         : https://contosoekmkeyvault.vault.azure.net:443/  
              keys/ContosoRSAKey0/<guid>  
```  

> [!IMPORTANT]
> 虽然密钥保管库支持同名密钥的多个版本，但 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 连接器使用的密钥不应受版本控制或对其进行滚动更新。 如果管理员想要滚动更新用于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 加密的密钥，则应在密钥保管库中创建一个具有不同名称的新密钥并用它来加密 DEK。  

---

## <a name="step-3-install-the-ssnoversion-connector"></a>步骤 3：安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 连接器

从 [Microsoft 下载中心](https://go.microsoft.com/fwlink/p/?LinkId=521700)下载 SQL Server 连接器。 此下载操作应由 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 计算机的管理员完成。  

> [!NOTE]
> - 已替换 SQL Server 连接器版本 1.0.0.440 及更低版本，且生产环境不再支持这些版本，并按照[升级 SQL Server 连接器](sql-server-connector-maintenance-troubleshooting.md#upgrade-of--connector)下的 [SQL Server 连接器维护与故障排除](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)页上的说明进行操作。
> - 从版本 1.0.3.0 开始，SQL Server 连接器向 Windows 事件日志报告相关错误消息，以便进行故障排除。
> - 从版本 1.0.4.0 开始，支持私有 Azure 云，包括 Azure 中国、Azure 德国和 Azure 政府。
> - 1\.0.5.0 版在指纹算法方面进行了一项重大更改。 升级到 1.0.5.0 后，可能会遭遇数据库还原失败。 有关详细信息，请参阅[知识库文章 447099](https://support.microsoft.com/help/4470999/db-backup-problems-to-sql-server-connector-for-azure-1-0-5-0)。
> - 从版本 1.0.5.0（时间戳：2020 年 9 月）开始，SQL Server 连接器支持筛选消息和网络请求重试逻辑。
> - 从更新版本 1.0.5.0（时间戳：2020 年 11 月）开始，SQL Server 连接器支持 RSA 2048、RSA 3072、RSA-HSM 2048 和 RSA-HSM 3072 密钥。
  
  ![SQL Server 连接器安装向导的屏幕截图](../../../relational-databases/security/encryption/media/ekm/ekm-connector-install.png)  
  
默认情况下，连接器安装在 C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault 中。 可在设置过程中更改此位置。 如果更改了此位置，请在下一部分中调整脚本。  
  
该连接器没有接口，但如果安装成功，将在计算机上安装 Microsoft.AzureKeyVaultService.EKM.dll。 此程序集是加密 EKM 提供程序 DLL，需要使用 `CREATE CRYPTOGRAPHIC PROVIDER` 语句在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中注册。  
  
SQL Server 连接器安装还允许你选择性地下载用于 SQL Server 加密的示例脚本。  
  
若要查看 SQL Server 连接器的错误代码说明、配置设置或维护任务，请参阅：  
  
- [A.SQL Server 连接器的维护说明](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixA)  
- [C.SQL Server 连接器的错误代码说明](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixC)  
  
## <a name="step-4-configure-ssnoversion"></a>步骤 4：配置 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  

有关此部分中每个操作所需的最低权限级别的说明，请参阅 [B. 常见问题](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixB)。  
  
1. 运行 sqlcmd.exe 或打开 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Management Studio。  
  
1. 通过运行以下 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 脚本，将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置为使用 EKM：  
  
    ```sql  
    -- Enable advanced options.  
    USE master;  
    GO  

    EXEC sp_configure 'show advanced options', 1;  
    GO  
    RECONFIGURE;  
    GO  

    -- Enable EKM provider  
    EXEC sp_configure 'EKM provider enabled', 1;  
    GO  
    RECONFIGURE;  
    ```  
  
1. 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 连接器注册为 EKM 提供程序。  
  
    使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 连接器（Azure 密钥保管库的 EKM 提供程序）创建加密提供程序。
    在本示例中，提供程序名称为 `AzureKeyVault_EKM`。  
  
    ```sql  
    CREATE CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM
    FROM FILE = 'C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll';  
    GO  
    ```  

    > [!NOTE]
    > 文件路径长度不能超过 256 个字符。  
  
1. 为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名设置 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 凭据以使用密钥保管库。  
  
    必须将凭据添加到要使用密钥保管库中的密钥执行加密的每个登录名。 这些登录名可能包括：  
  
    - 使用密钥保管库来设置和管理 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 加密方案的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 管理员登录名。  
  
    - 可能启用 TDE 或其他 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 加密功能的其他 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名。  
  
    凭据和登录名之间是一对一的映射关系。 也就是说，每个登录名必须具有唯一的凭据。  
  
    通过以下方式修改此 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 脚本：  
  
    - 编辑 `IDENTITY` 参数 (`ContosoEKMKeyVault`) 以指向 Azure 密钥保管库。
      - 如果使用全局 Azure，请将 `IDENTITY` 参数替换为[步骤 2：创建密钥保管库](#step-2-create-a-key-vault)中的 Azure 密钥保管库的名称。
      - 如果使用私有 Azure 云（例如，Azure 政府、Azure 中国世纪互联或 Azure 德国），请将 `IDENTITY` 参数替换为[使用 PowerShell 创建密钥保管库和密钥](#create-a-key-vault-and-key-by-using-powershell)部分的步骤 3 中返回的保管库 URI。 保管库 URI 中不能包含“https://”。
    - 将 `SECRET` 参数的第一部分替换为[步骤 1：设置 Azure AD 服务主体](#step-1-set-up-an-azure-ad-service-principal)中的 Azure Active Directory 客户端 ID。 在本示例中，“客户端 ID”为 `9A57CBC54C4C40E2B517EA677E0EFA00`。  
  
      > [!IMPORTANT]
      > 确保删除“应用(客户端) ID”中的连字符。  
  
    - 使用[步骤 1：设置 Azure AD 服务主体](#step-1-set-up-an-azure-ad-service-principal)中的“客户端密码”完成 `SECRET` 参数的第二部分。  在本示例中，“客户端密码”为 `08:k?[:XEZFxcwIPvVVZhTjHWXm7w1?m`。 `SECRET` 参数的最终字符串是一长串不带连字符的字母和数字。  
  
    ```sql  
    USE master;  
    CREATE CREDENTIAL sysadmin_ekm_cred
        WITH IDENTITY = 'ContosoEKMKeyVault',                            -- for public Azure
        -- WITH IDENTITY = 'ContosoEKMKeyVault.vault.usgovcloudapi.net', -- for Azure Government
        -- WITH IDENTITY = 'ContosoEKMKeyVault.vault.azure.cn',          -- for Azure China 21Vianet
        -- WITH IDENTITY = 'ContosoEKMKeyVault.vault.microsoftazure.de', -- for Azure Germany
               --<----Application (Client) ID ---><--Azure AD app (Client) ID secret-->
        SECRET = '9A57CBC54C4C40E2B517EA677E0EFA0008:k?[:XEZFxcwIPvVVZhTjHWXm7w1?m'
    FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM;  
  
    -- Add the credential to the SQL Server administrator's domain login
    ALTER LOGIN [<domain>\<login>]  
    ADD CREDENTIAL sysadmin_ekm_cred;  
    ```  
  
    有关使用 `CREATE CREDENTIAL` 参数的变量和以编程方式从客户端 ID 中删除连字符的示例，请参阅 [CREATE CREDENTIAL (Transact-SQL)](../../../t-sql/statements/create-credential-transact-sql.md)。  
  
1. 在 SQL Server 实例中打开 Azure 密钥保管库密钥。  

    无论是按照[步骤 2：创建密钥保管库](#step-2-create-a-key-vault)中所述创建新密钥还是导入非对称密钥，都需要打开该密钥。 可通过在以下 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 脚本中提供密钥名称来打开它。  
  
    - 将 `EKMSampleASYKey` 替换为希望该密钥在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中显示的名称。  
    - 将 `ContosoRSAKey0` 替换为 Azure 密钥保管库中密钥的名称。  
  
    ```sql  
    CREATE ASYMMETRIC KEY EKMSampleASYKey
    FROM PROVIDER [AzureKeyVault_EKM]  
    WITH PROVIDER_KEY_NAME = 'ContosoRSAKey0',  
    CREATION_DISPOSITION = OPEN_EXISTING;  
    ```  

1. 使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中在前面步骤创建的非对称密钥来创建新的登录名。

     ```sql  
    --Create a Login that will associate the asymmetric key to this login
    CREATE LOGIN TDE_Login
    FROM ASYMMETRIC KEY EKMSampleASYKey;
    ```  

1. 根据 SQL Server 中的非对称密钥创建新的登录名。 删除“步骤 4：配置 SQL Server”中的凭据映射，以便将凭据映射到新的登录名。

     ```sql  
    --Now drop the credential mapping from the original association
    ALTER LOGIN [<domain>\<login>]
    DROP CREDENTIAL sysadmin_ekm_cred;
    ```

1. 更改新登录名，然后将 EKM 凭据映射到新登录名。

     ```sql  
    --Now add the credential mapping to the new Login
    ALTER LOGIN TDE_Login
    ADD CREDENTIAL sysadmin_ekm_cred;
    ```  

1. 创建将使用 Azure 密钥保管库密钥加密的测试数据库。

     ```sql
    --Create a test database that will be encrypted with the Azure key vault key
    CREATE DATABASE TestTDE
    ```  

1. 使用非对称密钥 (EKMSampleASYKey) 创建数据库加密密钥。

    ```sql  
    --Create an ENCRYPTION KEY using the ASYMMETRIC KEY (EKMSampleASYKey)
    CREATE DATABASE ENCRYPTION KEY
    WITH ALGORITHM = AES_256
    ENCRYPTION BY SERVER ASYMMETRIC KEY EKMSampleASYKey;
    ```
  
1. 加密测试数据库。 通过将 ENCRYPTION 设置为 ON 来启用 TDE。

     ```sql  
    --Enable TDE by setting ENCRYPTION ON
    ALTER DATABASE TestTDE
    SET ENCRYPTION ON;  
     ```  

1. 清理测试对象。 删除在此测试脚本中创建的所有对象。

    ```sql  
    -- CLEAN UP
    USE Master
    ALTER DATABASE [TestTDE] SET SINGLE_USER WITH ROLLBACK IMMEDIATE
    DROP DATABASE [TestTDE]

    ALTER LOGIN [TDE_Login] DROP CREDENTIAL [sysadmin_ekm_cred]
    DROP LOGIN [TDE_Login]

    DROP CREDENTIAL [sysadmin_ekm_cred]  

    USE MASTER
    DROP ASYMMETRIC KEY [EKMSampleASYKey]
    DROP CRYPTOGRAPHIC PROVIDER [AzureKeyVault_EKM]
    ```  

有关示例脚本，请参阅博客 [SQL Server Transparent Data Encryption and Extensible Key Management with Azure Key Vault](https://techcommunity.microsoft.com/t5/sql-server/intro-sql-server-transparent-data-encryption-and-extensible-key/ba-p/1427549)（使用 Azure 密钥保管库的 SQL Server 透明数据加密和可扩展密钥管理）。

## <a name="next-steps"></a>后续步骤  
  
现在你已完成基本配置，请参阅[使用具有 SQL 加密功能的 SQL Server 连接器](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)。
  
## <a name="see-also"></a>另请参阅  

- [使用 Azure 密钥保管库的可扩展密钥管理](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)
- [SQL Server 连接器维护与故障排除](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)

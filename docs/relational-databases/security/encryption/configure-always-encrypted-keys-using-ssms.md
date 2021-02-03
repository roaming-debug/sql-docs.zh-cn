---
title: 使用 SQL Server Management Studio 预配 Always Encrypted 密钥 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 预配 Always Encrypted 的列主密钥和列加密密钥。
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.COLUMNMASTERKEY.PAGE.F1
- SQL13.SWB.COLUMNENCRYPTIONKEY.PAGE.F1
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f31a17d3f8bf2285bc41c7462deec4e53008efd1
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237076"
---
# <a name="provision-always-encrypted-keys-using-sql-server-management-studio"></a>使用 SQL Server Management Studio 预配 Always Encrypted 密钥
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

本文提供了使用 [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md) 预配 Always Encrypted 的列主密钥和列加密密钥的步骤。

有关 Always Encrypted 密钥管理的概述（包括最佳做法建议和重要的安全注意事项），请参阅 [Always Encrypted 密钥管理概述](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)。

<a name="provisioncmk"></a>
## <a name="provision-column-master-keys-with-the-new-column-master-key-dialog"></a>使用“新建列主密钥”对话框预配列主密钥

“新建列主密钥”  对话框允许你生成列主密钥或选择密钥存储中的现有密钥，并在数据库中为所创建或所选择的密钥创建列主密钥元数据。

1.  使用 **对象资源管理器** 导航到数据库下面的“安全”>“始终加密密钥”文件夹。
2.  右键单击“列主密钥”文件夹，然后选择“新建列主密钥...” 。 
3.  在“新建列主密钥”  对话框中，输入列主密钥元数据对象的名称。
4.  选择密钥存储：
    - **证书存储 - 当前用户** - 指示当前用户证书存储在 Windows 证书存储中的位置，这是你的个人存储。 
    - **证书存储 - 本地计算机** - 指示本地计算机证书存储在 Windows 证书存储中的位置。 
    - **Azure Key Vault** - 你需要登录到 Azure（单击“登录”）。 登录后，即可选择你的一个 Azure 订阅和密钥保管库。
    - **密钥存储提供程序(KSP)** - 指明可通过实现下一代加密技术 (CNG) API 的密钥存储提供程序 (KSP) 访问的密钥存储。 通常，这种存储是硬件安全模块 (HSM)。 选择此选项后，需要选择 KSP。 “Microsoft 软件密钥存储提供程序” 默认处于选中状态。 如果你想使用存储在 HSM 中的列主密钥，请为设备选择 KSP（必须在打开该对话框之前在计算机上安装并配置 KSP）。
    -   **加密服务提供程序(CSP)** - 可通过实现加密 API (CAPI) 的加密服务提供程序 (CSP) 访问的密钥存储。 通常，这种存储是硬件安全模块 (HSM)。 选择此选项后，需要选择 CSP。  如果你想使用存储在 HSM 中的列主密钥，请为设备选择 CSP（必须在打开该对话框之前在计算机上安装并配置 CSP）。
    
    > [!NOTE]
    > 由于 CAPI 是已弃用的 API，因此，“加密服务提供程序(CAPI)”选项默认处于禁用状态。 你可以通过在 Windows 注册表中的 **[HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\sql13\Tools\Client\Always Encrypted]** 项下面创建“已启用 CAPI 提供程序”DWORD 值并将其设置为 1，来启用该选项。 应使用 CNG 而不是 CAPI，除非密钥存储不支持 CNG。
   
    有关上述密钥存储的详细信息，请参阅[创建并存储 Always Encrypted 的列主密钥](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)。

5. 如果使用的是 [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)]，并且 SQL Server 实例配置有安全 enclave，则可以选中“允许 enclave 计算”复选框以启用主密钥 enclave。 有关详细信息，请参阅[具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves.md)。 

    > [!NOTE]
    > 如果 SQL Server 实例未正确配置有安全 enclave，则不会显示“允许 enclave 计算”复选框。

6.  选择密钥存储中的现有密钥，或者单击“生成密钥”  或“生成证书”  按钮，在密钥存储中创建一个密钥。 
7.  单击“确定”  ，新密钥随即显示在列表中。 

完成此对话框中的选择后，SQL Server Management Studio 会在数据库中为列主密钥创建元数据。 该对话框通过生成并发出 [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) 语句来实现此操作。

::: moniker range=">=sql-server-ver15"

如果要配置已启用 enclave 的列主密钥，SSMS 还会使用列主密钥对元数据进行签名。 

::: moniker-end

### <a name="permissions-for-provisioning-a-column-master-key"></a>预配列主密钥所需的权限

你需要具有数据库中的 ALTER ANY COLUMN MASTER KEY 数据库权限才能使此对话框创建列主密钥。 若要使用此对话框创建新的列主密钥或在密钥存储创建中使用现有密钥，你可能需要具有密钥存储或/和密钥的相关权限：
- **证书存储 - 本地计算机** - 必须对用作列主密钥的证书具有读取访问权限，或者是计算机上的管理员。
- **Azure Key Vault** - 你需要具有 get 和 list 权限才能选择和使用密钥，并且需要具有 create 权限才能创建新密钥  。 若要配置已启用 enclave 的列主密钥，你还需要具有 sign 权限才能生成密钥元数据的签名。
- **密钥存储提供程序(CNG)** - 使用密钥存储或密钥时可能提示你提供必要的权限和凭据，具体取决于存储和 KSP 配置。
- **加密服务提供程序(CAPI)** - 使用密钥存储或密钥时可能提示你提供必要的权限和凭据，具体取决于存储和 CSP 配置。

有关详细信息，请参阅[创建并存储 Always Encrypted 的列主密钥](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)。

<a name="provisioncek"></a> 
## <a name="provision-column-encryption-keys-with-the-new-column-encryption-key-dialog"></a>使用“新建列加密密钥”对话框预配列加密密钥

“新建列加密密钥”  对话框允许生成列加密密钥、使用列主密钥对其加密，以及在数据库中创建列加密密钥元数据。

1.  使用 **对象资源管理器** 导航到数据库下面的“安全”/“始终加密密钥”  文件夹。
2.  右键单击“列加密密钥”文件夹，然后选择“新建列加密密钥...” 。 
3.  在“新建列加密密钥”  对话框中，输入列加密密钥元数据对象的名称。
4.  在数据库中选择一个表示列主密钥的元数据对象。
5.  单击“确定”。 

完成此对话框中的选择后，SQL Server Management Studio 会生成新的列加密密钥，然后它会从数据库中检索你所选的列主密钥的元数据。 然后，SSMS 会使用此列主密钥元数据来访问包含你的列主密钥的密钥存储并对列加密密钥进行加密。 最后，SSMS 通过生成并发出 [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md) 语句，为数据库中新的列加密创建元数据数据。

### <a name="permissions-for-provisioning-a-column-encryption-key"></a>预配列加密密钥所需的权限

你需要具有数据库中的 ALTER ANY COLUMN ENCRYPTION KEY 和 VIEW ANY COLUMN MASTER KEY DEFINITION 数据库权限才能使此对话框创建列加密密钥元数据以及访问列主密钥元数据 。
若要访问密钥存储并使用列主密钥，可能需要密钥存储或/和密钥上的权限：
- **证书存储 - 本地计算机** - 必须对用作列主密钥的证书具有读取访问权限，或者是计算机上的管理员。
- **Azure Key Vault** - 需要包含列主密钥的保管库上的 get、unwrapKey、wrapKey、sign 和 verify 权限    。
- **密钥存储提供程序(CNG)** - 使用密钥存储或密钥时可能提示你提供必要的权限和凭据，具体取决于存储和 KSP 配置。
- **加密服务提供程序(CAPI)** - 使用密钥存储或密钥时可能提示你提供必要的权限和凭据，具体取决于存储和 CSP 配置。

有关详细信息，请参阅[创建并存储列主密钥 (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)。

## <a name="provision-always-encrypted-keys-using-the-always-encrypted-wizard"></a>使用 Always Encrypted 向导预配 Always Encrypted 密钥

[Always Encrypted 向导](../../../relational-databases/security/encryption/always-encrypted-wizard.md)是用于加密、解密和重新加密所选数据库列的工具。 尽管它可以使用已配置的密钥，但也使你可以生成新的列主密钥和新的列加密。 

## <a name="next-steps"></a>后续步骤
- [使用 Always Encrypted 向导配置列加密](always-encrypted-wizard.md)
- [使用 Always Encrypted 和 DAC 包配置列加密](configure-always-encrypted-using-dacpac.md)
- [使用 SQL Server Management Studio 轮换 Always Encrypted 密钥](rotate-always-encrypted-keys-using-ssms.md)
- [使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md)
- [通过 SQL Server 导入和导出向导在使用 Always Encrypted 的列之间迁移数据](always-encrypted-migrate-using-import-export-wizard.md)

## <a name="see-also"></a>另请参阅
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Always Encrypted 密钥管理概述](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [创建并存储 Always Encrypted 的列主密钥](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)
- [使用 SQL Server Management Studio 配置 Always Encrypted](configure-always-encrypted-using-sql-server-management-studio.md)
- [使用 PowerShell 预配 Always Encrypted 密钥](configure-always-encrypted-keys-using-powershell.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)

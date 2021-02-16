---
title: 使用 SQL Server Management Studio 轮换 Always Encrypted 密钥 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 来轮换 Always Encrypted 列主密钥和列加密密钥。
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.COLUMNMASTERKEY.ROTATION.F1
- sql13.SWB.COLUMNMASTERKEY.CLEANUP.F1
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 249e0d2bc53083839787543723fc16dc11a4ff57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340132"
---
# <a name="rotate-always-encrypted-keys-using-sql-server-management-studio"></a>使用 SQL Server Management Studio 轮换 Always Encrypted 密钥
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

本文介绍如何使用 [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md) 来轮换 Always Encrypted 列主密钥和列加密密钥。

有关 Always Encrypted 密钥管理的概述（包括最佳做法建议和重要的安全注意事项），请参阅 [Always Encrypted 密钥管理概述](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)。

<a name="rotatecmk"></a>
## <a name="rotate-column-master-keys"></a>轮换列主密钥 

列主密钥轮替是将现有列主密钥替换为新列主密钥的过程。 如果密钥已泄漏，或者为了遵守强制规定必须定期轮换加密密钥的组织的策略或符合性规则，则可能需要轮换密钥。 列主密钥轮替包括对使用当前列主密钥保护的列加密密钥解密，使用新的列主密钥对其重新加密，以及更新密钥元数据。 

### <a name="step-1-provision-a-new-column-master-key"></a>步骤 1：预配新的列主密钥

按照[使用“新建列主密钥”对话框预配列主密钥](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog)的说明进行操作。

### <a name="step-2-encrypt-column-encryption-keys-with-the-new-column-master-key"></a>步骤 2：使用新的列主密钥加密列加密密钥

一个列主密钥通常保护一个或多个列加密密钥。 每个列加密密钥在数据库中存储有一个加密值，这是使用列主密钥加密列加密密钥的产物。
在这一步中，使用新列主密钥来加密使用要轮换的列主密钥保护的每个列加密密钥，并在数据库中存储新加密值。 因此，受轮替影响的每个列加密密钥有两个加密值：一个值是使用现有列主密钥加密的，一个新值是使用新的列主密钥加密的。

1.  使用对象资源管理器依次转到“安全性”>“Always Encrypted 密钥”>“列主密钥”文件夹，并找到要轮换的列主密钥。
2.  右键单击该列主密钥并选择“轮替” 。
3.  在“列主密钥轮替”对话框的“目标”字段中，选择在步骤 1 中创建的新列主密钥的名称 。
4.  查看现有列主密钥保护的列加密密钥的列表。 这些密钥将受到轮转的影响。
5.  单击“确定”。

SQL Server Management Studio 将获取使用旧列主密钥保护的列加密密钥的元数据，以及旧列主密钥和新列主密钥的元数据。 然后，SSMS 将使用列主密钥元数据来访问包含旧列主密钥的密钥存储并对列加密密钥解密。 随后，SSMS 将访问包含新列主密钥的密钥存储，以生成一组新的列加密密钥加密值，然后它会将新值添加到元数据中（生成并发出 [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md) 语句）。

> [!NOTE]
> 请确保使用旧列主密钥加密的每个列加密密钥未使用其他任何列主密钥进行加密。 换而言之，受轮转影响的每个列加密密钥在数据库中必须恰好有一个加密值。 如果受影响的任何列加密密钥具有多个加密值，则需要删除值，然后才能继续轮替（有关如何删除列加密密钥的加密值，请参阅 *步骤 4* ）。

### <a name="step-3-configure-your-applications-with-the-new-column-master-key"></a>步骤 3：使用新的列主密钥配置应用程序

在这一步中，需要确保所有查询以下内容的客户端应用程序都能访问新列主密钥：使用要轮换的列主密钥保护的数据库列（即使用要轮换的列主密钥加密的列加密密钥进行加密的数据库列）。 此步骤取决于新列主密钥所在密钥存储的类型。 例如：

- 如果新列主密钥是存储在 Windows 证书存储中的证书，则需要将该证书部署到数据库中由列主密钥的密钥路径指定的同一个证书存储位置（“当前用户” 或“本地计算机” ）。 该应用程序需要能够访问该证书：
  - 如果该证书存储在“当前用户”证书存储位置中，则需要将该证书导入到应用程序的 Windows 标识（用户）的“当前用户”存储中。
  - 如果该证书存储在“本地计算机”证书存储位置中，则应用程序的 Windows 标识必须有权访问该证书。
- 如果新的列主密钥存储在 Microsoft Azure 密钥保管库中，则必须实现应用程序，以便它可以在 Azure 中进行身份验证并有权访问该密钥。

有关详细信息，请参阅[创建并存储 Always Encrypted 的列主密钥](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)。

> [!NOTE]
> 在轮替的这一阶段，旧列主密钥和新列主密钥均有效并可用于访问数据。

### <a name="step-4-clean-up-column-encryption-key-values-encrypted-with-the-old-column-master-key"></a>步骤 4：清理使用旧列主密钥加密的列加密密钥值

将所有应用程序配置为使用新的列主密钥后，请从数据库中删除使用 *旧* 列主密钥加密的列加密密钥值。 删除旧值可确保随时能够进行下一次轮换（请注意，使用要轮换的列主密钥保护的每个列加密密钥都必须只有一个加密值）。

在存档或删除旧列主密钥之前清理旧值的另一个原因与性能相关：在查询加密列时，启用始终加密的客户端驱动程序可能需要尝试解密两个值：旧值和新值。 驱动程序不知道这两个列主密钥中哪个密钥在应用程序环境中有效，因此它会从服务器中同时检索两个加密值。 如果因为值是使用不可用的列主密钥（例如，已从存储中删除的旧列主密钥）进行保护而无法解密值之一，驱动程序会尝试使用新列主密钥来解密另一个值。

> [!WARNING]
> 如果你在列加密密钥的对应列主密钥可供应用程序使用之前删除列加密密钥的值，应用程序将再也无法对数据库列解密。

1.  使用 **对象资源管理器** 导航到“安全”>“始终加密密钥”文件夹并找到要替换的现有列主密钥。
2.  右键单击现有的列主密钥，然后选择“清理” 。
3.  查看要删除的列加密密钥值列表。
4.  单击“确定”。

SQL Server Management Studio 将发出 [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md) 语句，以删除用旧列主密钥加密的列加密密钥的加密值。

### <a name="step-5-delete-metadata-for-your-old-column-master-key"></a>步骤 5：删除旧列主密钥的元数据

如果你选择从数据库中删除旧列主密钥的定义，可使用以下步骤。

1. 使用 **对象资源管理器** 导航到“安全”>“始终加密密钥”>“列主密钥”文件夹，并找到要从数据库中删除的旧列主密钥。
2. 右键单击旧列主密钥，然后选择“删除” 。 （这将生成并发出 [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md) 语句，以删除列主密钥元数据。）
3. 单击“确定”。

> [!NOTE]
> 强烈建议你不要在轮换后永久地删除旧的列主密钥。 相反，应该在其当前密钥存储中保留旧的列主密钥，或将其存档在另一个安全位置。 如果在配置新的列主密钥之前，从备份文件将数据库还原到某个时间点，则需要使用旧密钥来访问数据。

### <a name="permissions-for-rotating-column-master-key"></a>轮换列主密钥所需的权限

轮替列主密钥要求具备以下数据库权限：

- **更改任意列主密钥** - 在为新列主密钥创建元数据以及删除旧列主密钥的元数据时需要。
- **更改任意列加密密钥** - 在修改列加密密钥元数据（添加新的加密值）时需要。

你还需要能够访问密钥存储中的旧列主密钥和新列主密钥。 若要访问密钥存储并使用列主密钥，可能需要密钥存储或/和密钥上的权限：
- **证书存储 - 本地计算机** - 必须对用作列主密钥的证书具有读取访问权限，或者是计算机上的管理员。
- **Azure Key Vault** - 需要包含列主密钥的保管库上的 create、get、unwrapKey、wrapKey、sign 和 verify 权限     。
- **密钥存储提供程序(CNG)** - 使用密钥存储或密钥时可能提示你提供必要的权限和凭据，具体取决于存储和 KSP 配置。
- **加密服务提供程序(CAPI)** - 使用密钥存储或密钥时可能提示你提供必要的权限和凭据，具体取决于存储和 CSP 配置。

有关详细信息，请参阅[创建并存储 Always Encrypted 的列主密钥](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)。

<a name="rotatecek"></a> 
## <a name="rotate-column-encryption-keys"></a>轮换列加密密钥

轮替列加密密钥包括对使用要轮替掉的密钥进行加密的所有列中的数据进行解密，并使用新的列加密密钥对数据重新加密。

>[!NOTE]
> 如果包含列（使用要轮替的密钥加密）的表较大，则轮替列加密密钥可能需要很长时间。 对数据重新加密时，应用程序无法写入到受影响的表。 因此，组织需要非常谨慎地计划列加密密钥轮替。
若要轮替列加密密钥，请使用始终加密向导。

1.  为数据库打开该向导：右键单击数据库，指向“任务”，然后单击“加密列”。 
2.  查看“简介”页，然后单击“下一步”。 
3.  在“列选择”  页上，展开表并找到你要替换的所有列，这些列当前使用旧的列加密密钥加密。
4.  对于使用旧列加密密钥进行加密的每个列，将“加密密钥”设置为自动生成的新密钥。 **注意：** 或者，也可以在运行该向导之前创建新的列加密密钥 - 请参阅 [使用“新建列加密密钥”对话框提供列加密密钥](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog)。
5.  在“主密钥配置”页上，选择一个位置来存储新密钥，并选择主密钥源，然后单击“下一步”。 **注意：** 若要使用现有列加密密钥（而不是自动生成的密钥），无需在此页上执行任何操作。
6.  在“验证”页上，选择是要立即运行脚本还是创建 PowerShell 脚本，然后单击“下一步”。 
7.  在“摘要”页上，先审阅已选择的选项，再单击“完成”并在完成后关闭向导。
8.  使用 **对象资源管理器** 导航到“安全”/“始终加密密钥”/“列加密密钥”文件夹，并找到要从数据库中删除的旧列加密密钥。  右键单击该密钥，然后选择“删除” 。

### <a name="permissions-for-rotating-column-encryption-keys"></a>轮换列加密密钥所需的权限

轮替列加密密钥要求具备以下数据库权限：**更改任意列主密钥** - 如果使用自动生成的新列加密密钥（新列主密钥及其新元数据还将生成），此为必需权限。
**更改任意列加密密钥** - 在为新列加密密钥添加元数据时需要。

你还需要能够访问新列加密密钥和旧列加密密钥的列主密钥。 若要访问密钥存储并使用列主密钥，可能需要密钥存储或/和密钥上的权限：
- **证书存储 - 本地计算机** - 必须对用作列主密钥的证书具有读取访问权限，或者是计算机上的管理员。
- **Azure Key Vault** - 需要包含列主密钥的保管库上的 get、unwrapKey 和 verify 权限。
- **密钥存储提供程序(CNG)** - 使用密钥存储或密钥时可能提示你提供必要的权限和凭据，具体取决于存储和 KSP 配置。
- **加密服务提供程序(CAPI)** - 使用密钥存储或密钥时可能提示你提供必要的权限和凭据，具体取决于存储和 CSP 配置。

有关详细信息，请参阅[创建并存储 Always Encrypted 的列主密钥](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)。

## <a name="next-steps"></a>后续步骤
- [通过 SQL Server Management Studio 查询使用 Always Encrypted 的列](always-encrypted-query-columns-ssms.md)
- [使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md)

## <a name="see-also"></a>另请参阅
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Always Encrypted 密钥管理概述](overview-of-key-management-for-always-encrypted.md) 
- [使用 SQL Server Management Studio 配置 Always Encrypted](configure-always-encrypted-using-sql-server-management-studio.md)
- [使用 PowerShell 配置“始终加密”功能](configure-always-encrypted-using-powershell.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)

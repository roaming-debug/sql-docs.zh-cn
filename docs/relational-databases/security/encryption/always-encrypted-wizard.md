---
title: 使用 Always Encrypted 向导配置列加密 | Microsoft Docs
description: 了解如何在 SQL Server 中使用 Always Encrypted 向导为数据库列设置 Always Encrypted 配置。
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.alwaysencryptedwizard.encryption.f1
- sql13.swb.alwaysencryptedwizard.f1
- sql13.swb.alwaysencryptedwizard.masterkey.f1
helpviewer_keywords:
- Wizard, Always Encrypted
ms.assetid: 68daddc9-ce48-49aa-917f-6dec86ad5af5
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bedf502949e206b45bf7e1e332962c8c093d1bcb
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236700"
---
# <a name="configure-column-encryption-using-always-encrypted-wizard"></a>使用 Always Encrypted 向导配置列加密
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Always Encrypted 向导是一个功能强大的工具，可让你为所选数据库列设置所需的 [Always Encrypted](always-encrypted-database-engine.md) 配置。 根据当前配置和所需的目标配置，该向导可以对列加密或解密（删除加密），或重新对其加密（例如，使用为列配置的新列加密密钥或不同于当前类型的加密类型）。 可以在该向导的单次运行中配置多个列。

该向导允许使用现有列加密密钥对列进行加密，也可以选择生成新的列加密密钥，或同时生成新的列加密密钥和新的列主密钥。 

该向导的工作方式是将数据移出数据库，并在 SSMS 进程中执行加密操作。 该向导会在数据库中创建具有所需加密配置的一个或多个新表，从原始表中加载所有数据，执行请求的加密操作，将数据上传到新表中，然后将原始表换为新表。

> [!NOTE]
> 运行加密操作可能需要很长时间。 在此期间，数据库不可用于写入事务。 建议将 PowerShell 作为对较大表执行加密操作的工具。 请参阅[通过 PowerShell 配置使用 Always Encrypted 的列加密](configure-column-encryption-using-powershell.md)。

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> 如果使用的是 [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)]，并且 SQL Server 实例配置了安全 Enclave，则无需将数据移出数据库，即可就地运行加密操作。 请参阅[使用具有安全 enclave 的 Always Encrypted 就地配置列加密](always-encrypted-enclaves-configure-encryption.md)。 请注意，该向导不支持就地加密。

::: moniker-end

建议使用 PowerShell 

 - 有关显示如何使用向导配置 Always Encrypted 并在客户端应用程序中使用它的端到端演练，请参阅以下 Azure SQL 数据库教程：
    - [使用 Always Encrypted 和 Windows 证书存储中的列主密钥保护 Azure SQL 数据库中的敏感数据](/azure/azure-sql/database/always-encrypted-certificate-store-configure)
    - [使用 Always Encrypted 和 Azure Key Vault 中的列主密钥保护 Azure SQL 数据库中的敏感数据](/azure/sql-database/sql-database-always-encrypted-azure-key-vault)

 - 有关包括使用该向导的视频，请参阅 [使用始终加密保持敏感数据的安全](https://channel9.msdn.com/events/DataDriven/SQLServer2016/AlwaysEncrypted)。 此外，还可参阅 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安全团队博客 [SSMS Encryption Wizard - Enabling Always Encrypted in a Few Easy Steps](https://techcommunity.microsoft.com/t5/SQL-Server/SSMS-Encryption-Wizard-Enabling-Always-Encrypted-in-a-Few-Easy/ba-p/384545)（SSMS 加密向导 - 用几个简单的步骤启用 Always Encrypted）。  
 - 有关 Always Encrypted 密钥的信息，请参阅 [Always Encrypted 密钥管理概述](overview-of-key-management-for-always-encrypted.md)。
 - 有关 Always Encrypted 中支持的加密类型的信息，请参阅[选择确定性加密或随机加密](always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption)。
 
 ## <a name="permissions"></a>权限
若要使用该向导执行加密操作，必须拥有“查看任意列主密钥定义”和“查看任意列加密密钥定义”权限。 还必须拥有访问所使用的列主密钥的权限（在包含密钥的密钥存储中）：
- **证书存储 - 本地计算机** - 必须对用作列主密钥的证书具有读取访问权限，或者是计算机上的管理员。
- **Azure Key Vault** - 需要包含列主密钥的保管库上的 get、unwrapKey 和 verify 权限。
- **密钥存储提供程序(CNG)** - 使用密钥存储或密钥时可能提示你提供必要的权限和凭据，具体取决于存储和 KSP 配置。
- **加密服务提供程序(CAPI)** - 使用密钥存储或密钥时可能提示你提供必要的权限和凭据，具体取决于存储和 CSP 配置。

此外，如果使用该向导创建新密钥，则必须拥有在[使用“新建列主密钥”对话框预配列主密钥](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog)和[使用“新建列加密密钥”对话框预配列加密密钥](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog)中列出的其他权限。

## <a name="open-the-always-encrypted-wizard"></a>打开 Always Encrypted 向导
可以在三个不同的级别启动该向导： 
- 在数据库级别 - 如果要加密位于不同表中的多列。
- 在表级别 - 如果要加密位于相同表中的多列。
- 在列级别 - 如果要加密一个特定列。
 
 1. 使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的对象资源管理器组件连接到 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]。  
   
 2. 若要进行加密，请执行以下操作：
     1. 对于位于数据库不同表中的多列，请右键单击数据库，指向“任务”，然后选择“加密列”。
     1. 对于位于相同表中的多列，请导航到该表，右键单击它，然后选择“加密列”。
     1. 对于单个列，请导航到该列，右键单击它，然后选择“加密列”。


   
 ## <a name="column-selection-page"></a>列选择页
在此页中，可选择要加密、重新加密或解密的列，并为所选列定义目标加密配置。

若要加密纯文本列（未加密的列），请为列选择加密类型（“确定性”或“随机”）和加密密钥。 

若要为已加密列更改加密类型或轮换（更改）列加密密钥，请选择所需加密类型和密钥。 

如果希望该向导使用新的列加密密钥加密或重新加密一列或多列，请选取在名称中包含“(New)”的密钥。 该向导会生成密钥。

若要解密当前已加密的列，请为加密类型选择“纯文本”。


> [!NOTE]
> 该向导不支持对临时表和内存中表执行加密操作。 可以使用 Transact-SQL 创建空的临时表或内存中表，并使用应用程序插入数据。

## <a name="master-key-configuration-page"></a>主密匙配置页
如果在上一页中为任何列选择了自动生成的列加密密钥，则在此页中需要选择现有列主密钥，或者配置将加密列加密密钥的新的列主密钥。 

配置新的列主密钥时，可以在 Windows 证书存储或 Azure Key Vault 中选取现有密钥，并让该向导仅在数据库中为密钥创建元数据对象，也可以选择在数据库中同时生成密钥和用于描述密钥的元数据对象。 

有关在 Windows 证书存储、Azure Key Vault 或其他密钥存储中创建和存储列主密钥的详细信息，请参阅[创建并存储 Always Encrypted 的列主密钥](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)。

> [!TIP]
> 该向导仅允许在 Windows 证书存储和 Azure Key Vault 中浏览和创建密钥。 它还会自动生成新密钥和用于描述密钥的数据库元数据对象的名称。 如果你需要更好地控制密钥预配方式（并对包含列主密钥的密钥存储有更多选择），可以使用“新建列主密钥”和“新建列加密密钥”对话框先创建密钥，然后运行该向导并选取所创建的密钥。 请参阅[使用“新建列主密钥”对话框预配列主密钥](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog)和[使用“新建列加密密钥”对话框预配列加密密钥](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog)。 

## <a name="next-steps"></a>后续步骤
- [通过 SQL Server Management Studio 查询使用 Always Encrypted 的列](always-encrypted-query-columns-ssms.md)
- [使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md)

## <a name="see-also"></a>另请参阅  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
 - [Always Encrypted 密钥管理概述](overview-of-key-management-for-always-encrypted.md) 
 - [使用 SQL Server Management Studio 配置 Always Encrypted](configure-always-encrypted-using-sql-server-management-studio.md)
 - [使用 PowerShell 预配 Always Encrypted 密钥](configure-always-encrypted-keys-using-powershell.md)
 - [通过 PowerShell 配置使用 Always Encrypted 的列加密](configure-column-encryption-using-powershell.md)
 - [使用 Always Encrypted 和 DAC 包配置列加密](configure-always-encrypted-using-dacpac.md)
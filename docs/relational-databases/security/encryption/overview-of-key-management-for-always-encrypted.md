---
title: Always Encrypted 密钥管理概述 | Microsoft Docs
description: 了解如何管理 Always Encrypted 用于在 SQL Server 中保护数据的两种加密密钥：列加密密钥和列主密钥。
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.prod_service: security, sql-database"
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 07a305b1-4110-42f0-b7aa-28a4e32e912a
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 234f6233e2f6b024e6ad74a82f403e3df7db9a05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340181"
---
# <a name="overview-of-key-management-for-always-encrypted"></a>Always Encrypted 密钥管理概述
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]


[始终加密](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) 功能使用两种类型的加密密钥来保护数据 - 一个密钥用于加密数据，另一个密钥对加密数据的密钥进行加密。 列加密密钥对数据进行加密，列主密钥对列加密密钥进行加密。 本文提供了有关管理这些加密密钥的详细概述。  

讨论“始终加密”密钥和密钥管理时，了解实际的加密密钥之间的区别和用于描述这些密钥的元数据对象很重要。 我们使用术语 **列加密密钥** 和 **列主密钥** 来指代实际的加密密钥，以及使用 **列加密密钥元数据** 和 **列主密钥元数据** 来指代数据库中的“始终加密”密钥说明。

- ***列加密密钥*** 是用于加密数据的内容加密密钥。 正如其名称所暗示的，列加密密钥用于加密数据库列中的数据。 可以使用相同的列加密密钥对一个或多个列进行加密，或者可以根据应用程序的要求使用多个列加密密钥。 列加密密钥本身也被加密，只有列加密密钥的加密的值存储在数据库中（作为列加密密钥元数据的一部分）。 列加密密钥元数据存储在 [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md) 和 [sys.column_encryption_key_values (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md) 目录视图中。 使用 AES-256 算法的列加密密钥的长度为 256 位。


- ***列主密钥*** 是用于加密列加密密钥的保护密钥的密钥。 列主密钥必须存储在受信任的密钥存储，例如 Windows 证书存储、Azure 密钥保管库或硬件安全模块。 数据库仅包含有关列主密钥的元数据（密钥存储的类型和位置）。 列主密钥元数据存储在 [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md) 目录视图中。  

需要注意的是，数据库系统中的密钥元数据不包含纯文本列主密钥或纯文本列加密密钥。 数据库仅包含有关列主密钥的类型和位置，以及列加密密钥的加密值的信息。 这意味着始终不会向数据库系统暴露纯文本密钥，这样可以确保即使数据库系统遭到入侵，使用“始终加密”功能保护的数据也是安全的。 若要确保数据库系统无法访问纯文本密钥，请务必在除托管数据库的计算机以外的其他计算机上运行密钥管理工具 - 请查看下面的 [密钥管理的安全注意事项](#security-considerations-for-key-management) 一节了解详细信息。

因为数据库仅包含加密的数据（位于“始终加密”保护的列），并且不能访问纯文本密钥，因此无法解密数据。 这意味着查询“始终加密”列将只返回加密的值，因此，需要加密或解密受保护的数据的客户端应用程序必须能够访问列主密钥和相关的列加密密钥。 有关详细信息，请参阅[使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md)。



## <a name="key-management-tasks"></a>密钥管理任务

管理密钥的过程可以分为以下几个高级任务：

- **密钥预配** - 在受信任的密钥存储（例如，在 Windows 证书存储、Azure 密钥保管库或硬件安全模块）中创建物理密钥、使用列主密钥加密列加密密钥，以及在数据库中为两种类型的密钥创建元数据。

- **密钥轮换** - 定期使用新密钥替换现有密钥。 如果密钥已泄漏，或者为了遵守强制规定必须轮换加密密钥的组织的策略或遵从性规则，则可能需要轮换密钥。 


## <a name="key-management-roles"></a><a name="KeyManagementRoles"></a> 密钥管理角色

存在两种不同的用户角色来管理“始终加密”密钥：安全管理员和数据库管理员 (DBA)：

- **安全管理员** - 生成列加密密钥和列主密钥，以及管理包含列主密钥的密钥存储。 若要执行这些任务，安全管理员需要能够访问密钥和密钥存储，但不需要访问数据库。
- **DBA** - 管理数据库中的密钥的元数据。 若要执行密钥管理任务，DBA 需要能够管理数据库中的密钥元数据，但不需要访问密钥或托管列主密钥的密钥存储。

考虑到上述角色，执行“始终加密”的密钥管理任务有两种不同方式：使用角色分离和不使用角色分离。 根据你的组织的需求，你可以选择最适合你的要求的密钥管理过程。

## <a name="managing-keys-with-role-separation"></a>使用角色分离管理密钥
当使用角色分离管理“始终加密”密钥时，将由组织中的不同人员来分别承担安全管理员和 DBA 角色。 使用角色分离的密钥管理过程可确保 DBA 不具有访问密钥或托管实际密钥的密钥存储的权限，而安全管理员则不具有访问包含敏感数据的数据库的权限。 如果你的目标是确保组织中的 DBA 无法访问敏感数据，则建议使用角色分离管理密钥。 

**注意：** 安全管理员生成并处理纯文本密钥，因此他们不得在托管数据库系统的相同计算机上执行任务，也不得在可被 DBA 或可能成为潜在对手的其他任何人访问的计算机上执行任务。 

## <a name="managing-keys-without-role-separation"></a>不使用角色分离管理密钥
当不使用角色分离管理“始终加密”密钥时，一个人可以同时承担安全管理员和 DBA 角色，这表明此人需要能够访问并管理密钥/密钥存储和密钥元数据。 如果组织使用的是 DevOps 模型，或者数据库托管在云中且主要目的是限制云管理员（而不是本地 DBA）访问敏感数据，则推荐不使用角色分离管理密钥。



## <a name="tools-for-managing-always-encrypted-keys"></a>管理“始终加密”密钥的工具

可以使用 [SQL Server Management Studio (SSMS)](../../../ssms/sql-server-management-studio-ssms.md) 和 [PowerShell](../../../powershell/sql-server-powershell.md)管理“始终加密”密钥：

- **SQL Server Management Studio (SSMS)** - 通过对话框和向导合并涉及密钥存储访问和数据库访问的任务，因此 SSMS 不支持角色分离，但是它会让你的密钥配置很轻松。 有关使用 SSMS 管理密钥的详细信息，请参阅：
    - [使用 SQL Server Management Studio 预配 Always Encrypted 密钥](configure-always-encrypted-keys-using-ssms.md)
    - [使用 SQL Server Management Studio 轮换 Always Encrypted 密钥](rotate-always-encrypted-keys-using-ssms.md)

- **SQL Server PowerShell** - 包括使用和不使用角色分离管理“Always Encrypted”密钥的 cmdlet。 有关详细信息，请参阅：
    - [使用 PowerShell 配置 Always Encrypted 密钥](../../../relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell.md)
    - [使用 PowerShell 轮换 Always Encrypted 密钥](../../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell.md)


## <a name="security-considerations-for-key-management"></a>密钥管理的安全注意事项

“始终加密”的主要目的是确保即使数据库系统或其宿主环境遭到入侵，存储在数据库中的敏感数据也是安全的。 “始终加密”可在以下安全攻击示例中帮助防止敏感数据泄露：

- 存心不良的具有高权限的数据库用户（例如 DBA）查询敏感数据列。
- 托管 SQL Server 实例的计算机的心怀恶意的管理员扫描 SQL Server 进程的内存或 SQL Server 进程转储文件。
- 心怀恶意的数据中心操作员查询客户数据库、检查 SQL Server 转储文件，或检查托管云中的客户数据的计算机的内存。
- 在托管数据库的计算机上运行的恶意软件。

若要确保“始终加密”在阻止这些类型的攻击中有效，密钥管理过程必须确保列主密钥和列加密密钥，以及包含列主密钥的密钥存储的凭据永远不会暴露给潜在的攻击者。 下面是需要遵守的一些准则：

- 不在托管数据库的计算机上生成列主密钥或列加密密钥。 在单独的计算机上生成密钥，该计算机专门用于密钥管理，或者是该计算机中托管的应用程序需要访问密钥。 这意味着不要在托管数据库的计算机上运行用于生成密钥的工具，因为如果攻击者访问用于预配或维护“Always Encrypted”密钥的计算机，即使密钥仅在工具的内存中出现了一会，攻击者也有可能获取密钥。
- 为确保密钥管理过程不会无意中泄露列主密钥或列加密密钥，在定义并执行密钥管理过程之前识别潜在的攻击者和安全威胁很重要。 例如，如果你的目标是要确保 DBA 没有访问敏感数据的权限，那么 DBA 就不能负责生成密钥。 但是，DBA 可以管理数据库中的密钥元数据，因为元数据不包含纯文本密钥。

## <a name="next-steps"></a>后续步骤
- [使用 Always Encrypted 向导配置列加密](always-encrypted-wizard.md)
- [创建并存储 Always Encrypted 的列主密钥](create-and-store-column-master-keys-always-encrypted.md)
- [使用 SQL Server Management Studio 预配 Always Encrypted 密钥](configure-always-encrypted-keys-using-ssms.md)
- [使用 PowerShell 预配 Always Encrypted 密钥](configure-always-encrypted-keys-using-powershell.md)

## <a name="see-also"></a>另请参阅
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [始终加密向导教程（Azure 密钥保管库）](/azure/azure-sql/database/always-encrypted-azure-key-vault-configure)
- [始终加密向导教程（Windows 证书存储）](/azure/azure-sql/database/always-encrypted-certificate-store-configure)
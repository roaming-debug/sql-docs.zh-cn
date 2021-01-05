---
description: 使用 Always Encrypted 和 DAC 包配置列加密
title: 使用 Always Encrypted 和 DAC 包配置列加密 | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 506abf476e04ab1a40059d04031949fa80b31acf
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97559319"
---
# <a name="configure-column-encryption-using-always-encrypted-with-a-dac-package"></a>使用 Always Encrypted 和 DAC 包配置列加密 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

[数据层应用程序 (DAC) 包](../../data-tier-applications/data-tier-applications.md)（也称为 DACPAC）是 SQL Server 数据库部署的一个可移植单元，它定义了所有 SQL Server 对象，包括表和表中的列。 将 DACPAC 发布到数据库时（使用 DACPAC 升级数据库），目标数据库的架构将会更新以匹配 DACPAC 中的架构。 可以使用 SQL Server Management Studio、[PowerShell](../../data-tier-applications/upgrade-a-data-tier-application.md#UpgradeDACPowerShell) 或 [sqlpackage](../../../tools/sqlpackage/sqlpackage-publish.md) 中的[升级数据层应用程序向导](../../data-tier-applications/upgrade-a-data-tier-application.md#UsingDACUpgradeWizard)来发布 DACPAC。

本文介绍了在 DACPAC 或/和目标数据库包含受 [Always Encrypted](always-encrypted-database-engine.md) 保护的列时升级数据库的特殊注意事项。 如果 DACPAC 中列的加密方案与目标数据库中现有列的加密方案不同，则发布 DACPAC 将导致对该列中存储的数据进行加密、解密或重新加密。 有关详细信息，请参阅下表。

| 条件|操作|
|:---|:---|
|列已在 DACPAC 中加密，但未在数据库中加密。| 将加密该列中的数据。|
|列未在 DACPAC 中加密，但已在数据库中加密。| 将解密该列中的数据（将为该列删除加密）。|
| 列在 DACPAC 和数据库中均已加密，但 DACPAC 中的列所使用的加密类型或/和列加密密钥与数据库中的相应列不同。|将解密该列中的数据，然后重新加密，以匹配 DACPAC 中的加密配置。|

部署 DAC 包也可能会导致针对 Always Encrypted 为列主密钥或列加密密钥创建或删除元数据对象。

## <a name="performance-considerations"></a>性能注意事项
要执行加密操作，用于部署 DACPAC 的工具需要将数据移出数据库。 该工具会在数据库中创建具有所需加密配置的一个或多个新表，从原始表加载所有数据，执行请求的加密操作，将数据上传到新表，然后将原始表换为新表。 运行加密操作可能需要很长时间。 在此期间，数据库不可用于写入事务。 

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> 如果使用的是 [!INCLUDE [sssqlv15-md](../../../includes/sssqlv15-md.md)]，并且 SQL Server 实例配置了安全 Enclave，则无需将数据移出数据库，即可就地运行加密操作。 请参阅[使用具有安全 enclave 的 Always Encrypted 就地配置列加密](always-encrypted-enclaves-configure-encryption.md)。 请注意，就地加密不适用于 DACPAC 部署。

::: moniker-end

## <a name="permissions-for-publishing-a-dac-package-if-always-encrypted-is-set-up"></a>用于发布 DAC 包的权限（如果设置了 Always Encrypted）

要在 DACPAC 和/或目标数据库中设置了 Always Encrypted 的情况下发布 DAC 包，可能需要以下部分或所有权限，具体视 DACPAC 架构与目标数据库架构的差异而定。

*更改任意列主密钥密钥*、 *更改任意列加密密钥*、 *查看任意列主密钥定义*、 *查看任意列加密密钥定义*

如果升级操作触发数据加密操作，则还需要能够访问为受影响的列配置的列主密钥：

- **证书存储 - 本地计算机** - 必须对用作列主密钥的证书具有读取访问权限，或者是计算机上的管理员。
- **Azure Key Vault** - 需要包含列主密钥的保管库上的 create、get、unwrapKey、wrapKey、sign 和 verify 权限。
- **密钥存储提供程序(CNG)** - 使用密钥存储或密钥时可能提示你提供必要的权限和凭据，具体取决于存储和 KSP 配置。
- **加密服务提供程序(CAPI)** - 使用密钥存储或密钥时可能提示你提供必要的权限和凭据，具体取决于存储和 CSP 配置。

有关详细信息，请参阅 [创建并存储列主密钥 (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)。 

 
## <a name="next-steps"></a>后续步骤
- [使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md)
- [通过 SQL Server Management Studio 查询使用 Always Encrypted 的列](always-encrypted-query-columns-ssms.md)

## <a name="see-also"></a>另请参阅  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
 - [Always Encrypted 密钥管理概述](overview-of-key-management-for-always-encrypted.md) 
 - [使用 SQL Server Management Studio 配置 Always Encrypted](configure-always-encrypted-using-sql-server-management-studio.md)
 - [使用 Always Encrypted 向导配置列加密](always-encrypted-wizard.md)
 - [通过 PowerShell 配置使用 Always Encrypted 的列加密](configure-column-encryption-using-powershell.md)
 

---
description: 轮换已启用 enclave 的密钥
title: 轮换 | Microsoft Docs
ms.custom: ''
ms.date: 10/31/2019
ms.prod: sql
ms.reviewer: vanto
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 472b37589167899314e293ff15c920bba3208962
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477548"
---
# <a name="rotate-enclave-enabled-keys"></a>轮换已启用 enclave 的密钥
[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

在 Always Encrypted 中，密钥轮换是将现有列主密钥或列加密密钥替换为新密钥的过程。 本文介绍当初始密钥和/或目标（新）密钥是已启用 enclave 的密钥时，特定于[具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves.md) 的密钥轮换的用例和注意事项。 有关管理 Always Encrypted 密钥的一般准则和流程，请参阅 [Always Encrypted 密钥管理概述](overview-of-key-management-for-always-encrypted.md)。 

出于安全或合规性原因，可能需要轮换密钥。 例如，如果某个密钥已泄露，或者组织的策略要求定期替换密钥。 此外，具有安全 enclave 的 Always Encrypted 密钥轮换提供了一种方法，可为加密列启用或禁用服务器端安全 enclave 的功能。
- 使用已启用 enclave 的密钥替换未启用 enclave 的密钥时，会向对使用密钥保护的一列或多列进行的查询解锁安全 enclave 的功能。 请参阅[为现有加密列启用具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves-enable-for-encrypted-columns.md)。
 - 使用未启用 enclave 的密钥替换已启用 enclave 的密钥时，会向对使用密钥保护的一列或多列进行的查询禁用安全 enclave 的功能。

如果只是出于安全/合规性原因轮换密钥，而不是为列启用或禁用 enclave 计算，请确保目标密钥在 enclave 方面具有与源密钥相同的配置。 例如，如果源密钥已启用 enclave，则目标密钥也应已启用 enclave。

以下高级步骤包括指向详细文章的链接，具体取决于轮换方案：

1. 预配新密钥（列主密钥或列加密密钥）。
    - 若要预配已启用 enclave 的新 enclave 密钥，请参阅[预配已启用 enclave 的密钥](always-encrypted-enclaves-provision-keys.md)。
    - 若要预配未启用 enclave 的密钥，请参阅[使用 SQL Server Management Studio 预配 Always Encrypted 密钥](configure-always-encrypted-keys-using-ssms.md)和[使用 PowerShell 预配 Always Encrypted 密钥](configure-always-encrypted-keys-using-powershell.md)。
2. 使用新密钥替换现有密钥。
    - 如果要轮换列加密密钥，并且源密钥和目标密钥都已启用 enclave，则可以运行就地轮换（这涉及重新加密数据）。 请参阅[使用具有安全 enclave 的 Always Encrypted 就地配置列加密](always-encrypted-enclaves-configure-encryption.md)。
    - 有关轮换密钥的详细步骤，请参阅[使用 SQL Server Management Studio 轮换 Always Encrypted 密钥](rotate-always-encrypted-keys-using-ssms.md)和[使用 PowerShell 轮换 Always Encrypted 密钥](rotate-always-encrypted-keys-using-powershell.md)。

    
## <a name="next-steps"></a>后续步骤
- [查询使用具有安全 enclave 的 Always Encrypted 的列](always-encrypted-enclaves-query-columns.md)
- [使用具有安全 Enclave 的 Always Encrypted 就地配置列加密](always-encrypted-enclaves-configure-encryption.md)
- [为现有加密列启用具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves-enable-for-encrypted-columns.md)
- [使用具有安全 enclave 的 Always Encrypted 开发应用程序](always-encrypted-enclaves-client-development.md)  

## <a name="see-also"></a>另请参阅  
- [管理具有安全 enclave 的 Always Encrypted 的密钥](always-encrypted-enclaves-manage-keys.md)


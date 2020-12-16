---
description: 使用具有安全 Enclave 的 Always Encrypted 就地配置列加密
title: 使用具有安全 Enclave 的 Always Encrypted 就地配置列加密 | Microsoft Docs
ms.custom: ''
ms.date: 10/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: de9860fdf161d9ed43a1ae2c63e1210dd2079e42
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477708"
---
# <a name="configure-column-encryption-in-place-using-always-encrypted-with-secure-enclaves"></a>使用具有安全 Enclave 的 Always Encrypted 就地配置列加密 
[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[具有安全 Enclave 的 Always Encrypted](always-encrypted-enclaves.md)支持对数据库列执行就地加密操作 - 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中的安全 enclave 内。 使用就地加密，无需将此类操作的数据移出数据库，从而使加密操作更快、更可靠。 

> [!NOTE]
> 虽然就地加密具有性能优势，但对大型表进行加密操作可能会花费很长时间并且消耗大量资源，从而可能会影响和降低应用程序的性能和可用性。

就地加密还可以使用 [ALTER TABLE ALTER COLUMN (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md) 语句来触发加密操作，如果没有 enclave，则此操作无法实现。

## <a name="prerequisites"></a>先决条件
支持的加密操作以及用于该操作的列加密密钥的要求如下：
- 加密明文列。 用于加密列的列加密密钥必须已启用 enclave。
- 使用新的加密类型或/和新的列加密密钥重新加密已加密的列。 当前列加密密钥和新列加密密钥（如果不同于当前密钥）都必须已启用 enclave。
- 解密已加密的列 - 保护列的列加密密钥必须已启用 enclave。

请参阅[管理具有安全 Enclave 的 Always Encrypted 的密钥](always-encrypted-enclaves-manage-keys.md)，了解有关如何确保列加密密钥已启用 enclave 的信息。

就地加密还需要具有正确初始化的安全 enclave 的 SQL Server 实例。 请参阅[为 Always Encrypted 服务器配置选项配置 enclave 类型](../../../database-engine/configure-windows/configure-column-encryption-enclave-type.md)。

触发加密操作的用户或应用程序必须有权对包含受影响的列的表进行架构更改，并有权访问操作中涉及的列主密钥以及数据库中的相关密钥元数据。

只能使用 SQL Server Management Studio 或自定义应用程序中的 [ALTER TABLE ALTER COLUMN (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md) 触发就地加密。 请参阅[使用 Transact-SQL 就地配置列加密](always-encrypted-enclaves-configure-encryption-tsql.md)。

> [!NOTE]
> 当前，[Always Encrypted 向导](always-encrypted-wizard.md)和 [Set-SqlColumnEncryption](/powershell/module/sqlserver/set-sqlcolumnencryption) cmdlet 不支持就地加密，即使配置满足上述要求，也始终下载用于加密操作的数据。 

## <a name="next-steps"></a>后续步骤
- [使用 Transact-SQL 就地配置列加密](always-encrypted-enclaves-configure-encryption-tsql.md)
- [使用具有安全 Enclave 的 Always Encrypted 对列创建和使用索引](always-encrypted-enclaves-create-use-indexes.md)
- [使用具有安全 enclave 的 Always Encrypted 开发应用程序](always-encrypted-enclaves-client-development.md)
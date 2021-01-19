---
description: 管理具有安全 enclave 的 Always Encrypted 的密钥
title: 管理具有安全 enclave 的 Always Encrypted 的密钥 | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: vanto
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 68cc07fdf37dc358d0de024c3227f7225ebea27c
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534433"
---
# <a name="manage-keys-for-always-encrypted-with-secure-enclaves"></a>管理具有安全 enclave 的 Always Encrypted 的密钥

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves.md) 通过引入已启用 enclave 的密钥来扩展 [Always Encrypted](always-encrypted-database-engine.md) 的密钥管理： 

- **已启用 enclave 的列主密钥** - 使用数据库内的列主密钥元数据对象中指定的 `ENCLAVE_COMPUTATIONS` 属性创建的列主密钥。 
- **已启用 enclave 的列加密密钥** – 使用已启用 enclave 的列主密钥加密的列加密密钥。 只有已启用 enclave 的列加密密钥才能用于服务器端安全 enclave 中的计算。 

[管理 Always Encrypted 密钥](overview-of-key-management-for-always-encrypted.md)的一般准则和流程适用于管理已启用 enclave 的密钥。 

## <a name="managing-keys"></a>管理密钥

以下文章介绍了管理已启用 enclave 的密钥的特定方面。

- [预配已启用 enclave 的密钥](always-encrypted-enclaves-provision-keys.md)
- [轮换已启用 enclave 的密钥](always-encrypted-enclaves-rotate-keys.md)

## <a name="next-steps"></a>后续步骤
- [预配已启用 enclave 的密钥](always-encrypted-enclaves-provision-keys.md)

## <a name="see-also"></a>另请参阅  
- [Always Encrypted 密钥管理概述](overview-of-key-management-for-always-encrypted.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)

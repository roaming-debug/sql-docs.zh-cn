---
description: 查询使用具有安全 enclave 的 Always Encrypted 的列
title: 查询使用具有安全 enclave 的 Always Encrypted 的列 | Microsoft Docs
ms.custom: ''
ms.date: 10/31/2019
ms.reviewer: vanto
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: 8daabf320e0f736bcfabc5addb8508320b10bb8f
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88482217"
---
# <a name="query-columns-using-always-encrypted-with-secure-enclaves"></a>查询使用具有安全 enclave 的 Always Encrypted 的列
[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

本文介绍有关对使用[具有 enclave 的 Always Encrypted](always-encrypted-enclaves.md) 的服务器端安全 enclave 的已加密列运行查询的常规注意事项。 

以下类型的查询涉及使用安全 enclave：
- 使用已启用 enclave 的密钥触发就地加密操作的查询 - 请参阅 [使用 Transact-SQL 就地配置列加密](always-encrypted-enclaves-configure-encryption-tsql.md)。
- 丰富查询 - 对使用随机加密和已启用 enclave 的密钥加密的列进行的范围比较或模式匹配操作。
- 在使用随机加密和已启用 enclave 的密钥加密的列上创建或更新索引的查询。 有关详细信息，请参阅[对使用具有安全 enclave 的 Always Encrypted 的列创建和使用索引](always-encrypted-enclaves-create-use-indexes.md)。

> [!NOTE]
> 仅在使用随机加密且已启用 enclave 的列（使用已启用 enclave 的列加密密钥加密的列）上才支持丰富查询和索引。 确定性加密不支持丰富查询或索引。

> [!NOTE]
> 对已加密字符串列进行的丰富查询要求列使用包含 BIN2 排序顺序的排序规则（即 BIN2 排序规则）。 


## <a name="next-steps"></a>后续步骤
- [通过 SSMS 查询使用具有安全 enclave 的 Always Encrypted 的列](always-encrypted-enclaves-query-columns-ssms.md)

## <a name="see-also"></a>另请参阅
- [教程：通过 SSMS 开始使用具有安全 enclave 的 Always Encrypted](../tutorial-getting-started-with-always-encrypted-enclaves.md)


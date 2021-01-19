---
description: 为现有加密列启用具有安全 enclave 的 Always Encrypted
title: 为现有加密列启用具有安全 enclave 的 Always Encrypted | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 03a57949624897b2cd2e0552892803c21a408b52
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534806"
---
# <a name="enable-always-encrypted-with-secure-enclaves-for-existing-encrypted-columns"></a>为现有加密列启用具有安全 enclave 的 Always Encrypted 
[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

本文介绍如何解锁具有安全 enclave 的 Always Encrypted 的功能，以及如何对现有加密列启用 enclave 计算。  

如果已使用未启用 enclave 的密钥加密了现有列，则可以使用已启用 enclave 的密钥来加密这些列。 这样做可使你在查询中对列使用安全的 enclave。

可以通过几种不同的方式为现有加密列启用 enclave 计算，具体取决于以下内容：

- 作用域/粒度：你是否想要为列子集或使用给定列主密钥保护的所有列启用 enclave 功能？
- 数据大小：包含想要启用 enclave 的列的表的大小是什么？
- 此外，是否要更改列的加密类型？ 请记住，唯一的随机的加密支持丰富计算（模式匹配、比较运算符）。 如果列是使用确定性加密进行加密，则还需要使用随机加密对它进行重新加密，才能解锁丰富的计算功能。

下面几节介绍用于为现有列启用 enclave 的三种方法。

## <a name="method-1-rotate-the-column-master-key-to-replace-it-with-an-enclave-enabled-column-master-key"></a>方法 1：轮换列主密钥，将其替换为已启用 enclave 的列主密钥
将未启用 enclave 的现有列主密钥替换为已启用 enclave 的新列主密钥，这可以有效地为所有与列主密钥相关联的列加密密钥也启用 enclave。

- 优点：
  - 不涉及重新加密数据，因此这种方法通常是最快的。 建议对包含大量数据的列（已启用丰富的计算功能并使用确定性加密）使用此方法。
  - 可以大规模地为多个列启用 enclave 功能。 将列主密钥替换为启用 enclave 的列主密钥，这可以为与原始列主密匙相关联的所有列加密密钥和所有加密列启用 enclave。
  
- 缺点：
  - 不支持将加密类型从确定性更改为随机。 虽然该方法可为使用确定性加密进行加密的列解锁就地加密功能，但不会启用丰富的计算功能。 需要使用随机加密来重新加密这些列。
  - 禁止选择性地转换一些与给定列主密钥关联的列。
  - 产生密钥管理开销。 需要创建新的列主密钥并使其可用于查询受影响列的应用程序。

有关如何轮换列主密钥的信息，请参阅[轮换启用 enclave 的密钥](always-encrypted-enclaves-rotate-keys.md)。

## <a name="method-2-rotate-the-column-master-key-and-re-encrypt-columns-using-randomized-encryption-in-place"></a>方法 2：轮换列主密钥并使用就地随机加密来重新加密列
此方法首先执行方法 1，然后重新加密列。 列开始使用确定性加密，然后使用随机加密进行重新加密以解锁丰富的查询功能。

- 优点：
  - 就地重新加密数据。 如果需要对当前使用确定性加密并包含大量数据的加密列启用丰富的查询功能，则建议使用此方法。 步骤 1（列主密钥轮换）为使用确定性加密的列解锁就地加密功能，步骤 2（重新加密列）可以就地执行。
  - 可以大规模地为多个列启用 enclave 功能。
  
- 缺点：
  - 禁止选择性地转换一些与给定列主密钥关联的列。
  - 这会产生密钥管理开销。 需要创建新的列主密钥并使其可用于查询受影响列的应用程序。

有关如何轮换列主密钥和重新就地加密列以轮换列加密密钥的信息，请参阅[轮换启用 enclave 的密钥](always-encrypted-enclaves-rotate-keys.md)。

## <a name="method-3-re-encrypt-a-selected-column-with-an-enclave-enabled-column-encryption-key-on-the-client-side"></a>方法 3：在客户端使用启用 enclave 的列加密密钥重新加密所选列
此方法涉及使用启用 enclave 的列加密密钥对列进行重新加密，可使用随机加密来启用丰富查询功能。 由于当前的列加密密钥未启用 enclave，因此无法就地重新加密该列。 使用 Always Encrypted 向导或 Set-SqlColumnEncryption cmdlet 重新加密数据库外部的列。

- 优点：
  - 允许选择性地为一列或一小部分列启用 enclave 功能（如果要使用随机加密重新加密列，即为就地加密和丰富的查询功能）。
  - 此方法可通过一个步骤为列启用丰富的计算功能。
  - 由于无需新建列主密钥，因此它对应用程序的影响较小。
  
- 缺点：
  - 为了重新加密数据，工具会将该数据移出数据库，这可能需要很长时间，而且容易出现网络错误。

有关如何通过客户端工具轮换列加密的详细信息，请参阅[使用 SQL Server Management Studio 轮换 Always Encrypted 密钥](rotate-always-encrypted-keys-using-ssms.md)和[使用 PowerShell 轮换 Always Encrypted 密钥](rotate-always-encrypted-keys-using-powershell.md)。

## <a name="next-steps"></a>后续步骤
- [使用安全 enclave 运行 Transact-SQL 语句](always-encrypted-enclaves-query-columns.md)
- [使用具有安全 enclave 的 Always Encrypted 开发应用程序](always-encrypted-enclaves-client-development.md)

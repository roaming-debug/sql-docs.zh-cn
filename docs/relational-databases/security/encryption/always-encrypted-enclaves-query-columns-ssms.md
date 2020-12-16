---
description: 通过 SSMS 查询使用具有安全 enclave 的 Always Encrypted 的列
title: '通过 SSMS 查询使用具有安全 enclave 的 Always Encrypted 的列 | Microsoft Docs '
ms.custom: ''
ms.date: 10/31/2019
ms.reviewer: vanto
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: cf739bc33c505de3edac3869f4e5b095b651ed8a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477568"
---
# <a name="query-columns-using-always-encrypted-with-secure-enclaves-with-ssms"></a>通过 SSMS 查询使用具有安全 enclave 的 Always Encrypted 的列
[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

本文介绍如何使用 SQL Server Management Studio 发出对[具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves.md) 使用服务器端安全 enclave 的查询，包括：
- 触发就地加密操作的查询。
- 触发丰富计算的查询，包括对使用已启用 enclave 的密钥加密的列进行的范围比较或模式匹配操作。
- 在使用随机加密和已启用 enclave 的密钥加密的列上创建或更新索引的查询。  

若要使用 SSMS 对使用安全 enclave 的加密列运行查询，请按照[通过 SQL Server Management Studio 查询使用 Always Encrypted 的列](always-encrypted-query-columns-ssms.md)中的说明进行操作。 下面是一些应注意的特定于 enclave 的事项：

- 需要 SSMS 版本 18.3 或更高版本。
- 请确保从启用了 Always Encrypted 和 enclave 计算的连接使用安全 enclave，在查询窗口中运行查询。 有关详细说明，请参阅[教程：通过 SSMS 开始使用具有安全 enclave 的 Always Encrypted](../tutorial-getting-started-with-always-encrypted-enclaves.md) 和[为数据库连接启用和禁用 Always Encrypted](always-encrypted-query-columns-ssms.md#en-dis)。

## <a name="next-steps"></a>后续步骤
- [使用具有安全 enclave 的 Always Encrypted 开发应用程序](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>另请参阅  
- [教程：通过 SSMS 开始使用具有安全 enclave 的 Always Encrypted](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [使用 Transact-SQL 就地配置列加密](always-encrypted-enclaves-configure-encryption-tsql.md)
- [对使用具有安全 enclave 的 Always Encrypted 的列创建和使用索引](always-encrypted-enclaves-create-use-indexes.md)


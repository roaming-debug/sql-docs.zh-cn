---
description: 对使用具有安全 enclave 的 Always Encrypted 的列创建和使用索引
title: 对使用具有安全 enclave 的 Always Encrypted 的列创建和使用索引 | Microsoft Docs
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
ms.openlocfilehash: 4799cd725dce4eb8300717b8c89d601e9915f7d2
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534826"
---
# <a name="create-and-use-indexes-on-columns-using-always-encrypted-with-secure-enclaves"></a>对使用具有安全 enclave 的 Always Encrypted 的列创建和使用索引

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

本文介绍如何对使用启用 enclave 的列加密密钥和[具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves.md) 进行加密的列创建和使用索引。

具有安全 enclave 的 Always Encrypted 支持以下内容：
- 使用确定性加密和启用 enclave 的密钥加密的列上的聚集索引和非聚集索引。
  - 此类索引按已加密文本排序。 此类索引无特殊注意事项。 可以按照使用确定性加密和未启用 enclave 的密钥（与 Always Encrypted 相同）进行加密的列上的索引的相同方式来管理和使用此类索引。 
- 使用随机加密和启用 enclave 的密钥进行加密的列上的非聚集索引。
  - 索引数据结构（B 树）中的密钥值将根据其明文值进行加密和排序。 有关详细信息，请参阅[已启用 enclave 的列上的索引](always-encrypted-enclaves.md#indexes-on-enclave-enabled-columns)。

> [!NOTE]
> 本文的其余部分介绍了使用随机加密和启用 enclave 的密钥进行加密的列上的非聚集索引。

由于使用随机加密和启用 enclave 加密密钥的列上的索引包含加密（已加密文本）数据，而这些数据根据纯文本进行排序，因此 SQL Server 引擎必须将 enclave 用于任何涉及创建、搜索或更新索引的操作，包括：

- 创建或重新生成索引。
- 在包含索引列/加密列的表中插入、更新或删除行，这些操作会触发向索引插入索引键和/或从索引中删除索引键。
- 运行涉及检查索引完整性的 `DBCC` 命令，例如，[DBCC CHECKDB (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) 或 [DBCC CHECKTABLE (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)。
- 数据库恢复（例如，在 SQL Server 出现故障并重启后），前提是 SQL Server 需要撤消对索引的任何更改（如需了解更多详情，请参阅下文）。

以上所有操作都要求 enclave 必须包含索引列的列加密密钥。 需要密钥来解密索引密钥。 通常情况下，enclave 可以通过下面两种方式之一获取列加密密钥：
- 直接从客户端应用程序获取。
- 从列加密密钥缓存获取。

## <a name="invoke-indexing-operations-with-column-encryption-keys-provided-directly-by-the-client"></a>使用客户端直接提供的列加密密钥调用索引操作
若要使用这种方法来调用索引操作，发出查询来对索引触发操作的应用程序（包括 SQL Server Management Studio (SSMS) 等工具）必须：

- 连接到数据库，为数据库连接同时启用 Always Encrypted 和 enclave 计算。
- 应用程序必须有权访问保护索引列的列加密密钥的列主密钥。

在 SQL Server 引擎分析应用程序查询，并确定需要更新加密列上的索引来执行查询后，它便会指示客户端驱动程序通过安全通道向 enclave 发布必需的列加密密钥。 这与用于向 enclave 提供列加密密钥以处理所有其他不使用索引的查询的机制完全相同。 例如，使用模式匹配和范围比较的就地加密或查询。

这种方法可用于确保加密列上存在的索引对以下应用程序是透明的：已连接到已启用 Always Encrypted 和 enclave 计算的数据库。 应用程序连接可以使用 enclave 处理查询。 当你在列上创建索引后，应用程序内的驱动程序便会以透明方式向 enclave 提供列加密密钥，以调用索引操作。 创建索引可能会增加需要查询数，这些查询要求应用程序必需向 enclave 发送列加密密钥。

要使用此方法，请遵循[运行使用安全 enclave 的 Transact-SQL 语句](always-encrypted-enclaves-query-columns.md)中的通用指南来运行使用安全 enclave 的 Transact-SQL 语句。

有关如何使用这种方法的分步说明，请参阅[教程：在使用随机加密且已启用 enclave 的列上创建并使用索引。](../tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md)

## <a name="invoke-indexing-operations-using-cached-column-encryption-keys"></a>使用缓存列加密密钥调用索引操作

在客户端应用程序向 enclave 发送列加密密钥以处理需要 enclave 计算的任意查询后，enclave 会将列加密密钥缓存在内部缓存中。 该缓存位于 enclave 内，无法从外部访问。

如果（相同或其他用户使用的）相同或其他客户端应用程序对索引触发操作，但未直接提供必需的列加密密钥，enclave 便会在缓存中查找列加密密钥。 这样一来，可以成功对索引触发操作，尽管客户端应用程序尚未提供密钥。

若要使用这种方法来调用索引操作，应用程序必须连接到数据库，但无需为连接启用 Always Encrypted，并且 enclave 内的缓存中必须有必需的列加密密钥。

只有无需使用列加密密钥来执行其他与索引无关的操作的查询，才支持使用这种方法来调用操作。 例如，如果应用程序使用 `INSERT` 语句将行插入包含加密列的表中，应用程序必须连接到数据库，同时在连接字符串中启用 Always Encrypted，并且必须有权访问键，无论加密列是否有索引。

这种方法可用于：
 - 确保使用随机加密且已启用 enclave 的列上存在的索引对无权访问纯文本键和数据的应用程序和用户是透明的。 
 - 确保对加密列创建索引不会中断现有查询。 如果应用程序对包含加密列的表发出查询，而无需有权访问键，应用程序可以在 DBA 创建索引后继续运行，而无需有权访问键。 例如，假设一个应用程序对包含加密列的“Employees”表运行以下查询。 DBA 尚未对任何加密列创建索引。

   ```sql
   DELETE FROM [dbo].[Employees] WHERE [EmployeeID] = 1;
   GO
   ```

   如果应用程序通过未启用 Always Encrypted 和 enclave 计算的连接提交查询，查询会成功。 查询不会对加密列触发任何计算。 当 DBA 在任意加密列上创建索引后，查询便会触发从索引中删除索引键。 在此情况下，enclave 需要列加密密钥。 不过，只要数据所有者已向 enclave 提供列加密密钥，应用程序就能够继续通过相同连接来运行此查询。

 - 在管理索引时实现角色分隔，因为它启用 DBA 在加密列上创建并更改索引，而无需有权访问敏感数据。 

> [!TIP] 
> 通过使用 [ sp_enclave_send_keys (Transact-SQL)](../../system-stored-procedures/sp-enclave-send-keys-sql.md)，可以轻松地将用于索引的所有启用 enclave 的列加密密钥发送到 enclave，并填充密钥缓存。

有关如何使用这种方法的分步说明，请参阅[教程：在使用随机加密且已启用 enclave 的列上创建并使用索引。](../tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md) 

## <a name="next-steps"></a>后续步骤
- [使用安全 enclave 运行 Transact-SQL 语句](always-encrypted-enclaves-query-columns.md)

## <a name="see-also"></a>另请参阅  
- [教程：在使用随机加密且已启用 enclave 的列上创建并使用索引。](../tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md)
- [sp_enclave_send_keys (Transact-SQL)](../../system-stored-procedures/sp-enclave-send-keys-sql.md)

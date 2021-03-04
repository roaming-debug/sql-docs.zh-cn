---
title: 具有安全 Enclave 的 Always Encrypted
description: 了解 SQL Server 中具有安全 enclave 的 Always Encrypted 功能。
ms.custom: seo-lt-2019
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: cbec7eb15ed8acad746e3fcb2013ee8100bcdd26
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837690"
---
# <a name="always-encrypted-with-secure-enclaves"></a>具有安全 Enclave 的 Always Encrypted

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

通过启用就地加密和更丰富的机密查询，具有安全 enclave 的 Always Encrypted 扩展了 [Always Encrypted](always-encrypted-database-engine.md) 的机密计算功能。 具有安全 enclave 的 Always Encrypted 在 [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] 和 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]中可用（预览版）。

[!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]（2015 年）和 [!INCLUDE[sssql16](../../../includes/sssql16-md.md)] 中引入的 Always Encrypted 可保护敏感数据的机密性免受恶意软件以及具有高度特权但未经授权的用户的攻击：DBA、计算机管理员、云管理员，或可以合法访问服务器实例、硬件等但不应有权访问部分或全部实际数据的任何其他用户。  

如果没有本文中所讨论的增强功能，Always Encrypted 会通过在客户端加密数据并且从不允许数据或相应的加密密钥以纯文本形式显示在 [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] 引擎中来保护数据。 因此，数据库内的加密列上的功能受到严格限制。 [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] 可以对加密数据执行的唯一操作是相等比较（仅适用于[确定性加密](always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption)）。 数据库内不支持所有其他操作，包括加密操作（初始数据加密或密钥轮换）和更丰富的查询（例如模式匹配）。 用户需要将数据移出数据库才能对客户端执行这些操作。

使用具有安全 enclave 的 Always Encrypted，可以在服务器端对安全 enclave 内的纯文本数据进行某些计算，从而解决这些限制。 安全 enclave 是 [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] 过程中受保护的内存区域。 安全 enclave 对托管计算机上的 [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] 其余部分和其他进程显示为不透明盒。 无法从外部查看 enclave 内的任何数据或代码，即使采用调试程序也是如此。 这些属性使安全 enclave 成为受信任的执行环境，该执行环境可安全访问明文中的加密密钥和敏感数据，而不会破坏数据的保密性。

Always Encrypted 使用安全 enclave，如下图中所示：

![数据流](./media/always-encrypted-enclaves/ae-data-flow.png)

在分析应用程序提交的 Transact-SQL 语句时，[!INCLUDE[ssde-md](../../../includes/ssde-md.md)] 会确定语句是否包含对需要使用安全 enclave 的加密数据执行的任何操作。 对于此类语句：

- 客户端驱动程序向安全 enclave 发送操作所需的列加密密钥（通过安全通道），并提交要执行的语句。

- 处理该语句时，[!INCLUDE[ssde-md](../../../includes/ssde-md.md)] 将加密列上的加密操作或计算委托给安全 enclave。 如果需要，enclave 将对数据进行解密，并在纯文本上执行计算。

在语句处理期间，不会在 [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] 中以纯文本形式公开安全 enclave 之外的数据或列加密密钥。

## <a name="supported-enclave-technologies"></a>受支持的 enclave 技术

在 [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)][ 中，具有安全 enclave 的 Always Encrypted 使用](https://www.microsoft.com/security/blog/2018/06/05/virtualization-based-security-vbs-memory-enclaves-data-protection-through-isolation/)基于虚拟化的安全 (VBS) 保护 Windows 中的内存 enclave（也称为虚拟安全模式或 VSM enclave）。

在 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] 中，具有安全 enclave 的 Always Encrypted 使用 [Intel Software Guard Extensions (Intel SGX)](https://itpeernetwork.intel.com/microsoft-azure-confidential-computing/) enclave。 Intel SGX 是使用 [DC 系列](/azure/azure-sql/database/service-tiers-vcore?tabs=azure-portal#dc-series)硬件配置的数据库中支持的一种基于硬件的受信任执行环境技术。

## <a name="secure-enclave-attestation"></a>安全 enclave 证明

[!INCLUDE[ssde-md](../../../includes/ssde-md.md)] 中的安全 enclave 可以以纯文本形式访问敏感数据和列加密密钥。 因此，在将涉及 enclave 计算的语句提交到 [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] 之前，应用程序中的客户端驱动程序必须验证安全 enclave 是否为正版 VBS 或 SGX enclave，以及安全 enclave 内运行的代码是否为正版 Always Encrypted 库（该库实现了 Always Encrypted 加密算法），以进行机密查询中支持的就地加密和操作。

验证 enclave 的过程称为“enclave 证明”，同时涉及应用程序内的客户端驱动程序和用于联系外部证明服务的 [!INCLUDE[ssde-md](../../../includes/ssde-md.md)]。 证明过程的细节取决于 enclave 类型（VBS 或 SGX）和证明服务。

[!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] 中的 VBS 安全 enclave 的证明过程是 [Windows Defender System Guard 运行时证明](https://www.microsoft.com/security/blog/2018/06/05/virtualization-based-security-vbs-memory-enclaves-data-protection-through-isolation/)，该证明要求将主机保护者服务 (HGS) 用作证明服务。 

在 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] 中证明 Intel SGX enclave 需要 [Microsoft Azure 证明](/azure/attestation/overview)。

> [!NOTE]
> [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] 不支持 Microsoft Azure 证明。 主机保护者服务是 [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] 中 VBS enclave 唯一支持的证明解决方案。

## <a name="supported-client-drivers"></a>受支持的客户端驱动程序

若要使用具有安全 enclave 的 Always Encrypted，应用程序必须使用支持该功能的客户端驱动程序。 配置应用程序和客户端驱动程序，以启用 enclave 计算和 enclave 证明。 有关详细信息（包括受支持的客户端驱动程序的列表），请参阅[使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md)。

## <a name="terminology"></a>术语

### <a name="enclave-enabled-keys"></a>已启用 enclave 的密钥

具有安全 enclave 的 Always Encrypted 引入了已启用 enclave 的密钥的概念：

- 已启用 enclave 的列主密钥 - 具有数据库内的列主密钥元数据对象中指定的 `ENCLAVE_COMPUTATIONS` 属性的列主密钥。 列主密钥元数据对象还必须包含元数据属性的有效签名。 有关详细信息，请参阅 [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- **已启用 enclave 的列加密密钥** – 使用已启用 enclave 的列主密钥加密的列加密密钥。 只有已启用 enclave 的列加密密钥才能用于安全 enclave 中的计算。 

有关详细信息，请参阅[管理具有安全 Enclave 的 Always Encrypted 的密钥](always-encrypted-enclaves-manage-keys.md)。

### <a name="enclave-enabled-columns"></a>已启用 enclave 的列

已启用 enclave 的列是使用已启用 enclave 的列加密密钥加密的数据库列。

## <a name="confidential-computing-capabilities-for-enclave-enabled-columns"></a>已启用 enclave 的列的机密计算功能

具有安全 enclave 的 Always Encrypted 的两个主要优点是就地加密和丰富的机密查询。

### <a name="in-place-encryption"></a>就地加密

借助就地加密，可以对安全 enclave 中的数据库列进行加密操作，而无需将数据移出数据库。 就地加密可提高加密的性能和可靠性。 可以使用 [ALTER TABLE (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md) 语句执行就地加密。 

就地支持的加密操作包括：

- 使用已启用 enclave 的列加密密钥加密纯文本列。
- 将已加密的已启用 enclave 的列重新加密为：
  - 轮换列加密密钥 - 使用新的已启用 enclave 的列加密密钥重新加密列。
  - 更改已启用 enclave 的列的加密类型，例如从确定性改为随机。
- 解密已启用 enclave 的列中存储的数据（将该列转换为纯文本列）。

只要加密操作中涉及的列加密密钥已启用 enclave，就允许确定性加密和随机加密进行就地加密。

### <a name="confidential-queries"></a>机密查询

机密查询是 [DML 查询](../../../t-sql/queries/queries.md)，涉及在安全 enclave 内执行的已启用 enclave 的列上的操作。

安全 enclave 内支持的操作包括：

| Operation| [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] | [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] |
|:---|:---|:---|
| [比较运算符](../../../mdx/comparison-operators.md) | 支持 | 支持 |
| [BETWEEN (Transact-SQL)](../../../t-sql/language-elements/between-transact-sql.md) | 支持 | 支持 |
| [IN (Transact-SQL)](../../../t-sql/language-elements/in-transact-sql.md) | 支持 | 支持 |
| [LIKE (Transact-SQL)](../../../t-sql/language-elements/like-transact-sql.md) | 支持 | 支持 |
| [DISTINCT](../../../t-sql/queries/select-transact-sql.md#c-using-distinct-with-select) | 支持 | 支持 |
| [联接](../../performance/joins.md) | 仅支持嵌套循环联接 | 支持 |
| [SELECT - ORDER BY 子句 (Transact-SQL)](../../../t-sql/queries/select-order-by-clause-transact-sql.md) | 不支持 | 支持 |
| [SELECT - GROUP BY- Transact-SQL](../../../t-sql/queries/select-group-by-transact-sql.md) | 不支持 | 支持 |

> [!NOTE]
> 只有在使用随机加密（而非确定性加密）的已启用 enclave 的列上的安全 enclave 中，上述操作才受支持。 相等比较仍是使用确定性加密的列支持的唯一计算，它是通过比较 enclave 之外的已加密文本来执行的，不管列是否已启用 enclave。 确定性加密支持以下涉及相等比较的操作： 
> - 点查找、搜索和联接中的 [= (Equals)](../../../t-sql/language-elements/equals-transact-sql.md)
> - [IN](../../../t-sql/language-elements/in-transact-sql.md)
> - [SELECT - GROUP BY](../../../t-sql/queries/select-group-by-transact-sql.md)
> - [DISTINCT](../../../t-sql/queries/select-transact-sql.md#c-using-distinct-with-select)
>
> 在 [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] 中，对字符串列（`char``nchar`）使用 enclave 的机密查询要求列使用 binary2 排序顺序 (BIN2) 排序规则。 在 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] 中，对字符串的机密查询要求 BIN2 排序规则或 UTF-8 排序规则。 

### <a name="indexes-on-enclave-enabled-columns"></a>已启用 enclave 的列上的索引

可以在使用随机加密在已启用 enclave 的列上创建非聚集索引，以加快使用安全 enclave 的机密 DML 查询的运行速度。

为了确保使用随机加密进行加密的列上的索引不会泄露敏感数据，索引数据结构（B 树）中的键值会根据纯文本值进行加密和排序。 按纯文本值进行排序还有助于处理 enclave 中的查询。 当 [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] 中的查询执行器使用加密列上的索引在 enclave 内执行计算时，它搜索索引来查找列中存储的特定值。 每个搜索都可能涉及多个比较。 查询执行器将所有比较都委托给 enclave，它解密列中存储的值以及要比较的加密索引键值，以纯文本形式执行比较，并向执行器返回比较结果。

仍不支持在使用随机加密但未启用 enclave 的列上创建索引。

使用确定性加密的列上的索引是根据已加密文本（而不是纯文本）进行排序，无论列是否已启用 enclave。

有关详细信息，请参阅[对使用具有安全 enclave 的 Always Encrypted 的列创建和使用索引](always-encrypted-enclaves-create-use-indexes.md)。 有关 [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] 中的索引工作原理的一般信息，请参阅[描述的聚集索引和非聚集索引](../../indexes/clustered-and-nonclustered-indexes-described.md)一文。

### <a name="database-recovery"></a>数据库恢复

如果 SQL Server 实例出现故障，它的数据库可能处于以下状态：数据文件可能包含来自未完成事务的一些修改。 启动后，此实例运行[数据库恢复](../../logs/the-transaction-log-sql-server.md#recovery-of-all-incomplete-transactions-when--is-started)过程，其中涉及回滚在事务日志中找到的所有未完成事务，以确保维护数据库完整性。 如果未完成事务对索引进行了任何更改，也需要撤消这些更改。 例如，可能需要删除或重新插入索引中的一些键值。

> [!IMPORTANT]
> Microsoft 强烈建议，先为数据库启用[加速数据库恢复 (ADR)](../../backup-restore/restore-and-recovery-overview-sql-server.md#adr)，再在使用随机加密进行加密且已启用 enclave 的列上创建首个索引。 默认情况下，[!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] 中启用了 ADR，但 [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] 中未启用。

如果使用遵循 [ARIES](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf) 恢复模式的[传统数据库恢复过程](/azure/sql-database/sql-database-accelerated-database-recovery#the-current-database-recovery-process)，SQL Server 必须等到应用程序向 enclave 提供列的列加密密钥，才能撤消对索引的更改，而这可能需要很长时间才能完成。 加速的数据库恢复 (ADR) 大大减少了因无法从 enclave 内的缓存获取列加密密钥而必须延迟的撤消操作数。 因此，它通过最大限度地降低新事务受阻的可能性，极大地提高了数据库可用性。 启用 ADR 后，SQL Server 可能仍需要使用列加密密钥来完成旧数据版本清理，但是作为不影响数据库可用性或用户事务的后台任务来完成。 不过，错误日志中可能会显示错误消息，指明清理操作因缺少列加密密钥而失败。

## <a name="security-considerations"></a>安全注意事项

下面的安全注意事项适用于含安全 enclave 的 Always Encrypted。

- enclave 内数据的安全性取决于证明协议和证明服务。 因此，必须确保证明服务及其强制执行的证明策略是由受信任的管理员管理。 此外，证明服务通常还支持各种策略和证明协议，其中一些对 enclave 及其环境执行最小验证，旨在用于测试和开发目的。 请严格遵循证明服务专用指南，以确保对生产部署使用建议的配置和策略。 
- 如果结合使用随机加密和已启用 enclave 的列加密密钥来加密列，可能会导致列中存储的数据顺序泄露，因为此类列支持范围比较。 例如，如果包含员工薪金的加密列有索引，恶意 DBA 可以通过扫描此索引来查找最高加密薪金值，并确定薪金最高的员工（假设员工姓名未加密）。 
- 如果使用 Always Encrypted 来防止 DBA 未经授权地访问敏感数据，请不要将列主密钥或列加密密钥与 DBA 共享。 借助 enclave 内的列加密密钥缓存，DBA 无需拥有对密钥的直接访问权限，即可管理加密列上的索引。

## <a name="considerations-for-business-continuity-disaster-recovery-and-data-migration"></a><a name="anchorname-1-considerations-availability-groups-db-migration"></a> 业务连续性、灾难恢复和数据迁移的注意事项

使用具有安全 enclave 的 Always Encrypted 为数据库配置高可用性或灾难恢复解决方案时，请确保所有数据库副本均可使用安全 enclave。 如果 enclave 可用于主要副本，但不可用于次要副本，则在故障转移后，任何尝试使用具有安全 enclave 的 Always Encrypted 功能的语句都将失败。

使用具有安全 enclave 的 Always Encrypted 复制或迁移数据库时，请确保目标环境始终支持 enclave。 否则，使用 enclave 的语句将不适用于副本或迁移的数据库。

下面是应注意的一些具体事项：

- **SQL Server**
  - 配置 [AlwaysOn 可用性组](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)时，请确保在可用性组中托管数据库的每个 SQL Server 实例都支持具有安全 enclave 的 Always Encrypted，并且已配置 enclave 和证明。
  - 如果数据库使用具有安全 enclave 的 Always Encrypted 功能，同时你在未配置 enclave 的 SQL Server 实例上还原它的备份文件，还原操作会成功，且不依赖 enclave 的所有功能都可用。 不过，所有使用 enclave 功能的后续语句都会失败，使用随机加密且已启用 enclave 的列上的索引也会失效。 在未配置 enclave 的实例上附加使用含安全 enclave 的 Always Encrypted 的数据库时，亦是如此。
  - 如果数据库包含使用随机加密且已启用 enclave 的列上的索引，请确保先在数据库中启用[加速数据库恢复 (ADR)](../../backup-restore/restore-and-recovery-overview-sql-server.md#adr)，再创建数据库备份。 ADR 可确保数据库（包括索引）在你还原数据库后立即可用。 有关详细信息，请参阅[数据库恢复](#database-recovery)。
  
- **Azure SQL 数据库**
  - 在配置[活动异地复制](/azure/azure-sql/database/active-geo-replication-overview)时，如果主数据库支持安全 enclave，请确保辅助数据库也支持。

在 SQL Server 和 Azure SQL 数据库中，使用 bacpac 文件迁移数据库时，必须确保先删除使用随机加密且已启用 enclave 的列上的所有索引，再创建 bacpac 文件。

## <a name="known-limitations"></a>已知限制

具有安全 enclave 的 Always Encrypted 通过对索引支持就地加密和更丰富的机密查询来解决 Always Encrypted 的某些限制问题，如[已启用 enclave 的列的机密计算功能](#confidential-computing-capabilities-for-enclave-enabled-columns)中所述。

[功能详细信息](always-encrypted-database-engine.md#feature-details)中列出的所有其他 Always Encrypted 限制也适用于具有安全 enclave 的 Always Encrypted。

下面介绍了含安全 enclave 的 Always Encrypted 的专属限制：

- 无法在使用随机加密且已启用 enclave 的列上创建聚集索引。
- 使用随机加密且已启用 enclave 的列无法作为主键列，并且无法被外键约束或唯一键约束引用。
- 在 [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] 中（此限制不适用于 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]），仅在使用随机加密且已启用 enclave 的列上才支持嵌套循环联接（在可用时使用索引）。 有关不同产品间的其他差异的信息，请参阅[机密查询](#confidential-queries)。
- 就地加密操作无法与列元数据的其他任何更改合并，更改同一代码页内的排序规则以及更改为 Null 性除外。 例如，不能在单个 `ALTER TABLE`/`ALTER COLUMN` Transact-SQL 语句中加密、重新加密或解密列并更改列的数据类型。 请单独使用两个语句。
- 不支持将已启用 enclave 的密钥用于内存中表内的列。
- 定义计算列的表达式无法对使用随机加密且已启用 enclave 的列执行任何计算（即使这些计算属于[机密查询](#confidential-queries)中列出的受支持操作）。
- 在使用随机加密且已启用 enclave 的列上，LIKE 运算符的参数中不支持转义字符。
- 如果查询中的 LIKE 运算符或比较运算符包含使用以下数据类型（加密后成为大型对象）之一的查询参数，查询会忽略索引，并执行表扫描。
  - `nchar[n]` 和 `nvarchar[n]`（如果 n 大于 3967）。
  - `char[n]`、`varchar[n]`、`binary[n]`、`varbinary[n]`（如果 n 大于 7935）。
- 工具限制：
  - 用于存储已启用 enclave 的列主密钥的唯一受支持密钥存储是 Windows 证书存储和 Azure 密钥保管库。
  - 不支持导入/导出包含已启用 enclave 的密钥的数据库。
  - 若要通过 `ALTER TABLE`/`ALTER COLUMN` 语句触发就地加密操作，需要使用 SSMS 中的查询窗口发出该语句，或者可以自行编写可发出该语句的程序。 当前，SqlServer PowerShell 模块中的 Set-SqlColumnEncryption cmdlet 和 SQL Server Management Studio 中的 Always Encrypted 向导不支持就地加密 - 它们会将数据移出数据库以执行加密操作，即使用于这些操作的列加密密钥已启用 enclave 也是如此。

## <a name="next-steps"></a>后续步骤

- [教程：在 SQL Server 中开始使用具有安全 enclave 的 Always Encrypted](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [教程：在 Azure SQL 数据库中开始使用具有安全 enclave 的 Always Encrypted](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
- [配置和使用具有安全 enclave 的 Always Encrypted](configure-always-encrypted-enclaves.md)

## <a name="see-also"></a>另请参阅

- [管理具有安全 enclave 的 Always Encrypted 的密钥](always-encrypted-enclaves-manage-keys.md)
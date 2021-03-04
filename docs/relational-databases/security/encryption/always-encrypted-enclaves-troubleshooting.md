---
title: 排查具有安全 enclave 的 Always Encrypted 的常见问题
description: 排查具有安全 enclave 的 Always Encrypted 的常见问题
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: vanto
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: how-to
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: dc50006d17228bf3b5ec03c2a672ed113260bf3b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839254"
---
# <a name="troubleshoot-common-issues-for-always-encrypted-with-secure-enclaves"></a>排查具有安全 enclave 的 Always Encrypted 的常见问题

本文介绍如何确定并解决在使用[具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves.md) 运行 Transact-SQL (TSQL) 语句时可能会遇到的常见问题。

若要详细了解如何使用安全 enclave 运行查询，请参阅[使用安全 enclave 运行 Transact-SQL 语句](always-encrypted-enclaves-query-columns.md)。

## <a name="database-connection-errors"></a>数据库连接错误

若要使用安全 enclave 运行语句，你需要启用 Always Encrypted 并为数据库连接指定证明 URL，如[运行使用安全 enclave 的语句的先决条件](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves)所述。 但是，如果指定证明 URL，但 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] 或目标 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 实例中的数据库不支持安全 enclave 或配置错误，则连接将失败。

- 如果使用的是 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]，请检查数据库是否使用 [DC 系列](/azure/azure-sql/database/service-tiers-vcore?tabs=azure-portal#dc-series)硬件配置。 有关详细信息，请参阅[为 Azure SQL 数据库启用 Intel SGX](/azure/azure-sql/database/always-encrypted-enclaves-enable-sgx)。
- 如果使用的是 [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)]，请检查是否为实例正确配置了安全 enclave。 有关详细信息，请参阅[在 SQL Server 中配置安全 enclave](always-encrypted-enclaves-configure-enclave-type.md)。

## <a name="attestation-errors-when-using-microsoft-azure-attestation"></a>使用 Microsoft Azure 证明时发生证明错误

> [!NOTE]
> 本部分仅适用于 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]。

在客户端驱动程序向 Azure SQL 逻辑服务器提交 T-SQL 语句以供执行之前，驱动程序会使用 Microsoft Azure 证明触发以下 enclave 证明工作流。

1. 客户端驱动程序将数据库连接中指定的证明 URL 传递给 Azure SQL 逻辑服务器。
2. Azure SQL 逻辑服务器收集有关 enclave 及其承载环境以及 enclave 内运行的代码的证据。 然后，服务器将证明请求发送给证明 URL 中引用的证明提供程序。
3. 证明提供程序根据配置的策略验证证据，并为 Azure SQL 逻辑服务器颁发证明令牌。 证明提供程序使用其私钥对证明令牌进行签名。
4. Azure SQL 逻辑服务器将证明令牌发送到客户端驱动程序。
5. 客户端通过指定的证明 URL 与证明提供程序联系以检索其公钥，并验证证明令牌中的签名。

由于配置错误，上述工作流的各个步骤均可能发生错误。 常见的证明错误、相应的根本原因以及建议的故障排除步骤如下：

- Azure SQL 逻辑服务器无法连接到证明 URL 中指定的 Azure 证明（上述工作流的步骤 2）中的证明提供程序。 可能的原因包括：
  - 证明 URL 不正确或不完整。 有关详细信息，请参阅[确定证明策略的证明 URL](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation#determine-the-attestation-url-for-your-attestation-policy)。
  - 已意外删除证明提供程序。
  - 为证明提供程序配置的防火墙禁止访问 Microsoft 服务。
  - 间歇性网络错误导致证明提供程序不可用。
- Azure SQL 逻辑服务器无权向证明提供程序发送证明请求。 确保证明提供程序的管理员已将数据库服务器添加到了证明读取者角色。 有关详细信息，请参阅[授予 Azure SQL 数据库服务器对证明提供程序的访问权限](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation#grant-your-azure-sql-database-server-access-to-your-attestation-provider)。
- 证明策略的验证失败（上述工作流的步骤 3）。
  - 证明策略错误可能是根本原因。 确保使用的是 Microsoft 推荐的策略。 有关详细信息，请参阅[创建并配置证明提供程序](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation#create-and-configure-an-attestation-provider)。
  - 由于安全漏洞损害服务器端 enclave，因此策略验证也可能失败。
- 客户端应用程序无法连接到证明提供程序且无法检索公共签名密钥（步骤 5 中）。 可能的原因包括：
  - 应用程序与证明提供程序之间的防火墙配置可能会阻止连接。 若要对受阻的连接进行故障排除，请验证是否可以连接到证明提供程序的 OpenId 终结点。 例如，在托管应用程序的计算机上通过 Web 浏览器查看是否可以连接到 OpenID 终结点。 有关详细信息，请参阅[元数据配置 - Get](/rest/api/attestation/metadataconfiguration/get)。

## <a name="attestation-errors-when-using-host-guardian-service"></a>使用主机监护服务时发生证明错误

> [!NOTE]
> 本部分仅适用于 [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)]。

在客户端驱动程序向 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 提交 T-SQL 语句以供执行之前，驱动程序会使用主机监护服务 (HGS) 触发以下 enclave 证明工作流。

1. 客户端驱动程序调用 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 来启动证明。
2. [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 收集有关其 enclave、其承载环境以及 enclave 内运行的代码的证据。 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 从 HGS 实例请求运行状况证书，已向托管 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 的计算机注册。 有关详细信息，请参阅[向主机保护者服务注册计算机](always-encrypted-enclaves-host-guardian-service-register.md)。
3. HGS 验证证据，并向 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 颁发运行状况证书。 HGS 使用其私钥对运行状况证书进行签名。
4. [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 将运行状况证书发送给客户端驱动程序。
5. 客户端驱动程序通过为数据库连接指定的证明 URL 与 HGS 联系，以检索 HGS 公共密钥。 客户端驱动程序验证运行状况证书中的签名。

由于配置错误，上述工作流的各个步骤均可能发生错误。 部分常见的证明错误、相应的根本原因以及建议的故障排除步骤如下：

- 由于间歇性网络错误，[!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 无法连接到 HGS（上述工作流的步骤 2）。 若要对连接问题进行故障排除，[!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机的管理员应验证计算机是否可以连接到 HGS 计算机。
- 步骤 3 中的验证失败。 若要排查验证问题：
  - [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员应与客户端应用程序管理员合作，验证是否已在客户端的证明 URL 中引用的 HGS 实例中注册 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机。
  - [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员应确认 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机是否可以成功证明，具体可按照[步骤 5：确认主机是否可以成功证明](always-encrypted-enclaves-host-guardian-service-register.md#step-5-confirm-the-host-can-attest-successfully)中的说明操作。
- 客户端应用程序无法连接到 HGS 且无法检索其公共签名密钥（步骤 5 中）。 原因可能是：
  - 应用程序与证明提供程序之间的某一防火墙配置可能会阻止连接。 验证托管应用的计算机是否可以连接到 HGS 计算机。

## <a name="in-place-encryption-errors"></a>就地加密错误

此部分列出了使用 `ALTER TABLE`/`ALTER COLUMN` 进行就地加密时可能会遇到的常见错误（以及前面各部分中介绍的证明错误）。 有关详细信息，请参阅[使用具有安全 enclave 的 Always Encrypted 就地配置列加密](always-encrypted-enclaves-configure-encryption.md)。

- 尝试用于对数据进行加密、解密或重新加密的列加密密钥没有启用 enclave。 有关就地加密先决条件的详细信息，请参阅[先决条件](always-encrypted-enclaves-configure-encryption.md#prerequisites)。 若要详细了解如何预配已启用 enclave 的密钥，请参阅[预配已启用 enclave 的密钥](always-encrypted-enclaves-provision-keys.md)。
- 你尚未为数据库连接启用 Always Encrypted 和 enclave 计算。 请参阅[运行使用安全 enclave 的语句的先决条件](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves)。
- `ALTER TABLE`/`ALTER COLUMN` 语句会触发加密操作，并且会更改列数据类型或通过代码页（非当前排序规则代码页）设置排序规则。 禁止将加密操作与数据类型或排序规则页面更改结合起来。 若要解决该问题，请使用独立语句；一个语句用于更改数据类型或排序规则代码页，另一个语句用于实现就地加密。

## <a name="errors-when-running-confidential-dml-queries-using-secure-enclaves"></a>运行使用安全 enclave 的机密 DML 查询时发生错误

此部分列出了运行使用安全 enclave 的机密 DML 查询时可能会遇到的常见错误（以及前面各部分中介绍的证明错误）。

- 为要查询的列配置的列加密密钥没有启用 enclave。
- 你尚未为数据库连接启用 Always Encrypted 和 enclave 计算。 有关详细信息，请参阅[运行使用安全 enclave 的语句的先决条件](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves)。
- 要查询的列使用确定性加密。 确定性加密不支持使用安全 enclave 的机密 DML 查询。 若要详细了解如何将加密类型更改为随机类型，请参阅[为现有加密列启用具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves-enable-for-encrypted-columns.md)。
- 要查询的字符串列所使用的排序规则不是 BIN2 或 UTF-8 排序规则。 将排序规则更改为 BIN2 或 UTF-8。 有关详细信息，请参阅[使用安全 enclave 的 DML 语句](always-encrypted-enclaves-query-columns.md#dml-statements-using-secure-enclaves)。
- 查询触发了不受支持的操作。 有关 enclave 内支持的操作的列表，请参阅[使用安全 enclave 的 DML 语句](always-encrypted-enclaves-query-columns.md#dml-statements-using-secure-enclaves)。
## <a name="next-steps"></a>后续步骤

- [使用具有安全 enclave 的 Always Encrypted 开发应用程序](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>另请参阅

- [使用安全 enclave 运行 Transact-SQL 语句](always-encrypted-enclaves-query-columns.md)。
- [使用具有安全 Enclave 的 Always Encrypted 就地配置列加密](always-encrypted-enclaves-configure-encryption.md)
- [对使用具有安全 enclave 的 Always Encrypted 的列创建和使用索引](always-encrypted-enclaves-create-use-indexes.md)
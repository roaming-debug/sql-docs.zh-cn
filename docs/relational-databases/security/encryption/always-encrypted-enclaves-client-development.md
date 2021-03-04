---
description: 使用具有安全 enclave 的 Always Encrypted 开发应用程序
title: 使用具有安全 enclave 的 Always Encrypted 开发应用程序 | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
dev_langs:
- CSharp
ms.assetid: 9595eb66-284c-4474-828f-8961a05ce989
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 14bfbdd1a9c6bd176c89513674c5680388ef1452
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838881"
---
# <a name="develop-applications-using-always-encrypted-with-secure-enclaves"></a>使用具有安全 enclave 的 Always Encrypted 开发应用程序
[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves.md) 扩展了 [Always Encrypted](always-encrypted-database-engine.md)，以便对已加密敏感数据库列实现更丰富的应用程序查询功能。 它利用安全 enclave 技术允许 [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] 中的查询执行器将对加密列进行的计算委托给 [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] 进程中的安全 enclave。

## <a name="prerequisites"></a>先决条件

- [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 实例或 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] 中的数据库和服务器必须正确配置为支持 enclave 和证明。 有关详细信息，请参阅[设置安全 enclave 和证明](configure-always-encrypted-enclaves.md#set-up-the-secure-enclave-and-attestation)。
- 你需要从证明服务管理员处获取环境的证明 URL。

  - 如果使用的是 [!INCLUDE[ssnoversion-md](../../../includes/ssnoversion-md.md)] 和主机保护者服务 (HGS)，请参阅[确定并共享 HGS 证明 URL](../../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url)。
  - 如果使用的是 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] 和 Microsoft Azure 证明，请参阅[确定证明策略的证明 URL](./always-encrypted-enclaves.md?view=sql-server-ver15#secure-enclave-attestation)。

- 应用程序必须使用支持安全 enclave 的 SQL 客户端驱动程序版本。 有关详细信息，请参阅以下部分。

- 你需要为数据库连接配置证明协议和证明 URL。 如何配置证明协议和证明 URL 取决于所用的客户端驱动程序。

## <a name="client-drivers-for-always-encrypted-with-secure-enclaves"></a>具有安全 enclave 的 Always Encrypted 的客户端驱动程序

若要使用具有安全 enclave 的 Always Encrypted 开发应用程序，需要支持安全 enclave 的 SQL 客户端驱动程序版本。 客户端驱动程序会发挥以下关键作用：

- 在将使用安全 enclave 的查询提交给 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 进行执行之前，驱动程序会启动 enclave 证明，以验证安全 enclave 是否可信并且可安全地用于处理敏感数据。 有关证明的详细信息，请参阅[安全 Enclave 证明](always-encrypted-enclaves.md#secure-enclave-attestation)。
- 证明成功后，客户端驱动程序便会通过协商共享机密与 enclave 建立安全会话。
- 驱动程序会使用共享机密加密 enclave 需要用于处理查询的列加密密钥，并将密钥发送给 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]，后者会将它们转发给解密密钥的安全 enclave。 
- 最后，驱动程序会提交查询以便执行，这会在安全 enclave 中触发计算。

## <a name="next-steps"></a>后续步骤

以下客户端驱动程序支持具有安全 enclave 的 Always Encrypted：

- .NET Framework 4.6 或更高版本中以及 .NET Core 2.1 或更高版本中用于 SQL Server 的 Microsoft .NET 数据提供程序。 
    - 有关详细信息，请参阅[对用于 SQL Server 的 Microsoft .NET 数据提供程序使用 Always Encrypted](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md)。
    - 有关分步教程，请参阅[教程：使用具有安全 enclave 的 Always Encrypted 开发 .NET 应用程序](../../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)。
    - 此外，请参阅[展示如何使用 Azure Key Vault 提供程序和具有安全 Enclave 的 Always Encrypted 的示例](../../../connect/ado-net/sql/azure-key-vault-enclave-example.md)。
- Microsoft ODBC Driver for SQL Server 版本 17.4 或更高版本。 
    - 有关详细信息，请参阅[在 ODBC 驱动程序中使用 Always Encrypted](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md)。 
    - 有关如何使用 ODBC 为数据库连接启用 enclave 计算的信息，请参阅[启用具有安全 Enclave 的 Always Encrypted](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-with-secure-enclaves)部分。
- Microsoft JDBC Driver for SQL Server 版本 8.2 或更高版本。
    - 有关详细信息，请参阅[对 JDBC 驱动程序使用具有安全 enclave 的 Always Encrypted](../../../connect/jdbc/using-always-encrypted-with-secure-enclaves-with-the-jdbc-driver.md)
- .NET Framework 4.7.2 或更高版本中的用于 SQL Server 的 .NET Framework 数据提供程序。 
    - 有关详细信息，请参阅[对用于 SQL Server 的 .NET Framework 数据提供程序使用 Always Encrypted](../../../relational-databases/security/encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)。
    - 有关分步教程，请参阅[教程：开发使用具有安全 enclave 的 Always Encrypted 的 .NET Framework 应用程序](../tutorial-always-encrypted-enclaves-develop-net-framework-apps.md)

## <a name="see-also"></a>另请参阅

- [排查具有安全 enclave 的 Always Encrypted 的常见问题](always-encrypted-enclaves-troubleshooting.md)
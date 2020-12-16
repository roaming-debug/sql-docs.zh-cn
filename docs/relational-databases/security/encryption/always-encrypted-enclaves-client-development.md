---
description: 使用具有安全 enclave 的 Always Encrypted 开发应用程序
title: 使用具有安全 enclave 的 Always Encrypted 开发应用程序 | Microsoft Docs
ms.custom: ''
ms.date: 10/15/2019
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
ms.openlocfilehash: 0a358095cbf9c87aa6f767bf9f4dd52f8dfe6b9e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477698"
---
# <a name="develop-applications-using-always-encrypted-with-secure-enclaves"></a>使用具有安全 enclave 的 Always Encrypted 开发应用程序
[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves.md) 扩展了 [Always Encrypted](always-encrypted-database-engine.md)，以便对已加密敏感数据库列实现更丰富的应用程序查询功能。 它利用安全 enclave 技术允许 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 中的查询执行器将对加密列进行的计算委托给 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 进程中的安全 enclave。

## <a name="client-driver-for-always-encrypted-with-secure-enclaves"></a>具有安全 enclave 的 Always Encrypted 的客户端驱动程序

若要使用具有安全 enclave 的 Always Encrypted 开发应用程序，需要支持安全 enclave 的 SQL 客户端驱动程序版本。 客户端驱动程序会发挥以下关键作用：
- 在将使用安全 enclave 的查询提交给 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 进行执行之前，驱动程序会启动 enclave 证明，以验证安全 enclave 是否可信并且可安全地用于处理敏感数据。 有关证明的详细信息，请参阅[安全 Enclave 证明](always-encrypted-enclaves.md#secure-enclave-attestation)。
- 证明成功后，客户端驱动程序便会通过协商共享机密与 enclave 建立安全会话。
- 驱动程序会使用共享机密加密 enclave 需要用于处理查询的列加密密钥，并将密钥发送给 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]，后者会将它们转发给解密密钥的安全 enclave。 
- 最后，驱动程序会提交查询以便执行，这会在安全 enclave 中触发计算。

若要使用安全 enclave 的功能，需要将应用程序和客户端驱动程序配置为在连接到数据库时启用 enclave 计算，并指定指向 enclave 的证明服务的证明服务终结点（enclave 证明 URL）。 详细信息取决于所使用的驱动程序和证明服务/协议。

## <a name="next-steps"></a>后续步骤

以下客户端驱动程序支持具有安全 enclave 的 Always Encrypted：
- .NET Framework 4.7.2 或更高版本中的用于 SQL Server 的 .NET Framework 数据提供程序。 
    - 有关详细信息，请参阅[对用于 SQL Server 的 .NET Framework 数据提供程序使用 Always Encrypted](../../../relational-databases/security/encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)。
    - 有关分步教程，请参阅[教程：开发使用具有安全 enclave 的 Always Encrypted 的 .NET Framework 应用程序](../tutorial-always-encrypted-enclaves-develop-net-framework-apps.md)
- .NET Framework 4.6 或更高版本中以及 .NET Core 2.1 或更高版本中用于 SQL Server 的 Microsoft .NET 数据提供程序。 
    - 有关详细信息，请参阅[对用于 SQL Server 的 Microsoft .NET 数据提供程序使用 Always Encrypted](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md)。
    - 有关分步教程，请参阅[教程：使用具有安全 enclave 的 Always Encrypted 开发 .NET 应用程序](../../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)
- Microsoft ODBC Driver for SQL Server 版本 17.4 或更高版本。 
    - 有关详细信息，请参阅[在 ODBC 驱动程序中使用 Always Encrypted](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md)。 
    - 有关如何使用 ODBC 为数据库连接启用 enclave 计算的信息，请参阅[启用具有安全 Enclave 的 Always Encrypted](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-with-secure-enclaves)部分。

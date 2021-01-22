---
title: 配置和使用具有安全 enclave 的 Always Encrypted | Microsoft Docs
description: 了解如何在 SQL Server 和 Azure SQL 数据库中配置和使用具有安全 enclave 的 Always Encrypted，这实现敏感数据上更丰富的功能。
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
ms.openlocfilehash: 481493a50fdefc22f6eb4d77feb13cfc4848388d
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534646"
---
# <a name="configure-and-use-always-encrypted-with-secure-enclaves"></a>配置和使用具有安全 enclave 的 Always Encrypted 

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves.md) 扩展现有 [Always Encrypted](always-encrypted-database-engine.md) 功能，以便对敏感数据启动更丰富的功能，同时保持数据的机密性。 本文列出了用于配置和使用该功能的常见任务。

有关演示如何快速开始使用具有安全 enclave 的 Always Encrypted 的教程，请参阅：

- [教程：在 SQL Server 中开始使用具有安全 enclave 的 Always Encrypted](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [教程：在 Azure SQL 数据库中开始使用具有安全 enclave 的 Always Encrypted](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)

## <a name="set-up-the-secure-enclave-and-attestation"></a>设置安全 enclave 和证明

需要配置环境以确保该安全 enclave 可用于数据库，然后才能使用具有安全 enclave 的 Always Encrypted。 还需要设置 [enclave 证明](always-encrypted-enclaves.md#secure-enclave-attestation)。 

设置环境的过程取决于你使用的是 [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] 还是 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]。

### <a name="set-up-the-secure-enclave-and-attestation-in-ssnoversion-md"></a>在 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 中设置安全 enclave 和证明

有关详细信息，请参阅以下文章：
- [规划主机保护者服务证明](./always-encrypted-enclaves-host-guardian-service-plan.md)
- [为 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]](./always-encrypted-enclaves-host-guardian-service-deploy.md) 部署主机保护者服务
- [使用主机保护者服务注册计算机](./always-encrypted-enclaves-host-guardian-service-register.md)
- [在 SQL Server 中配置安全 enclave](always-encrypted-enclaves-configure-enclave-type.md)

### <a name="set-up-the-secure-enclave-and-attestation-in-sssdsfull"></a>在 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] 中设置安全 enclave 和证明

有关详细信息，请参阅以下文章：
- [在 [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] 中规划 Intel SGX enclave 和证明](/azure/azure-sql/database/always-encrypted-enclaves-sqldbmi-plan)
- [为 Azure SQL 数据库启用 Intel SGX](/azure/azure-sql/database/always-encrypted-enclaves-sqldbmi-enable-sgx)
- [为 Azure SQL 数据库逻辑服务器配置 Azure 证明](/azure/azure-sql/database/always-encrypted-enclaves-sqldbmi-configure-attestation)

## <a name="manage-keys-for-always-encrypted-with-secure-enclaves"></a>管理具有安全 enclave 的 Always Encrypted 的密钥
有关详细信息，请参阅以下文章：
- [管理具有安全 enclave 的 Always Encrypted 的密钥 - 概述](always-encrypted-enclaves-manage-keys.md)
- [预配已启用 enclave 的密钥](always-encrypted-enclaves-provision-keys.md)
- [轮换已启用 enclave 的密钥](always-encrypted-enclaves-rotate-keys.md)

## <a name="configure-columns-with-always-encrypted-with-secure-enclaves"></a>使用具有安全 enclave 的 Always Encrypted 配置列
有关详细信息，请参阅以下文章：
- [使用具有安全 Enclave 的 Always Encrypted 就地配置列加密 - 概述](always-encrypted-enclaves-configure-encryption.md)
- [使用 Transact-SQL 就地配置列加密](always-encrypted-enclaves-configure-encryption-tsql.md)
- [为现有加密列启用具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves-enable-for-encrypted-columns.md)

## <a name="run-transact-sql-statements-using-secure-enclaves"></a>使用安全 enclave 运行 Transact-SQL 语句
有关详细信息，请参阅以下文章：
- [使用安全 enclave 运行 Transact-SQL 语句](always-encrypted-enclaves-query-columns.md)
- [排查具有安全 enclave 的 Always Encrypted 的常见问题](always-encrypted-enclaves-troubleshooting.md)

## <a name="create-and-use-indexes-on-enclave-enabled-columns"></a>在已启用 enclave 的列上创建和使用索引
有关详细信息，请参阅以下文章：
- [对使用具有安全 enclave 的 Always Encrypted 的列创建和使用索引](always-encrypted-enclaves-create-use-indexes.md)
  
## <a name="develop-applications-using-always-encrypted-with-secure-enclaves"></a>使用具有安全 enclave 的 Always Encrypted 开发应用程序
有关详细信息，请参阅以下文章：
- [使用具有安全 enclave 的 Always Encrypted 开发应用程序](always-encrypted-enclaves-client-development.md)

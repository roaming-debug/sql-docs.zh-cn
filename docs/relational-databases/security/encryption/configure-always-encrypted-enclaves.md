---
title: 配置和使用具有安全 enclave 的 Always Encrypted | Microsoft Docs
description: 了解如何在 SQL Server 中配置和使用具有安全 enclave 的 Always Encrypted，这实现敏感数据上更丰富的功能。
ms.custom: ''
ms.date: 10/18/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 53026845a3cb41a53bec6c0d817b04b6d485f1be
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468518"
---
# <a name="configure-and-use-always-encrypted-with-secure-enclaves"></a>配置和使用具有安全 enclave 的 Always Encrypted 

[!INCLUDE[SQL Server 2019](../../../includes/applies-to-version/sqlserver2019.md)]

[具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves.md) 扩展现有 [Always Encrypted](always-encrypted-database-engine.md) 功能，以便对敏感数据启动更丰富的功能，同时保持数据的机密性。 本文列出了用于配置和使用该功能的常见任务。

有关演示如何快速开始使用具有安全 enclave 的 Always Encrypted 的教程，请参阅[教程：通过 SSMS 开始使用具有安全 enclave 的 Always Encrypted](../tutorial-getting-started-with-always-encrypted-enclaves.md)。

## <a name="set-up-your-environment-to-support-enclaves-and-attestation"></a>设置环境，使其支持 enclave 和证明
有关详细信息，请参阅以下文章：
- [规划主机保护者服务证明](./always-encrypted-enclaves-host-guardian-service-plan.md)
- [为 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]](./always-encrypted-enclaves-host-guardian-service-deploy.md) 部署主机保护者服务
- [使用主机保护者服务注册计算机](./always-encrypted-enclaves-host-guardian-service-register.md)

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

> [!NOTE]
> 有关如何在 SSMS 中设置测试环境并尝试使用具有安全 enclave 的 Always Encrypted 功能的分步教程，请参阅[教程：通过 SSMS 开始使用具有安全 enclave 的 Always Encrypted](../tutorial-getting-started-with-always-encrypted-enclaves.md)。

## <a name="query-columns-using-always-encrypted-with-secure-enclaves"></a>查询使用具有安全 enclave 的 Always Encrypted 的列
有关详细信息，请参阅以下文章：
- [查询使用具有安全 enclave 的 Always Encrypted 的列 - 概述](always-encrypted-enclaves-query-columns.md)
- [通过 SSMS 查询使用具有安全 enclave 的 Always Encrypted 的列](always-encrypted-enclaves-query-columns-ssms.md)

## <a name="create-and-use-indexes-on-enclave-enabled-columns"></a>在已启用 enclave 的列上创建和使用索引
有关详细信息，请参阅以下文章：
- [对使用具有安全 enclave 的 Always Encrypted 的列创建和使用索引](always-encrypted-enclaves-create-use-indexes.md)

## <a name="develop-applications-using-always-encrypted-with-secure-enclaves"></a>使用具有安全 enclave 的 Always Encrypted 开发应用程序
有关详细信息，请参阅以下文章：
- [使用具有安全 enclave 的 Always Encrypted 开发应用程序](always-encrypted-enclaves-client-development.md)

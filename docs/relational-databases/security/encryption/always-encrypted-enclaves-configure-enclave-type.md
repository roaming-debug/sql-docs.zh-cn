---
title: 在 SQL Server 中配置安全 enclave
description: 在 SQL Server 中为具有安全 enclave 的 Always Encrypted 配置安全 enclave。
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: b2bd51dffb858f30e29b4a1b60e1c728afdbeb93
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067242"
---
# <a name="configure-the-secure-enclave-in-sql-server"></a>在 SQL Server 中配置安全 enclave

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

需要配置实例以在启动期间初始化安全 enclave，然后才能在 SQL Server 中使用[具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves.md)。 默认情况下，SQL Server 不会初始化安全 enclave。 可以通过将“列加密 enclave 类型”服务器配置选项设置为表示环境有效 enclave 类型的值来更改此值。

> [!NOTE]
> 负责配置安全 enclave 的角色是 DBA。 请参阅[使用 HGS 配置证明时的角色和职责](always-encrypted-enclaves-host-guardian-service-plan.md#roles-and-responsibilities-when-configuring-attestation-with-hgs)。

[!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] 支持的 enclave 类型是基于虚拟化的安全性 (VBS)。 在配置 VBS enclave 类型之前，建议使用主机保护者服务 (HGS) 为托管实例的计算机设置证明。 若要开始使用 HGS，请参阅[规划主机保护者服务证明](always-encrypted-enclaves-host-guardian-service-plan.md)。 设置证明还会启用基于虚拟化的安全性，这是 VBS enclave 正确初始化所必需的。 有关详细信息，请参阅[验证基于虚拟化的安全性组件是否正在运行](always-encrypted-enclaves-host-guardian-service-register.md#step-2-verify-virtualization-based-security-is-running)。

有关如何配置 enclave 类型的详细说明，请参阅[为 Always Encrypted 服务器配置选项配置 enclave 类型](../../../database-engine/configure-windows/configure-column-encryption-enclave-type.md)。

## <a name="next-steps"></a>后续步骤

 [管理具有安全 enclave 的 Always Encrypted 的密钥](always-encrypted-enclaves-manage-keys.md)

## <a name="see-also"></a>另请参阅  
 
 [服务器配置选项 (SQL Server)](../../../database-engine/configure-windows/server-configuration-options-sql-server.md)

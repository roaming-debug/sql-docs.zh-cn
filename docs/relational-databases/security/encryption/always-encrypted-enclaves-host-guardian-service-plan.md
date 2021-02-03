---
title: 规划主机保护者服务证明
description: 使用具有安全 enclave 的 Always Encrypted 为 SQL Server 规划主机保护者服务证明。
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: rpsqrd
ms.author: ryanpu
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d156b2c02d67c947041de268962579d6417202e2
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235746"
---
# <a name="plan-for-host-guardian-service-attestation"></a>规划主机保护者服务证明

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

使用[具有安全 enclave 的 Always Encrypted](always-encrypted-enclaves.md) 时，需要确保客户端应用程序在 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 进程中与可信 enclave 通信。 对于基于虚拟化的安全性 (VBS) enclave，此要求包括验证 enclave 内的代码是否有效，以及承载 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 的计算机是否可信。 远程证明通过引入可以验证 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机的标识（以及可以选择验证配置）的第三方来实现此目标。 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 必须先向证明服务提供有关其操作环境的信息以获取健康证书，然后才能使用 enclave 运行查询。 此健康证书随后会发送到客户端，后者可以通过证明服务独立验证其真实性。 客户端信任健康证书后，便知道它在与可信 VBS enclave 通信，并会发出将使用该 enclave 的查询。

Windows Server 2019 中的主机保护者服务 (HGS) 角色为具有 VBS enclave 的 Always Encrypted 提供远程证明功能。
本文将指导你完成使用具有 VBS enclave 和 HGS 证明的 Always Encrypted 预先部署决策和要求。

## <a name="architecture-overview"></a>体系结构概述

主机保护者服务 (HGS) 是在 Windows Server 2019 上运行的群集 Web 服务。
在典型部署中，会有 1-3 台 HGS 服务器、至少一个运行 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 的计算机以及一个运行客户端应用程序或工具（如 SQL Server Management Studio）的计算机。
由于 HGS 负责确定运行 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 的哪些计算机可信，因此它需要与它所保护的 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 实例进行物理隔离和逻辑隔离。
如果相同管理员有权访问 HGS 和 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机，则他们可以将证明服务配置为允许恶意计算机运行 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]，使其可以破坏 VBS enclave。

### <a name="hgs-domain"></a>HGS 域

HGS 安装程序会为 HGS 服务器、故障转移群集资源和管理员帐户自动创建新 Active Directory 域。

运行 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 的计算机无需位于域中，但如果位于某个域中，则该域应与 HGS 服务器使用的域不同。

### <a name="high-availability"></a>高可用性

HGS 功能会自动安装和配置故障转移群集。
在生产环境中，建议使用三台 HGS 服务器来实现高可用性。 请参阅[故障转移群集文档](/windows-server/failover-clustering/manage-cluster-quorum)以了解有关群集仲裁确定方式和备用配置的详细信息（包括具有外部见证服务器的两节点群集）。

HGS 节点之间无需共享存储。 证明数据库的副本存储在每台 HGS 服务器上，由群集服务自动通过网络进行复制。

### <a name="network-connectivity"></a>网络连接

SQL 客户端和 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 都需要能够通过 HTTP 与 HGS 通信。
使用 TLS 证书配置 HGS，以加密 SQL 客户端与 HGS 之间以及 SQL Server 与 HGS 之间的所有通信。
此配置有助于防止中间人攻击，并确保与正确的 HGS 服务器通信。

HGS 服务器要求群集中每个节点之间建立连接，以确保证明服务数据库保持同步。连接一个网络上的 HGS 节点以进行群集通信，并将不同网络用于其他客户端以便与 HGS 通信，这是故障转移群集最佳做法。

### <a name="attestation-modes"></a>证明模式

运行 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 的计算机尝试使用 HGS 进行证明时，它会首先询问 HGS 应如何证明。
HGS 支持对 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 使用两种证明模式：

| 证明模式 | 说明 |
| ---------------- | ------- |
| TPM | 受信任的平台模块 (TPM) 证明在使用 HGS 进行证明的计算机的标识和完整性方面提供最强保证。 它要求运行 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 的计算机安装 TPM 版本 2.0。 每个 TPM 芯片都包含唯一且不可变的标识（认可密钥），该标识可以用于识别特定计算机。 TPM 还度量计算机的启动过程，将安全敏感度量的哈希存储在操作系统可以读取但无法修改的平台控制寄存器 (PCR) 中。 在证明期间使用这些度量来提供计算机在其声明的安全配置中的加密证明。 |
| 主机密钥 | 主机密钥证明是一种形式更简单的证明，仅使用非对称密钥对来验证计算机的标识。 私钥存储在运行 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 的计算机上，公钥会提供给 HGS。 不会度量计算机的安全配置，运行 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 的计算机上不需要 TPM 2.0 芯片。 保护 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机上安装的私钥非常重要，因为获取此密钥的任何人都可以模拟合法的 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机和在 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]内运行的 VBS enclave。 |

通常，我们提供以下建议：

- 对于物理生产服务器，我们建议使用 TPM 证明来实现它所提供的其他保证  。
- 对于虚拟生产服务器，我们建议使用主机密钥证明，因为大多数虚拟机都没有虚拟 TPM 或安全启动  。 如果你使用的是安全增强型 VM（例如，[本地防护型 VM](/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node)），则可以选择使用 TPM 模式。 在所有虚拟化部署中，证明过程仅分析 VM 环境，而不分析 VM 所在的虚拟化平台。
- **对于开发/测试方案**，我们建议使用主机密钥证明，因为它更易于设置。

### <a name="trust-model"></a>信任模型

在 VBS enclave 信任模式下，加密查询和数据会在基于软件的 enclave 中进行评估，以通过主机 OS 对其进行保护。
对此 enclave 进行的访问受虚拟机监控程序保护，如同相同计算机上运行的两个虚拟机无法访问彼此的内存一样。
若要使客户端相信它在与合法 VBS 实例进行通信，必须使用在 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机上建立硬件信任根的基于 TPM 的证明。
启动过程中捕获的 TPM 度量包括 VBS 实例的唯一标识密钥，从而确保健康证书仅在该确切计算机上有效。
而且，当 TPM 在运行 VBS 的计算机上可用时，VBS 标识密钥的私有部分会受 TPM 保护，从而防止任何人尝试模拟该 VBS 实例。

TPM 证明需要安全启动，以确保 UEFI 加载了由 Microsoft 签名的合法引导加载程序，并且没有 Rootkit 拦截虚拟机监控程序启动过程。
此外，默认情况下需要 IOMMU 设备来确保具有直接内存访问的任何硬件设备都无法检查或修改 enclave 内存。

这些保护全部假设运行 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 的计算机是物理计算机。
如果虚拟化运行 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 的计算机，则无法再保证 VM 的内存免于受虚拟机监控程序或虚拟机监控程序管理员检查。例如，虚拟机监控程序管理员可以转储 VM 的内存并访问 enclave 中查询和数据的纯文本版本。
同样，即使 VM 具有虚拟 TPM，它也只能度量 VM 操作系统和启动环境的状态和完整性。
它无法度量控制 VM 的虚拟机监控程序的状态。

但是，即使在 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 进行虚拟化时，enclave 仍会受到保护，免受来自 VM 操作系统内部的攻击。
如果信任虚拟机监控程序或云提供商，并且主要是担心数据库管理员和 OS 管理员对敏感数据的攻击，则虚拟化 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 可能会满足要求。

同样，对于未在运行 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 的计算机上安装 TPM 2.0 模块的情况，或是安全不是至关重要的开发/测试方案，主机密钥证明仍然十分有价值。
仍可以使用上面提到的许多安全功能（包括安全启动和 TPM 1.2 模块）来更好地保护整个 VBS 和操作系统。
但是，由于 HGS 无法通过主机密钥证明验证计算机实际上是否启用了这些设置，因此客户端不保证主机确实使用了所有可用保护。

## <a name="prerequisites"></a>先决条件

### <a name="hgs-server-prerequisites"></a>HGS 服务器先决条件

运行主机保护者服务角色的计算机应满足以下要求：

| 组件 | 要求 |
| --------- | ----------- |
| 操作系统 | Windows Server 2019 Standard 或 Datacenter 版本 |
| CPU | 2 个核心（最少），4 个核心（建议） |
| RAM | 8 GB（最少） |
| NIC | 建议使用 2 个具有静态 IP 的 NIC（1 个用于群集流量，1 个用于 HGS 服务） |

由于需要加密和解密的操作数，HGS 是 CPU 绑定角色。
使用具有加密加速功能的新式处理器会提高 HGS 性能。
证明数据的存储要求非常低，每个唯一计算机证明的范围是 10 KB 到 1 MB。

在开始之前，请勿将 HGS 计算机加入域。

### <a name="ssnoversion-md-computer-prerequisites"></a>[!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机先决条件

运行 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 的计算机必须满足[安装 SQL Server 的要求](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)和 [Hyper-V 硬件要求](/virtualization/hyper-v-on-windows/reference/hyper-v-requirements#hardware-requirements)。

这些要求包括：

- [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] 或更高版本
- Windows 10 企业版 1809 或更高版本；或是 Windows Server 2019 Datacenter 版本。 其他版本的 Windows 10 和 Windows Server 不支持通过 HGS 进行证明。
- 针对虚拟化技术的 CPU 支持：
  - 具有扩展页表的 Intel VT-x。
  - 具有快速虚拟化索引的 AMD-V。
  - 如果在 VM 中运行 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]，则虚拟机监控程序和物理 CPU 必须提供嵌套虚拟化功能。 请参阅[信任模型](#trust-model)部分，以了解有关在 VM 中运行 VBS enclave 时的保证的信息。
    - 在 Hyper-V 2016 或更高版本中，[在 VM 处理器上启用嵌套虚拟化扩展](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization#configure-nested-virtualization)。
    - 在 Azure 中，选择支持嵌套虚拟化的 VM 大小。 所有 v3 系列的 VM 都支持嵌套虚拟化，例如 Dv3 和 Ev3。 请参阅[创建可嵌套的 Azure VM](/azure/virtual-machines/windows/nested-virtualization#create-a-nesting-capable-azure-vm)。
    - 在 VMware vSphere 6.7 或更高版本中，为 VM 启用基于虚拟化的安全支持，如 [VMware 文档](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-C2E78F3E-9DE2-44DB-9B0A-11440800AADD.html)中所述。
    - 其他虚拟机监控程序和公有云可能支持嵌套虚拟化功能，这些功能也实现具有 VBS Enclave 的 Always Encrypted。 有关兼容性和配置说明，请查看虚拟化解决方案的文档。
- 如果计划使用 TPM 证明，则需要准备好 TPM 2.0 修订版 1.16 芯片，以便在服务器中使用。 当前，HGS 证明不适用于 TPM 2.0 修订版 1.38 芯片。 此外，TPM 必须具有有效的认可密钥证书。

## <a name="roles-and-responsibilities-when-configuring-attestation-with-hgs"></a>使用 HGS 配置证明时的角色和职责

使用 HGS 设置证明需要配置各种类型的组件：HGS、[!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机、[!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 实例，以及触发 enclave 证明的应用程序。 具有以下角色之一的用户配置每种类型的组件：

- HGS 管理员 - 部署 HGS，在 HGS 中注册 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机，并与 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员和客户端应用程序管理员共享 HGS 证明 URL。
- [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员 - 安装证明客户端组件，在 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机上启用 VBS，为 HGS 管理员提供在 HGS 中注册 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机所需信息，在 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机上配置证明 URL，并验证 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机是否可以成功使用 HGS 进行证明。
- DBA - 在 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 实例中配置安全 enclave。
- 应用程序管理员 - 使用从 HGS 管理员处获得的证明 URL 配置应用程序。

在生产环境中（处理真实的敏感数据），组织在配置证明时务必遵守角色分离，在这种情况下，不同人员具有不同角色。 特别是，如果在组织中部署 Always Encrypted 的目的是确保 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员和 DBA 无法访问敏感数据，从而减少攻击外围应用，则 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 管理员和 DBA 不应控制 HGS 服务器。

## <a name="devtest-environment-considerations"></a>开发/测试环境注意事项

如果在开发或测试环境中使用具有 VBS enclave 的 Always Encrypted，并且不需要运行 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 的计算机的高可用性或强大保护，则可以进行以下任何或所有让步，以简化部署：

- 仅部署一个 HGS 节点。 即使 HGS 安装了故障转移群集，但如果不关心高可用性，也无需添加其他节点。
- 使用主机密钥模式而不是 TPM 模式，以实现更简单的设置体验。
- 虚拟化 HGS 和/或 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 来保存物理资源。
- 运行 SSMS 或其他工具以便在与 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 相同的计算机上配置具有安全 enclave 的 Always Encrypted。 这会将列主密钥保留在与 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 相同的计算机上，因此请勿在生产环境中这样做。

## <a name="next-steps"></a>后续步骤

- [为 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]](./always-encrypted-enclaves-host-guardian-service-deploy.md) 部署主机保护者服务
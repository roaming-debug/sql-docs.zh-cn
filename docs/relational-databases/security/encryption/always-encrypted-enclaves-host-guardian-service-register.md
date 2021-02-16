---
title: 向主机保护者服务注册计算机
description: 向主机保护者服务注册 SQL Server 计算机，以实现具有安全 enclave 的 Always Encrypted。
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: rpsqrd
ms.author: ryanpu
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 64971ecd3ed3a821ca823ad3cc5d0d05d08669f4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031369"
---
# <a name="register-computer-with-host-guardian-service"></a>向主机保护者服务注册计算机

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

本文介绍如何注册 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机以通过主机保护者服务 (HGS) 进行证明。

> [!NOTE]
> 在 HGS 中注册 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 这一过程种需要 HGS 管理员和 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员配合。 请参阅[使用 HGS 配置证明时的角色和职责](always-encrypted-enclaves-host-guardian-service-plan.md#roles-and-responsibilities-when-configuring-attestation-with-hgs)。

在开始之前，请确保已部署至少一台 HGS 计算机并已设置 HGS 证明服务。
有关详细信息，请参阅[部署适用于 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 的主机保护者服务](./always-encrypted-enclaves-host-guardian-service-deploy.md)。

## <a name="step-1-install-the-attestation-client-components"></a>步骤 1：安装证明客户端组件

> [!NOTE]
> 本步骤应由 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员执行。

要允许 SQL 客户端验证它是否在与可信的 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机通信，[!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机必须成功地通过主机保护者服务进行证明。
证明过程由称为 HGS 客户端的可选 Windows 组件管理。
以下步骤将有助于安装此组件并开始证明。

1. 确保 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机满足 [HGS 规划文档中列出的先决条件](./always-encrypted-enclaves-host-guardian-service-plan.md#prerequisites)。

2. 在提升的 PowerShell 控制台中运行以下命令，以安装主机保护者 Hyper-V 支持功能，其中包含 HGS 客户端和证明组件。

    ```powershell
    Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All
    ```

3. 重启以完成安装。

## <a name="step-2-verify-virtualization-based-security-is-running"></a>步骤 2：验证基于虚拟化的安全性组件是否正在运行

> [!NOTE]
> 本步骤应由 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员执行。

安装主机保护者 Hyper-V 支持功能时，会自动配置并启用基于虚拟化的安全性组件 (VBS)。
[!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] Always Encrypted 的 enclave 受到 VBS 环境的保护并在该环境内运行。
如果计算机未安装并启用 IOMMU 设备，则 VBS 可能无法启动。
要检查 VBS 是否正在运行，请通过运行 `msinfo32.exe` 打开“系统信息”工具，并在“系统摘要”的底部找到 `Virtualization-based security` 项目。

![显示基于虚拟化的安全性组件状态和配置的“系统信息”屏幕截图](./media/always-encrypted-enclaves/msinfo32-vbs-status.png)

要检查的第一个项目是 `Virtualization-based security`，它可能具有以下三个值：

- `Running` 表示已正确配置 VBS，并且 VBS 可以成功启动。 如果计算机显示此状态，则可以跳到步骤 3。
- `Enabled but not running` 表示已将 VBS 配置为运行，但硬件不满足运行 VBS 的最低安全要求。 你可能需要更改 BIOS 或 UEFI 中的硬件配置，以启用可选的处理器功能（如 IOMMU），或者如果硬件确实不支持所需的功能，你可能需要降低 VBS 安全要求。 请继续阅读本部分以了解详细信息。
- `Not enabled` 表示未将 VBS 配置为运行。 主机保护者 Hyper-V 支持功能会自动启用 VBS，因此，如果你看到此状态，建议你重复步骤 1。

如果未在计算机上运行 VBS，请检查 `Virtualization-based security` 属性。 将 `Required Security Properties` 项目中的值与 `Available Security Properties` 项目中的值进行比较。
必需属性必须等于或为可用安全属性的子集，才能运行 VBS。

在证明 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] enclave 的上下文中，安全属性的重要性如下：

- `Base virtualization support` 始终是必需的，因为它表示运行虚拟机监控程序所需的最低硬件功能。
- 建议使用 `Secure Boot`，但对于 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] Always Encrypted 不是必需的。 安全启动可在 UEFI 初始化完成后要求由 Microsoft 签名的引导加载程序立即运行，从而防止 Rootkit。 如果使用的是受信任的平台模块 (TPM) 证明，那么无论是否将 VBS 配置为要求安全启动，都将衡量并强制实施安全启动支持。
- 建议使用 `DMA Protection`，但对于 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] Always Encrypted 不是必需的。 DMA 保护使用 IOMMU 来保护 VBS 和 enclave 内存免受直接内存访问攻击。 在生产环境中，应始终使用具有 DMA 保护的计算机。 在开发/测试环境中，可以不要求具有 DMA 保护。 如果对 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 实例进行了虚拟化，那你很可能没有可用的 DMA 保护，并且将取消此要求才能运行 VBS。 查看[信任模型](./always-encrypted-enclaves-host-guardian-service-plan.md#trust-model)以了解关于在 VM 中运行时安全保证降低的信息。

在降低 VBS 必需的安全功能之前，请与 OEM 或云服务提供商联系，确认是否有办法在 UEFI 或 BIOS 中启用缺少的平台要求（例如，启用安全启动、Intel VT-d 或 AMD IOV）。

要更改 VBS 所需的平台安全功能，请在提升的 PowerShell 控制台中运行以下命令：

```powershell
# Value 0 = No security features required (use this for Azure VMs)
# Value 1 = Only Secure Boot is required
# Value 2 = Only DMA protection is required (default configuration)
# Value 3 = Both Secure Boot and DMA protection are required
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name RequirePlatformSecurityFeatures -Value 0
```

更改注册表后，请重启 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机并再次检查 VBS 是否正在运行。

如果计算机由你的公司管理，那么在你重启之后，组策略或 Microsoft 终结点管理器可能会覆盖你对这些注册表项所做的任何更改。
请与 IT 支持人员联系，了解他们是否部署了管理 VBS 配置的策略。

## <a name="step-3-configure-the-attestation-url"></a>步骤 3：配置证明 URL

> [!NOTE]
> 本步骤应由 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员执行。

接下来，你将通过从 HGS 管理员处获得的 HGS 证明服务的 URL 配置 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机。

在提升的 PowerShell 控制台中，更新并运行以下命令以配置证明 URL。

- 将 `hgs.bastion.local` 替换为 HGS 群集名称
- 可以在任何 HGS 计算机上运行 `Get-HgsServer` 以获取群集名称
- 证明 URL 应始终以 `/Attestation` 结尾
- SQL Server 不利用 HGS 的密钥保护功能，因此请向 `-KeyProtectionServerUrl` 提供类似 `http://localhost` 的任何虚拟 URL

```powershell
Set-HgsClientConfiguration -AttestationServerUrl "https://hgs.bastion.local/Attestation" -KeyProtectionServerUrl "http://localhost"
```

除非之前已向 HGS 注册此计算机，否则该命令会报告证明失败。 此结果是正常的。

cmdlet 输出中的 `AttestationMode` 字段指示 HGS 使用的证明模式。

继续执行[步骤 4A](#step-4a-register-a-computer-in-tpm-mode)，以在 TPM 模式下注册计算机，或执行[步骤 4B](#step-4b-register-a-computer-in-host-key-mode)，以在主机密钥模式下注册计算机。

## <a name="step-4a-register-a-computer-in-tpm-mode"></a>步骤 4A：在 TPM 模式下注册计算机

> [!NOTE]
> 本步骤应由 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员和 HGS 管理员共同执行。 有关详细信息，请参阅以下说明。

### <a name="prepare"></a>准备

> [!NOTE]
> 本操作应由 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员执行。

在此步骤中，你将收集有关计算机 TPM 状态的信息，并将其注册到 HGS。

如果将 HGS 证明服务配置为使用主机密钥模式，请跳到[步骤 4B](#step-4b-register-a-computer-in-host-key-mode)。

在开始收集 TPM 度量之前，请确保你正在使用 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机的已知良好配置。
计算机应安装所有必要的硬件，并应用最新的固件和软件更新。
HGS 在证明时会基于此基线衡量计算机，因此在收集 TPM 度量时，尽可能处于最安全和预期的状态非常重要。

下面是收集用于 TPM 证明的三个数据文件，其中一些文件可以在配置相同的计算机中重复使用。

| 证明项目 | 衡量对象 | 唯一性 |
| -------------------- | ---------------- | ---------- |
| 平台标识符  | 计算机 TPM 中的公共认可密钥和 TPM 制造商提供的认可密钥证书。 | 每台计算机 1 个标识符 |
| TPM 基线 | TPM 中的平台控制注册表 (PCR)，用于衡量在启动过程中加载的固件和 OS 配置。 示例包括安全启动状态以及故障转储是否已加密。 | 每个唯一的计算机配置一个基线（相同的硬件和软件可以使用相同的基线） |
| 代码完整性策略 | 你信任的用于保护计算机的 [Windows Defender 应用程序控制](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control)策略 | 每个部署到计算机的唯一 CI 策略一个。 |

你可以在 HGS 上为每种证明项目配置多个策略，以支持混合组合硬件和软件。
HGS 只要求计算机证明与每种策略类别中的一个策略匹配。
例如，如果你在 HGS 上注册了三个 TPM 基线，那么计算机度量与其中一个基线匹配即可满足策略要求。

### <a name="configure-a-code-integrity-policy"></a>配置代码完整性策略

> [!NOTE]
> 以下步骤应由 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员执行。

HGS 要求对 TPM 模式下的每个计算机证明都应用 Windows Defender 应用程序控制 (WDAC) 策略。
WDAC 代码完整性策略限制可在计算机上运行的软件，方法是检查尝试针对受信任的发布服务器和文件哈希列表执行代码的每个进程。
对于 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 用例，enclave 受基于虚拟化的安全性组件保护，无法从主机 OS 进行修改，因此，WDAC 策略的严格性不会影响加密查询的安全性。
因此，建议你将审核模式策略部署到 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机以满足证明要求，同时避免对系统施加额外的限制。

如果已在计算机上使用自定义 WDAC 代码完整性策略来强化 OS 配置，则可以跳到[收集 TPM 证明信息](#collect-tpm-attestation-information)。

1. 每个 Windows Server 2019、Windows 10 版本 1809 和更高版本的操作系统上都提供了预先生成的示例策略。 `AllowAll` 策略允许任何软件不受限制地在计算机上运行。 将策略转换为 OS 和 HGS 能够理解的二进制格式，以便使用。 在提升的 PowerShell 控制台中，运行以下命令以编译 `AllowAll` 策略：

    ```powershell
    # We are changing the policy to disable enforcement and user mode code protection before compiling
    $temppolicy = "$HOME\Desktop\allowall_edited.xml"
    Copy-Item -Path "$env:SystemRoot\schemas\CodeIntegrity\ExamplePolicies\AllowAll.xml" -Destination $temppolicy
    Set-RuleOption -FilePath $temppolicy -Option 0 -Delete
    Set-RuleOption -FilePath $temppolicy -Option 3

    ConvertFrom-CIPolicy -XmlFilePath $temppolicy -BinaryFilePath "$HOME\Desktop\allowall_cipolicy.bin"
    ```

2. 按照 [Windows Defender 应用程序控制部署指南](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide)中的指导，使用[组策略](/windows/security/threat-protection/windows-defender-application-control/deploy-windows-defender-application-control-policies-using-group-policy)将 `allowall_cipolicy.bin` 文件部署到 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机。 对于工作组计算机，请使用本地组策略编辑器 (`gpedit.msc`) 执行相同的过程。

3. 在 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机上运行 `gpupdate /force` 以配置新的代码完整性策略，然后重启计算机以应用策略。

### <a name="collect-tpm-attestation-information"></a>收集 TPM 证明信息

> [!NOTE]
> 以下步骤应由 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员执行。

对将通过 HGS 进行证明的每台 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机重复以下步骤：

1. 如果计算机处于已知的良好状态，请在 PowerShell 中运行以下命令以收集 TPM 证明信息：

    ```powershell
    # Collects the TPM EKpub and EKcert
    $name = $env:computername
    $path = "$HOME\Desktop"
    (Get-PlatformIdentifier -Name $name).Save("$path\$name-EK.xml")

    # Collects the TPM baseline (current PCR values)
    Get-HgsAttestationBaselinePolicy -Path "$path\$name.tcglog" -SkipValidation

    # Collects the applied CI policy, if one exists
    Copy-Item -Path "$env:SystemRoot\System32\CodeIntegrity\SIPolicy.p7b" -Destination "$path\$name-CIpolicy.bin"
    ```

2. 与 HGS 管理员共享三个证明文件。 

### <a name="register-the-sql-server-computer-with-hgs"></a>在 HGS 中注册 SQL Server 计算机

> [!NOTE]
> 以下步骤应由 HGS 计算机管理员执行。

对将通过 HGS 进行证明的每台 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机重复以下步骤：

1. 将从 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员处获得的证明文件复制到 HGS 服务器。 
2. 在 HGS 服务器上，在提升的 PowerShell 控制台中运行以下命令，以注册 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机：

    ```powershell
    # TIP: REMEMBER TO CHANGE THE FILENAMES
    # Registers the unique TPM with HGS (required for every computer)
    Add-HgsAttestationTpmHost -Path "C:\temp\SQL01-EK.xml"

    # Registers the TPM baseline (required ONCE for each unique hardware and software configuration)
    Add-HgsAttestationTpmPolicy -Name "MyHWSoftwareConfig" -Path "C:\temp\SQL01.tcglog"

    # Registers the CI policy (required ONCE for each unique CI policy)
    Add-HgsAttestationCiPolicy -Name "AllowAll" -Path "C:\temp\SQL01-CIpolicy.bin"
    ```

    > [!TIP]
    > 如果在尝试注册唯一的 TPM 标识符时遇到错误，请确保已在使用的 HGS 计算机上[导入 TPM 中间证书和根证书](./always-encrypted-enclaves-host-guardian-service-deploy.md#switch-to-tpm-attestation)。

除平台标识符、TPM 基线和代码完整性策略以外，还存在通过 HGS 配置和强制实施内置策略，你需要对这些策略进行更改。
这些内置策略根据从服务器收集的 TPM 基线进行衡量，并表示应启用以保护计算机的各种安全设置。
如果你的任何计算机不具有 IOMMU 来防御 DMA 攻击（例如，VM），则需要禁用 IOMMU 策略。

要禁用 IOMMU 要求，请在 HGS 服务器上运行以下命令：

```powershell
Disable-HgsAttestationPolicy Hgs_IommuEnabled
```

> [!NOTE]
> 如果禁用 IOMMU 策略，则通过 HGS 进行证明的任何计算机将不需要 IOMMU。
> 无法只为一台计算机禁用 IOMMU 策略。

你可通过以下 PowerShell 命令查看已注册的 TPM 主机和策略列表：

```powershell
Get-HgsAttestationTpmHost
Get-HgsAttestationTpmPolicy
```

## <a name="step-4b-register-a-computer-in-host-key-mode"></a>步骤 4B：在主机密钥模式下注册计算机

> [!NOTE]
> 本步骤应由 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员和 HGS 管理员共同执行。 有关详细信息，请参阅以下说明。

此步骤将引导你完成为主机生成唯一密钥并将其注册到 HGS 的过程。
如果将 HGS 证明服务配置为使用 TPM 模式，请改为按照[步骤 4A](#step-4a-register-a-computer-in-tpm-mode) 中的指导进行操作。

### <a name="generate-a-key-for-a-ssnoversion-md-computer"></a>为 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机生成密钥

> [!NOTE]
> 本部分应由 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员协助执行。

主机密钥证明的工作原理是在 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机上生成非对称密钥对，并为 HGS 提供该密钥的公共部分。

对将通过 HGS 进行证明的每台 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机重复以下步骤：

1. 要生成密钥对，请在提升的 PowerShell 控制台中运行以下命令：

    ```powershell
    Set-HgsClientHostKey
    Get-HgsClientHostKey -Path "$HOME\Desktop\$env:computername-key.cer"
    ```

    如果已创建主机密钥并想要生成新的密钥对，请改用以下命令：

    ```powershell
    Remove-HgsClientHostKey
    Set-HgsClientHostKey
    Get-HgsClientHostKey -Path "$HOME\Desktop\$env:computername-key.cer"
    ```

2. 与 HGS 管理员共享证书文件。

### <a name="register-the-sql-server-computer-with-hgs"></a>在 HGS 中注册 SQL Server 计算机

> [!NOTE]
> 以下步骤应由 HGS 计算机管理员执行。

对将通过 HGS 进行证明的每台 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机重复以下步骤：

1. 将从 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员处获得的证书文件复制到 HGS 服务器。
2. 在提升的 PowerShell 控制台中运行以下命令，以注册 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机：

    ```powershell
    Add-HgsAttestationHostKey -Name "YourComputerName" -Path "C:\temp\yourcomputername.cer"
   ```

## <a name="step-5-confirm-the-host-can-attest-successfully"></a>步骤 5：确认主机可成功证明

> [!NOTE]
> 本步骤应由 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机管理员执行。

向 HGS 注册 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机（对于 TPM 模式，执行[步骤 4A](#step-4a-register-a-computer-in-tpm-mode)，对于主机密钥模式，执行[步骤 4B](#step-4b-register-a-computer-in-host-key-mode)）之后，应确认它能够成功证明。

可以检查 HGS 证明客户端的配置，并随时使用 [Get-HgsClientConfiguration](/powershell/module/hgsclient/get-hgsclientconfiguration?view=win10-ps&preserve-view=true) 执行证明尝试。

该命令的输出应如下所示：

```
PS C:\> Get-HgsClientConfiguration


IsHostGuarded                  : True
Mode                           : HostGuardianService
KeyProtectionServerUrl         : http://localhost
AttestationServerUrl           : http://hgs.bastion.local/Attestation
AttestationOperationMode       : HostKey
AttestationStatus              : Passed
AttestationSubstatus           : NoInformation
FallbackKeyProtectionServerUrl :
FallbackAttestationServerUrl   :
IsFallbackInUse                : False
```

输出中最重要的两个字段是 `AttestationStatus`（告知你计算机是否已通过证明）和 `AttestationSubStatus`（说明由于何种策略导致计算机证明失败）。

下面说明了 `AttestationStatus` 中最常出现的值：

| `AttestationStatus` | 说明 |
| ----------------- | ----------- |
| 已过期 | 主机之前通过了证明，但颁发给它的健康证书已过期。 请确保主机和 HGS 时间同步。 |
| `InsecureHostConfiguration` | 计算机不符合 HGS 服务器上配置的一个或多个证明策略。 有关详细信息，请参阅 `AttestationSubStatus`。 |
| NotConfigured | 计算机未配置证明 URL。 [配置证明 URL](#step-3-configure-the-attestation-url) |
| Passed | 计算机已通过证明并且受信任，可运行 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] enclave。 |
| `TransientError` | 由于临时错误，证明尝试失败。 此错误通常表示通过网络联系 HGS 时出现问题。 请检查网络连接并确保计算机可以解析和路由到 HGS 服务名称。 |
| `TpmError` | 计算机的 TPM 设备在尝试证明期间报告了一个错误。 检查 TPM 日志以了解详细信息。 清除 TPM 可能会解决该问题，但在清除 TPM 之前，请务必暂停 BitLocker 及其他依赖于 TPM 的服务。 |
| `UnauthorizedHost` | HGS 不知道主机密钥。 请按照[步骤 4B](#step-4b-register-a-computer-in-host-key-mode) 中的说明向 HGS 注册计算机。 |

当 `AttestationStatus` 显示 `InsecureHostConfiguration` 时，`AttestationSubStatus` 字段将填充一个或多个失败的策略名称。
下表将说明最常用的值以及如何修正错误。

| AttestationSubStatus | 含义以及如何操作 |
| -------------------- | ---------------------------- |
| CodeIntegrityPolicy | 计算机上的代码完整性策略未向 HGS 注册，或者计算机当前未使用代码完整性策略。 请参阅[配置代码完整性策略](#configure-a-code-integrity-policy)以获取指导。 |
| DumpsEnabled | 计算机配置为允许故障转储，但 Hgs_DumpsEnabled 策略不允许转储。 请在此计算机禁用转储或禁用 Hgs_DumpsEnabled 策略以继续操作。 |
| FullBoot | 计算机从睡眠状态或休眠中恢复，使得 TPM 度量改变。 请重启计算机以生成新的 TPM 度量。 |
| HibernationEnabled | 计算机配置为允许休眠并生成未加密的休眠文件。 请在计算机上禁用休眠以解决此问题。 |
| HypervisorEnforcedCodeIntegrityPolicy | 计算机未配置为使用代码完整性策略。 请查看“组策略”或“本地组策略”>“计算机配置”>“管理模板”>“系统”>“Device Guard”>“打开基于虚拟化的安全组件”>“基于虚拟化的代码完整性保护”。 此策略项应为“无 UEFI 锁启用”。 |
| Iommu | 此计算机未启用 IOMMU 设备。 如果是物理计算机，请在 UEFI 配置菜单中启用 IOMMU。 如果是虚拟机并且 IOMMU 不可用，请在 HGS 服务器上运行 `Disable-HgsAttestationPolicy Hgs_IommuEnabled`。 |
| SecureBoot | 此计算机上未启用安全启动。 请在 UEFI 配置菜单中启用安全启动，以解决此错误。 |
| VirtualSecureMode | 此计算机上未运行基于虚拟化的安全性组件。 请按照[步骤2：验证 VBS 是否在计算机上运行](#step-2-verify-virtualization-based-security-is-running)中的指南进行操作。 |

## <a name="next-steps"></a>后续步骤

- [在 SQL Server 中配置安全 enclave](always-encrypted-enclaves-configure-enclave-type.md)
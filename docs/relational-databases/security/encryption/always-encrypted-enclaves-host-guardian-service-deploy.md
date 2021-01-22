---
title: 部署主机保护者服务
description: 为具有安全 enclave 的 Always Encrypted 部署主机保护者服务。
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: rpsqrd
ms.author: ryanpu
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 88e79166a8b44139f58192feece211bc3b3d2db3
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534786"
---
# <a name="deploy-the-host-guardian-service-for-ssnoversion-md"></a>为 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 部署主机保护者服务

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

本文介绍如何将主机保护者服务 (HGS) 部署为 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 的证明服务。
在开始之前，请务必阅读[规划主机保护者服务证明](./always-encrypted-enclaves-host-guardian-service-plan.md)一文，以获取先决条件和体系结构指南的完整列表。

> [!NOTE]
> HGS 管理员负责执行本文所述的所有步骤。 请参阅[使用 HGS 配置证明时的角色和职责](always-encrypted-enclaves-host-guardian-service-plan.md#roles-and-responsibilities-when-configuring-attestation-with-hgs)。

## <a name="step-1-set-up-the-first-hgs-computer"></a>步骤 1：设置第一台 HGS 计算机

主机保护者服务 (HGS) 在一台或多台计算机上作为群集服务运行。
在此步骤中，你将在第一台计算机上设置新的 HGS 群集。
如果你已有一个 HGS 群集，并且准备向它添加额外的计算机以实现高可用性，请跳到[步骤 2：向群集添加更多 HGS 计算机](#step-2-add-more-hgs-computers-to-the-cluster)。

在开始之前，请确保所使用的计算机正在运行 Windows Server 2019 Standard 或 Datacenter 版本，你拥有本地管理员特权，并且计算机尚未加入到 Active Directory 域。

1. 以本地管理员身份登录第一台 HGS 计算机，并打开提升的 Windows PowerShell 控制台。 运行以下命令以安装主机保护者服务角色。 计算机将自动重启以应用更改。

    ```powershell
    Install-WindowsFeature -Name HostGuardianServiceRole -IncludeManagementTools -Restart
    ```

2. HGS 计算机重启后，在提升的 Windows PowerShell 控制台中运行以下命令，以安装新的 Active Directory 林：

    ```powershell
    # Select the name for your new Active Directory root domain.
    # Make sure the name does not conflict with, and is not subordinate to, any existing domains on your network.
    $HGSDomainName = 'bastion.local'

    # Specify a Directory Services Restore Mode password that can be used to recover your domain in safe mode.
    # This password is not, and will not change, your admin account password.
    # Save this password somewhere safe and include it in your disaster recovery plan.
    $DSRMPassword = Read-Host -AsSecureString -Prompt "Directory Services Restore Mode Password"

    Install-HgsServer -HgsDomainName $HgsDomainName -SafeModeAdministratorPassword $DSRMPassword -Restart
    ```

    HGS 计算机将再次重启，以完成 Active Directory 林的配置。 下一次登录时，你的管理员帐户将是域管理员帐户。 建议查看 [Active Directory 域服务操作文档](/windows-server/identity/ad-ds/manage/component-updates/ad-ds-operations)以了解有关管理和保护新林的详细信息。

3. 接下来，你将设置 HGS 群集并安装证明服务，方法是在提升的 Windows PowerShell 控制台中运行以下命令：

    ```powershell
    # Note: the name you provide here will be shared by all HGS nodes and used to point your SQL Server computers to the HGS cluster.
    # For example, if you provide "attsvc" here, a DNS record for "attsvc.yourdomain.com" will be created for every HGS computer.
    Initialize-HgsAttestation -HgsServiceName 'hgs'
    ```

## <a name="step-2-add-more-hgs-computers-to-the-cluster"></a>步骤 2：向群集添加更多 HGS 计算机

设置第一台 HGS 计算机和群集后，可以添加其他 HGS 服务器来提供高可用性。
如果仅设置一台 HGS 服务器（例如，在开发/测试环境中），则可以跳到步骤 3。

与第一台 HGS 计算机一样，请确保要加入到的群集正在运行 Windows Server 2019 Standard 或 Datacenter 版本，你拥有本地管理员特权，并且计算机尚未加入到 Active Directory 域。

1. 以本地管理员身份登录计算机，并打开提升的 Windows PowerShell 控制台。 运行以下命令以安装主机保护者服务角色。 计算机将自动重启以应用更改。

    ```powershell
    Install-WindowsFeature -Name HostGuardianServiceRole -IncludeManagementTools -Restart
    ```

2. 检查计算机上的 DNS 客户端配置，以确保它能够解析 HGS 域。 以下命令应返回 HGS 服务器的 IP 地址。 如果无法解析 HGS 域，则可能需要更新网络适配器上的 DNS 服务器信息，然后才能使用 HGS DNS 服务器进行名称解析。

    ```powershell
    # Change 'bastion.local' to the domain name you specified in Step 1.2
    nslookup bastion.local

    # If it fails, use sconfig.exe, option 8, to set the first HGS computer as your preferred DNS server.
    ```

3. 计算机重启后，在提升的 Windows PowerShell 控制台中运行以下命令，以将计算机加入到由第一台 HGS 服务器创建的 Active Directory 域。 你需要 AD 域的域管理员凭据才能运行此命令。 命令完成后，计算机将重启，并成为 HGS 域的 Active Directory 域控制器。

    ```powershell
    # Provide the fully qualified HGS domain name
    $HGSDomainName = 'bastion.local'

    # Provide a domain administrator's credential for the HGS domain
    $DomainAdminCred = Get-Credential

    # Specify a Directory Services Restore Mode password that can be used to recover your domain in safe mode.
    # This password is not, and will not change, your admin account password.
    # Save this password somewhere safe and include it in your disaster recovery plan.
    $DSRMPassword = Read-Host -AsSecureString -Prompt "Directory Services Restore Mode Password"

    Install-HgsServer -HgsDomainName $HgsDomainName -HgsDomainCredential $DomainAdminCred -SafeModeAdministratorPassword $DSRMPassword -Restart
    ```

4. 计算机重启后，请使用域管理员凭据登录。 打开提升的 PowerShell 控制台，运行以下命令以配置证明服务。 由于证明服务能够感知群集，因此它将从其他群集成员复制其配置。 在任何 HGS 节点上对证明策略所做的更改将应用于所有其他节点。

    ```powershell
    # Provide the IP address of an existing, initialized HGS server
    # If you are using separate networks for cluster and application traffic, choose an IP address on the cluster network.
    # You can find the IP address of your HGS server by signing in and running "ipconfig /all"
    Initialize-HgsAttestation -HgsServerIPAddress '172.16.10.20'
    ```

5. 对要添加到 HGS 群集的每台计算机重复步骤 2。

## <a name="step-3-configure-a-dns-forwarder-to-your-hgs-cluster"></a>步骤 3：为 HGS 群集配置 DNS 转发器

HGS 运行其自己的 DNS 服务器，其中包含解析证明服务所需的名称记录。
在将网络的 DNS 服务器配置为将请求转发到 HGS DNS 服务器之前，SQL Server 计算机将无法解析这些记录。

配置 DNS 转发器的过程是因供应商而异，因此，建议联系网络管理员，以获取针对特定网络的正确指导。

如果你在企业网络中使用的是 Windows Server DNS 服务器角色，那么 DNS 管理员可以创建 HGS 域的条件转发器，以便仅转发 HGS 域的请求。
例如，如果 HGS 服务器使用“bastion.local”域名，并且 IP 地址为 172.16.10.20、172.16.10.21 和 172.16.10.22，那么你可以在企业 DNS 服务器上运行以下命令来配置条件转发器：

```powershell
# Tip: make sure to provide every HGS server's IP address
# If you use separate NICs for cluster and application traffic, use the application traffic NIC IP addresses here
Add-DnsServerConditionalForwarderZone -Name 'bastion.local' -ReplicationScope "Forest" -MasterServers "172.16.10.20", "172.16.10.21", "172.16.10.22"
```

## <a name="step-4-configure-the-attestation-service"></a>步骤 4：配置证明服务

HGS 支持两种证明模式：TPM 证明（用于验证每台 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机的完整性和身份）和主机密钥证明（用于简单验证 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机的身份）。
如果尚未选择证明模式，请查看[规划指南](./always-encrypted-enclaves-host-guardian-service-plan.md#attestation-modes)中的证明信息，详细了解每种模式的安全保证和用例。

本部分中的步骤将针对特定的证明模式配置基本证明策略。
你将在步骤 4 中注册主机特定的信息。
如果以后需要更改证明模式，请使用所需的证明模式重复步骤 3 和 4。

### <a name="switch-to-tpm-attestation"></a>切换到 TPM 证明

要通过 HGS 配置 TPM 证明，你需要一台具有 Internet 访问权限的计算机，以及至少一台具有 TPM 2.0 rev 1.16 芯片的 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机。

要将 HGS 配置为使用 TPM 模式，请打开提升的 PowerShell 控制台并运行以下命令：

```powershell
Set-HgsServer -TrustTpm
```

当 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机尝试证明时，群集中的所有 HGS 计算机现在都将使用 TPM 模式。

你需要安装来自 TPM 供应商的认可密钥 (EK) 根证书，然后才能将来自 SQL Server 计算机的 TPM 信息注册到 HGS。
每个物理 TPM 都是在工厂中使用唯一认可密钥进行配置的，该密钥附带了一个可识别制造商的认可密钥证书。
此证书可确保 TPM 为正版。
当你向 HGS 注册新的 TPM 时，HGS 通过将证书链与受信任的根证书进行比较来验证认可密钥证书。

Microsoft 发布了一系列已知良好的 TPM 供应商根证书，你可以将这些根证书导入到 HGS 信任的 TPM 根证书存储中。
如果对 SQL Server 计算机进行了虚拟化，则需要联系云服务提供商或虚拟化平台供应商，以了解如何获取虚拟 TPM 认可密钥的证书链。

要从 Microsoft 下载适用于物理 TPM 的受信任 TPM 根证书包，请完成以下步骤：

1. 在具有 Internet 访问权限的计算机上，从 [https://go.microsoft.com/fwlink/?linkid=2097925](https://go.microsoft.com/fwlink/?linkid=2097925) 下载最新的 TPM 根证书包

2. 验证 cab 文件的签名，以确保它是可信的。

    ```powershell
    # Note: replace the path below with the correct one to the file you downloaded in step 1
    Get-AuthenticodeSignature ".\TrustedTpm.cab"
    ```

    > [!WARNING]
    > 如果签名无效，请勿继续操作，请联系 Microsoft 支持部门以获取帮助。

3. 将 cab 文件扩展到新目录。

    ```powershell
    mkdir .\TrustedTpmCertificates
    expand.exe -F:* ".\TrustedTpm.cab" ".\TrustedTpmCertificates"
    ```

4. 在新目录中，你将看到每个 TPM 供应商的目录。 可以删除不使用的供应商的目录。

5. 将整个“TrustedTpmCertificates”目录复制到 HGS 服务器。

6. 在 HGS 服务器上打开提升的 PowerShell 控制台，并运行以下命令以导入所有 TPM 根证书和中间证书：

    ```powershell
    # Note: replace the path below with the correct location of the TrustedTpmCertificates folder on your HGS computer
    cd "C:\scratch\TrustedTpmCertificates"
    .\setup.cmd
    ```

7. 对每台 HGS 计算机重复步骤 5 和 6。

如果已从 OEM、云服务提供商或虚拟化平台供应商处获取了中间 CA 证书和根 CA 证书，则可以直接将证书导入到相应的本地计算机证书存储：`TrustedHgs_RootCA` 或 `TrustedHgs_IntermediateCA`。 例如，在 PowerShell 中：

```powershell
# Imports MyCustomTpmVendor_Root.cer to the local machine's "TrustedHgs_RootCA" store
Import-Certificate -FilePath ".\MyCustomTpmVendor_Root.cer" -CertStoreLocation "Cert:\LocalMachine\TrustedHgs_RootCA"
```

### <a name="switch-to-host-key-attestation"></a>切换到主机密钥证明

要使用主机密钥证明，请在提升的 PowerShell 控制台中对 HGS 服务器运行以下命令：

```powershell
Set-HgsServer -TrustHostKey
```

当 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机尝试证明时，群集中的所有 HGS 计算机现在都将使用主机密钥模式。

## <a name="step-5-configure-the-hgs-https-binding"></a>步骤 5：配置 HGS HTTPS 绑定

在默认安装中，HGS 只公开 HTTP（端口 80）绑定。
可以配置 HTTPS（端口 443）绑定，以加密 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机和 HGS 之间的所有通信。
建议 HGS 的所有生产实例都使用 HTTPS 绑定。

1. 使用步骤 1.3 中的完全限定 HGS 服务名称作为使用者名称，从证书颁发机构获取 TLS 证书。 如果你不知道自己的服务名称，可以通过在任何 HGS 计算机上运行 `Get-HgsServer` 来找到它。 如果 [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] 计算机使用不同的 DNS 名称来访问 HGS 群集（例如，如果 HGS 位于具有不同地址的网络负载均衡器后面），则可以将 DNS 替代名称添加到“使用者替代名称”列表。

2. 在 HGS 计算机上，使用 [Set-HgsServer](/powershell/module/hgsserver/set-hgsserver) 启用 HTTPS 绑定，并指定在上一步中获得的 TLS 证书。 如果证书已安装到本地证书存储中的计算机上，请使用以下命令将其注册到 HGS：

    ```powershell
    # Note: you'll need to know the thumbprint for your certificate to configure HGS this way
    Set-HgsServer -Http -Https -HttpsCertificateThumbprint "54A043386555EB5118DB367CFE38776F82F4A181"
    ```

    如果已将证书（带私钥）导出到受密码保护的 PFX 文件，则可通过运行以下命令将其注册到 HGS：

    ```powershell
    $PFXPassword = Read-Host -AsSecureString -Prompt "PFX Password"
    Set-HgsServer -Http -Https -HttpsCertificatePath "C:\path\to\hgs_tls.pfx" -HttpsCertificatePassword $PFXPassword
    ```

3. 对群集中的每台 HGS 计算机重复步骤 1 和 2。 不会在 HGS 节点之间自动复制 TLS 证书。 此外，只要使用者与 HGS 服务名称匹配，每台 HGS 计算机都可以拥有自己的唯一 TLS 证书。

## <a name="step-6-determine-and-share-the-hgs-attestation-url"></a>步骤 6：确定并共享 HGS 证明 URL

作为 HGS 管理员，你需要与组织中的 SQL Server 计算机管理员和应用程序管理员共享 HGS 的证明 URL。 SQL Server 计算机管理员将需要证明 URL，以验证 SQL Server 计算机能否使用 HGS 进行证明。 应用程序管理员将需要证明 URL 来配置应用如何连接到 SQL Server。

若要确定证明 URL，请运行以下 cmdlet。

```powershell
Get-HGSServer
```
该命令的输出应如下所示：

```
Name                           Value                                                                         
----                           -----                                                                         
AttestationOperationMode       HostKey                                                                       
AttestationUrl                 {http://hgs.bastion.local/Attestation}                                        
KeyProtectionUrl               {}         
```

HGS 的证明 URL 是 AttestationUrl 属性的值。

## <a name="next-steps"></a>后续步骤

- [将计算机注册到 HGS](./always-encrypted-enclaves-host-guardian-service-register.md)
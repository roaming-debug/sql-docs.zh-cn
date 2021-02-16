---
title: 将 Linux 上的 SQL Server 加入 Active Directory
titleSuffix: SQL Server
description: 本文提供了将 SQL Server Linux 主机连接到 AD 域的指导。 你可以使用内置的 SSSD 包或使用第三方 AD 提供程序。
author: tejasaks
ms.author: tejasaks
ms.reviewer: vanto
ms.date: 11/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 33ce3edfd7afee30e09db78b908bdaebd9a4e322
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345662"
---
# <a name="join-sql-server-on-a-linux-host-to-an-active-directory-domain"></a>将 Linux 主机上的 SQL Server 加入 Active Directory 域

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文提供有关如何将 SQL Server Linux 主机加入 Active Directory (AD) 域的常规指南。 有两种方法：使用内置 SSSD 包或使用第三方 Active Directory 提供程序。 第三方域加入产品的示例包括 [PowerBroker 标识服务 (PBIS)](https://www.beyondtrust.com/)、[One Identity](https://www.oneidentity.com/products/authentication-services/) 和 [Centrify](https://www.centrify.com/)。 本指南涵盖用于检查 Active Directory 配置的步骤。 但本指南不提供在使用第三方实用程序时如何将计算机加入域的相关说明。

## <a name="prerequisites"></a>先决条件

在配置 Active Directory 身份验证之前，需要先在自己的网络上设置 Active Directory 域控制器 (Windows)。 然后将自己的 Linux 主机上的 SQL Server 加入 Active Directory 域。

> [!IMPORTANT]
> 本文中所述的示例步骤仅用于指导，请参考 Ubuntu 16.04、Red Hat Enterprise Linux (RHEL) 7.x 和 SUSE Enterprise Linux (SLES) 12 操作系统。 根据你整体环境的配置和操作系统版本，实际步骤可能略有不同。 例如，Ubuntu 18.04 使用 netplan，而 Red Hat Enterprise Linux (RHEL) 8.x 使用 nmcli 等工具来管理和配置网络。 建议让你环境的系统和域管理员进行特定的工具、配置、自定义以及所需的任何故障排除。

### <a name="reverse-dns-rdns"></a>反向 DNS (RDNS)

将运行 Windows Server 的计算机设置为域控制器时，可能不会默认获得 RDNS 区域。 请确保将运行 SQL Server 的 Linux 计算机的域控制器和 IP 地址都存在适用的 RDNS 区域。

还请确保存在一个 PTR 记录指向域控制器。

## <a name="check-the-connection-to-a-domain-controller"></a>检查与域控制器的连接

检查你能否通过域的短名称和完全限定的名称连接域控制器，且你能否使用域控制器的主机名实现连接。 域控制器的 IP 也应解析为域控制器的 FQDN：

```bash
ping contoso
ping contoso.com
ping dc1.contoso.com
nslookup <IP address of dc1.contoso.com>
```

> [!TIP]
> 本教程分别使用 contoso.com 和 CONTOSO.COM 作为示例域和领域名。 它还使用 DC1.CONTOSO.COM 作为域控制器的完全限定的域名示例。 需要使用自己的值替换这些名称。

如果其中任何一个名称未能通过检查，请更新域搜索列表。 以下部分分别提供有关 Ubuntu、Red Hat Enterprise Linux (RHEL) 和 SUSE Linux Enterprise Server (SLES) 的说明。

### <a name="ubuntu-1604"></a>Ubuntu 16.04

1. 编辑“/etc/network/interfaces”文件，以使 Active Directory 域出现在域搜索列表中：

   ```/etc/network/interfaces
   # The primary network interface
   auto eth0
   iface eth0 inet dhcp
   dns-nameservers **<AD domain controller IP address>**
   dns-search **<AD domain name>**
   ```

   > [!NOTE]
   > 不同计算机的网络接口 `eth0` 可能略有不同。 若要找出正在使用的接口，请运行“ifconfig”。 然后复制具有 IP 地址且传输和接收了字节的接口。

1. 编辑此文件后，重启网络服务：

   ```bash
   sudo ifdown eth0 && sudo ifup eth0
   ```

1. 下一步，检查“/etc/resolv.conf”文件是否包含类似以下示例的行：

   ```/etc/resolv.conf
   search contoso.com com  
   nameserver **<AD domain controller IP address>**
   ```

### <a name="ubuntu-1804"></a>Ubuntu 18.04

1. 编辑 [sudo vi /etc/netplan/******.yaml] 文件，使 Active Directory 域出现在域搜索列表中：

   ```/etc/netplan/******.yaml
   network:
     ethernets:
       eth0:
               dhcp4: true

               dhcp6: true
               nameservers:
                       addresses: [ **<AD domain controller IP address>**]
                       search: [**<AD domain name>**]
     version: 2
   ```

   > [!NOTE]
   > 不同计算机的网络接口 `eth0` 可能略有不同。 若要找出正在使用的接口，请运行“ifconfig”。 然后复制具有 IP 地址且传输和接收了字节的接口。

1. 编辑此文件后，重启网络服务：

   ```bash
   sudo netplan apply
   ```

1. 下一步，检查“/etc/resolv.conf”文件是否包含类似以下示例的行  ：

   ```/etc/resolv.conf
   search contoso.com com  
   nameserver **<AD domain controller IP address>**
   ```

### <a name="rhel-7x"></a>RHEL 7.x

1. 编辑“/etc/sysconfig/network-scripts/ifcfg-eth0”文件，以使 Active Directory 域出现在域搜索列表中。 或根据需要编辑其他的接口配置文件：

   ```/etc/sysconfig/network-scripts/ifcfg-eth0
   PEERDNS=no
   DNS1=**<AD domain controller IP address>**
   DOMAIN="contoso.com com"
   ```

1. 编辑此文件后，重启网络服务：

   ```bash
   sudo systemctl restart network
   ```

1. 现在检查“/etc/resolv.conf”文件是否包含类似以下示例的行：

   ```/etc/resolv.conf
   search contoso.com com  
   nameserver **<AD domain controller IP address>**
   ```

1. 如果仍无法对域控制器进行 ping 操作，请查找域控制器的完全限定的域名和 IP 地址。 示例域名为“DC1.CONTOSO.COM”。 请将以下条目添加到“/etc/hosts”：

   ```/etc/hosts
   **<IP address>** DC1.CONTOSO.COM CONTOSO.COM CONTOSO
   ```

### <a name="sles-12"></a>SLES 12

1. 编辑“/etc/sysconfig/network/config”文件，以使 Active Directory 域控制器 IP 用于 DNS 查询，且 Active Directory 域出现在域搜索列表中：

   ```/etc/sysconfig/network/config
   NETCONFIG_DNS_STATIC_SEARCHLIST=""
   NETCONFIG_DNS_STATIC_SERVERS="**<AD domain controller IP address>**"
   ```

1. 编辑此文件后，重启网络服务：

   ```bash
   sudo systemctl restart network
   ```

1. 下一步，检查“/etc/resolv.conf”文件是否包含类似以下示例的行：

   ```/etc/resolv.conf
   search contoso.com com
   nameserver **<AD domain controller IP address>**
   ```

## <a name="join-to-the-ad-domain"></a>加入 AD 域

验证完基本配置和与域控制器的连接性后，可以通过两个选项将 SQL Server Linux 主机与 Active Directory 域控制器联接：

- [选项 1：使用 SSSD 包](#option1)
- [选项 2：使用第三方 openldap 提供程序实用工具](#option2)

### <a name="option-1-use-sssd-package-to-join-ad-domain"></a><a id="option1"></a> 选项 1：使用 SSSD 包加入 AD 域

此方法使用 realmd 和 sssd 包将 SQL Server 主机加入到 AD 域。

> [!NOTE]
> 这是将 Linux 主机加入 AD 域控制器的首选方法。

下面的步骤用于将 SQL Server 主机加入 Active Directory 域：

1. 使用 [realmd](https://www.freedesktop.org/software/realmd/docs/guide-active-directory-join) 将主机加入 AD 域。 需要先使用 Linux 分发的包管理器在 SQL Server 主机上安装 realmd 和 Kerberos 客户端包：

   **RHEL：**

   ```base
   sudo yum install realmd krb5-workstation
   ```
   
   **SLES 12：**
   
   请注意，这些步骤特定于 SLES 12，它是面向 Linux 的唯一官方支持的 SUSE 版本。

   ```bash
   sudo zypper addrepo https://download.opensuse.org/repositories/network/SLE_12/network.repo
   sudo zypper refresh
   sudo zypper install realmd krb5-client sssd-ad
   ```

   **Ubuntu 16.04：**

   ```bash
   sudo apt-get install realmd krb5-user software-properties-common python-software-properties packagekit
   ```

   **Ubuntu 18.04：**

   ```bash
   sudo apt-get install realmd krb5-user software-properties-common python3-software-properties packagekit
   sudo apt-get install adcli libpam-sss libnss-sss sssd sssd-tools
   ```

1. 安装 Kerberos 客户端包时，如果系统提示输入领域名，请以大写形式输入自己的域名。

1. 确认自己的 DNS 配置正确后，请通过运行以下命令加入域。 需要使用在 AD 中具有足够权限的 AD 帐户进行身份验证，才能将新计算机加入域。 此命令在 AD 中创建新的计算机帐户，创建“/etc/krb5.keytab”主机 keytab 文件，在“/etc/sssd/sssd.conf”中配置域，并更新“/etc/krb5.conf”。

   由于 realmd 出现问题，请先将计算机主机名设置为 FQDN，而不是设置为计算机名称。 否则，realmd 可能不会为计算机创建某些必需 SPN，而且 DNS 条目也不会自动更新，即使你的域控制器支持动态 DNS 更新也是如此。
   
   ```bash
   sudo hostname <old hostname>.contoso.com
   ```
   
   运行上述命令后，/etc/hostname 文件应包含 <old hostname>.contoso.com。

   ```bash
   sudo realm join contoso.com -U 'user@CONTOSO.COM' -v
   ```

   应会显示一条消息：`Successfully enrolled machine in realm`。

   下表列出了可能会收到的一些错误消息以及解决建议：

   | 错误消息 | 建议 |
   |---|---|
   | `Necessary packages are not installed` | 请先使用 Linux 分发的包管理器安装这些包，然后再次运行领域加入命令。 |
   | `Insufficient permissions to join the domain` | 请与域管理员核实自己是否拥有足够的权限将 Linux 计算机加入自己的域。 |
   | `KDC reply did not match expectations` | 你可能没有为用户指定正确的领域名。 领域名区分大小写（通常为大写），可使用命令领域发现 contoso.com 进行标识。 |

   SQL Server 使用 SSSD 和 NSS 将用户帐户和组映射到安全标识符 (SID)。 若要成功创建 AD 登录名，必须为 SQL Server 配置 SSSD 并运行它。 “realmd”通常会在加入域时自动执行此操作，但在某些情况下，你必须单独执行此操作。

   有关详细信息，请参阅如何[手动配置 SSSD](https://access.redhat.com/articles/3023951) 和[配置 NSS 以使用 SSSD](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system-level_authentication_guide/configuring_services#Configuration_Options-NSS_Configuration_Options)。

1. 请确认自己现在是否可以从域中收集有关用户的信息，及是否可以该用户的身份获取 Kerberos 工单。 下面的示例使用 id、[kinit](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/kinit.html) 和 [klist](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/klist.html) 命令执行此操作。

   ```bash
   id user@contoso.com

   uid=1348601103(user@contoso.com) gid=1348600513(domain group@contoso.com) groups=1348600513(domain group@contoso.com)

   kinit user@CONTOSO.COM

   Password for user@CONTOSO.COM:

   klist
   Ticket cache: FILE:/tmp/krb5cc_1000
   Default principal: user@CONTOSO.COM
   ```

   > [!NOTE]
   > - 如果 **id user\@contoso.com** 返回 `No such user`，请通过运行命令 `sudo systemctl status sssd` 确保 SSSD 服务成功启动。 如果服务运行且仍显示该错误，请尝试为 SSSD 启用详细日志记录。 有关详细信息，请参阅 Red Hat 文档：[对 SSSD 进行故障排除](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/System-Level_Authentication_Guide/trouble.html#SSSD-Troubleshooting)。
   >
   > - 如果 **kinit user\@CONTOSO.COM** 返回 `KDC reply did not match expectations while getting initial credentials`，请确保用大写字母指定领域。

有关详细信息，请参阅 Red Hat 文档：[发现和加入标识域](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/realmd-domain.html)。

### <a name="option-2-use-third-party-openldap-provider-utilities"></a><a id="option2"></a> 选项 2：使用第三方 openldap 提供程序实用工具

可以使用 [PBIS](https://www.beyondtrust.com/)、[VAS](https://www.oneidentity.com/products/authentication-services/) 或 [Centrify](https://www.centrify.com/) 等第三方实用程序。 本文不提供每个实用程序的相关步骤。 需先使用其中某个实用程序将 SQL Server 的 Linux 主机加入域，然后才能继续操作。  

SQL Server 不使用第三方集成器的代码或任何与 AD 相关的查询的库。 SQL Server 始终在此设置中直接使用 openldap 库调用查询 AD。 第三方集成器仅用于将 Linux 主机加入 AD 域，SQL Server 不与这些实用程序直接通信。

> [!IMPORTANT]
> 请查看[配合使用 Active Directory 身份验证与 Linux 上的 SQL Server](sql-server-linux-active-directory-authentication.md#additionalconfig) 一文“其他配置选项”部分中关于使用 mssql-conf `network.disablesssd` 配置选项的建议 。

验证是否已正确配置“/etc/krb5.conf”。 对于大多数第三方 Active Directory 提供程序，此配置是自动完成的。 但是，请检查“/etc/krb5.conf”的以下值，以防止将来出现任何问题：

```/etc/krb5.conf
[libdefaults]
default_realm = CONTOSO.COM

[realms]
CONTOSO.COM = {
}

[domain_realm]
contoso.com = CONTOSO.COM
.contoso.com = CONTOSO.COM
```

## <a name="check-that-the-reverse-dns-is-properly-configured"></a>检查反向 DNS 是否已正确配置

以下命令应返回运行 SQL Server 的主机的完全限定的域名 (FQDN)。 例如，“SqlHost.contoso.com”。

```bash
host **<IP address of SQL Server host>**
```

此命令的输出应类似于 `**<reversed IP address>**.in-addr.arpa domain name pointer SqlHost.contoso.com`。 如果此命令未返回主机的 FQDN，或者 FQDN 不正确，请将 Linux 主机上的 SQL Server 的反向 DNS 条目添加到 DNS 服务器。

## <a name="next-steps"></a>后续步骤

本文介绍了使用 Active Directory 身份验证在 Linux 主机上配置 SQL Server 的先决条件。 要配置 Linux 上的 SQL Server 以使其支持 Active Directory 帐户，请按照[配合使用 Active Directory 身份验证与 Linux 上的 SQL Server](sql-server-linux-active-directory-authentication.md) 中的说明进行操作。

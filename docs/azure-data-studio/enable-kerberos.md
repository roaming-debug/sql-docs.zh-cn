---
title: 使用 Windows 身份验证 (Kerberos) 连接 SQL Server 实例
description: 了解如何使用 Microsoft Kerberos 集成身份验证将 Azure Data Studio 连接到 SQL Server 实例。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 2a4e92577f9938b764a3754e19c27a8e7f22658b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048617"
---
# <a name="connect-azure-data-studio-to-sql-server-using-windows-authentication---kerberos"></a>使用 Windows 身份验证将 Azure Data Studio 连接到 SQL Server - Kerberos

Azure Data Studio 支持使用 Kerberos 连接到 SQL Server。

若要在 macOS 或 Linux 上使用集成身份验证（Windows 身份验证），需要设置 Kerberos 工单，将当前用户链接到 Windows 域帐户。

## <a name="prerequisites"></a>先决条件

要开始，需要：

- 访问已加入域的 Windows 计算机，以查询 Kerberos 域控制器。
- 应将 SQL Server 配置为允许 Kerberos 身份验证。 对于在 Unix 上运行的客户端驱动程序，只有使用 Kerberos 才支持集成身份验证。 有关详细信息，请参阅[使用 Kerberos 集成身份验证连接到 SQL Server](../connect/jdbc/using-kerberos-integrated-authentication-to-connect-to-sql-server.md)。 应该为尝试连接到的每个 SQL Server 实例注册服务主体名称 (SPN)。 有关详细信息，请参阅[注册服务主体名称](/previous-versions/sql/sql-server-2008-r2/ms191153(v=sql.105)#SPN%20Formats)。


## <a name="check-if-sql-server-has-a-kerberos-setup"></a>检查 SQL Server 是否设置了 Kerberos

登录到 SQL Server 的主机。 在 Windows 命令提示符下，使用 `setspn -L %COMPUTERNAME%` 列出主机的所有 SPN。 你应该会看到以 MSSQLSvc/HostName.Domain.com 开头的条目，这意味着 SQL Server 已注册 SPN 并且已准备好接受 Kerberos 身份验证。

如果无权访问 SQL Server 实例的主机，那么，可以在加入同一 Active Directory 的任何其他 Windows 操作系统中使用命令 `setspn -L <SQLSERVER_NETBIOS>`，其中 <SQLSERVER_NETBIOS> 是 SQL Server 实例主机的计算机名称。


## <a name="get-the-kerberos-key-distribution-center"></a>获取 Kerberos 密钥发行中心

找到 Kerberos 密钥发行中心 (KDC) 配置值。 在加入 Active Directory 域的 Windows 计算机上运行以下命令：

启动 `cmd.exe` 并运行 `nltest`。

```
nltest /dsgetdc:DOMAIN.COMPANY.COM (where "DOMAIN.COMPANY.COM" maps to your domain's name)

Sample Output
DC: \\dc-33.domain.company.com
Address: \\2111:4444:2111:33:1111:ecff:ffff:3333
...
The command completed successfully
```
复制所需 KDC 配置值的 DC 名称。 在本例中，它是 dc-33.domain.company.com。

## <a name="join-your-os-to-the-active-directory-domain-controller"></a>将操作系统加入 Active Directory 域控制器

### <a name="ubuntu"></a>Ubuntu
```bash
sudo apt-get install realmd krb5-user software-properties-common python-software-properties packagekit
```

编辑 `/etc/network/interfaces` 文件，以便将 Active Directory 域控制器的 IP 地址列为 dns-nameserver。 例如：

```/etc/network/interfaces
<...>
# The primary network interface
auth eth0
iface eth0 inet dhcp
dns-nameservers **<AD domain controller IP address>**
dns-search **<AD domain name>**
```

> [!NOTE]
> 不同计算机的网络接口 (eth0) 可能不同。 若要查明正在使用哪个接口，请运行 ifconfig 并复制具有 IP 地址以及传输和接收字节的接口。

编辑此文件后，重启网络服务：

```bash
sudo ifdown eth0 && sudo ifup eth0
```

现在检查 `/etc/resolv.conf` 文件是否包含如下所示的行：

```Code
nameserver **<AD domain controller IP address>**
```

```bash
sudo realm join contoso.com -U 'user@CONTOSO.COM' -v
<...>
* Success
```
   
### <a name="redhat-enterprise-linux"></a>RedHat Enterprise Linux
```bash
sudo yum install realmd krb5-workstation
```

编辑 `/etc/sysconfig/network-scripts/ifcfg-eth0` 文件（或其他适当的接口配置文件），以便将 Active Directory 域控制器的 IP 地址列为 DNS 服务器：

```/etc/sysconfig/network-scripts/ifcfg-eth0
<...>
PEERDNS=no
DNS1=**<AD domain controller IP address>**
```

编辑此文件后，重启网络服务：

```bash
sudo systemctl restart network
```

现在检查 `/etc/resolv.conf` 文件是否包含如下所示的行：  

```Code
nameserver **<AD domain controller IP address>**
```

```bash
sudo realm join contoso.com -U 'user@CONTOSO.COM' -v
<...>
* Success
   
```

### <a name="macos"></a>macOS

按照以下步骤将 macOS 加入 Active Directory 域控制器：

## <a name="configure-kdc-in-krb5conf"></a>在 krb5.conf 中配置 KDC

在所选编辑器中编辑 `/etc/krb5.conf` 文件。 配置下列密钥：

```bash
sudo vi /etc/krb5.conf

[libdefaults]
  default_realm = DOMAIN.COMPANY.COM
 
[realms]
DOMAIN.COMPANY.COM = {
   kdc = dc-33.domain.company.com
}
```

然后保存 krb5.conf 文件并退出。

> [!NOTE]
> 域必须全部大写。


## <a name="test-the-ticket-granting-ticket-retrieval"></a>测试票证授予票证检索

从 KDC 获取票证授予票证 (TGT)。

```bash
kinit username@DOMAIN.COMPANY.COM
```

使用 klist 查看可用工单。 如果 kinit 成功，应会看到一个票证。

```bash
klist

krbtgt/DOMAIN.COMPANY.COM@ DOMAIN.COMPANY.COM.
```

## <a name="connect-by-using-azure-data-studio"></a>使用 Azure Data Studio 连接

1. 创建新的连接配置文件。

1. 选择“Windows 身份验证”作为身份验证类型。

1. 完成连接配置文件，选择“连接”。

成功连接后，你的服务器将显示在“服务器”边栏中。

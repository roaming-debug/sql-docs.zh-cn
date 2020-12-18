---
title: 教程：为 Linux 上的 SQL Server 使用 AD 身份验证
titleSuffix: SQL Server
description: 本教程介绍为 Linux 上的 SQL Server 配置 Active Directory (AD) 身份验证的步骤。
author: Dylan-MSFT
ms.author: dygray
ms.reviewer: vanto
ms.date: 12/18/2019
ms.topic: tutorial
ms.prod: sql
ms.custom: seodec18
ms.technology: linux
helpviewer_keywords:
- Linux, AAD authentication
ms.openlocfilehash: f1e526621d9ff769094830af5cf312eb8c1f17f9
ms.sourcegitcommit: 2991ad5324601c8618739915aec9b184a8a49c74
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97323510"
---
# <a name="tutorial-use-active-directory-authentication-with-sql-server-on-linux"></a>教程：对 Linux 上的 SQL Server 使用 Active Directory 身份验证

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本教程介绍如何在 Linux 上配置 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 以支持 Active Directory (AD) 身份验证（也称为集成身份验证）。 有关概述，请参阅 [Linux 上的 SQL Server 的 Active Directory 身份验证](sql-server-linux-active-directory-auth-overview.md)。

本教程包含以下任务：

> [!div class="checklist"]
> * 将 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 主机加入 AD 域
> * 为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 创建 AD 用户并设置 SPN
> * 配置 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 服务 keytab
> * 保护 keytab 文件
> * 将 SQL Server 配置为使用 keytab 文件进行 Kerberos 身份验证
> * 在 Transact-SQL 中创建基于 AD 的登录名
> * 使用 AD 身份验证连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

## <a name="prerequisites"></a>先决条件

在配置 AD 身份验证前，需要：

* 在网络上设置 AD 域控制器 (Windows)  
* 安装 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]
  * [Red Hat Enterprise Linux (RHEL)](quickstart-install-connect-red-hat.md)
  * [SUSE Linux Enterprise Server (SLES)](quickstart-install-connect-suse.md)
  * [Ubuntu](quickstart-install-connect-ubuntu.md)

## <a name="join-ssnoversion-host-to-ad-domain"></a><a id="join"></a> 将 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 主机加入 AD 域

将 SQL Server Linux 主机加入 Active Directory 域控制器。 有关如何加入 Active Directory 域的信息，请参阅[将 Linux 主机上的 SQL Server 加入 Active Directory 域](sql-server-linux-active-directory-join-domain.md)。

## <a name="create-ad-user-or-msa-for-ssnoversion-and-set-spn"></a><a id="createuser"></a> 为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 创建 AD 用户（或 MSA）并设置 SPN

> [!NOTE]
> 以下步骤使用[完全限定的域名](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)。 如果位于 **Azure** 中，则必须 **[创建一个](/azure/virtual-machines/linux/portal-create-fqdn)** 才能继续操作。

1. 在域控制器上，运行 [New-ADUser](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee617253(v=technet.10)) PowerShell 命令以创建密码永不过期的新 AD 用户。 以下示例将帐户命名为 `mssql`，但帐户名称可以是你喜欢的任何名称。 系统将提示输入帐户的新密码。

   ```PowerShell
   Import-Module ActiveDirectory

   New-ADUser mssql -AccountPassword (Read-Host -AsSecureString "Enter Password") -PasswordNeverExpires $true -Enabled $true
   ```

   > [!NOTE]
   > 为 SQL Server 提供专用 AD 帐户是最佳安全做法，这样一来，SQL Server 的凭据就不会与使用同一帐户的其他服务共享。 但是，如果你知道帐户的密码（在下一步中生成 keytab 文件时需要），则可以选择重复使用现有 AD 帐户。 此外，应在用户帐户上启用该帐户以支持 128 位和 256 位 Kerberos AES 加密（msDS-SupportedEncryptionTypes 属性）  。 若要验证帐户是否已启用 AES 加密，请在“Active Directory 用户和计算机”实用工具中找到帐户，并选择“属性”。  在“属性”中找到“帐户”选项卡，并验证是否选中了具有以下标题的两个复选框。  
   >
   > 1. **此帐户支持 Kerberos AES 128 位加密**
   >
   > 2. **此帐户支持 Kerberos AES 256 位加密**

2. 使用 **setspn.exe** 工具为此帐户设置 ServicePrincipalName (SPN)。 必须完全按照以下示例设置 SPN 的格式。 可通过在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 主机上运行 `hostname --all-fqdns` 来查找 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 主机的完全限定域名。 除非已将 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置为使用其他端口号，否则 TCP 端口应为 1433。

   ```PowerShell
   setspn -A MSSQLSvc/<fully qualified domain name of host machine>:<tcp port> mssql
   setspn -A MSSQLSvc/<netbios name of the host machine>:<tcp port> mssql
   ```

   > [!NOTE]
   > 如果收到错误 (`Insufficient access rights`)，请与域管理员联系，确保你有足够的权限在此帐户上设置 SPN。 用于注册 SPN 的帐户将需要“写入 servicePrincipalName”权限。 有关详细信息，请参阅 [为 Kerberos 连接注册服务主体名称](../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md)。
   >
   > 如果以后更改 TCP 端口，则必须使用新端口号再次运行 **setspn** 命令。 还需要按照下一部分中的步骤将新的 SPN 添加到 SQL Server 服务 keytab。

有关详细信息，请参阅 [为 Kerberos 连接注册服务主体名称](../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md)。

## <a name="configure-ssnoversion-service-keytab"></a><a id="configurekeytab"></a> 配置 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 服务 keytab

为 Linux 上的 SQL Server 配置 AD 身份验证，需要 AD 帐户（MSA 或 AD 用户帐户）和在上一部分创建的 SPN。

> [!IMPORTANT]
> 如果更改了 AD 帐户的密码或更改了 SPN 所分配到的帐户的密码，则必须使用新密码和密钥版本号 (KVNO) 更新 keytab。 某些服务可能还会自动轮换密码。 查看相关帐户的所有密码轮换策略，并使其与计划性维护活动保持一致，以避免产生意外停机时间。

### <a name="spn-keytab-entries"></a><a id="spn"></a> SPN keytab 条目

1. 检查上一步中创建的 AD 帐户的密钥版本号 (KVNO)。 它通常为 2，但如果多次更改帐户的密码，则它可能是其他整数。 在 SQL Server 主机上，运行以下命令：

    - 以下示例假定 `user` 在 `@CONTOSO.COM` 域中。 将用户和域名修改为你的用户和域名。

   ```bash
   kinit user@CONTOSO.COM
   kvno user@CONTOSO.COM
   kvno MSSQLSvc/<fully qualified domain name of host machine>:<tcp port>@CONTOSO.COM
   ```

   > [!NOTE]
   > SPN 可能需要几分钟才能在域中传播，特别是在域很大的情况下。 如果收到错误 (`kvno: Server not found in Kerberos database while getting credentials for MSSQLSvc/<fully qualified domain name of host machine>:<tcp port>@CONTOSO.COM`)，请等待几分钟，然后重试。</br></br> 以上命令只有在服务器已加入 AD 域时才起作用，上一部分已说明。

1. 使用 [ktpass](/windows-server/administration/windows-commands/ktpass)，然后在 Windows 计算机命令提示符处使用以下命令为每个 SPN 添加 keytab 条目：

    - `<DomainName>\<UserName>` - 可以是 MSA 或 AD 用户帐户
    - `@CONTOSO.COM` - 使用你的域名
    - `/kvno <#>` - 将 `<#>` 替换为之前步骤中获得的 KVNO
    - `<StrongPassword>` - 使用强密码

   ```bash
   ktpass /princ MSSQLSvc/<fully qualified domain name of host machine>:<tcp port>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto aes256-sha1 /mapuser <DomainName>\<UserName> /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ MSSQLSvc/<fully qualified domain name of host machine>:<tcp port>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto rc4-hmac-nt /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ MSSQLSvc/<netbios name of the host machine>:<tcp port>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto aes256-sha1 /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ MSSQLSvc/<netbios name of the host machine>:<tcp port>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto rc4-hmac-nt /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ <UserName>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto aes256-sha1 /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ <UserName>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto rc4-hmac-nt /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>
   ```

   > [!NOTE]
   > 以上命令允许使用 AES 和 RC4 加密密码进行 AD 身份验证。 RC4 是一个比较旧的加密密码，如果需要更高的安全性，可以选择仅使用 AES 加密密码创建 keytab 条目。
   > 最后两个 `UserName` 条目必须为小写，否则权限身份验证可能会失败。

1. 执行上述命令后，你将拥有一个名为 mssql.keytab 的 keytab 文件。 将该文件复制到 `/var/opt/mssql/secrets` 文件夹下的 SQL Server 计算机。

1. 保护 keytab 文件。

    有权访问此 keytab 文件的任何人都可以在域上模拟 SQL Server，因此，请确保限制对该文件的访问，以便只有 mssql 帐户具有读取访问权限：

    ```bash
    sudo chown mssql:mssql /var/opt/mssql/secrets/mssql.keytab
    sudo chmod 400 /var/opt/mssql/secrets/mssql.keytab
    ```

1. 以下配置选项需要使用 mssql-conf 工具来设置，以指定访问 keytab 文件时要使用的帐户。

   ```bash
   sudo mssql-conf set network.privilegedadaccount <username>
   ```

   > [!NOTE]
   > 仅包括用户名，而不是域名\用户名或 username@domain。 SQL Server 在内部添加域名以及该用户名（如果需要使用）。

1. 使用以下步骤配置 SQL Server，以开始使用 keytab 文件进行 Kerberos 身份验证。

    ```bash
    sudo mssql-conf set network.kerberoskeytabfile /var/opt/mssql/secrets/mssql.keytab
    sudo systemctl restart mssql-server
    ```

    > [!TIP]
    > （可选）禁用与域控制器的 UDP 连接以提高性能。 在许多情况下，UDP 连接在连接到域控制器时始终会失败，因此，可在 **/etc/krb5.conf** 中设置配置选项以跳过 UDP 调用。 编辑 **/etc/krb5.conf** 并设置以下选项：
    > ```bash
    > /etc/krb5.conf
    > [libdefaults]
    > udp_preference_limit=0
    > ```

此时，你已准备好在 SQL Server 中使用基于 AD 的登录名。

## <a name="create-ad-based-logins-in-transact-sql"></a><a id="createsqllogins"></a> 在 Transact-SQL 中创建基于 AD 的登录名

1. 连接到 SQL Server 并创建基于 AD 的新登录名：

   ```sql
   CREATE LOGIN [CONTOSO\user] FROM WINDOWS;
   ```

1. 验证登录名现在是否在 [sys.server_principals](../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) 系统目录视图中列出：

   ```sql
   SELECT name FROM sys.server_principals;
   ```

## <a name="connect-to-sql-server-using-ad-authentication"></a><a id="connect"></a> 使用 AD 身份验证连接到 SQL Server

使用域凭据登录客户端计算机。 现在，你可以使用 AD 身份验证连接到 SQL Server，而无需重新输入密码。 如果为 AD 组创建登录名，则身为该组成员的任何 AD 用户都能够以相同方式连接。

客户端用于 AD 身份验证的特定连接字符串参数取决于你使用的驱动程序。 请考虑以下部分中的示例。

### <a name="sqlcmd-on-a-domain-joined-linux-client"></a>已加入域的 Linux 客户端上的 sqlcmd

使用 **ssh** 和域凭据登录已加入域的 Linux 客户端：

```bash
ssh -l user@contoso.com client.contoso.com
```

确保已安装 [mssql-tools](sql-server-linux-setup-tools.md) 包，然后使用 **sqlcmd** 进行连接，而无需指定任何凭据：

```bash
sqlcmd -S mssql-host.contoso.com
```
与 SQL Windows 不同，Kerberos 身份验证用于 SQL Linux 中的本地连接。 但是，你依旧需要提供 SQL Linux 主机的 FQDN，并且如果你尝试连接到“.”、“localhost”、“127.0.0.1”等，AD 身份验证将不起作用。

### <a name="ssms-on-a-domain-joined-windows-client"></a>已加入域的 Windows 客户端上的 SSMS

使用域凭据登录已加入域的 Windows 客户端。 确保已安装 SQL Server Management Studio，然后通过在“连接到服务器”对话框中指定“Windows 身份验证”来连接到 SQL Server 实例（例如，`mssql-host.contoso.com`）。

### <a name="ad-authentication-using-other-client-drivers"></a>使用其他客户端驱动程序的 AD 身份验证

下表介绍适用于其他客户端驱动程序的建议：

| 客户端驱动程序 | 建议 |
|---|---|
| **JDBC** | 使用 Kerberos 集成身份验证连接到 SQL Server。 |
| **ODBC** | 使用集成身份验证。 |
| **ADO.NET** | 连接字符串语法。 |

## <a name="additional-configuration-options"></a><a id="additionalconfig"></a> 其他配置选项

如果使用第三方实用工具（例如 [PBIS](https://www.beyondtrust.com/)、[VAS](https://www.oneidentity.com/products/authentication-services/) 或 [Centrify](https://www.centrify.com/)）将 Linux 主机加入 AD 域，并且希望强制 SQL Server 直接使用 openldap 库，则可使用 **mssql-conf** 配置 **disablesssd** 选项，如下所示：

```bash
sudo mssql-conf set network.disablesssd true
systemctl restart mssql-server
```

> [!NOTE]
> 有些实用工具（例如 **realmd**）会设置 SSSD，而其他工具（例如 PBIS、VAS 和 Centrify）则不会设置 SSSD。 如果用于加入 AD 域的实用工具不会设置 SSSD，则建议将 **disablesssd** 选项配置为 `true`。 虽然不是必需的（因为 SQL Server 会在回退到 openldap 机制前尝试将 SSSD 用于 AD），但配置该选项可提高性能，以便 SQL Server 直接绕过 SSSD 机制来进行 openldap 调用。

如果域控制器支持 LDAPS，则可以强制从 SQL Server 到域控制器的所有连接都通过 LDAPS 完成。 若要检查客户端是否可以通过 ldaps 与域控制器通信，请运行以下 bash 命令：`ldapsearch -H ldaps://contoso.com:3269`。 若要将 SQL Server 设置为仅使用 LDAPS，请运行以下命令：

```bash
sudo mssql-conf set network.forcesecureldap true
systemctl restart mssql-server
```

如果在主机上通过 SSSD 包加入 AD 域，且 **disablesssd** 未设置为 true，则此设置将使用 LDAPS，而不使用 SSSD。 如果 **disablesssd** 设置为 true 且 **forcesecureldap** 也设置为 true，则它将通过 SQL Server 发出的 openldap 库调用使用 LDAPS 协议。

### <a name="post-sql-server-2017-cu14"></a>发布 SQL Server 2017 CU14

从 SQL Server 2017 CU14 开始，如果 SQL Server 使用第三方提供程序加入 AD 域控制器并配置为通过将 **disablesssd** 设置为 true 来使用 openldap 调用进行常规 AD 查找，则还可以使用 **enablekdcfromkrb5** 选项强制 SQL Server 使用 krb5 库进行 KDC 查找，而不是针对 KDC 服务器进行反向 DNS 查找。

这对于想要手动配置 SQL Server 尝试与之通信的域控制器的方案可能很有用。 通过使用 **krb5.conf** 中的 KDC 列表来使用 openldap 库机制。

首先，将 disablessd 和 enablekdcfromkrb5conf 设置为 true，然后重启 SQL Server：

```bash
sudo mssql-conf set network.disablesssd true
sudo mssql-conf set network.enablekdcfromkrb5conf true
systemctl restart mssql-server
```

接下来配置 **/etc/krb5.conf** 中的 KDC 列表，如下所示：

```/etc/krb5.conf
[realms]
CONTOSO.COM = {
  kdc = dcWithGC1.contoso.com
  kdc = dcWithGC2.contoso.com
}
```

> [!NOTE]
> 尽管不建议这样做，但在将 **disablesssd** 配置为 true 时，可以使用 **realmd** 等实用工具（这些实用工具在将 Linux 主机加入域时设置 SSSD），以便 SQL Server 使用 openldap 调用（而不是 SSSD）进行与 Active Directory 相关的调用。

## <a name="next-steps"></a>后续步骤

在本教程中，我们介绍了如何对 Linux 上的 SQL Server 设置 Active Directory 身份验证。 你已了解如何执行以下操作：
> [!div class="checklist"]
> * 将 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 主机加入 AD 域
> * 为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 创建 AD 用户并设置 SPN
> * 配置 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 服务 keytab
> * 在 Transact-SQL 中创建基于 AD 的登录名
> * 使用 AD 身份验证连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

接下来，探索 Linux 上的 SQL Server 的其他安全方案。

> [!div class="nextstepaction"]
> [加密与 Linux 上 SQL Server 的连接](sql-server-linux-encrypted-connections.md)
---
title: 使用 adutil 为 Linux 上的 SQL Server 配置 Active Directory 身份验证
description: 逐步说明如何使用 adutil 为 Linux 上的 SQL Server 配置 Active Directory 身份验证
author: amvin87
ms.author: amitkh
ms.reviewer: vanto
ms.date: 12/10/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 || =sqlallproducts-allversions'
ms.openlocfilehash: f4b084d020c40bf57ae5cbaa2c5a1306808ec1d2
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101834784"
---
# <a name="tutorial-configure-active-directory-authentication-with-sql-server-on-linux-using-adutil"></a>教程：使用 adutil 为 Linux 上的 SQL Server 配置 Active Directory 身份验证

> [!NOTE]
> adutil 目前以公共预览版提供 

此教程介绍如何使用 adutil 为 Linux 上的 SQL Server 配置 Active Directory (AD) 身份验证。 有关使用 ktpass 配置 AD 身份验证的另一种方法，请参阅[教程：对 Linux 上的 SQL Server 使用 Active Directory 身份验证](sql-server-linux-active-directory-authentication.md)。

本教程包含以下任务：

> [!div class="checklist"]
> - 安装 adutil-preview
> - 将 Linux 计算机加入 AD 域
> - 使用 adutil 工具为 SQL Server 创建 AD 用户并设置 ServicePrincipalName (SPN)
> - 创建 SQL Server 服务 keytab 文件
> - 配置 SQL Server 以使用 keytab 文件
> - 使用 Transact-SQL 创建基于 AD 的 SQL Server 登录名
> - 使用 AD 身份验证连接到 SQL Server

## <a name="prerequisites"></a>先决条件

在配置 AD 身份验证之前，需要满足以下要求：

- 在网络上具有 AD 域控制器 (Windows)。
- 在 Linux 主机计算机上安装 adutil-preview 工具。 按照下面基于你所运行 Linux 分发版的部分安装 adutil-preview。

## <a name="install-adutil-preview"></a>安装 adutil-preview

在 Linux 主机上，使用以下命令来安装 adutil-preview。

> [!NOTE]
> 对于此预览版，我们意识到，在某些 Linux 分发版上，如果在没有 `ACCEPT_EULA` 参数的情况下尝试 adutil 安装，则会影响安装体验。 下面的建议是通过 `ACCEPT_EULA=Y` 集安装 adutil-preview 工具。 可在安装之前阅读预览版 [EULA](https://go.microsoft.com/fwlink/?linkid=2151376)。 我们正在积极地处理此问题，应可在正式发布版本中解决问题。

### <a name="rhel"></a>RHEL

1. 下载 Microsoft Red Hat 存储库配置文件。

    ```bash
    sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/8/prod.repo
    ```

1. 如果安装了早期版本的 adutil，请删除所有旧的 adutil 包。

    ```bash
    sudo yum remove adutil
    ```

1. 运行以下命令以安装 adutil-preview。 `ACCEPT_EULA=Y` 接受 adutil 的预览版 EULA。 EULA 位于路径“/usr/share/adutil/”中。

    ```bash
    sudo ACCEPT_EULA=Y yum install -y adutil-preview
    ```

### <a name="ubuntu"></a>Ubuntu

1. 导入公共存储库 GPG 密钥，然后注册 Microsoft Ubuntu 存储库。

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    sudo curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
    ```

1. 如果安装了早期版本的 adutil，请使用以下命令删除所有旧的 adutil 包

    ```bash
    sudo apt-get remove adutil
    ```

1. 运行以下命令以安装 adutil-preview。 `ACCEPT_EULA=Y` 接受 adutil 的预览版 EULA。 EULA 位于路径“/usr/share/adutil/”中。

    ```bash
    sudo apt-get update
    sudo ACCEPT_EULA=Y apt-get install -y adutil-preview
    ```

### <a name="sles"></a>SLES

1. 将 Microsoft SQL Server 存储库添加到 Zypper。

    ```bash
    sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo 
    ```

1. 如果安装了早期版本的 adutil，请删除所有旧的 adutil 包。

    ```bash
    sudo zypper remove adutil
    ```

1. 运行以下命令以安装 adutil-preview。 `ACCEPT_EULA=Y` 接受 adutil 的预览版 EULA。 EULA 位于路径“/usr/share/adutil/”中。

    ```bash
    sudo ACCEPT_EULA=Y zypper install -y adutil-preview
    ```

## <a name="domain-machine-preparation"></a>域计算机准备

确保在 Active Directory 中为 Linux 主机 IP 地址添加了转发主机 (A) 条目。 在本教程中，`myubuntu` 主机的 IP 地址为 `10.0.0.10`。 在 Active Directory 中添加转发主机条目，如下所示。 该条目可确保用户连接到 myubuntu.contoso.com 时，它会到达正确的主机。

:::image type="content" source="media/sql-server-linux-ad-auth-adutil-tutorial/host-a-record.png" alt-text="添加主机记录":::

对于本教程，我们将在 Azure 中使用具有三个 VM 的环境。 一个作为 Windows 域控制器 (DC) 的 VM，域名 `contoso.com`。 域控制器名为 `adVM.contoso.com`。 第二台计算机是名为 `winbox` 的 Windows 计算机，它运行 Windows 10 桌面，用作客户端盒子，并安装了 SQL Server Management Studio (SSMS)。 第三台计算机是名为 `myubuntu` 的 Ubuntu 18.04 LTS 计算机，它承载了 SQL Server。

## <a name="join-the-linux-host-machine-to-your-ad-domain"></a>将 Linux 主机计算机加入 AD 域

将 SQL Server Linux 主机加入 Active Directory 域控制器。 有关如何加入 Active Directory 域的信息，请参阅[将 Linux 主机上的 SQL Server 加入 Active Directory 域](sql-server-linux-active-directory-join-domain.md)。

## <a name="create-an-ad-user-for-sql-server-and-set-the-serviceprincipalname-spn-using-the-adutil-tool"></a>使用 adutil 工具为 SQL Server 创建 AD 用户并设置 ServicePrincipalName (SPN)

1. 使用 `kinit` 命令获取或续订 Kerberos TGT（票证授予票证）。 使用 `kinit` 命令的特权帐户。 此帐户需要具有连接到域的权限，并且还应能够在域中创建帐户和 SPN。

    > [!IMPORTANT]
    > 在运行此命令之前，主机应已加入域，如上一步中所示。

    ```bash
    kinit privilegeduser@DOMAIN.COM
    ```

    示例：对于上面所述的环境，我的特权帐户是 `amvin@CONTOSO.COM`

    ```bash
    kinit amvin@CONTOSO.COM
    ```

2. 使用 adutil 工具，创建将由 SQL Server 用作特权 AD 帐户的新用户。

   ```bash
   adutil user create --name sqluser --distname CN=sqluser,CN=Users,DC=CONTOSO,DC=COM --password 'P@ssw0rd'
   ```

    > [!NOTE]
    > 可以通过以下三种方式之一来指定密码：
    >
    > - 密码标志：--password \<password\>
    > - 环境变量 - `ADUTIL_ACCOUNT_PWD`
    > - 交互式输入
    >
    > 密码输入方法的优先级遵循上面列出的选项的顺序。 建议的选项是使用环境变量或交互式输入提供密码，因为它们比密码标志更安全。

    你可以使用上面所示的可分辨名称 (`-distname`) 指定帐户的名称，也可以使用组织单位 (OU) 名称。 如果同时指定 OU 名称 (`--ou`) 和可分辨名称，将优先使用前者。 可运行以下命令以了解更多详细信息：

    ```bash
    adutil user create --help
    ```

3. 向上面创建的主体注册 SPN。 使用计算机 FQDN。 在本教程中，我们将使用 SQL Server 的默认端口 1433。 你的端口号可能不同。

    ```bash
    adutil spn addauto -n sqluser -s MSSQLSvc -H myubuntu.contoso.com -p 1433
    ```

    > [!NOTE]
    >
    > - `addauto` 将自动创建 SPN，前提是 kinit 帐户有足够的权限。
    > - `-n`：要将 SPN 分配到的帐户的名称。
    > - `-s`：用于生成 SPN 的服务名称。 在本例中，它用于 SQL Server 服务，因此服务名称为 `MSSQLSvc`。
    > - `-H`：用于生成 SPN 的主机名。 如果未指定，将使用本地主机的 FQDN。 在本例中，主机名为 `myubuntu`，FQDN 为 `myubuntu.contoso.com`。
    > - `-p`：用于生成 SPN 的端口。 如果未指定，将在没有端口的情况下生成 SPN。 SQL 连接仅在 SQL Server 侦听默认端口 1433 时才有效。

## <a name="create-the-sql-server-service-keytab-file"></a>创建 SQL Server 服务 keytab 文件

创建 keytab 文件，使其包含分别对应于之前创建的 4 个 SPN 的条目，以及一个对应于用户的条目。

```bash
adutil keytab createauto -k /var/opt/mssql/secrets/mssql.keytab -p 1433 -H myubuntu.contoso.com --password 'P@ssw0rd' -s MSSQLSvc 
```

> [!NOTE]
>
> - `-k`：要在其中创建 `mssql.keytab` 文件的路径。 在上面的示例中，目录 `/var/opt/mssql/secrets/` 应该已经存在于主机上。
> - `-p`：用于生成 SPN 的端口。 如果未指定，将在没有端口的情况下生成 SPN。
> - `-H`：用于生成 SPN 的主机名。 如果未指定，将使用本地主机的 FQDN。 在本例中，主机名为 `myubuntu`，FQDN 为 `myubuntu.contoso.com`。
> - `-s`：用于生成 SPN 的服务名称。 在本例中，它用于 SQL Server 服务，因此服务名称为 `MSSQLSvc`。
> - `--password`：这是之前创建的特权 AD 用户帐户的密码。
> - `-e` 或 `--enctype`：keytab 条目的加密类型。 使用以逗号分隔的值列表。 如果未指定，将显示交互式提示。

当可以选择加密类型时，你可以选择多个加密类型。 在本示例中，我们选择了 `aes256-cts-hmac-sha1-96` 和 `arcfour-hmac`。 确保选择主机和域支持的加密类型。

如果要以非交互方式选择加密类型，可以在上述命令中使用 -e 参数指定你选择的加密类型。 有关 adutil 命令的其他帮助，请运行下面的命令。

```bash
adutil keytab createauto --help
```

> [!NOTE]
> `arcfour-hmac` 是弱加密，不推荐在生产环境中使用此加密类型。

在 keytab 中为主体名称及其密码添加一个条目，SQL Server 将使用该条目来连接到 AD：

```bash
adutil keytab create -k /var/opt/mssql/secrets/mssql.keytab -p sqluser --password 'P@ssw0rd!'
```

> [!NOTE]
>
> - `-k`：要在其中创建 `mssql.keytab` 文件的路径。
> - `-p`：要添加到 keytab 的主体。

adutil keytab 创建/自动创建不会覆盖以前的文件，如果文件已经存在，则它只会追加到其中。

确保创建的 keytab 由 `mssql` 用户拥有，并且只有 `mssql` 用户具有该文件的读/写访问权限。 可以运行 `chown` 和 `chmod` 命令，如下所示：

```bash
chown mssql. /var/opt/mssql/secrets/mssql.keytab
chmod 440 /var/opt/mssql/secrets/mssql.keytab
```

## <a name="configure-sql-server-to-use-the-keytab"></a>配置 SQL Server 以使用 keytab

运行以下命令，以将 SQL Server 配置为使用上一步中创建的 keytab，并将特权 AD 帐户设置为上面创建的用户。 在本示例中，用户名为 `sqluser`。

```bash
/opt/mssql/bin/mssql-conf set network.kerberoskeytabfile /var/opt/mssql/secrets/mssql.keytab
/opt/mssql/bin/mssql-conf set network.privilegedadaccount sqluser
```

## <a name="restart-sql-server"></a>重新启动 SQL Server

运行以下命令以重启 SQL Server 服务：

```bash
sudo systemctl restart mssql-server
```

## <a name="create-ad-based-sql-server-logins-in-transact-sql"></a>在 Transact-SQL 中创建基于 AD 的 SQL Server 登录名

连接到 SQL Server，运行以下命令以创建登录名，并确认列出了该登录名。

```sql
create login [contoso\amvin] From Windows
SELECT name FROM sys.server_principals;
```

## <a name="connect-to-sql-server-using-ad-authentication"></a>使用 AD 身份验证连接到 SQL Server。

若要使用 [SSMS](../ssms/download-sql-server-management-studio-ssms.md) 或 [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) 进行连接，请使用 Windows 凭据登录到 SQL Server。

还可以使用 [sqlcmd](../tools/sqlcmd-utility.md) 等工具通过 Windows 身份验证连接到 SQL Server。

```bash
sqlcmd -E -S 'myubuntu.contoso.com'
```

## <a name="next-steps"></a>后续步骤

- [将 Linux 主机上的 SQL Server 加入 Active Directory 域](sql-server-linux-active-directory-auth-overview.md)
- 如果有兴趣了解如何为 Linux 上的 SQL Server 容器配置 AD 身份验证，请参阅[为 Linux 上的 SQL Server 容器配置 Active Directory 身份验证](sql-server-linux-containers-ad-auth-adutil-tutorial.md)

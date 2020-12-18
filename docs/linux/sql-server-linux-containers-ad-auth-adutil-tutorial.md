---
title: 使用 adutil 为基于 Linux 上的 SQL Server 的容器配置 Active Directory 身份验证
description: 逐步说明如何使用 adutil 为 Linux 上的 SQL Server 容器配置 Active Directory 身份验证
author: amvin87
ms.author: amitkh
ms.reviewer: vanto
ms.date: 12/10/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 || =sqlallproducts-allversions'
ms.openlocfilehash: 318fb046adc25cc2ff485b14974bb756e586162b
ms.sourcegitcommit: 18e2f0706e03d0b2b6324845244fbafaa077a8dd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97103279"
---
# <a name="tutorial-configure-active-directory-authentication-with-sql-server-on-linux--containers"></a>教程：为 Linux 上的 SQL Server 容器配置 Active Directory 身份验证

> [!NOTE]
> adutil 目前以公共预览版提供 

本教程介绍如何配置 Linux 上的 SQL Server 容器以支持 Active Directory (AD) 身份验证（也称为集成身份验证）。 有关概述，请参阅 [Linux 上的 SQL Server 的 Active Directory 身份验证](sql-server-linux-active-directory-auth-overview.md)。

本教程包含以下任务：

> [!div class="checklist"]
> - 安装 adutil-preview
> - 将 Linux 主机加入到 AD 域
> - 使用 adutil 工具为 SQL Server 创建 AD 用户并设置 ServicePrincipalName (SPN)
> - 创建 SQL Server 服务 keytab 文件
> - 创建 SQL Server 容器要使用的 mssql.conf 和 krb5.conf 文件
> - 装载配置文件并部署 SQL Server 容器
> - 使用 Transact-SQL 创建基于 AD 的 SQL Server 登录名
> - 使用 AD 身份验证连接到 SQL Server

## <a name="prerequisites"></a>先决条件

在配置 AD 身份验证之前，需要满足以下要求：

- 在网络上具有 AD 域控制器 (Windows)。
- 在加入域的 Linux 主机上安装 adutil-preview 工具。 按照下面基于你所运行 Linux 分发版的[安装 adutil-preview](#install-adutil-preview) 部分，安装 adutil-preview 工具。

## <a name="container-deployment-and-preparation"></a>容器部署和准备

若要设置容器，需要事先知道主机上容器将使用的端口。 默认端口 1433 在你的容器主机上可能有不同的映射。 对于本教程，主机上的端口 5433 将映射到容器的端口 1433。 有关详细信息，请参阅快速入门[使用 Docker 运行 SQL Server 容器映像](quickstart-install-connect-docker.md)

注册服务主体名称 (SPN) 时，可以使用计算机的主机名或容器的名称，但应根据连接到外部容器时要查看的内容来进行设置。

确保在 Active Directory 中为 Linux 主机 IP 地址添加了转发主机 (A) 条目，映射到 SQL Server 容器的名称。 在本教程中，`myubuntu` 主机的 IP 地址为 `10.0.0.10`，SQL Server 容器名称为 `sql1`。 在 Active Directory 中添加转发主机条目，如下所示。 该条目可确保用户连接到 sql1.contoso.com 时可到达正确的主机。

:::image type="content" source="media/sql-server-linux-containers-ad-auth-adutil-tutorial/host-a-record.png" alt-text="添加主机记录":::

对于本教程，我们将在 Azure 中使用具有三个 VM 的环境。 一个作为 Windows 域控制器 (DC) 的 VM，域名 `contoso.com`。 域控制器名为 `adVM.contoso.com`。 第二台计算机是名为 `winbox` 的 Windows 计算机，它运行 Windows 10 桌面，用作客户端盒子，并安装了 SQL Server Management Studio (SSMS)。 第三台计算机是名为 `myubuntu` 的 Ubuntu 18.04 LTS 计算机，它承载了 SQL Server 容器。 所有计算机都已加入到 `contoso.com` 域。 有关详细信息，请参阅[将 Linux 主机上的 SQL Server 加入 Active Directory 域](sql-server-linux-active-directory-join-domain.md)。

> [!NOTE]
> 在本文后面部分可以看到，不一定要将主机容器计算机加入域。

## <a name="install-adutil-preview"></a>安装 adutil-preview

在 Linux 主机上，使用以下命令来安装基于 Linux 分发版的 adutil-preview。

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

1. 运行以下命令以安装 adutil-preview。 `ACCEPT_EULA=Y` 接受 adutil 的预览版 EULA。 EULA 位于路径 `/usr/share/adutil/` 中。

    ```bash
    sudo ACCEPT_EULA=Y yum install -y adutil-preview
    ```

### <a name="ubuntu"></a>Ubuntu

1. 注册 Microsoft Ubuntu 存储库。

    ```bash
    sudo curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
    ```

1. 如果安装了早期版本的 adutil，请使用以下命令删除所有旧的 adutil 包

    ```bash
    sudo apt-get remove adutil
    ```

1. 运行以下命令以安装 adutil-preview。 `ACCEPT_EULA=Y` 接受 adutil 的预览版 EULA。 EULA 位于路径 `/usr/share/adutil/` 中。

    ```bash
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

1. 运行以下命令以安装 adutil-preview。 `ACCEPT_EULA=Y` 接受 adutil 的预览版 EULA。 EULA 位于路径 `/usr/share/adutil/` 中。

    ```bash
    sudo ACCEPT_EULA=Y zypper install -y adutil-preview
    ```

## <a name="creating-the-ad-user-spns-and-sql-server-service-keytab"></a>创建 AD 用户、SPN 和 SQL Server 服务 keytab

如果你不希望将 Linux 容器主机上的 SQL Server 加入域，所以没有遵循将计算机加入域的步骤，则在另一台已加入 AD 域的 Linux 计算机上，请按照以下步骤操作：

 1. 使用 adutil 工具为 SQL Server 创建 AD 用户并设置 SPN。

 2. 创建并配置 SQL Server 服务 keytab 文件。

将创建的 mssql.keytab 文件复制到将运行 SQL Server 容器的主机，并将该容器配置为使用复制的 mssql.keytab。 你还可以选择将要用于运行 SQL Server 容器的 Linux 主机加入 AD 域，并在同一台计算机上执行以下步骤。

### <a name="create-an-ad-user-for-sql-server-and-set-the-serviceprincipalname-using-the-adutil-tool"></a>使用 adutil 工具为 SQL Server 创建 AD 用户并设置 ServicePrincipalName

要对 Linux 上的 SQL Server 容器启用 AD 身份验证，需要在已加入 AD 域的 Linux 计算机上运行下述步骤 1-3。

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
   adutil user create --name sqluser -distname CN=sqluser,CN=Users,DC=CONTOSO,DC=COM --password 'P@ssw0rd'
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

3. 向上面创建的用户注册 SPN。 根据你喜欢的连接显示方式，可以使用主机名而不是容器名称。 在本教程中，将使用端口 5433 而不是 1433。 这是容器的端口映射。 你的端口号可能不同。

    ```bash
    adutil spn addauto -n sqluser -s MSSQLSvc -H sql1.contoso.com -p 5433
    ```

    > [!NOTE]
    >
    > - `addauto` 将自动创建 SPN，前提是 kinit 帐户有足够的权限。
    > - `-n`：要将 SPN 分配到的帐户的名称。
    > - `-s`：用于生成 SPN 的服务名称。 在本例中，它用于 SQL Server 服务，因此服务名称为 MSSQLSvc。
    > - `-H`：用于生成 SPN 的主机名。 如果未指定，将使用本地主机的 FQDN。 请同时提供容器名称的 FQDN。 在本例中，容器名称为 `sql1`，FQDN 为 `sql1.contoso.com`。
    > - `-p`：用于生成 SPN 的端口。 如果未指定，将在没有端口的情况下生成 SPN。 SQL 连接仅在 SQL Server 侦听默认端口 1433 时才有效。

### <a name="create-the-sql-server-service-keytab-file"></a>创建 SQL Server 服务 keytab 文件

创建 keytab 文件，使其包含分别对应于之前创建的 4 个 SPN 的条目，以及一个对应于用户的条目。 会将 keytab 文件装载到容器中，以便在主机上的任何位置创建该文件。 只要在使用 Docker/podman 部署容器时正确装载生成的 keytab，就可以安全地更改此路径。

若要为所有 SPN 创建 keytab，可以使用 `createauto` 选项：

```bash
adutil keytab createauto -k /container/sql1/secrets/mssql.keytab -p 5433 -H sql1.contoso.com --password 'P@ssw0rd' -s MSSQLSvc
```

> [!NOTE]
>
> - `-k`：要在其中创建 `mssql.keytab` 文件的路径。 在上面的示例中，目录“/container/sql1/secrets”应该已经存在于主机上。
> - `-p`：用于生成 SPN 的端口。 如果未指定，将在没有端口的情况下生成 SPN。
> - `-H`：用于生成 SPN 的主机名。 如果未指定，将使用本地主机的 FQDN。 请同时提供容器名称的 FQDN。 在本例中，容器名称为 `sql1`，FQDN 为 `sql1.contoso.com`。
> - `-s`：用于生成 SPN 的服务名称。 在本例中，它用于 SQL Server 服务，因此服务名称为 MSSQLSvc。
> - `--password`：这是之前创建的特权 AD 用户帐户的密码。
> - `-e` 或 `--enctype`：keytab 条目的加密类型。 使用以逗号分隔的值列表。 如果未指定，将显示交互式提示。

当可以选择加密类型时，你可以选择多个加密类型。 在本示例中，我们选择了 `aes256-cts-hmac-sha1-96` 和 `arcfour-hmac`。 确保选择主机和域支持的加密类型。

如果要以非交互方式选择加密类型，可以在上述命令中使用 -e 参数指定你选择的加密类型。 有关 adutil 命令的其他帮助，请运行下面的命令。

```bash
adutil keytab createauto --help
```

> [!NOTE]
> `arcfour-hmac` 是弱加密，不推荐在生产环境中使用此加密类型。

若要为用户创建 keytab，命令为：

```bash
adutil keytab create -k /container/sql1/secrets/mssql.keytab -p sqluser --password 'P@ssw0rd!'
```

> [!NOTE]
>
> - `-k`：要在其中创建 `mssql.keytab` 文件的路径。 在上面的示例中，目录“/container/sql1/secrets”应该已经存在于主机上。
> - `-p`：要添加到 keytab 的主体。

adutil keytab 创建/自动创建不会覆盖以前的文件，如果文件已经存在，则它只会追加到其中。

确保创建的 keyta 在部署容器时设置正确的权限。

```bash
chmod 440 /container/sql1/secrets/mssql.keytab
```

> [!NOTE]
> 此时，你可以将 mssql.keytab 从当前 Linux 主机复制到将部署 SQL Server 容器的 Linux 主机，并在将运行 SQL Server 容器的 Linux 主机上执行剩余步骤。 如果在要将 SQL 容器部署到的 Linux 主机上执行上述步骤，请在该主机上执行后续步骤。

## <a name="create-the-config-files-to-be-used-by-the-sql-server-container"></a>创建 SQL Server 容器要使用的 config 文件

1. 创建具有 AD 设置的 `mssql.conf` 文件。 可以在主机上的任意位置创建此文件，在 docker run 命令执行期间需正确装载它。 在本示例中，我们将此文件 `mssql.conf` 放置在 `/container/sql1 ` 下，这是我们的容器目录。 `mssql.conf` 的内容如下所示：

    ```output
    [network]
    privilegedadaccount = sqluser
    kerberoskeytabfile = /var/opt/mssql/secrets/mssql.keytab
    ```

    > [!NOTE]
    >
    > - `privilagedadaccount`：要用于 AD 身份验证的特权 AD 用户。
    > - `kerberoskeytabfile`：要将 mssql.keytab 文件放置到的容器内路径。

1. 创建 krb5.conf 文件。 下面显示了一个示例。 这些文件区分大小写。

    ```output
    [libdefaults]
    default_realm = DOMAIN.COM

    [realms]
     CONTOSO.COM = {
         kdc = adVM.contoso.com
         admin_server = adVM.contoso.com
         default_domain = CONTOSO.COM
     }

    [domain_realm]
     .contoso.com = CONTOSO.COM
     contoso.com = CONTOSO.COM


1. Copy all files, `mssql.conf`, `krb5.conf`, `mssql.keytab` to a location that will be mounted to the SQL Server container. In this example, these files are placed on the host at the following locations: `mssql.conf` and `krb5.conf` at `/container/sql1/`. `mssql.keytab` is placed at the location `/container/sql1/secrets/`.

1. Make sure there's enough permission on these folders for the user running the docker/podman command. When the container starts, the user needs access to the folder path created. In this example, we provided the below permissions given to the folder path:

    ```bash
    sudo chmod 755 /container/sql1/
    ```

## <a name="mount-the-config-files-and-deploy-the-sql-server-container"></a>装载配置文件并部署 SQL Server 容器

运行 SQL Server 容器，并装载之前创建的正确 AD 配置文件，如下所示：

```bash
sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=\<YourStrong@Passw0rd\>" \
-p 5433:1433 --name sql1 \
-v /container/sql1:/var/opt/mssql \
-v /container/sql1/krb5.conf:/etc/krb5.conf \
-d mcr.microsoft.com/mssql/server:2019-latest
```

> [!NOTE]
> 在启用 SELinux 的主机等 LSM（Linux 安全模块）上运行容器时，需要使用 `Z` 选项装载卷，该选项会让 Docker 使用专用的未共享标签标记内容。 有关详细信息，请参阅[配置 SE Linux 标签](https://docs.docker.com/storage/bind-mounts/#configure-the-selinux-label)。

本示例将包含以下命令：

```bash
sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=P@ssw0rd" -p 5433:1433 --name sql1 \
-v /container/sql1:/var/opt/mssql/ \
-v /container/sql1/krb5.conf:/etc/krb5.conf \
--dns-search contoso.com \
--dns 10.0.0.4 \
--add-host adVM.contoso.com:10.0.0.4 \
--add-host contoso.com:10.0.0.4 \
--add-host contoso:10.0.0.4 \
-d mcr.microsoft.com/mssql/server:2019-latest
```

> [!NOTE]
>
> - 文件 `mssql.conf` 和 `krb5.conf` 位于主机文件路径 `/container/sql1` 中。
> - 创建的 `mssql.keytab` 位于主机文件路径 `/container/sql1/secrets` 中。
> - 由于我们的主机位于 Azure 上，因此需要将 AD 详细信息按相同顺序追加到 docker run 命令。 在本示例中，域控制器 `adVM` 在域 `contoso.com` 中，IP 地址为 `10.0.0.4`。 域控制器运行 DNS 和 KDC。

## <a name="create-ad-based-sql-server-logins-in-transact-sql"></a>在 Transact-SQL 中创建基于 AD 的 SQL Server 登录名

连接到 SQL 容器，运行以下命令以创建登录名，并确认列出了该登录名。 可以从运行 SSMS、Azure Data Studio (ADS) 或其他任何命令行接口 (CLI) 工具的客户端计算机（Windows 或 Linux）运行此命令。

```sql
create login [contoso\amvin] From Windows
SELECT name FROM sys.server_principals;
```

## <a name="connect-to-sql-server-using-ad-authentication"></a>使用 AD 身份验证连接到 SQL Server。

若要使用 [SSMS](../ssms/download-sql-server-management-studio-ssms.md) 或 [ADS](../azure-data-studio/download-azure-data-studio.md) 进行连接，请使用 SQL Server 名称和端口号（名称可以是容器名称或主机名）通过 Windows 凭据登录到 SQL Server。 对于我们的示例，服务器名称为 `sql1.contoso.com, 5433`。

还可以使用 [sqlcmd](../tools/sqlcmd-utility.md) 等工具连接到容器中的 SQL Server。

```bash
sqlcmd -E -S 'sql1.contoso.com, 5433'
```

## <a name="next-steps"></a>后续步骤

- [快速入门：通过 Docker 运行 SQL Server 容器映像](quickstart-install-connect-docker.md)
- [将 Linux 主机上的 SQL Server 加入 Active Directory 域](sql-server-linux-active-directory-auth-overview.md)

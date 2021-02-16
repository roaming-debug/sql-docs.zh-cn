---
title: Linux 上的 SQL Server 的安装指南
titleSuffix: SQL Server
description: 安装、更新和卸载 Linux 上的 SQL Server。 本文介绍了联机、脱机和无人参与的方案。
author: VanMSFT
ms.author: vanto
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: sqlfreshmay19
ms.technology: linux
ms.assetid: 565156c3-7256-4e63-aaf0-884522ef2a52
ms.openlocfilehash: eacd871674d15bfa96cd77dd290ff9640520b58a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346424"
---
# <a name="installation-guidance-for-sql-server-on-linux"></a>Linux 上的 SQL Server 的安装指南

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文提供有关在 Linux 上安装、更新和卸载 SQL Server 2017 和 SQL Server 2019 的指导。

有关其他部署方案，请参阅：

- [Windows](../database-engine/install-windows/install-sql-server.md)
- [Docker 容器](./sql-server-linux-docker-container-deployment.md)
- [Kubernetes - 大数据群集](../big-data-cluster/deploy-get-started.md)

> [!TIP]
> 本指南涵盖了多种部署方案。 如果只是寻找逐步安装说明，请跳转到其中一个快速入门：
> - [RHEL 快速入门](quickstart-install-connect-red-hat.md)
> - [SLES 快速入门](quickstart-install-connect-suse.md)
> - [Ubuntu 快速入门](quickstart-install-connect-ubuntu.md)
> - [Docker 快速入门](quickstart-install-connect-docker.md)

有关常见问题的解答，请参阅 [Linux 上的 SQL Server 常见问题解答](../linux/sql-server-linux-faq.md)。

## <a name="supported-platforms"></a><a id="supportedplatforms"></a> 支持的平台

SQL Server 在 Red Hat Enterprise Linux (RHEL)、SUSE Linux Enterprise Server (SLES) 和 Ubuntu 上受支持。 此外，它也可作为 Docker 映像提供，可在 Linux 上的 Docker 引擎或用于 Windows/Mac 的 Docker 上运行。

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

| 平台 | 支持的版本 | 获取
|-----|-----|-----
| **Red Hat Enterprise Linux** | 7.3、7.4、7.5、7.6、8  | [获取 RHEL 7.6](https://access.redhat.com/products/red-hat-enterprise-linux/evaluation)
| **SUSE Linux Enterprise Server** | v12 SP2 | [获取 SLES v12 SP2](https://www.suse.com/products/server)
| **Ubuntu** | 16.04 | [获取 Ubuntu 16.04](http://releases.ubuntu.com/xenial/)
| **Docker 引擎** | 1.8+ | [获取 Docker](https://www.docker.com/get-started)

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

[!INCLUDE [linux-supported-platfoms-2019](../includes/linux-supported-platfoms-2019.md)]

::: moniker-end

Microsoft 还支持使用 OpenShift 和 Kubernetes 部署和管理 SQL Server 容器。

> [!NOTE]
> SQL Server 在 Linux 上针对之前列出的发行版进行了测试且受支持。 如果选择在不受支持的操作系统上安装 SQL Server，请查看 [Microsoft SQL Server 的技术支持策略](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server)的“支持策略”部分，以了解支持含义。

## <a name="system-requirements"></a><a id="system"></a> 系统要求

SQL Server 对 Linux 具有以下系统要求：

|||
|-----|-----|
| **内存** | 2 GB |
| **文件系统** | XFS 或 EXT4（其他文件系统均不受支持，如 BTRFS）  。 |
| **磁盘空间** | 6 GB |
| **处理器速度** | 2 GHz |
| **处理器核心数** | 2 个核心 |
| **处理器类型** | 仅兼容 x64 |

如果在生产中使用网络文件系统 (NFS) 远程共享，请注意以下支持要求：

- 使用 NFS 版本 4.2 或更高版本。 较早版本的 NFS 不支持现代文件系统常用的必需功能，例如 fallocate 和稀疏文件创建。
- 仅在 NFS 装载上查找 /var/opt/mssql 目录。 不支持其他文件，例如 SQL Server 系统二进制文件。
- 安装远程共享时，请确保 NFS 客户端使用“nolock”选项。

## <a name="configure-source-repositories"></a><a id="repositories"></a> 配置源存储库

安装或升级 SQL Server 时，从配置的 Microsoft 存储库中获取最新版本的 SQL Server。 快速入门使用 SQL Server 的累积更新 (CU) 存储库。 但是可以改为配置 GDR 存储库。 有关存储库以及如何配置存储库的详细信息，请参阅[为 Linux 上的 SQL Server 配置存储库](sql-server-linux-change-repo.md)。

## <a name="install-sql-server"></a><a id="platforms"></a> 安装 SQL Server

可以从命令行在 Linux 上安装 SQL Server 2017 或 SQL Server 2019。 有关分步说明，请参阅以下快速入门之一：

| 平台 | 安装快速入门 |
|---|---|
| Red Hat Enterprise Linux (RHEL) | [2017](quickstart-install-connect-red-hat.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-red-hat.md?view=sql-server-linux-ver15&preserve-view=true) |
| SUSE Linux Enterprise Server (SLES) | [2017](quickstart-install-connect-suse.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-suse.md?view=sql-server-linux-ver15&preserve-view=true) |
| Ubuntu | [2017](quickstart-install-connect-ubuntu.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-ubuntu.md?view=sql-server-linux-ver15&preserve-view=true) |
| Docker | [2017](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true) |

还可以在 Azure 虚拟机中运行 Linux 上的 SQL Server。 有关详细信息，请参阅[在 Azure 中预配 SQL VM](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)。

安装后，请考虑进行其他配置更改以实现最佳性能。 有关详细信息，请参阅 [Linux 上的 SQL Server 的性能最佳做法和配置指南](sql-server-linux-performance-best-practices.md)。

## <a name="update-or-upgrade-sql-server"></a><a id="upgrade"></a> 更新或升级 SQL Server

若要将“mssql-server”包更新到最新版本，请根据你的平台使用以下命令之一：

| 平台 | 包更新命令 |
|-----|-----|
| RHEL | `sudo yum update mssql-server` |
| SLES | `sudo zypper update mssql-server` |
| Ubuntu | `sudo apt-get update`<br/>`sudo apt-get install mssql-server` |

这些命令将下载最新包，并替换 `/opt/mssql/` 下的二进制文件。 此操作不会影响到用户生成的数据库和系统数据库。

若要升级 SQL Server，请首先[将配置的存储库更改](sql-server-linux-change-repo.md)为所需的 SQL Server 版本。 然后使用同一个 update 命令升级 SQL Server 版本。 这仅当两个存储库之间支持升级路径时才可行。

## <a name="rollback-sql-server"></a><a id="rollback"></a> 回滚 SQL Server

若要将 SQL Server 回滚或降级到以前的版本，请使用以下步骤：

1. 标识要降级到的 SQL Server 包的版本号。 有关包版本号的列表，请参阅[发行说明](../linux/sql-server-linux-release-notes.md)。

1. 降级到 SQL Server 的早期版本。 在以下命令中，将 `<version_number>` 替换为步骤 1 中标识的 SQL Server 版本号。

   | 平台 | 包更新命令 |
   |-----|-----|
   | RHEL | `sudo yum downgrade mssql-server-<version_number>.x86_64` |
   | SLES | `sudo zypper install --oldpackage mssql-server=<version_number>` |
   | Ubuntu | `sudo apt-get install mssql-server=<version_number>`<br/>`sudo systemctl start mssql-server` |

> [!NOTE]
> 只支持降级到相同主版本（如 SQL Server 2019）内的版本。

## <a name="check-installed-sql-server-version"></a><a id="versioncheck"></a> 检查已安装的 SQL Server 版本

若要验证 Linux 上的 SQL Server 的当前版本和版本，请使用以下过程：

1. 如果尚未安装，请安装 [SQL Server 命令行工具](sql-server-linux-setup-tools.md)。

1. 使用“sqlcmd”运行显示 SQL Server 版本的 Transact-SQL 命令。

   ```bash
   sqlcmd -S localhost -U SA -Q 'select @@VERSION'
   ```

## <a name="uninstall-sql-server"></a><a id="uninstall"></a> 卸载 SQL Server

若要删除 Linux 上的“mssql-server”包，请根据你的平台使用以下命令之一：

| 平台 | 包删除命令 |
|-----|-----|
| RHEL | `sudo yum remove mssql-server` |
| SLES | `sudo zypper remove mssql-server` |
| Ubuntu | `sudo apt-get remove mssql-server` |

删除包不会删除生成的数据库文件。 如果希望删除数据库文件，请使用以下命令：

```bash
sudo rm -rf /var/opt/mssql/
```

## <a name="unattended-install"></a><a id="unattended"></a> 无人参与安装

可以通过以下方式执行无人参与安装：

- 按照[快速入门](#platforms)中的初始步骤注册存储库并安装 SQL Server。
- 运行 `mssql-conf setup` 时，设置[环境变量](sql-server-linux-configure-environment-variables.md)并使用 `-n`（无提示）选项。

以下示例使用“MSSQL_PID”环境变量配置 SQL Server 的开发人员版本。 它还接受 EULA (ACCEPT_EULA) 并设置 SA 用户密码 (MSSQL_SA_PASSWORD) 。 该 `-n` 参数执行无提示安装，安装期间从环境变量中提取配置值。

```bash
sudo MSSQL_PID=Developer ACCEPT_EULA=Y MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' /opt/mssql/bin/mssql-conf -n setup
```

还可以创建执行其他操作的脚本。 例如，可安装其他 SQL Server 包。

有关更详细的示例脚本，请参阅以下示例：

- [Red Hat 无人参与安装脚本](sample-unattended-install-redhat.md)
- [SUSE 无人参与安装脚本](sample-unattended-install-suse.md)
- [Ubuntu 无人参与安装脚本](sample-unattended-install-ubuntu.md)

## <a name="offline-install"></a><a id="offline"></a> 脱机安装

如果 Linux 计算机无法访问[快速入门](#platforms)中使用的联机存储库，则可以直接下载包文件。 这些包位于 Microsoft 存储库中，网址为 [https://packages.microsoft.com](https://packages.microsoft.com)。

> [!TIP]
> 如果通过快速入门中的步骤成功安装了包，则无需下载或者手动安装 SQL Server 包。 这部分仅适用于脱机情况。

1. **下载适用于平台的数据库引擎包**。 在[发行说明](../linux/sql-server-linux-release-notes.md)的包详细信息部分找到包下载链接。

1. **将下载的包移动到 Linux 计算机**。 如果使用了不同的计算机下载包，则可以通过“scp”命令将包移至你的 Linux 计算机。

1. **安装数据库引擎包**。 根据你的平台使用以下命令之一。 将此示例中的包文件名替换为下载的确切名称。

   | 平台 | 包安装命令 |
   |-----|-----|
   | RHEL | `sudo yum localinstall mssql-server_versionnumber.x86_64.rpm` |
   | SLES | `sudo zypper install mssql-server_versionnumber.x86_64.rpm` |
   | Ubuntu | `sudo dpkg -i mssql-server_versionnumber_amd64.deb` |

    > [!NOTE]
    > 还可使用 `rpm -ivh` 命令安装 RPM 包（RHEL 和 SLES），但如果可以从批准的存储库中获得，则上表中的命令会自动安装依赖项。

1. **解决缺少依赖项的问题**：此时可能会出现缺少依赖项的情况。 如果没有，可以跳过此步骤。 在 Ubuntu 上，如果能够访问包含这些依赖项的已批准的存储库，最简单的解决办法是使用 `apt-get -f install` 命令。 此命令还会完成 SQL Server 的安装。 若要手动检查依赖项，请使用以下命令：

   | 平台 | 列出依赖项命令 |
   |-----|-----|
   | RHEL | `rpm -qpR mssql-server_versionnumber.x86_64.rpm` |
   | SLES | `rpm -qpR mssql-server_versionnumber.x86_64.rpm` |
   | Ubuntu | `dpkg -I mssql-server_versionnumber_amd64.deb` |

   解决缺少的依赖项后，尝试再次安装 mssql-server 包。

1. **完成 SQL Server 安装**。 使用“mssql-conf”完成 SQL Server 安装：

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

## <a name="licensing-and-pricing"></a>许可和定价

对于 Linux 和 Windows，SQL Server 获得的许可都相同。 有关 SQL Server 许可和定价的详细信息，请参阅[如何授予 SQL Server 许可](https://www.microsoft.com/sql-server/sql-server-2017-pricing)。

## <a name="optional-sql-server-features"></a>可选的 SQL Server 功能

安装后，还可以安装或启用可选的 SQL Server 功能。

- [SQL Server 命令行工具](sql-server-linux-setup-tools.md)
- [SQL Server 代理](sql-server-linux-setup-sql-agent.md)
- [SQL Server 全文搜索](sql-server-linux-setup-full-text-search.md)
- [机器学习服务（R、Python）](sql-server-linux-setup-machine-learning.md)
- [SQL Server Integration Services](sql-server-linux-setup-ssis.md)

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]

> [!TIP]
> 有关常见问题的解答，请参阅 [Linux 上的 SQL Server 常见问题解答](sql-server-linux-faq.md)。

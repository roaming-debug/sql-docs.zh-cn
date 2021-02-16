---
title: 在 Linux 上安装 SQL Server 命令行工具
titleSuffix: SQL Server
description: 了解如何在 Linux 上安装 SQL Server 命令行工具、Microsoft ODBC 驱动程序及其依赖项。
author: VanMSFT
ms.author: vanto
ms.date: 06/30/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: sqlfreshmay19
ms.technology: linux
ms.assetid: eff8e226-185f-46d4-a3e3-e18b7a439e63
ms.openlocfilehash: 36508869b30688e7a87035b50a7a06ca1e2a083a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346342"
---
# <a name="install-sqlcmd-and-bcp-the-sql-server-command-line-tools-on-linux"></a>在 Linux 上安装 sqlcmd 和 bcp SQL Server 命令行工具

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

通过以下步骤安装命令行工具、Microsoft ODBC 驱动程序及其依赖项。 **mssql-tools** 包包含：

- **sqlcmd**：命令行查询实用工具。
- **bcp**：批量导入-导出实用工具。

为你的平台安装工具：

- [Red Hat Enterprise Linux](#RHEL)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#SLES)
- [macOS](#macos)
- [Docker](#docker)

本文介绍如何安装命令行工具。 如果正在寻找有关如何使用 **sqlcmd** 或 **bcp** 的示例，请参阅本主题末尾的 [链接](#next-steps)。

## <a name="a-idrhelinstall-tools-on-rhel-8"></a><a id="RHEL"><a/>在 RHEL 8 上安装工具

通过以下步骤在 Red Hat Enterprise Linux 上安装 **mssql-tools**。 

1. 进入超级用户模式。

   ```bash
   sudo su
   ```

1. 下载 Microsoft Red Hat 存储库配置文件。

   ```bash
   curl https://packages.microsoft.com/config/rhel/8/prod.repo > /etc/yum.repos.d/msprod.repo
   ```

1. 退出超级用户模式。

   ```bash
   exit
   ```

1. 如果安装了早期版本的 **mssql-tools**，请删除所有旧的 unixODBC 包。

   ```bash
   sudo yum remove mssql-tools unixODBC-utf16-devel
   ```

1. 运行以下命令，以使用 unixODBC 开发人员包安装 **mssql-tools**。

   ```bash
   sudo yum install mssql-tools unixODBC-devel
   ```

   > [!Note] 
   > 若要将 mssql-tools 更新至最新版本，请运行以下命令：
   >    ```bash
   >   sudo yum check-update
   >   sudo yum update mssql-tools
   >   ```

1. **可选**：向 bash shell 中的 PATH 环境变量添加 `/opt/mssql-tools/bin/`。

   要使 sqlcmd/bcp 能从登陆会话的 bash shell 进行访问，请使用下列命令修改 ~/.bash_profile 文件中的 PATH  ：

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   ```

   要使 sqlcmd/bcp 能从交互式/非登录会话的 bash shell 进行访问，请使用下列命令修改 ~/.bashrc 文件中的 PATH  ：

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a name="install-tools-on-ubuntu-1604"></a><a id="ubuntu"></a> 在 Ubuntu 16.04 上安装工具

通过以下步骤在 Ubuntu 上安装 **mssql-tools**。

> [!NOTE]
> 从 SQL Server 2019 CU3 开始，支持 Ubuntu 18.04。 如果使用的是 Ubuntu 18.04，请将存储库路径从 `/ubuntu/16.04` 更改为 `/ubuntu/18.04`。

1. 导入公共存储库 GPG 密钥。

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. 注册 Microsoft Ubuntu 存储库。

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
   ```

1. 更新源列表，并使用 unixODBC 开发人员包运行安装命令。

   ```bash
   sudo apt-get update 
   sudo apt-get install mssql-tools unixodbc-dev
   ```

   > [!Note] 
   > 若要将 mssql-tools 更新至最新版本，请运行以下命令：
   >    ```bash
   >   sudo apt-get update 
   >   sudo apt-get install mssql-tools 
   >   ```

1. **可选**：向 bash shell 中的 PATH 环境变量添加 `/opt/mssql-tools/bin/`。

   要使 sqlcmd/bcp 能从登陆会话的 bash shell 进行访问，请使用下列命令修改 ~/.bash_profile 文件中的 PATH  ：

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   ```

   要使 sqlcmd/bcp 能从交互式/非登录会话的 bash shell 进行访问，请使用下列命令修改 ~/.bashrc 文件中的 PATH  ：

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a name="install-tools-on-sles-12"></a><a id="SLES"></a> 在 SLES 12 上安装工具

通过以下步骤在 SUSE Linux Enterprise Server 上安装 **mssql-tools**。 

1. 将 Microsoft SQL Server 存储库添加到 Zypper。

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo 
   sudo zypper --gpg-auto-import-keys refresh
   ```

1. 使用 unixODBC 开发人员包安装 **mssql-tools**。

   ```bash
   sudo zypper install mssql-tools unixODBC-devel
   ```

   > [!Note] 
   > 若要将 mssql-tools 更新至最新版本，请运行以下命令：
   >    ```bash
   >   sudo zypper refresh
   >   sudo zypper update mssql-tools
   >   ```

1. **可选**：向 bash shell 中的 PATH 环境变量添加 `/opt/mssql-tools/bin/`。

   要使 sqlcmd/bcp 能从登陆会话的 bash shell 进行访问，请使用下列命令修改 ~/.bash_profile 文件中的 PATH  ：

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   ```

   要使 sqlcmd/bcp 能从交互式/非登录会话的 bash shell 进行访问，请使用下列命令修改 ~/.bashrc 文件中的 PATH  ：

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a name="install-tools-on-macos"></a><a id="macos"></a> 在 macOS 上安装工具

macOS 上目前提供 **sqlcmd** 和 **bcp** 的预览版。 有关详细信息，请参阅[公告](https://blogs.technet.microsoft.com/dataplatforminsider/2017/05/16/sql-server-command-line-tools-for-macos-released/)。

*如果尚未安装 [Homebrew](https://brew.sh)，请进行安装：*

- `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

若要为 Mac El Capitan 和 Sierra 安装工具，请使用以下命令：

```bash
# brew untap microsoft/mssql-preview if you installed the preview version 
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
brew install mssql-tools
#for silent install: 
#HOMEBREW_NO_ENV_FILTERING=1 ACCEPT_EULA=y brew install mssql-tools
```

## <a name="docker"></a><a id="docker"></a> Docker

如果[在 Docker 容器中运行 SQL Server](quickstart-install-connect-docker.md)，则 SQL Server 命令行工具已包含在 SQL Server Linux 容器映像中。 如果使用交互式 bash shell 附加到正在运行的容器，则可以在本地运行这些工具。

## <a name="offline-installation"></a>脱机安装

[!INCLUDE[SQL Server Linux offline package installation](../includes/sql-server-linux-offline-package-install-intro.md)]

1. 首先，找到并复制适用于你的 Linux 分发版的 **mssql-tools** 包：

   | Linux 分发版 | **mssql-tools** 包位置 |
   |---|---|
   | Red Hat | [https://packages.microsoft.com/rhel/7.3/prod](https://packages.microsoft.com/rhel/7.3/prod) |
   | SLES | [https://packages.microsoft.com/sles/12/prod](https://packages.microsoft.com/sles/12/prod)|
   | Ubuntu 16.04 | [https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/mssql-tools](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/mssql-tools) |

1. 此外，找到并复制 **msodbcsql** 包，它是一个依赖项。 **msodbcsql** 包对 **unixODBC-devel**（Red Hat 和 SLES）或 **unixodbc-dev** (Ubuntu) 也具有依赖项。 下表列出了 **msodbcsql** 包的所在位置：

   | Linux 分发版 | ODBC 包位置 |
   |---|---|
   | Red Hat | [https://packages.microsoft.com/rhel/8/prod](https://packages.microsoft.com/rhel/8/prod) |
   | SLES | [https://packages.microsoft.com/sles/12/prod](https://packages.microsoft.com/sles/12/prod)|
   | Ubuntu 16.04 | [**msodbcsql**](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql)<br/>[**unixodbc-dev**](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/u/unixodbc/) |

1. **将下载的包移至 Linux 计算机**。 如果使用了不同的计算机下载包，则可以通过“scp”命令将包移至你的 Linux 计算机。

1. **安装包**：安装 **mssql-tools** 和 **msodbc** 包。 如果遇到任何依赖项错误，请忽略，直到出现下一步操作。

    | 平台 | 包安装命令 |
    |-----|-----|
    | Red Hat | `sudo yum localinstall msodbcsql-<version>.rpm`<br/>`sudo yum localinstall mssql-tools-<version>.rpm` |
    | SLES | `sudo zypper install msodbcsql-<version>.rpm`<br/>`sudo zypper install mssql-tools-<version>.rpm` |
    | Ubuntu | `sudo dpkg -i msodbcsql_<version>.deb`<br/>`sudo dpkg -i mssql-tools_<version>.deb` |

1. **解决缺少依赖项的问题**：此时可能会出现缺少依赖项的情况。 如果没有，可以跳过此步骤。 在某些情况下，必须手动查找并安装这些依赖项。

    对于 RPM 包，可通过以下命令检查必需的依赖项：

    ```bash
    rpm -qpR msodbcsql-<version>.rpm
    rpm -qpR mssql-tools-<version>.rpm
    ```

    对于 Debian 包，如果能够访问包含这些依赖项的已批准存储库，则最简单的解决办法是使用 **apt-get** 命令：

    ```bash
    sudo apt-get -f install
    ```

    > [!NOTE]
    > 此命令还会完成 SQL Server 包的安装。

    如果此命令对 Debian 包不起作用，可通过以下命令检查必需的依赖项：

    ```bash
    dpkg -I msodbcsql_<version>_amd64.deb | grep "Depends:"
    dpkg -I mssql-tools_<version>_amd64.deb | grep "Depends:"
    ```

## <a name="next-steps"></a>后续步骤

有关如何使用 **sqlcmd** 连接到 SQL Server 并创建数据库的示例，请参阅以下快速入门之一：

- [在 Red Hat Enterprise Linux 上安装](quickstart-install-connect-red-hat.md)
- [在 SUSE Linux Enterprise Server 上安装](quickstart-install-connect-suse.md)
- [在 Ubuntu 上安装](quickstart-install-connect-ubuntu.md)
- [在 Docker 上运行](quickstart-install-connect-ubuntu.md)

有关如何使用 **bcp** 批量导入和导出数据的示例，请参阅 [将数据批量复制到 Linux 上的 SQL Server](sql-server-linux-migrate-bcp.md)。

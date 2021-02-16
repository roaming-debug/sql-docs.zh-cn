---
title: SUSE：在 Linux 上安装 SQL Server
description: 本快速入门介绍了如何在 SUSE Linux Enterprise Server 上安装 SQL Server 2017 或 SQL Server 2019，然后使用 sqlcmd 创建和查询数据库。
author: VanMSFT
ms.author: vanto
ms.date: 04/10/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 31ddfb80-f75c-4f51-8540-de6213cb68b8
ms.openlocfilehash: 7aadd757e9e825ace17b69d28f2ab8ecc78123da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340400"
---
# <a name="quickstart-install-sql-server-and-create-a-database-on-suse-linux-enterprise-server"></a>快速入门：在 SUSE Linux Enterprise Server 上安装 SQL Server 并创建数据库

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

在本快速入门中，将在 SUSE Linux Enterprise Server (SLES) v12 SP2 上安装 SQL Server 2017 或 SQL Server 2019。 然后使用 sqlcmd 进行连接，创建第一个数据库并运行查询  。

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

在本快速入门中，将在 SUSE Linux Enterprise Server (SLES) v12 上安装 SQL Server 2019。 然后使用 sqlcmd 进行连接，创建第一个数据库并运行查询  。

> [!IMPORTANT]
> SUSE Enterprise Linux Server v12 SP2、SP3、SP4 或 SP5 支持 SQL Server 2019。

::: moniker-end

> [!TIP]
> 本教程需要用户输入和 Internet 连接。 如果对[无人参与](sql-server-linux-setup.md#unattended)或[脱机](sql-server-linux-setup.md#offline)安装过程感兴趣，请参阅 [Linux 上的 SQL Server 的安装指南](sql-server-linux-setup.md)。

## <a name="prerequisites"></a>先决条件

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

必须拥有 SLES v12 SP2 计算机（内存至少为 2 GB）  。 文件系统必须是 XFS 或 EXT4   。 其他文件系统（如 BTRFS）均不受支持  。

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

必须拥有 SLES v12 SP2、SP3、SP4 或 SP5 计算机（内存至少为 2 GB）  。 文件系统必须是 XFS 或 EXT4   。 其他文件系统（如 BTRFS）均不受支持  。

::: moniker-end

若要在自己的计算机上安装 SUSE Linux Enterprise Server，请转到[https://www.suse.com/products/server](https://www.suse.com/products/server)。 也可以在 Azure 中创建 SLES 虚拟机。 请参阅 [使用 Azure CLI 创建和管理 Linux VM](/azure/virtual-machines/linux/tutorial-manage-vm)并在对 `az vm create` 的调用中使用 `--image SLES`。

如果以前安装了 SQL Server 的 CTP 或 RC 版本，则必须先删除旧存储库，然后再执行这些步骤。 有关详细信息，请参阅[为 SQL Server 2017 和 2019 配置 Linux 存储库](sql-server-linux-change-repo.md)。

> [!NOTE]
> 目前，不支持将适用于 Windows 10 的 [Linux 的 Windows 子系统](/windows/wsl/about)作为安装目标。

有关其他系统要求，请参阅 [Linux 上的 SQL Server 的系统要求](sql-server-linux-setup.md#system)。

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="install-sql-server-2017"></a><a id="install"></a>安装 SQL Server 2017

若要在 SLES 上配置 SQL Server 2017，请在终端运行以下命令来安装 mssql-server 包：

1. 下载 Microsoft SQL Server 2017 SLES 存储库配置文件：

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo
   ```

   > [!TIP]
   > 如果想安装 SQL Server 2019，必须改为注册 SQL Server 2019 存储库。 使用以下命令安装 SQL Server 2019：
   >
   > ```bash
   > sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019.repo
   > ```

2. 刷新存储库。

   ```bash
   sudo zypper --gpg-auto-import-keys refresh 
   ```
   
   若要确保你的系统上安装了 Microsoft 包签名密钥，请使用以下命令导入它： 
   
   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```
   
3. 运行以下命令以安装 SQL Server：

   ```bash
   sudo zypper install -y mssql-server
   ```

4. 包安装完成后，运行 **mssql-conf setup**，按照提示设置 SA 密码并选择版本。

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!TIP]
   > 以下 SQL Server 2017 版本是免费提供许可的：Evaluation、Developer 和 Express 版。

   > [!NOTE]
   > 请确保为 SA 帐户指定强密码（最少 8 个字符，包括大写和小写字母、十进制数字和/或非字母数字符号）。

5. 完成配置后，验证服务是否正在运行：

   ```bash
   systemctl status mssql-server
   ```

6. 如果计划远程连接，可能还需要在防火墙上打开 SQL Server TCP 端口（默认值为 1433）。 如果使用 SuSE 防火墙，需要编辑 /etc/sysconfig/SuSEfirewall2 配置文件  。 修改 FW_SERVICES_EXT_TCP 条目以包括 SQL Server 端口号  。

   ```
   FW_SERVICES_EXT_TCP="1433"
   ```

此时，SQL Server 在 SLES 计算机上运行，随时可以使用！

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="install-sql-server-2019"></a><a id="install"></a>安装 SQL Server 2019

若要在 SLES 上配置 SQL Server 2019，请在终端运行以下命令来安装 mssql-server 包：

1. 下载 Microsoft SQL Server 2019 SLES 存储库配置文件：

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019.repo
   ```

2. 刷新存储库。

   ```bash
   sudo zypper --gpg-auto-import-keys refresh 
   ```

   若要确保你的系统上安装了 Microsoft 包签名密钥，请使用以下命令导入该密钥： 

   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```
   
3. 运行以下命令以安装 SQL Server：

   ```bash
   sudo zypper install -y mssql-server
   ```

4. 包安装完成后，运行 **mssql-conf setup**，按照提示设置 SA 密码并选择版本。

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!NOTE]
   > 请确保为 SA 帐户指定强密码（最少 8 个字符，包括大写和小写字母、十进制数字和/或非字母数字符号）。

5. 完成配置后，验证服务是否正在运行：

   ```bash
   systemctl status mssql-server
   ```

6. 如果计划远程连接，可能还需要在防火墙上打开 SQL Server TCP 端口（默认值为 1433）。 如果使用 SuSE 防火墙，需要编辑 /etc/sysconfig/SuSEfirewall2 配置文件  。 修改 FW_SERVICES_EXT_TCP 条目以包括 SQL Server 端口号  。

   ```
   FW_SERVICES_EXT_TCP="1433"
   ```

此时，SQL Server 2019 在 SLES 计算机上运行，随时可以使用！

::: moniker-end


## <a name="install-the-sql-server-command-line-tools"></a><a id="tools"></a>安装 SQL Server 命令行工具

若要创建数据库，则需要使用可在 SQL Server 上运行 Transact-SQL 语句的工具进行连接。 以下步骤将安装 SQL Server 命令行工具：[sqlcmd](../tools/sqlcmd-utility.md) 和 [bcp](../tools/bcp-utility.md)。

1. 将 Microsoft SQL Server 存储库添加到 Zypper。

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo 
   sudo zypper --gpg-auto-import-keys refresh
   ```

1. 使用 unixODBC 开发人员包安装 **mssql-tools**。 有关详细信息，请参阅[安装 Microsoft ODBC Driver for SQL Server (Linux)](../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)。

   ```bash
   sudo zypper install -y mssql-tools unixODBC-devel
   ```

1. 为方便起见，向 PATH 环境变量添加 `/opt/mssql-tools/bin/`。 这样可以在不指定完整路径的情况下运行这些工具。 运行以下命令以修改登录会话和交互式/非登录会话的路径：

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

[!INCLUDE [Connect, create, and query data](../includes/sql-linux-quickstart-connect-query.md)]
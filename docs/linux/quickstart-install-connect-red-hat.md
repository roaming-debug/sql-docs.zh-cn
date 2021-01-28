---
title: RHEL：在 Linux 上安装 SQL Server
titleSuffix: SQL Server
description: 本快速入门介绍如何在 Red Hat Enterprise Linux (RHEL) 上安装 SQL Server 2017 或 SQL Server 2019，然后使用 sqlcmd 创建和查询数据库。
author: VanMSFT
ms.custom: seo-lt-2019
ms.author: vanto
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 92503f59-96dc-4f6a-b1b0-d135c43e935e
ms.openlocfilehash: cb266632fccaceb4639c7fdf0cfbf5368aad2c80
ms.sourcegitcommit: 2bdf1f1ee88f4fe3e872227d025e965e95d1b2b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98711973"
---
# <a name="quickstart-install-sql-server-and-create-a-database-on-red-hat"></a>快速入门：在 Red Hat 上安装 SQL Server 并创建数据库

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

在本快速入门中，将在 Red Hat Enterprise Linux (RHEL) 上安装 SQL Server 2017 或 SQL Server 2019。 然后使用 sqlcmd 进行连接，创建第一个数据库并运行查询。

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

本快速入门介绍如何在 Red Hat Enterprise Linux (RHEL) 7/8 上安装 SQL Server 2019。 然后使用 sqlcmd 进行连接，创建第一个数据库并运行查询。

::: moniker-end

> [!TIP]
> 本教程需要用户输入和 Internet 连接。 如果对[无人参与](sql-server-linux-setup.md#unattended)或[脱机](sql-server-linux-setup.md#offline)安装过程感兴趣，请参阅 [Linux 上的 SQL Server 的安装指南](sql-server-linux-setup.md)。

## <a name="prerequisites"></a>先决条件

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

必须拥有 RHEL 7.3 - 7.8 或 8.0 - 8.3 计算机（内存至少为 2 GB）。

::: moniker-end

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

必须拥有 RHEL 7.3 - 7.8 或 8.0 - 8.3 计算机（内存至少为 2 GB）。

::: moniker-end

若要在自己的计算机上安装 Red Hat Enterprise Linux，请转至 [https://access.redhat.com/products/red-hat-enterprise-linux/evaluation](https://access.redhat.com/products/red-hat-enterprise-linux/evaluation)。 也可以在 Azure 中创建 RHEL 虚拟机。 请参阅 [使用 Azure CLI 创建和管理 Linux VM](/azure/virtual-machines/linux/tutorial-manage-vm)并在对 `az vm create` 的调用中使用 `--image RHEL`。

如果以前安装了 SQL Server 的 CTP 或 RC 版本，则必须先删除旧存储库，然后再执行这些步骤。 有关详细信息，请参阅[为 SQL Server 2017 和 2019 配置 Linux 存储库](sql-server-linux-change-repo.md)。

有关其他系统要求，请参阅 [Linux 上的 SQL Server 的系统要求](sql-server-linux-setup.md#system)。

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="install-sql-server"></a><a id="install"></a>安装 SQL Server

> [!NOTE]
> 自 CU20 起，SQL Server 2017 开始支持 RHEL 8。 以下用于 SQL Server 2017 的命令指向 RHEL 8 存储库。 RHEL 8 未预安装 SQL Server 所需的 python2。 在开始 SQL Server 的安装步骤之前，请执行以下命令，并验证是否选择了 python2 作为解释器：
>
> ```
> sudo alternatives --config python
> # If not configured, install python2 and openssl10 using the following commands: 
> sudo yum install python2
> sudo yum install compat-openssl10
> # Configure python2 as the default interpreter using this command: 
> sudo alternatives --config python
> ```
> 有关详细信息，请参阅以下博客，了解如何安装 python2 并将其配置为默认解释器： https://www.redhat.com/en/blog/installing-microsoft-sql-server-red-hat-enterprise-linux-8-beta 。
>

若要在 RHEL 上配置 SQL Server，请在终端中运行以下命令以安装 **mssql-server** 包：

1. 下载 Microsoft SQL Server 2017 Red Hat 存储库配置文件：

   对于 RHEL7：
   
   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo
   ```
   
   对于 RHEL8：
   
   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2017.repo
   ```

   > [!TIP]
   > 如果想安装 SQL Server 2019，必须改为注册 SQL Server 2019 存储库。 使用以下命令安装 SQL Server 2019：
   >
   > 对于 RHEL7：
   >
   > ```bash
   > sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2019.repo
   > ```
   >
   > 对于 RHEL8：
   >
   > ```bash
   > sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2019.repo
   > ```

2. 运行以下命令以安装 SQL Server：

   ```bash
   sudo yum install -y mssql-server
   ```

3. 包安装完成后，运行 **mssql-conf setup**，按照提示设置 SA 密码并选择版本。

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!TIP]
   > 以下 SQL Server 2017 版本是免费提供许可的：Evaluation、Developer 和 Express 版。

   > [!NOTE]
   > 请确保为 SA 帐户指定强密码（最少 8 个字符，包括大写和小写字母、十进制数字和/或非字母数字符号）。

4. 完成配置后，验证服务是否正在运行：

   ```bash
   systemctl status mssql-server
   ```

5. 若要允许远程连接，请在 RHEL 的防火墙上打开 SQL Server 端口。 默认的 SQL Server 端口为 TCP 1433。 如果为防火墙使用的是 **FirewallD**，则可以使用以下命令：

   ```bash
   sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
   sudo firewall-cmd --reload
   ```

此时，SQL Server 正在 RHEL 计算机上运行，随时可以使用！

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="install-sql-server"></a><a id="install"></a>安装 SQL Server

> [!NOTE]
> 以下用于 SQL Server 2019 的命令指向 RHEL 8 存储库。 RHEL 8 未预安装 SQL Server 所需的 python2。 在开始 SQL Server 的安装步骤之前，请执行以下命令，并验证是否选择了 python2 作为解释器： 
>
> ```
> sudo alternatives --config python
> # If not configured, install python2 and openssl10 using the following commands: 
> sudo yum install python2
> sudo yum install compat-openssl10
> # Configure python2 as the default interpreter using this command: 
> sudo alternatives --config python
> ``` 
> 
> 有关这些步骤的详细信息，请参阅以下博客，了解如何安装 python2 并将其配置为默认解释器： https://www.redhat.com/en/blog/installing-microsoft-sql-server-red-hat-enterprise-linux-8-beta 。
> 

若要在 RHEL 上配置 SQL Server，请在终端中运行以下命令以安装 **mssql-server** 包：

1. 下载 Microsoft SQL Server 2019 Red Hat 存储库配置文件：
   
   对于 RHEL7：
   
   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2019.repo
   ```
   
   对于 RHEL8：
   
   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2019.repo
   ```

2. 运行以下命令以安装 SQL Server：

   ```bash
   sudo yum install -y mssql-server
   ```

3. 包安装完成后，运行 **mssql-conf setup**，按照提示设置 SA 密码并选择版本。

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!NOTE]
   > 请确保为 SA 帐户指定强密码（最少 8 个字符，包括大写和小写字母、十进制数字和/或非字母数字符号）。

4. 完成配置后，验证服务是否正在运行：

   ```bash
   systemctl status mssql-server
   ```

5. 若要允许远程连接，请在 RHEL 的防火墙上打开 SQL Server 端口。 默认的 SQL Server 端口为 TCP 1433。 如果为防火墙使用的是 **FirewallD**，则可以使用以下命令：

   ```bash
   sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
   sudo firewall-cmd --reload
   ```

此时，SQL Server 2019 正在 RHEL 计算机上运行，随时可以使用！

::: moniker-end

## <a name="install-the-sql-server-command-line-tools"></a><a id="tools"></a>安装 SQL Server 命令行工具

若要创建数据库，则需要使用可在 SQL Server 上运行 Transact-SQL 语句的工具进行连接。 以下步骤将安装 SQL Server 命令行工具：[sqlcmd](../tools/sqlcmd-utility.md) 和 [bcp](../tools/bcp-utility.md)。

1. 下载 Microsoft Red Hat 存储库配置文件。

   对于 RHEL7：
   
   ```bash
   sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/7/prod.repo
   ```
   
   对于 RHEL8：
   
   ```bash
   sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/8/prod.repo
   ```

1. 如果安装了早期版本的 **mssql-tools**，请删除所有旧的 unixODBC 包。

   ```bash
   sudo yum remove unixODBC-utf16 unixODBC-utf16-devel
   ```

1. 运行以下命令，以使用 unixODBC 开发人员包安装 **mssql-tools**。 有关详细信息，请参阅[安装 Microsoft ODBC Driver for SQL Server (Linux)](../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)。

   ```bash
   sudo yum install -y mssql-tools unixODBC-devel
   ```

1. 为方便起见，向 PATH 环境变量添加 `/opt/mssql-tools/bin/`。 这样可以在不指定完整路径的情况下运行这些工具。 运行以下命令以修改登录会话和交互式/非登录会话的路径：

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

[!INCLUDE [Connect, create, and query data](../includes/sql-linux-quickstart-connect-query.md)]

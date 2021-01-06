---
title: Docker：为 Linux 上的 SQL Server 安装容器
description: 本快速入门介绍如何使用 Docker 来运行 SQL Server 2017 和 2019 容器映像。 然后使用 sqlcmd 创建并查询数据库。
ms.custom: seo-lt-2019, contperf-fy21q1
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.date: 09/07/2020
ms.topic: quickstart
ms.prod: sql
ms.technology: linux
ms.prod_service: linux
ms.assetid: 82737f18-f5d6-4dce-a255-688889fdde69
monikerRange: '>= sql-server-linux-2017 || >= sql-server-2017'
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: 615e3d2ec9c64be70535c9a45a9f31e94d34b139
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878761"
---
# <a name="quickstart-run-sql-server-container-images-with-docker"></a>快速入门：使用 Docker 运行 SQL Server 容器映像
[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

> [!NOTE]
> 下面显示的示例使用 docker.exe，但其中的大多数命令也使用 Podman。 它提供与 Docker 容器引擎类似的 CLI。 可以在[此处](http://docs.podman.io/en/latest)详细了解 Podman。

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

在本快速入门教程中，你使用 Docker 请求和运行 SQL Server 2017 容器映像 [mssql server linux](https://hub.docker.com/_/microsoft-mssql-server)。 然后使用 **sqlcmd** 连接，以创建第一个数据库并运行查询。

> [!TIP]
> 如果要运行 SQL Server 2019 容器，请参阅[本文的 SQL Server 2019 版本](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true)。

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

> [!NOTE]
> 自 SQL Server 2019 CU3 起，支持 Ubuntu 18.04。

在本快速入门中，请使用 Docker 拉取和运行 SQL Server 2019 容器映像 [mssql-server](https://hub.docker.com/r/microsoft/mssql-server)。 然后使用 **sqlcmd** 连接，以创建第一个数据库并运行查询。

> [!TIP]
> 本快速入门将创建 SQL Server 2019 容器。 如果希望创建 SQL Server 2017 容器，请参阅[本文的 SQL Server 2017 版本](quickstart-install-connect-docker.md?view=sql-server-linux-2017&preserve-view=true)。

::: moniker-end

此映像包含在基于 Ubuntu 18.04 的 Linux 上运行的 SQL Server。 它可与 Linux 上或用于 Mac/Windows 的 Docker 上的 Docker 引擎 1.8+ 配合使用。 本快速入门专门介绍 **Linux** 上 SQL Server 映像的使用。 虽然未介绍 Windows 映像，但可在 [mssql-server-windows-developer Docker 中心页](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/)上找到关于它的详细信息。

## <a name="prerequisites"></a><a id="requirements"></a>先决条件

- 任何受支持的 Linux 分发或用于 Mac/Windows 的 Docker 上的 Docker 引擎 1.8+。 有关详细信息，请参阅 [Install Docker](https://docs.docker.com/engine/installation/)（安装 Docker）。
- Docker **overlay2** 存储驱动程序。 这是大多数用户的默认设置。 如果发现自己未使用此存储提供程序并且需要进行更改，请参阅 [docker 文档](https://docs.docker.com/storage/storagedriver/overlayfs-driver/#configure-docker-with-the-overlay-or-overlay2-storage-driver)中有关配置 overlay2 的说明和警告。
- 至少 2 GB 的磁盘空间。
- 至少 2 GB 的 RAM。
- [Linux 上的 SQL Server 的系统要求](sql-server-linux-setup.md#system)。

<!--The following H2 is versioned for 2017 and 2019. Much of the content is duplicated, so
any changes to one section should be duplicated in the other-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="pull-and-run-the-2017-container-image"></a><a id="pullandrun2017"></a> 拉取并运行 2017 容器映像

在开始执行以下步骤之前，请确保已在本文顶部选择了首选的 shell（bash、PowerShell 或 cmd）。

1. 从 Microsoft 容器注册表中拉取 SQL Server 2017 Linux 容器映像。

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   > [!TIP]
   > 如果要运行 SQL Server 2019 容器，请参阅[本文的 SQL Server 2019 版本](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true#pullandrun2019)。

   前一个命令请求最新的 SQL Server 2017 容器映像。 如果想请求某个特定映像，需添加一个冒号和标记名称（例如 `mcr.microsoft.com/mssql/server:2017-GA-ubuntu`。 若要查看所有可用映像，请参阅 [mssql-server Docker hub 页](https://hub.docker.com/r/microsoft/mssql-server)。

   ::: zone pivot="cs1-bash"
   对于本文中的 bash 命令，将使用 `sudo`。 在 MacOS 上，可能不需要 `sudo`。 在 Linux 上，如果不想使用 `sudo` 来运行 Docker，可以配置一个 **docker** 组，并将用户添加到该组。 有关详细信息，请参阅 [Post-installation steps for Linux](https://docs.docker.com/install/linux/linux-postinstall/)（适用于 Linux 的安装后步骤）。

   ::: zone-end

2. 要使用 Docker 运行容器映像，可以从 Bash Shell (Linux/macOS) 或提升的 PowerShell 命令提示符使用以下命令。

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" \
      -p 1433:1433 --name sql1 -h sql1 \
      -d \
      mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   
   > [!NOTE]
   > 如果使用 PowerShell Core，请将双引号替换为单引号。
   
   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d `
      mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d `
      mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   > [!NOTE]
   > 密码应符合 SQL Server 默认密码策略，否则容器无法设置 SQL Server，将停止工作。 默认情况下，密码的长度必须至少为 8 个字符，并且必须包含以下四种字符中的三种：大写字母、小写字母、十进制数字和符号。 你可以通过执行 [docker logs](https://docs.docker.com/engine/reference/commandline/logs/) 命令检查错误日志。
   >
   > 默认情况下，这会创建一个使用 SQL Server 2017 开发人员版的容器。 在容器中运行生产版本的过程略有不同。 有关详细信息，请参阅[运行生产容器映像](./sql-server-linux-docker-container-deployment.md#production)。

   下表对前一个 `docker run` 示例中的参数进行了说明：

   | 参数 | 说明 |
   |-----|-----|
   | **-e "ACCEPT_EULA=Y"** |  将 **ACCEPT_EULA** 变量设置为任意值，以确认接受 [最终用户许可协议](https://go.microsoft.com/fwlink/?linkid=857698)。 SQL Server 映像的必需设置。 |
   | -e "SA_PASSWORD=\<YourStrong@Passw0rd\>" | 指定至少包含 8 个字符且符合 [SQL Server 密码要求](../relational-databases/security/password-policy.md)的强密码。 SQL Server 映像的必需设置。 |
   | **-p 1433:1433** | 将主机环境中的 TCP 端口（第一个值）映射到容器中的 TCP 端口（第二个值）。 在此示例中，SQL Server 侦听容器中的 TCP 1433，并对主机上的端口 1433 公开。 |
   | **--name sql1** | 为容器指定一个自定义名称，而不是使用随机生成的名称。 如果运行多个容器，则无法重复使用相同的名称。 |
   | -h sql1 | 用于显式设置容器主机名，如果不指定它，则默认为容器 ID，该 ID 是随机生成的系统 GUID。 |
   | **-d** | 在后台运行容器（守护程序） |
   | **mcr.microsoft.com/mssql/server:2017-latest** | SQL Server 2017 Linux 容器映像。 |

3. 要查看 Docker 容器，请使用 `docker ps` 命令。


   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker ps -a
   ```
   ::: zone-end

   应会看到与以下屏幕截图类似的输出：

   ![Docker ps 命令输出](./media/sql-server-linux-setup-docker/docker-ps-command.png)

4. 如果“状态”列显示“正常运行”，则 SQL Server 将在容器中运行，并侦听“端口”列中指定的端口  。 如果 SQL Server 容器的“状态”列显示“已退出”，则参阅[配置指南的疑难解答部分](./sql-server-linux-docker-container-troubleshooting.md) 。

如上所述的 `-h`（主机名）参数将容器的内部名称更改为自定义值。 也就是以下 Transact-SQL 查询中返回的名称：

```sql
SELECT @@SERVERNAME,
    SERVERPROPERTY('ComputerNamePhysicalNetBIOS'),
    SERVERPROPERTY('MachineName'),
    SERVERPROPERTY('ServerName')
```

将 `-h` 和 `--name` 设为相同的值是一种很好的方法，可以轻松地识别目标容器。

5. 最后一步，更改 SA 密码，因为 `SA_PASSWORD` 在 `ps -eax` 输出中可见，并存储在同名的环境变量中。 请参阅以下步骤。

::: moniker-end
<!--End of 2017 "Pull and run" section-->

<!--This is the 2019 version of the "Pull and run" section-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="pull-and-run-the-2019-container-image"></a><a id="pullandrun2019"></a> 拉取并运行 2019 容器映像

在开始执行以下步骤之前，请确保已在本文顶部选择了首选的 shell（bash、PowerShell 或 cmd）。

1. 从 Microsoft 容器注册表中拉取 SQL Server 2019 Linux 容器映像。

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   
   > [!NOTE]
   > 如果使用 PowerShell Core，请将双引号替换为单引号。
   
   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker pull mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   > [!TIP]
   > 本快速入门使用 SQL Server 2019 Docker 映像。 若要运行 SQL Server 2017 映像，请参阅[本文的 SQL Server 2017 版本](quickstart-install-connect-docker.md?view=sql-server-linux-2017&preserve-view=true#pullandrun2017)。

   前一命令拉取基于 Ubuntu 的 SQL Server 2019 容器映像。 若要改为使用基于 RedHat 的容器映像，请参阅[运行基于 RHEL 的容器映像](./sql-server-linux-docker-container-deployment.md#rhel)。 要查看所有可用映像，请参阅 [mssql-server-linux Docker 中心页](https://hub.docker.com/_/microsoft-mssql-server)。

   ::: zone pivot="cs1-bash"
   对于本文中的 bash 命令，将使用 `sudo`。 在 MacOS 上，可能不需要 `sudo`。 在 Linux 上，如果不想使用 `sudo` 来运行 Docker，可以配置一个 **docker** 组，并将用户添加到该组。 有关详细信息，请参阅 [Post-installation steps for Linux](https://docs.docker.com/install/linux/linux-postinstall/)（适用于 Linux 的安装后步骤）。
   ::: zone-end

2. 要使用 Docker 运行容器映像，可以从 Bash Shell (Linux/macOS) 或提升的 PowerShell 命令提示符使用以下命令。

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" \
      -p 1433:1433 --name sql1 -h sql1 \
      -d mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   > [!NOTE]
   > 密码应符合 SQL Server 默认密码策略，否则容器无法设置 SQL Server，将停止工作。 默认情况下，密码的长度必须至少为 8 个字符，并且必须包含以下四种字符中的三种：大写字母、小写字母、十进制数字和符号。 你可以通过执行 [docker logs](https://docs.docker.com/engine/reference/commandline/logs/) 命令检查错误日志。
   >
   > 默认情况下，这会创建一个使用 SQL Server 2019 开发人员版的容器。

   下表对前一个 `docker run` 示例中的参数进行了说明：

   | 参数 | 说明 |
   |-----|-----|
   | **-e "ACCEPT_EULA=Y"** |  将 **ACCEPT_EULA** 变量设置为任意值，以确认接受 [最终用户许可协议](https://go.microsoft.com/fwlink/?LinkId=746388)。 SQL Server 映像的必需设置。 |
   | -e "SA_PASSWORD=\<YourStrong@Passw0rd\>" | 指定至少包含 8 个字符且符合 [SQL Server 密码要求](../relational-databases/security/password-policy.md)的强密码。 SQL Server 映像的必需设置。 |
   | **-p 1433:1433** | 将主机环境中的 TCP 端口（第一个值）映射到容器中的 TCP 端口（第二个值）。 在此示例中，SQL Server 侦听容器中的 TCP 1433，并对主机上的端口 1433 公开。 |
   | **--name sql1** | 为容器指定一个自定义名称，而不是使用随机生成的名称。 如果运行多个容器，则无法重复使用相同的名称。 |
   | -h sql1 | 用于显式设置容器主机名，如果不指定它，则默认为容器 ID，该 ID 是随机生成的系统 GUID。 |
   | **mcr.microsoft.com/mssql/server:2019-latest** | SQL Server 2019 Ubuntu Linux 容器映像。 |

3. 要查看 Docker 容器，请使用 `docker ps` 命令。

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker ps -a
   ```
   ::: zone-end

   应会看到与以下屏幕截图类似的输出：

   ![Docker ps 命令输出](./media/sql-server-linux-setup-docker/docker-ps-command.png)

4. 如果“状态”列显示“正常运行”，则 SQL Server 将在容器中运行，并侦听“端口”列中指定的端口  。 如果 SQL Server 容器的“状态”列显示“已退出”，请参阅 [SQL Server Docker 容器故障排除](sql-server-linux-docker-container-troubleshooting.md) 。

如上所述的 `-h`（主机名）参数将容器的内部名称更改为自定义值。 这会将容器的内部名称更改为一个自定义值。 也就是以下 Transact-SQL 查询中返回的名称：

```sql
SELECT @@SERVERNAME,
    SERVERPROPERTY('ComputerNamePhysicalNetBIOS'),
    SERVERPROPERTY('MachineName'),
    SERVERPROPERTY('ServerName')
```

将 `-h` 和 `--name` 设为相同的值是一种很好的方法，可以轻松地识别目标容器。


5. 最后一步，更改 SA 密码，因为 `SA_PASSWORD` 在 `ps -eax` 输出中可见，并存储在同名的环境变量中。 请参阅以下步骤。


::: moniker-end
<!--End of 2019 "Pull and run" section-->



## <a name="change-the-sa-password"></a><a id="sapassword"></a> 更改 SA 密码

<!-- This section was pasted in from includes/sql-server-linux-change-docker-password.md, to better support zone pivots. 2019/02/11 -->

SA  帐户是安装过程中在 SQL Server 实例上创建的系统管理员。 创建 SQL Server 容器后，通过在容器中运行 `echo $SA_PASSWORD`，可发现指定的 `SA_PASSWORD` 环境变量。 出于安全考虑，请考虑更改 SA 密码。

1. 选择 SA 用户要使用的强密码。

1. 使用 `docker exec` 运行sqlcmd，以使用 Transact-SQL 更改密码。 在下面的示例中，将旧密码 `<YourStrong!Passw0rd>` 和新密码 `<YourNewStrong!Passw0rd>` 替换为你自己的密码值。

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P "<YourStrong@Passw0rd>" \
      -Q 'ALTER LOGIN SA WITH PASSWORD="<YourNewStrong@Passw0rd>"'
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourStrong@Passw0rd>" `
      -Q "ALTER LOGIN SA WITH PASSWORD='<YourNewStrong@Passw0rd>'"
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourStrong!Passw0rd>" `
      -Q "ALTER LOGIN SA WITH PASSWORD='<YourNewStrong@Passw0rd>'"
   ```
   ::: zone-end

## <a name="connect-to-sql-server"></a>连接到 SQL Server

下列步骤在容器内部使用 SQL Server 命令行工具 [sqlcmd](../tools/sqlcmd-utility.md) 来连接 SQL Server。

1. 使用 `docker exec -it` 命令在运行的容器内部启动交互式 Bash Shell。 在下面的示例中，`sql1` 是在创建容器时由 `--name` 参数指定的名称。

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker exec -it sql1 "bash"
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker exec -it sql1 "bash"
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker exec -it sql1 "bash"
   ```
   ::: zone-end

2. 在容器内部使用 sqlcmd 进行本地连接。 默认情况下，sqlcmd 不在路径之中，因此需要指定完整路径。

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "<YourNewStrong@Passw0rd>"
   ```

   > [!TIP]
   > 可以省略命令行上提示要输入的密码。

3. 如果成功，应会显示 sqlcmd 命令提示符：`1>`。

## <a name="create-and-query-data"></a>创建和查询数据

以下部分将引导你使用 sqlcmd 和 Transact-SQL 完成新建数据库、添加数据并运行查询的整个过程。

### <a name="create-a-new-database"></a>新建数据库

以下步骤创建一个名为 `TestDB` 的新数据库。

1. 在 sqlcmd 命令提示符中，粘贴以下 Transact-SQL 命令以创建测试数据库：

   ```sql
   CREATE DATABASE TestDB
   ```

2. 在下一行中，编写一个查询以返回服务器上所有数据库的名称：

   ```sql
   SELECT Name from sys.Databases
   ```

3. 前两个命令没有立即执行。 在新行中键入 `GO` 以执行以前的命令：

   ```sql
   GO
   ```

### <a name="insert-data"></a>插入数据

接下来创建一个新表 `Inventory`，然后插入两个新行。

1. 在 sqlcmd 命令提示符中，将上下文切换到新的 `TestDB` 数据库：

   ```sql
   USE TestDB
   ```

2. 创建名为 `Inventory` 的新表：

   ```sql
   CREATE TABLE Inventory (id INT, name NVARCHAR(50), quantity INT)
   ```

3. 将数据插入新表：

   ```sql
   INSERT INTO Inventory VALUES (1, 'banana', 150); INSERT INTO Inventory VALUES (2, 'orange', 154);
   ```

4. 要执行上述命令的类型 `GO`：

   ```sql
   GO
   ```

### <a name="select-data"></a>选择数据

现在，运行查询以从 `Inventory` 表返回数据。

1. 通过 sqlcmd 命令提示符输入查询，以返回 `Inventory` 表中数量大于 152 的行：

   ```sql
   SELECT * FROM Inventory WHERE quantity > 152;
   ```

2. 执行此命令：

   ```sql
   GO
   ```

### <a name="exit-the-sqlcmd-command-prompt"></a>退出 sqlcmd 命令提示符

1. 要结束 sqlcmd 会话，请键入 `QUIT`：

   ```sql
   QUIT
   ```

2. 要在容器中退出交互式命令提示，请键入 `exit`。 退出交互式 Bash Shell 后，容器将继续运行。

## <a name="connect-from-outside-the-container"></a><a id="connectexternal"></a> 从容器外连接

还可以从支持 SQL 连接的任何 Linux、Windows 或 macOS 外部工具连接到 Docker 计算机上的 SQL Server 实例。

以下步骤在容器外使用 **sqlcmd** 连接在容器中运行的 SQL Server。 这些步骤假定你已在容器外安装了 SQL Server 命令行工具。 使用其他工具时，同样的原则依然适用，但连接过程因工具而异。

1. 查找承载容器的计算机的 IP 地址。 在 Linux 上，使用 **ifconfig** 或 **ip addr**。在 Windows 上，使用 **ipconfig**。

1. 对于本示例，请在客户端计算机上安装 **sqlcmd** 工具。 有关详细信息，请参阅[在 Windows 上安装 sqlcmd](../tools/sqlcmd-utility.md) 或[在 Linux 上安装 sqlcmd](sql-server-linux-setup-tools.md)。

1. 运行 sqlcmd，指定 IP 地址和映射容器中的端口 1433 的端口。 本例中为主机上的相同端口 1433。 如果在主机上指定了不同的映射端口，则在此处使用它。 还需要在防火墙上打开相应的入站端口以允许连接。

   ::: zone pivot="cs1-bash"
   ```bash
   sqlcmd -S <ip_address>,1433 -U SA -P "<YourNewStrong@Passw0rd>"
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   sqlcmd -S <ip_address>,1433 -U SA -P "<YourNewStrong@Passw0rd>"
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   sqlcmd -S <ip_address>,1433 -U SA -P "<YourNewStrong@Passw0rd>"
   ```
   ::: zone-end

1. 运行 Transact-SQL 命令。 完成后，键入 `QUIT`。

连接到 SQL Server 的其他常见工具包括：

- [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)
- [适用于 Windows 的 SQL Server Management Studio (SSMS)](sql-server-linux-manage-ssms.md)
- [Azure Data Studio](../azure-data-studio/what-is.md)
- [mssql-cli（预览版）](https://github.com/dbcli/mssql-cli/blob/master/doc/usage_guide.md)
- [PowerShell Core](sql-server-linux-manage-powershell-core.md)

## <a name="remove-your-container"></a>删除容器

如果想删除本教程中使用的 SQL Server 容器，请运行以下命令：

::: zone pivot="cs1-bash"
```bash
sudo docker stop sql1
sudo docker rm sql1
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker stop sql1
docker rm sql1
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker stop sql1
docker rm sql1
```
::: zone-end

> [!WARNING]
> 停止并永久删除容器会删除容器中的所有 SQL Server 数据。 如果你需要保留数据，请[在容器外创建并复制备份文件](tutorial-restore-backup-in-sql-server-container.md)或使用[容器数据暂留技术](sql-server-linux-docker-container-configure.md#persist)。

## <a name="docker-demo"></a>Docker 演示

尝试对 Docker 使用 SQL Server 容器映像后，你可能想知道如何 Docker 是如何用于改进开发和测试的。 下面的视频介绍如何在持续集成和部署方案中使用 Docker。

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T152/player]

## <a name="next-steps"></a>后续步骤

有关如何将数据库备份文件还原到容器中的教程，请参阅[在 Linux Docker 容器中还原 SQL Server 数据库](tutorial-restore-backup-in-sql-server-container.md)。 浏览其他方案，例如运行[多个容器](sql-server-linux-docker-container-deployment.md#multiple)、[数据暂留](sql-server-linux-docker-container-configure.md#persist)和[故障排除](sql-server-linux-docker-container-troubleshooting.md)。

并且，请查看 [mssql-docker GitHub 存储库](https://github.com/Microsoft/mssql-docker)，了解资源、反馈和已知问题。

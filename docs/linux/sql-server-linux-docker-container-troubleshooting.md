---
title: SQL Server Docker 容器故障排除
description: 了解不同的故障排除方法，你可使用这些方法来解决在将 Linux Docker 容器与 SQL Server 映像配合使用时出现的常见错误
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperfq1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017'
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: 051dbe0d44cbd798653632df114beb6727f1c9af
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489817"
---
# <a name="troubleshooting-sql-server-docker-containers"></a>SQL Server Docker 容器故障排除

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍了在部署和使用 SQL Server Docker 容器时出现的常见错误，并提供了故障排除方法来帮助解决问题。

## <a name="docker-command-errors"></a>Docker 命令错误

如果遇到任何 `docker` 命令错误，请确保 Docker 服务正在运行，并尝试通过提升的权限运行。

例如，在 Linux 上运行 `docker` 命令时可能会遇到以下错误：

```output
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```

如果在 Linux 上遇到此错误，请尝试对相同的命令加上 `sudo` 前缀，然后再运行。 如果失败，请验证 Docker 服务是否正在运行；如果没有，请启动它。

```bash
sudo systemctl status docker
sudo systemctl start docker
```

在 Windows 上，验证并确保以管理员身份启动 PowerShell 或命令提示符。

## <a name="sql-server-container-startup-errors"></a>SQL Server 容器启动错误

如果 SQL Server 容器无法运行，请尝试以下测试：

- 如果收到 `failed to create endpoint CONTAINER_NAME on network bridge. Error starting proxy: listen tcp 0.0.0.0:1433 bind: address already in use.` 之类的错误，则尝试将容器端口 1433 映射到已在使用的端口。 在主机上本地运行 SQL Server 时可能发生此错误。 如果启动两个 SQL Server 容器，并尝试将它们映射到同一主机端口，也可能发生此错误。 如果发生此错误，请使用 `-p` 参数将容器端口 1433 映射到不同的主机端口。 例如： 

    <!--SQL Server 2017 on Linux -->
    ::: moniker range="= sql-server-linux-2017 || = sql-server-2017"
    
    ::: zone pivot="cs1-bash"
    ```bash
    docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-powershell"
    ```PowerShell
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-cmd"
    ```cmd
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: moniker-end
    
    <!--SQL Server 2019 on Linux-->
    ::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15"
    
    ::: zone pivot="cs1-bash"
    ```bash
    docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-powershell"
    ```PowerShell
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-cmd"
    ```cmd
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: moniker-end

- 如果在尝试启动容器时收到 `Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.30tdout=1&tail=all: dial unix /var/run/docker.sock: connect: permission denied` 之类的错误，请将用户添加到 Ubuntu 中的 Docker 组。 然后注销并重新登录，因为此更改将影响新会话。 

   ```bash
    usermod -aG docker $USER
   ```

- 检查是否有任何来自容器的错误消息。

   ```bash
   docker logs e69e056c702d
   ```

- 确保符合快速入门文章的[先决条件](quickstart-install-connect-docker.md#requirements)部分中指定的最低内存和磁盘要求。

- 如果使用任何容器管理软件，请确保它支持以根身份运行的容器进程。 容器中的 sqlservr 进程以根身份运行。

- 如果 SQL Server Docker 容器在启动后立即退出，请检查 Docker 日志。 如果通过 `docker run` 命令使用 Windows 上的 PowerShell，请使用双引号而不是单引号。 对于 PowerShell Core 时，请使用单引号。

- 查看 [SQL Server 安装和错误日志](#errorlogs)。

## <a name="enable-dump-captures"></a>启用转储捕获

如果 SQL Server 进程在容器内失败，则应创建一个启用了 **SYS_PTRACE** 的新容器。 此操作可添加用于跟踪进程的 Linux 功能，该功能是在出现异常时创建转储文件所必需的。 支持人员可以使用转储文件来帮助对问题进行故障排除。 以下 docker run 命令可启用此功能。

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -e 'MSSQL_PID=Developer' --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -e 'MSSQL_PID=Developer' --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

## <a name="sql-server-connection-failures"></a>SQL Server 连接故障

如果无法连接到在容器中运行的 SQL Server 实例，请尝试以下测试：

- 通过查看 `docker ps -a` 输出的“状态”列，确保 SQL Server 容器正在运行。 如果未在运行，则使用 `docker start <Container ID>` 启动它。

- 如果映射到非默认主机端口（非 1433），请确保在连接字符串中指定端口。 可在 `docker ps -a` 输出的“端口”列中查看端口映射。 例如，以下命令将 sqlcmd 连接至正在侦听端口 1401 的容器：

    ::: zone pivot="cs1-bash"
    ```bash
    sqlcmd -S 10.3.2.4,1401 -U SA -P '<YourPassword>'
    ```
    ::: zone-end

    ::: zone pivot="cs1-powershell"
    ```PowerShell
    sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
    ```
    ::: zone-end

    ::: zone pivot="cs1-cmd"
    ```cmd
    sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
    ```
    ::: zone-end

- 如果将 `docker run` 和现有的映射数据卷或数据卷容器配合使用，SQL Server 会忽略 `SA_PASSWORD` 的值。 改为使用来自 SQL Server 数据（数据卷或数据卷容器中）的预配置 SA 用户密码。 验证所使用的 SA 密码是否与附加到的数据相关联。

- 查看 [SQL Server 安装和错误日志](#errorlogs)。

## <a name="sql-server-availability-groups"></a>SQL Server 可用性组

如果将 Docker 与 SQL Server 可用性组配合使用，则还有两个附加要求。

- 映射用于副本通信的端口（默认为 5022）。 例如，将 `-p 5022:5022` 指定为 `docker run` 命令的一部分。

- 使用 `docker run` 命令的 `-h YOURHOSTNAME` 参数显式设置容器主机名。 配置可用性组时会使用此主机名。 如果不使用 `-h` 来指定主机名，它会默认使用容器 ID。

## <a name="sql-server-setup-and-error-logs"></a><a id="errorlogs"></a> SQL Server 安装和错误日志

可在 **/var/opt/mssql/log** 中查看 SQL Server 安装和错误日志。 如果容器未在运行，请先启动它。 然后使用交互式命令提示符来检查日志。 可以通过运行 `docker ps` 命令来获取容器 ID。

```bash
docker start <ContainerID>
docker exec -it <ContainerID> "bash"
```

从容器内的 bash 会话运行以下命令：

```bash
cd /var/opt/mssql/log
cat setup*.log
cat errorlog
```

> [!TIP]
> 如果在创建容器时将主机目录装载到了 **/var/opt/mssql**，可改为在主机上映射路径的 **log** 子目录中进行查找。

## <a name="execute-commands-in-a-container"></a>在容器中执行命令

如果正在运行容器，则可从主机终端在容器内部执行命令。

若要获取容器 ID，请运行：

```bash
docker ps -a
```

若要在容器中启动 Bash 终端，请运行：

```bash
docker exec -it <Container ID> /bin/bash
```

现在，可以像在容器内的终端上那样运行这些命令。 完成后，键入 `exit`。 这将退出交互式命令会话，但容器会继续运行。

## <a name="next-steps"></a>后续步骤

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- 通过查看[快速入门](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)，开始在 Docker 上使用 SQL Server 2017 容器映像。

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- 通过查看[快速入门](quickstart-install-connect-docker.md?view=sql-server-ver15)，开始在 Docker 上使用 SQL Server 2019 容器映像。

::: moniker-end

- [部署并连接到 SQL Server Docker 容器](sql-server-linux-docker-container-deployment.md)

- [引用 Docker 容器的其他配置和自定义](sql-server-linux-docker-container-configure.md)

- [保护 SQL Server Docker 容器](sql-server-linux-docker-container-security.md)

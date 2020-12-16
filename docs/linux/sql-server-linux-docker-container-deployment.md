---
title: 部署并连接到 SQL Server Docker 容器
description: 了解如何在 Docker 容器上部署 SQL Server，并了解用于从容器内部和外部连接到 SQL Server 的各种工具
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperfq1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 82737f18-f5d6-4dce-a255-688889fdde69
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: 6fbf5782ff67b3406cffad808b27c47112a48d97
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489857"
---
# <a name="deploy-and-connect-to-sql-server-docker-containers"></a>部署并连接到 SQL Server Docker 容器

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍如何部署和连接到 SQL Server Docker 容器。

有关其他部署方案，请参阅：

- [Windows](../database-engine/install-windows/install-sql-server.md)
- [Linux](../linux/sql-server-linux-setup.md)
- [Kubernetes - 大数据群集](../big-data-cluster/deploy-get-started.md)

> [!NOTE]
> 本文专门重点介绍 mssql-server-linux 映像的使用。 虽然没有介绍 Windows 映像，但可在 [mssql-server-windows Docker Hub 页](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/)上找到关于它的详细信息。

> [!IMPORTANT]
> 在选择运行 SQL Server 容器以用于生产用例之前，请查看 [SQL Server 容器的支持策略](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server)，以确保在支持的配置上运行。

本视频时长 6 分钟，介绍了如何在容器上运行 SQL Server：

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2019-in-Containers/player?WT.mc_id=dataexposed-c9-niner]

## <a name="pull-and-run-the-container-image"></a>拉取并运行容器映像

若要拉取并运行 SQL Server 2017 和 SQL Server 2019 的 Docker 容器映像，请按照以下快速入门中的先决条件和步骤执行操作：

- [使用 Docker 运行 SQL Server 2017 容器映像](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)
- [使用 Docker 运行 SQL Server 2019 容器映像](quickstart-install-connect-docker.md?view=sql-server-ver15&preserve-view=true)

本配置文章在以下部分中提供其他使用方案。

## <a name="connect-and-query"></a>连接和查询

可从容器外部或内部对容器中的 SQL Server 进行连接和查询。 以下部分介绍这两种方案。

### <a name="tools-outside-the-container"></a>容器外的工具

可从支持 SQL 连接的任何 Linux、Windows 或 macOS 外部工具连接到 Docker 计算机上的 SQL Server 实例。 一些常用工具包括：

- [Azure Data Studio](../azure-data-studio/quickstart-sql-server.md)
- [sqlcmd](sql-server-linux-setup-tools.md)
- [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)
- [适用于 Windows 的 SQL Server Management Studio (SSMS)](sql-server-linux-manage-ssms.md)

以下示例使用 **sqlcmd** 连接到在 Docker 容器中运行的 SQL Server。 连接字符串中的 IP 地址为运行容器的主机的 IP 地址。

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4 -U SA -P "<YourPassword>"
```
::: zone-end

如果映射的主机端口不是默认的 **1433**，请将该端口添加到连接字符串中。 例如，如果在 `docker run` 命令中指定了 `-p 1400:1433`，则请通过显式指定端口 1400 来进行连接。

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4,1400 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4,1400 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4,1400 -U SA -P "<YourPassword>"
```
::: zone-end

### <a name="tools-inside-the-container"></a>容器内的工具

从 SQL Server 2017 开始，容器映像中加入了 [SQL Server 命令行工具](sql-server-linux-setup-tools.md)。 如果使用交互式命令提示符附加至此映像，则可在本地运行工具。

1. 使用 `docker exec -it` 命令在运行的容器内部启动交互式 Bash Shell。 在以下示例中，`e69e056c702d` 是容器 ID。

    ```bash
    docker exec -it e69e056c702d "bash"
    ```

    > [!TIP]
    > 并非始终需要指定完整的容器 ID。 只需指定能够唯一标识它的足够字符即可。 因此，在本示例中，使用 `e6` 或 `e69` 足矣，无需使用完整 ID。 若要查找容器 ID，请运行命令 `docker ps -a`。

2. 在容器内部使用 sqlcmd 进行本地连接。 默认情况下，sqlcmd 不在路径之中，因此需要指定完整路径。

    ```bash
    /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '<YourPassword>'
    ```

3. 使用 sqlcmd 完成操作后，键入 `exit`。

4. 使用交互式命令提示符完成操作后，键入 `exit`。 退出交互式 Bash Shell 后，容器将继续运行。

## <a name="check-the-container-version"></a><a id="version"></a> 检查容器版本

如果想要了解正在运行的 docker 容器中的 SQL Server 的版本，请运行以下命令以显示它。 将 `<Container ID or name>` 替换为目标容器 ID 或名称。 将 `<YourStrong!Passw0rd>` 替换为 SA 登录名的 SQL Server 密码。

::: zone pivot="cs1-bash"
```bash
sudo docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd \
-S localhost -U SA -P '<YourStrong!Passw0rd>' \
-Q 'SELECT @@VERSION'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd `
-S localhost -U SA -P "<YourStrong!Passw0rd>" `
-Q "SELECT @@VERSION"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd ^
-S localhost -U SA -P "<YourStrong!Passw0rd>" ^
-Q "SELECT @@VERSION"
```
::: zone-end

还可以标识目标 docker 容器映像的 SQL Server 版本和生成号。 以下命令显示 mcr.microsoft.com/mssql/server:2017-latest 映像的 SQL Server 版本和生成信息  。 它通过运行具有环境变量 **PAL_PROGRAM_INFO=1** 的新容器来实现此目的。 生成的容器会立即退出，且 `docker rm` 命令会将其删除。

::: zone pivot="cs1-bash"
```bash
sudo docker run -e PAL_PROGRAM_INFO=1 --name sqlver \
-ti mcr.microsoft.com/mssql/server:2019-latest && \
sudo docker rm sqlver
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e PAL_PROGRAM_INFO=1 --name sqlver `
-ti mcr.microsoft.com/mssql/server:2019-latest; `
docker rm sqlver
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e PAL_PROGRAM_INFO=1 --name sqlver ^
-ti mcr.microsoft.com/mssql/server:2019-latest && ^
docker rm sqlver
```
::: zone-end

上一个命令显示类似以下输出的版本信息：

```output
sqlservr
  Version 15.0.4063.15
  Build ID 8a3bb4cca325e1d0b3071b3a193f6a1d74b440fbd95d2fb18881651a5b9ec8e8
  Build Type release
  Git Version 0335c462
  Built at Fri Aug 28 04:50:27 GMT 2020

PAL
  Build ID cc5ceea1b3d294f7d0166f99932f98c7eacfaaa81bcd7cf23c6a89f785829b63
  Build Type release
  Git Version ae9d66dff
  Built at Fri Aug 28 04:46:48 GMT 2020

Packages
  system.security                         6.2.9200.10,unset,
  system.certificates                     6.2.9200.10,unset,
  secforwarderxplat                       15.0.4063.15
  sqlservr                                15.0.4063.15
  system.common                           10.0.17134.1246.202005133
  system.netfx                            4.7.2.461814
  system                                  6.2.9200.10,unset,
  sqlagent                                15.0.4063.15
```

## <a name="run-a-specific-sql-server-container-image"></a><a id="tags"></a> 运行特定 SQL Server 容器映像

> [!NOTE]
> 自 SQL Server 2019 CU3 起，支持 Ubuntu 18.04。 可以在 <https://mcr.microsoft.com/v2/mssql/server/tags/list> 检索 mssql/server 所有可用标志的列表。

在某些情况下，可能不希望使用最新的 SQL Server 容器映像。 若要运行特定 SQL Server 容器映像，请使用以下步骤：

1. 确定想要使用的版本的 Docker **标记**。 若要查看可用标记，请参阅 [mssql-server-linux Docker Hub 页](https://hub.docker.com/_/microsoft-mssql-server)。

2. 使用标记拉取 SQL Server 容器映像。 例如，若要拉取 2019-CU7-ubuntu-18.04 映像，请将以下命令中的 `<image_tag>` 替换为 `2019-CU7-ubuntu-18.04`。

   ```bash
   docker pull mcr.microsoft.com/mssql/server:<image_tag>
   ```

3. 若要使用该映像运行新容器，请在 `docker run` 命令中指定标记名称。 在以下命令中，将 `<image_tag>` 替换为想要运行的版本。

   ::: zone pivot="cs1-bash"
   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

这些步骤也可用于降级现有容器。 例如，你可能会希望回滚或降级正在运行的容器以进行故障排除或测试。 若要降级正在运行的容器，必须对数据文件夹使用持久性技术。 按照[升级部分](#upgrade)所述的相同步骤进行操作，但在运行新容器时指定早期版本的标记名称。

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="run-rhel-based-container-images"></a><a id="rhel"></a> 运行基于 RHEL 的容器映像

SQL Server Linux 容器映像的文档指向基于 Ubuntu 的容器。 从 SQL Server 2019 开始，可使用基于 Red Hat Enterprise Linux (RHEL) 的容器。 RHEL 映像的示例类似于 mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8。

例如，以下命令拉取使用 RHEL 8 的 SQL Server 2019 容器的累积更新 1：

::: zone pivot="cs1-bash"
```bash
sudo docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: moniker-end

## <a name="run-production-container-images"></a><a id="production"></a> 运行生产容器映像

上一部分中的[快速入门](quickstart-install-connect-docker.md)从 Docker Hub 运行免费的 SQL Server Developer Edition。 如果想要运行生产容器映像（例如 Enterprise、Standard 或 Web Edition），大部分信息仍然适用。 但是，存在一些差异，此处将其列出。

- 如果拥有有效的许可证，则只能在生产环境中使用 SQL Server。 可在[此处](https://go.microsoft.com/fwlink/?linkid=857693)获取免费的 SQL Server Express 生产许可证。 SQL Server Standard 和 Enterprise Edition 许可证可通过 [Microsoft 批量许可](https://www.microsoft.com/licensing/default.aspx)获得。

- Developer 容器映像也可以配置为运行生产版本。 使用以下步骤运行生产版本：

查看[快速入门](quickstart-install-connect-docker.md)中的要求并运行过程。 必须使用 **MSSQL_PID** 环境变量指定生产版本。 以下示例介绍如何运行 Enterprise Edition 的最新 SQL Server 2017 容器映像：

::: zone pivot="cs1-bash"
```bash
docker run --name sqlenterprise \
-e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>' \
-e 'MSSQL_PID=Enterprise' -p 1433:1433 \
-d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run --name sqlenterprise `
-e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
-e "MSSQL_PID=Enterprise" -p 1433:1433 `
-d "mcr.microsoft.com/mssql/server:2019-latest"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run --name sqlenterprise `
-e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" ^
-e "MSSQL_PID=Enterprise" -p 1433:1433 ^
-d "mcr.microsoft.com/mssql/server:2019-latest"
```
::: zone-end

> [!IMPORTANT]
> 通过将值 **Y** 传递给环境变量 **ACCEPT_EULA** 并将版本值传递给 **MSSQL_PID**，你表明自己拥有打算使用的 SQL Server 版本的现行有效有许可证。 你还同意自己对在 Docker 容器映像中运行的 SQL Server 软件的使用将受 SQL Server 许可条款的约束。

> [!NOTE]
> 有关 **MSSQL_PID** 的可能值的完整列表，请参阅 [在 Linux 上使用环境变量配置 SQL Server 设置](sql-server-linux-configure-environment-variables.md)。

## <a name="run-multiple-sql-server-containers"></a><a id="multiple"></a>运行多个 SQL Server 容器

Docker 支持在同一主机上运行多个 SQL Server 容器。 对要求在同一主机上运行多个 SQL Server 实例的方案使用此解决办法。 每个容器必须在不同的端口上公开自己。

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

以下示例创建两个 SQL Server 2017 容器，并将它们映射到主机的 **1401** 和 **1402** 端口。

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

以下示例创建两个 SQL Server 2019 容器，并将它们映射到主机的 1401 和 1402 端口   。

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

现在，两个 SQL Server 实例在单独的容器内运行。 客户端可通过使用 Docker 主机的 IP 地址和容器的端口号连接到每个 SQL Server 实例。

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4,1401 -U SA -P '<YourPassword>'
sqlcmd -S 10.3.2.4,1402 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
sqlcmd -S 10.3.2.4,1402 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
sqlcmd -S 10.3.2.4,1402 -U SA -P "<YourPassword>"
```
::: zone-end

## <a name="upgrade-sql-server-in-containers"></a><a id="upgrade"></a> 升级容器中的 SQL Server

若要使用 Docker 升级容器映像，请先确定升级版本的标记。 使用 `docker pull` 命令从注册表中拉取此版本：

```command
docker pull mcr.microsoft.com/mssql/server:<image_tag>
```

此操作会更新创建的任何新容器的 SQL Server 映像，但不会更新任何正在运行的容器中的 SQL Server。 为此，必须使用最新的 SQL Server 容器映像创建新容器，并将数据迁移到该新容器。

1. 确保为现有 SQL Server 容器使用一种[数据持久性技术](sql-server-linux-docker-container-configure.md#persist)。 这样便可以启动具有相同数据的新容器。

1. 使用 `docker stop` 命令停止 SQL Server 容器。

1. 使用 `docker run` 创建新的 SQL Server 容器，并指定映射主机目录或数据卷容器。 确保使用特定标记进行 SQL Server 升级。 新容器现在使用新版 SQL Server 和现有 SQL Server 数据。

   > [!IMPORTANT]
   > 目前仅支持在 RC1、RC2 和 GA 之间升级。

1. 在新容器中验证数据库和数据。

1. 使用 `docker rm` 删除旧容器（可选）。

## <a name="next-steps"></a>后续步骤

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- 通过查看[快速入门](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)，开始在 Docker 上使用 SQL Server 2017 容器映像

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- 通过查看[快速入门](quickstart-install-connect-docker.md?view=sql-server-ver15)，开始在 Docker 上使用 SQL Server 2019 容器映像

::: moniker-end

- [引用 Docker 容器的其他配置和自定义](sql-server-linux-docker-container-configure.md)

- 请参阅 [mssql-docker GitHub 存储库](https://github.com/Microsoft/mssql-docker)，获取资源、反馈和已知问题

- [SQL Server Docker 容器故障排除](sql-server-linux-docker-container-troubleshooting.md)

- [探索 SQL Server 容器的高可用性](sql-server-linux-container-ha-overview.md)

- [保护 SQL Server Docker 容器](sql-server-linux-docker-container-security.md)

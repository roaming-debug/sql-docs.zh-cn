---
title: 分布式事务与 Docker 上的 SQL Server (MSDTC)
description: 了解如何在 Docker 上的 SQL Server 容器中使用 Microsoft 分布式事务处理协调器 (MSDTC) 执行分布式事务。
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: f6cc1ad3c66ae7f4ec1817af3bcaa3934a1858ef
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622885"
---
# <a name="how-to-use-distributed-transactions-with-sql-server-on-docker"></a>如何在 Docker 上使用 SQL Server 的分布式事务

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍如何在 Docker 上设置用于分布式事务的 SQL Server Linux 容器。

SQL Server 容器映像可以使用支持分布式事务所需的 Microsoft 分布式事务处理协调器 (MSDTC)。 若要理解 MSDTC 的通信要求，请参阅[如何在 Linux 上配置 Microsoft 分布式事务处理协调器 (MSDTC)](sql-server-linux-configure-msdtc.md)。 本文介绍与 SQL Server Docker 容器相关的特殊要求和方案。

## <a name="configuration"></a>配置

若要启用 Docker 容器中的 MSDTC 事务，则必须设置两个新的环境变量：

- MSSQL_RPC_PORT：RPC 端点映射程序服务绑定和侦听的 TCP 端口。  
- MSSQL_DTC_TCP_PORT：配置 MSDTC 服务侦听的端口。

### <a name="pull-and-run"></a>请求并运行

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

以下示例演示如何使用这些环境变量来请求和运行为 MSDTC 配置的单个 SQL Server 2017 容器。 通过此操作，该容器可以与任何主机上的任一应用程序通信。

```bash
docker run \
   -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>' \
   -e 'MSSQL_RPC_PORT=135' -e 'MSSQL_DTC_TCP_PORT=51000' \
   -p 51433:1433 -p 135:135 -p 51000:51000  \
   -d mcr.microsoft.com/mssql/server:2017-latest
```

```PowerShell
docker run `
   -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
   -e "MSSQL_RPC_PORT=135" -e "MSSQL_DTC_TCP_PORT=51000" `
   -p 51433:1433 -p 135:135 -p 51000:51000  `
   -d mcr.microsoft.com/mssql/server:2017-latest
```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

以下示例演示如何使用这些环境变量来拉取和运行为 MSDTC 配置的单个 SQL Server 2019 容器。 通过此操作，该容器可以与任何主机上的任一应用程序通信。

```bash
docker run \
   -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>' \
   -e 'MSSQL_RPC_PORT=135' -e 'MSSQL_DTC_TCP_PORT=51000' \
   -p 51433:1433 -p 135:135 -p 51000:51000  \
   -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

```PowerShell
docker run `
   -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
   -e "MSSQL_RPC_PORT=135" -e "MSSQL_DTC_TCP_PORT=51000" `
   -p 51433:1433 -p 135:135 -p 51000:51000  `
   -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

::: moniker-end

> [!IMPORTANT]
> 前一命令仅适用于在 Linux 上运行的 Docker。 对于 Windows 上的 Docker，Windows 主机已侦听 135 端口。 可以在 Windows 上删除 Docker 的 `-p 135:135` 参数，但该操作存在一些限制。 生成的容器则不能用于涉及主机的分布式事务，只能参与主机上 Docker 容器之间的分布式事务。

在此命令中，RPC 端点映射程序服务已绑定到 135 端口，并且 MSDTC 服务已绑定到 Docker 虚拟网络中的 51000 端口。 SQL Server TDS 通信发生在 Docker 虚拟网络中的 1433 端口。 这些端口已对外公开给主机，即 TDS 51433 端口、RPC 终结点映射器 135 端口和 MSDTC 51000 端口。

> [!NOTE]
> RPC 端点映射程序和 MSDTC 端口在主机和容器上不必相同。 因此，若 RPC 端点映射程序端口在容器上配置为 135，它可能会映射到 13501 端口或主机服务器上的任何其他可用端口。

## <a name="configure-the-firewall"></a>配置防火墙

为了与主机通信和通过主机通信，还必须在主机服务器上为容器配置防火墙。 为 Docker 容器公开用于外部通信的所有端口打开防火墙。 前一示例中，这些端口是 135 端口、51433 端口和 51000 端口。 这些端口是主机本身的端口，不是容器中映射到的端口。 因此，如果将容器的 RPC 端点映射程序 51000 端口映射到主机 51001 端口，则应在防火墙中打开 51001 端口（而非 51000 端口），以便与主机通信。  

以下示例演示如何在 Ubuntu 上创建这些规则。

```bash
sudo ufw allow from any to any port 51433 proto tcp
sudo ufw allow from any to any port 51000 proto tcp
sudo ufw allow from any to any port 135 proto tcp
```

以下示例演示如何在 Red Hat Enterprise Linux (RHEL) 上完成此操作：

```bash
sudo firewall-cmd --zone=public --add-port=51433/tcp --permanent
sudo firewall-cmd --zone=public --add-port=51000/tcp --permanent
sudo firewall-cmd --zone=public --add-port=135/tcp --permanent
sudo firewall-cmd --reload
```

## <a name="configure-port-routing-on-the-host"></a>在主机上配置端口路由

在前一示例中，由于单个 Docker 容器将 RPC 135 端口映射到主机上的 135 端口，所以主机的分布式事务现在不应进行进一步的配置。 请注意，由于 SQL Server 在这些容器中使用提升的权限运行，因此可直接在作为根运行的容器中使用 135 端口。 对于容器外的 SQL Server 或非根容器，必须在容器中使用不同的临时端口（如 13500），然后必须将进入 135 端口的流量路由到该端口。 还需要在容器内配置从容器端口 135 到临时端口的端口路由规则。

但是，如果决定将容器的 135 端口映射到主机上的其他端口（如 13500 端口），则必须在主机上配置端口路由。 这样，Docker 容器便可参与主机和其他外部服务器的分布式事务。

有关路由端口的详细信息，请参阅[配置端口路由](sql-server-linux-configure-msdtc.md#configure-port-routing)。

> [!NOTE]
> 默认情况下，SQL Server 2017 在根容器中运行，而 SQL Server 2019 容器以非根用户身份运行。

## <a name="next-steps"></a>后续步骤

有关 Linux 上 MSDTC 的详细信息，请参阅[如何在 Linux 上配置 Microsoft 分布式事务处理协调器 (MSDTC)](sql-server-linux-configure-msdtc.md)。

---
title: 如何在 Linux 上配置 MSDTC
description: 本文介绍如何在 Linux 上配置 Microsoft 分布式事务处理协调器 (MSDTC)。
author: VanMSFT
ms.author: vanto
ms.date: 08/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: a2013fb1e32d14bed4b92aa0cc2394ad3a5c9174
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349123"
---
# <a name="how-to-configure-the-microsoft-distributed-transaction-coordinator-msdtc-on-linux"></a>如何在 Linux 上配置 Microsoft 分布式事务处理协调器 (MSDTC)

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍如何在 Linux 上配置 Microsoft 分布式事务处理协调器 (MSDTC)。

> [!NOTE]
> 自累积更新 16 起，SQL Server 2017 支持 Linux 上的 MSDTC。

## <a name="overview"></a>概述

通过在 SQL Server 中引入 MSDTC 和 RPC 终结点映射程序功能，在 Linux 上的 SQL Server 中启用了分布式事务。 默认情况下，RPC 终结点映射进程在端口 135 上侦听传入的 RPC 请求，并向远程请求提供已注册的组件信息。 远程请求可以使用端点映射程序返回的信息与已注册的 RPC 组件（如 MSDTC 服务）进行通信。 进程需要超级用户权限才能绑定到 Linux 上的已知端口（端口号小于 1024）。 为了避免以 RPC 终结点映射程序进程的根权限启动 SQL Server，系统管理员需要使用 iptable 创建网络地址转换，以将端口 135 上的流量路由到 SQL Server 的 RPC 终结点映射程序进程。

MSDTC 为 mssql-conf 实用程序引入了两个配置参数：

| mssql-conf 设置 | 说明 |
|---|---|
| **network.rpcport** | RPC 终结点映射程序进程绑定到的 TCP 端口。 |
| **distributedtransaction.servertcpport** | MSDTC 服务器侦听的端口。 如果未设置，MSDTC 服务在服务重新启动时使用随机临时端口，并且需要重新配置防火墙例外情况以确保 MSDTC 服务可以继续通信。 |

有关这些设置和其他相关 MSDTC 设置的详细信息，请参阅[使用 mssql-conf 工具在 Linux 上配置 SQL Server](sql-server-linux-configure-mssql-conf.md)。

## <a name="supported-transaction-standards"></a>支持的事务标准

支持使用以下 MSDTC 配置：

| 事务标准 | 数据源 | ODBC 驱动程序 | JDBC 驱动程序|
|---|---|---|---|
| OLE-TX 事务 | Linux 上的 SQL Server | 是 | 否|
| XA 分布式事务 | 支持 XA 的 SQL Server、其他 ODBC 和 JDBC 数据源 | 是（需要版本 17.3 或更高版本） | 是 |
| 链接服务器上的分布式事务 | SQL Server | 是 | 否

有关详细信息，请参阅[了解 XA 事务](../connect/jdbc/understanding-xa-transactions.md#configuration-instructions)。

## <a name="msdtc-configuration-steps"></a>MSDTC 配置步骤

配置 MSDTC 通信和功能需要执行三个步骤。 如果不执行必要的配置步骤，SQL Server 不会启用 MSDTC 功能。

- 使用 mssql-conf 配置“network.rpcport”和“distributedtransaction.servertcpport”。
- 配置防火墙以允许在“distributedtransaction.servertcpport”和端口 135 上进行通信。
- 配置 Linux 服务器路由，以便将端口 135 上的 RPC 通信重定向到 SQL Server 的“network.rpcport”。

以下部分提供了每个步骤的详细说明。

## <a name="configure-rpc-and-msdtc-ports"></a>配置 RPC 和 MSDTC 端口

首先，使用 mssql-conf 配置“network.rpcport”和“distributedtransaction.servertcpport”。 此步骤特定于 SQL Server，并且在所有支持的分发中是通用的。

1. 使用 mssql-conf 设置“network.rpcport”的值。 以下示例将其设置为 13500。

   ```bash
   sudo /opt/mssql/bin/mssql-conf set network.rpcport 13500
   ```

2. 设置“distributedtransaction.servertcpport”的值。 以下示例将其设置为 51999。

   ```bash
   sudo /opt/mssql/bin/mssql-conf set distributedtransaction.servertcpport 51999
   ```

3. 请重新启动 SQL Server。

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="configure-the-firewall"></a>配置防火墙

第二步是配置防火墙以允许在“servertcpport”和端口 135 上进行通信。。  这使 RPC 终结点映射进程和 MSDTC 进程能够与外部的其他事务管理器和协调器进行通信。 实际步骤因 Linux 分发和防火墙而异。 

以下示例演示如何在“Ubuntu”上创建这些规则。

```bash
sudo ufw allow from any to any port 51999 proto tcp
sudo ufw allow from any to any port 135 proto tcp
```

以下示例演示如何在“Red Hat Enterprise Linux (RHEL)”上完成此操作：

```bash
sudo firewall-cmd --zone=public --add-port=51999/tcp --permanent
sudo firewall-cmd --zone=public --add-port=135/tcp --permanent
sudo firewall-cmd --reload
```

在进入下一部分配置端口路由之前，必须先配置防火墙。 刷新防火墙在某些情况下可以清除端口路由规则。

## <a name="configure-port-routing"></a>配置端口路由

配置 Linux 服务器路由表，以便将端口 135 上的 RPC 通信重定向到 SQL Server 的“network.rpcport”。 不同分发上端口转发的配置机制可能不同。 以下部分提供适用于 Ubuntu、SUS Enterprise Linux (SLES) 和 Red Hat Enterprise Linux (RHEL) 的相关指导。

### <a name="port-routing-in-ubuntu-and-sles"></a>Ubuntu 和 SLES 中的端口路由

Ubuntu 和 SLES 不使用“firewalld”服务，因此 iptable 规则就是实现端口路由的有效机制。 在重新启动期间，“iptable”规则可能不会永久发挥作用，因此以下命令还提供了在重新启动后恢复规则作用的说明。

1. 为端口 135 创建路由规则。 在以下示例中，端口 135 定向到上一节中定义的 RPC 端口 13500。 将 `<ipaddress>` 替换为服务器的 IP 地址。

   ```bash
   sudo iptables -t nat -A PREROUTING -d <ip> -p tcp --dport 135 -m addrtype --dst-type LOCAL  \
      -j DNAT --to-destination <ip>:13500 -m comment --comment RpcEndPointMapper
   sudo iptables -t nat -A OUTPUT -d <ip> -p tcp --dport 135 -m addrtype --dst-type LOCAL \
      -j DNAT --to-destination <ip>:13500 -m comment --comment RpcEndPointMapper
   ```

   先前命令中的 `--comment RpcEndPointMapper` 参数有助于在以后的命令中管理这些规则。

2. 使用以下命令查看创建的路由规则：

   ```bash
   sudo iptables -S -t nat | grep "RpcEndPointMapper"
   ```

3. 将路由规则保存到计算机上的文件中。

   ```bash
   sudo iptables-save > /etc/iptables.conf
   ```

4. 要在重新启动后重载规则，请将以下命令添加到 `/etc/rc.local`（Ubuntu）或 `/etc/init.d/after.local`（SLES）：

   ```bash
   iptables-restore < /etc/iptables.conf
   ```

   > [!NOTE]
   > 需要具有超级用户 (sudo) 权限才能编辑“rc.local”或“after.local”文件。

“iptables-save”和“iptables-restore”命令以及 `rc.local`/`after.local` 启动配置提供了保存和恢复 iptables 条目的基本机制 。 可能有更高级或更自动化的选项，具体取决于 Linux 分发。 例如，Ubuntu 替代方案是 iptables-persistent 包，用于使条目持久。

> [!IMPORTANT]
> 前面的步骤假设为固定的 IP 地址。 如果 SQL Server 实例的 IP 地址发生更改（由于手动干预或 DHCP），则需要删除并重新创建路由规则（如果它们是使用 iptables 创建的）。 如果需要重新创建或删除现有路由规则，可以使用以下命令删除旧 `RpcEndPointMapper` 规则：
> 
> ```bash
> sudo iptables -S -t nat | grep "RpcEndPointMapper" | sed 's/^-A //' | while read rule; do iptables -t nat -D $rule; done
> ```

### <a name="port-routing-in-rhel"></a>RHEL 中的端口路由

在使用“firewalld”服务的分发（例如 Red Hat Enterprise Linux）上，可以使用同一服务来打开服务器上的端口和实现内部端口转发。 例如，在 Red Hat Enterprise Linux 上，应使用“firewalld”服务（通过带有 `-add-forward-port` 或类似选项的“firewall-cmd”配置实用程序）来创建和管理永久性端口转发规则，而非使用 iptables。

```bash
sudo firewall-cmd --permanent --add-forward-port=port=135:proto=tcp:toport=13500
sudo firewall-cmd --reload
```

## <a name="verify"></a>Verify

此时，SQL Server 应已能够参与分布式事务。 若要验证 SQL Server 是否正在侦听，请运行“netstat”命令（如果使用的是 RHEL，则可能需要先安装“net-tools”包）：

```bash
sudo netstat -tulpn | grep sqlservr
```

应该会看到与下面类似的输出：

```bash
tcp 0 0 0.0.0.0:1433 0.0.0.0:* LISTEN 13911/sqlservr
tcp 0 0 127.0.0.1:1434 0.0.0.0:* LISTEN 13911/sqlservr
tcp 0 0 0.0.0.0:13500 0.0.0.0:* LISTEN 13911/sqlservr
tcp 0 0 0.0.0.0:51999 0.0.0.0:* LISTEN 13911/sqlservr
tcp6 0 0 :::1433 :::* LISTEN 13911/sqlservr
tcp6 0 0 ::1:1434 :::* LISTEN 13911/sqlservr
tcp6 0 0 :::13500 :::* LISTEN 13911/sqlservr
tcp6 0 0 :::51999 :::* LISTEN 13911/sqlservr
```

但是，重新启动后，SQL Server 在第一个分布式事务之前不会开始侦听“servertcpport”。 在本例中，第一个分布式事务发生之前，SQL Server 不会侦听端口 51999。

## <a name="configure-authentication-on-rpc-communication-for-msdtc"></a>为 MSDTC 配置 RPC 通信身份验证

默认情况下，Linux 上 SQL Server 的 MSDTC 不对 RPC 通信使用身份验证。 但是，当主机加入 Active Directory (AD) 域时，可使用以下“mssql-conf”设置将 MSDTC 配置为使用经过身份验证的 RPC 通信：

| 设置 | 描述 |
|---|---|
| **distributedtransaction.allowonlysecurerpccalls**          | 仅为分布式事务配置安全的 RPC 调用。 默认值为 0。 |
| **distributedtransaction.fallbacktounsecurerpcifnecessary** | 为分布式事务配置“仅安全”的 RPC 调用。 默认值为 0。 |
| **distributedtransaction.turnoffrpcsecurity**               | 为分布式事务启用或禁用 RPC 安全性。 默认值为 0。 |

## <a name="additional-guidance"></a>其他指南

### <a name="active-directory"></a>Active Directory

如果 SQL Server 已在 Active Directory (AD) 配置中注册，那么 Microsoft 建议使用启用了 RPC 的 MSDTC。 如果 SQL Server 配置为使用 AD 身份验证，则 MSDTC 默认使用相互身份验证 RPC 安全性。

### <a name="windows-and-linux"></a>Windows 和 Linux

如果 Windows 操作系统上的客户端需要使用 Linux 上的 SQL Server 登记到分布式事务中，则必须具有以下最低版本的 Windows 操作系统：

| 操作系统 | 最低版本 | OS 内部版本 |
|---|---|---|
| [Windows Server](/windows-server/get-started/windows-server-release-info) | 1903 | 18362.30.190401-1528 |
| [Windows 10](/windows/release-information/) | 1903 | 18362.267 |

## <a name="next-steps"></a>后续步骤

有关 Linux 上 SQL Server 的详细信息，请参阅 [Linux 上的 SQL Server](sql-server-linux-overview.md)。
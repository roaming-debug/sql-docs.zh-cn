---
title: 排查连接到 SQL Server 数据库引擎时的问题
description: 了解如何排查连接问题。 查看在无法使用 TCP/IP 连接到单个服务器上的 SQL Server 数据库引擎时应执行的步骤。
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: troubleshooting
helpviewer_keywords:
- troubleshooting, connecting to Database Engine
- connecting to Database Engine, troubleshooting
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: sqlfreshmay19
ms.date: 11/25/2019
ms.openlocfilehash: 2ef49ed5d3d46e4f5c5602972ea87347b686d9bb
ms.sourcegitcommit: 8a8c89b0ff6d6dfb8554b92187aca1bf0f8bcc07
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97617566"
---
# <a name="troubleshoot-connecting-to-the-sql-server-database-engine"></a>排查连接到 SQL Server 数据库引擎时的问题

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本文列出了在无法连接到单个服务器上的 SQL Server 数据库引擎实例时使用的故障排除技术。

>[!NOTE]
>对于其他情况，请参阅：
>- [可用性组侦听器](../availability-groups/windows/listeners-client-connectivity-application-failover.md)
>- [故障转移群集实例](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)

这些步骤不按你可能已尝试过的最有可能出现的问题顺序排序， 而按从最基本问题到更复杂问题的顺序排序。 这些步骤假定，你要使用 TCP/IP 协议从另一台计算机连接到 SQL Server 实例（这是最常见的情况）。

这些说明对排查“连接到服务器”错误（可能为 `Error Number: 11001 (or 53), Severity: 20, State: 0`）很有用。 下面是错误消息的示例：

> `A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections.`
>
> `(provider: Named Pipes Provider, error: 40 - Could not open a connection to SQL Server) (Microsoft SQL Server, Error: 53)`
>
> `(provider: TCP Provider, error: 0 - No such host is known.) (Microsoft SQL Server, Error: 11001)`

此错误通常意味着客户找不到 SQL Server 实例。 当至少存在以下其中一个问题时，通常会发生这种情况：

- 承载 SQL Server 实例的计算机的名称
- 实例不能解析正确的 IP
- 未正确指定 TCP 端口号

> [!TIP]
> 有关交互式故障排除页面，请参阅 [!INCLUDE[msCoName_md](../../includes/msconame-md.md)] 客户支持服务中的 [Solving Connectivity errors to SQL Server](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server)（解决到 SQL Server 的连接问题）。

## <a name="not-included"></a>不包含的信息

- 本主题不包含有关 SSPI 错误的信息。 有关 SSPI 错误，请参阅[如何排查“无法生成 SSPI 上下文”错误消息](https://support.microsoft.com/kb/811889)。
- 本主题不包含有关 Kerberos 错误的信息。 有关帮助，请参阅 [Microsoft Kerberos Configuration Manager for SQL Server](https://www.microsoft.com/download/details.aspx?id=39046)。
- 本主题不包含 Azure SQL 数据库连接的相关信息。 有关帮助，请参阅[排查 Microsoft Azure SQL 数据库的连接问题](/azure/sql-database/troubleshoot-connectivity-issues-microsoft-azure-sql-database)。

## <a name="get-instance-name-from-configuration-manger"></a>在配置管理器中获取实例名称

在承载 SQL Server 实例的服务器上，验证实例名称。 使用 [SQL Server 配置管理器](../../relational-databases/sql-server-configuration-manager.md)。

在 SQL Server 安装后，配置管理器也会自动安装在计算机上。 SQL Server 和 Windows 版本的配置管理器启动说明略有不同。 有关版本详情，请参阅 [SQL Server 配置管理器](../../relational-databases/sql-server-configuration-manager.md)。）

1. 登录托管 SQL Server 实例的计算机。
2. 启动 SQL Server 配置管理器。
3. 在左侧窗格中，选择“SQL Server Services”。
4. 在右侧窗格中，验证数据库引擎实例名称。

   - `SQL SERVER (MSSQLSERVER)` 表示 SQL Server 默认实例。 默认实例的名称是 `<computer name>`。
   - `SQL SERVER (<instance name>)` 表示 SQL Server 命名实例。 命名实例的名称是 `<computer name>\<instance name>`

## <a name="verify---the-instance-is-running"></a>验证实例是否正在运行

若要验证实例是否正在运行，请在配置管理器中查看 SQL Server 实例的符号。

- 绿色箭头表示实例正在运行。
- 红色方形表示实例已停止。

如果实例已停止，请右键单击实例，再单击“启动”。 此时，服务器实例启动，且指示器变成绿色箭头。

## <a name="verify---sql-server-browser-service-is-running"></a><a name = "startbrowser"></a> 验证 - SQL Server Browser 服务正在运行

要连接到命名实例，SQL Server Browser 服务必须处于运行状态。 在配置管理器中，找到“SQL Server Browser”服务，并验证它是否正在运行。 如果未运行，请启动它。 默认实例不需要 SQL Server Browser 服务。

SQL Server 的默认实例不需要 SQL Server Browser 服务。

## <a name="testing-a-local-connection"></a>测试本地连接

从另一台计算机排查连接问题前，请先测试能否从运行 SQL Server 的计算机上本地安装的客户端应用程序进行连接。 本地连接可避免出现网络问题和防火墙问题。 

此过程使用 SQL Server Management Studio。 如果未安装 Management Studio，请参阅[下载 SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)。 如果无法安装 Management Studio，可以使用 `sqlcmd.exe` 实用工具来测试连接。 `sqlcmd.exe` 随数据库引擎一起安装。 有关 `sqlcmd.exe`的信息，请参阅 [sqlcmd 实用工具](../../tools/sqlcmd-utility.md)。）

1. 使用有权访问 SQL Server 的登录名，登录已安装 SQL Server 的计算机。 （在安装期间，SQL Server 要求将至少一个登录名指定为 SQL Server 管理员。 如果不知道管理员，请参阅[在系统管理员被锁定时连接到 SQL Server](connect-to-sql-server-when-system-administrators-are-locked-out.md)。）
2. 在“开始”页上键入 **SQL Server Management Studio**，或在旧版 Windows 的“开始”菜单上，指向“所有程序”，指向“Microsoft SQL Server”，然后单击“SQL Server Management Studio”。
3. 在“连接到服务器”对话框的“服务器类型”框中，选择“数据库引擎”。 在“身份验证”框中，选择“Windows 身份验证” 。 在“服务器名称”框中，键入以下连接类型之一：

   |连接到|类型|示例|
   |:-----------------|:---------------|:-----------------|
   |默认实例|`<computer name>`|`ACCNT27`|
   |命名实例|`<computer name\instance name>`|`ACCNT27\PAYROLL`|

   > [!NOTE]
   > 当从同一台计算机上的客户端应用程序连接到 SQL Server 时，将使用共享内存协议。 共享内存是一种本地命名管道，因此有时会遇到管道方面的错误。

   如果此时收到一个错误，则必须先解决它，然后才能继续操作。 有许多可能属于问题的情况。 你的登录名可能未获权进行连接。 你的默认数据库可能已丢失。

   > [!NOTE]
   > 某些传递到客户端的错误消息故意不提供足够的信息来排查问题。 这是一项安全功能，目的是避免为攻击者提供有关 SQL Server 的信息。 若要查看有关错误的完整信息，请查找 SQL Server 错误日志。 那里提供了详细信息。 

4. 如果看到错误 `18456 Login failed for user`，请参阅联机丛书主题 [MSSQLSERVER_18456](../../relational-databases/errors-events/mssqlserver-18456-database-engine-error.md)，其中包含错误代码的其他信息。 Aaron Bertrand 的博客[故障排除错误 18456](https://sqlblog.org/2011/01/14/troubleshooting-error-18456) 中提供了详细的错误代码列表。 你可以在对象资源管理器的“管理”部分中使用 SSMS（如果可以连接）查看错误日志。 否则，可以使用 Windows 记事本程序查看错误日志。 默认位置因版本而异，且可以在安装过程中更改。 [!INCLUDE[ssSQL15_md](../../includes/sssqlv15-md.md)] 的默认位置是 `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Log\ERRORLOG`。 

5. 如果可以使用共享内存进行连接，请使用 TCP 测试连接。 可以通过在名称前指定 `tcp:` 来强制建立 TCP 连接。 例如：

   |连接到：|键入：|示例：|
   |-----------------|---------------|-----------------|
   |默认实例|`tcp:<computer name>`|`tcp:ACCNT27`|
   |命名实例|`tcp:<computer name/instance name>`|`tcp:ACCNT27\PAYROLL`|

6. 如果可以使用共享内存进行连接，但不能使用 TCP 进行连接，则必须修复 TCP 问题。 最有可能的问题是未启用 TCP。 要启用 TCP，请参阅上面的[启用协议](#enableprotocols)步骤。

7. 如果你的目标是使用非管理员帐户进行连接，那么一旦能以管理员身份连接后，请使用客户端应用程序使用的 Windows 身份验证登录名或 SQL Server 身份验证登录名重新尝试进行连接。

## <a name="get-the-ip-address-of-the-server"></a>获取服务器的 IP 地址

获取托管 SQL Server 实例的计算机的 IP 地址。

1. 在“开始”菜单上，单击“运行”。 在“运行”窗口中键入 cmd，然后单击“确定”  。
2. 在命令提示符窗口中，键入 `ipconfig`，然后按 Enter。 记下 **IPv4** 地址和 **IPv6** 地址。

  >SQL Server 可以使用 IP 版本 4 协议或 IP 版本 6 协议进行连接。 你的网络可能允许其中某种协议，或者两种协议都允许。 多数用户首先会排查 **IPv4** 地址问题。 它更短且更容易键入。

## <a name="get-the-sql-server-instance-tcp-port"></a><a name = "getTCP"></a>获取 SQL Server 实例 TCP 端口

在大多数情况下，你是使用 TCP 协议从另一台计算机连接到数据库引擎。

1. 在运行 SQL Server 的计算机上使用 SQL Server Management Studio 连接到 SQL Server 实例。 在对象资源管理器中，展开“管理”，展开“SQL Server 日志”，然后双击当前日志。
2. 在日志查看器中，单击工具栏上的“筛选器”按钮。 在“消息包含文本”框中，键入“`server is listening on`”，再依次单击“应用筛选器”和“确定”。
3. 此时，系统应该会列出如 `Server is listening on [ 'any' <ipv4> 1433]` 所示的消息。 

此消息表示，此 SQL Server 实例正在侦听此计算机上的所有 IP 地址（针对 IP 版本 4）和侦听 TCP 端口 1433。 （TCP 端口 1433 通常是由数据库引擎或 SQL Server 默认实例使用的端口。 一个端口只允许由一个 SQL Server 实例使用，因此，如果安装了多个 SQL Server 实例，某些实例必须使用其他端口号。）记下要连接到的 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 实例所使用的端口号。

  > [!NOTE]
  > 系统可能会列出 `IP address 127.0.0.1`。 这称为“环回适配器地址”。 只有同一台计算机上的进程才能使用它进行连接。 它可用于故障排除，但你不能使用它从另一台计算机进行连接。

## <a name="enable-protocols"></a><a name = "enableprotocols"></a>启用协议

在某些 SQL Server 安装中，不支持从另一台计算机连接到数据库引擎，除非管理员使用配置管理器启用这种连接。 若要支持从另一台计算机进行连接，请执行以下操作：

1. 如前面所述，打开 SQL Server 配置管理器。
2. 在配置管理器的左窗格中，展开“SQL Server 网络配置”，然后选择要连接到的 SQL Server 实例。 右窗格会列出可用的连接协议。 共享内存通常处于启用状态。 只能从同一台计算机使用共享内存，因此，大多数安装会将它保留为启用状态。 若要从另一台计算机连接到 SQL Server，通常使用 TCP/IP。 如果未启用 TCP/IP，请右键单击“TCP/IP”，然后单击“启用”。
3. 如果更改了任何协议已启用的设置，请重启数据库引擎。 在左侧窗格中，选择“SQL Server Services”。 在右窗格中，右键单击数据库引擎实例，然后单击“重新启动”。

## <a name="testing-tcpip-connectivity"></a><a name="testTCPIP"></a>测试 TCP/IP 连接

使用 TCP/IP 连接到 SQL Server 时，要求 Windows 可以建立连接。 使用 `ping` 工具测试 TCP。

1. 在“开始”菜单上，单击“运行”。 在“运行”窗口中键入 **cmd**，然后单击“确定” 。 
2. 在命令提示符窗口中，键入 `ping <ip address>`，然后键入正在运行 SQL Server 的计算机的 IP 地址。 例如：

    - IPv4：`ping 192.168.1.101`
    - IPv6：`ping fe80::d51d:5ab5:6f09:8f48%11`

3. 如果网络配置正确，`ping` 返回 `Reply from <IP address>`（后跟其他一些信息）。 如果 `ping` 返回 `Destination host unreachable` 或 `Request timed out`，则表明 TCP/IP 配置不正确。 此时的错误可能表示客户端计算机、服务器计算机或某些网络相关设备（如路由器）出现了问题。 若要排查网络问题，请参阅 [TCP/IP 问题的高级疑难解答](/windows/client-management/troubleshoot-tcpip)。
4. 接下来，如果使用 IP 地址成功进行了 ping 测试，请测试计算机名称能否解析为 TCP/IP 地址。 在客户端计算机上的命令提示符窗口中，键入 `ping` ，然后键入正在运行 SQL Server 的计算机的计算机名称。 例如，`ping newofficepc` 。
5. 如果对 IP 地址执行的 `ping` 操作成功，但对计算机执行的 `ping` 操作返回 `Destination host unreachable` 或 `Request timed out`，表明你在客户端计算机上缓存的名称解析信息可能较旧（过时）。 请键入 `ipconfig /flushdns` 以清除 DNS（动态名称解析）缓存。 然后再次使用名称 ping 计算机。 清除 DNS 缓存后，客户端计算机将检查服务器计算机的 IP 地址的最新相关信息。
6. 如果网络配置正确，`ping` 返回 `Reply from <IP address>`（后跟其他一些信息）。 如果可以按 IP 地址成功对服务器计算机执行 ping 操作，但在按计算机名称执行 ping 操作时看到 `Destination host unreachable.` 或 `Request timed out.` 等错误，则表明名称解析配置不正确。 （有关详细信息，请参阅之前引用的 2006 年发布的[如何排查基本 TCP/IP 问题](https://support.microsoft.com/kb/169790)一文。）连接到 SQL Server 不需要成功的名称解析，但如果计算机名称无法解析为 IP 地址，则必须通过指定 IP 地址来建立连接。 稍后可以修复名称解析。

## <a name="open-a-port-in-the-firewall"></a>在防火墙中开放端口

默认情况下，Windows 防火墙处于开启状态，并且会阻止来自另一台计算机的连接。 若要从另一台计算机使用 TCP/IP 进行连接，则必须在 SQL Server 计算机上，将防火墙配置为允许连接到数据库引擎所使用的 TCP 端口。 默认实例默认侦听 TCP 端口 1433。 如果你有命名实例或更改了默认实例端口，[!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] TCP 端口可能会侦听其他端口。 请参阅[获取 SQL Server 实例 TCP 端口](#getTCP)。

如果要连接到命名实例或 TCP 端口 1433 以外的端口，则还必须为 SQL Server Browser 服务打开 UDP 端口 1434。 有关在 Windows 防火墙中打开端口的分步说明，请参阅[为数据库引擎访问配置 Windows 防火墙](configure-a-windows-firewall-for-database-engine-access.md)。

## <a name="test-the-connection"></a>测试连接

一旦可以在同一台计算机上使用 TCP 进行连接，就可以尝试从客户端计算机进行连接。 从理论上讲，你可以使用任何客户端应用程序，但为了避免增加复杂性，请在客户端上安装 SQL Server 管理工具，并使用 SQL Server Management Studio 进行尝试。

1. 在客户端计算机上，使用 SQL Server Management Studio 通过 IP 地址和 TCP 端口号（格式为“IP 地址,端口号”）尝试进行连接。 例如，`192.168.1.101,1433`。 如果无法建立此连接，表明可能存在以下问题之一：

   - IP 地址的 `ping` 操作不起作用，表明出现了常规的 TCP 配置问题。 返回到[测试 TCP/IP 连接](#testTCPIP)部分。
   - SQL Server 未侦听 TCP 协议。 返回到[启用协议](#enableprotocols)部分。
   - SQL Server 正在侦听的端口不是你指定的端口。 请返回到[获取 TCP 端口号](#getTCP)部分。
   - SQL Server TCP 端口被防火墙阻止。 返回到[在防火墙中打开端口](#open-a-port-in-the-firewall)部分。

2. 可以使用 IP 地址和端口号进行连接之后，尝试使用不带端口号的 IP 地址进行连接。 对于默认实例，仅使用 IP 地址。 对于命名实例，使用格式为“IP 地址\实例名称”的 IP 地址和实例名称（例如，`192.168.1.101\<instance name>`）。如果这样做不起作用，表明可能存在以下问题之一：

   - 如果你正在连接到默认实例，它可能正在侦听 TCP 端口 1433 以外的端口，并且客户端不会尝试连接到正确的端口号。
   - 如果你正在连接到命名实例，则端口号不会返回到客户端。

   这两个问题都与向客户端提供端口号的 SQL Server Browser 服务有关。 解决方案如下：

   - 启动 SQL Server Browser 服务。 请参阅有关如何[在 SQL Server 配置管理器中启动浏览器](#startbrowser)的说明。
   - SQL Server Browser 服务被防火墙阻止。 请在防火墙中打开 UDP 端口 1434。 返回到[在防火墙中打开端口](#open-a-port-in-the-firewall)部分。 请确保打开的是 UDP 端口，而不是 TCP 端口。
   - UDP 端口 1434 信息被路由器阻止。 UDP 通信（用户数据报协议）未设计为可通过路由器。 这可以防止网络被低优先级流量占满。 可以将路由器配置为转发 UDP 流量，也可以决定在连接时始终提供端口号。
   - 如果客户端计算机正在使用 Windows 7 或 Windows Server 2008（或更新的操作系统），UDP 流量可能会被客户端操作系统删除，因为来自服务器的响应会从所查询 IP 地址以外的 IP 地址返回。 这是一项安全功能，目的是阻止“宽松源映射”。 有关详细信息，请参阅联机丛书主题[故障排除：超时时间已到](/previous-versions/sql/sql-server-2008-r2/ms190181(v=sql.105))的“多个服务器 IP 地址”一节。 这篇文章来自 SQL Server 2008 R2，但大体内容仍然适用。 可以将客户端配置为使用正确的 IP 地址，也可以决定在连接时始终提供端口号。

3. 可以使用 IP 地址（或 IP 地址和实例名称，如果是命名实例的话）进行连接后，立即尝试使用计算机名称（或计算机名称和实例名称，如果是命名实例的话）进行连接。 将 `tcp:` 放在计算机名称的前面，以强制建立 TCP/IP 连接。 例如，对于名为 `ACCNT27`的计算机上的默认实例，请使用 `tcp:ACCNT27` ；对于该计算机上名为 `PAYROLL`的命名实例，请使用 `tcp:ACCNT27\PAYROLL` 。如果可以使用 IP 地址进行连接，但不能使用计算机名称进行连接，则表明存在名称解析问题。 返回到“测试 TCP/IP 连接”部分的第 4 节。

4. 可以使用强制实施 TCP 的计算机名称进行连接之后，尝试使用不强制实施 TCP 的计算机名称进行连接。 例如，对于默认实例，仅使用计算机名称，如 `CCNT27`；对于命名实例，使用计算机名称和实例名称，如 `ACCNT27\PAYROLL`。如果在强制实施 TCP 时可以连接，但在不强制实施 TCP 时无法连接，则表明客户端可能正在使用另一种协议（例如命名管道）。

    1. 在客户端计算机上，在 SQL Server 配置管理器的左窗格中展开“SQL Native Client 版本配置”，然后选择“客户端协议” 。
    2. 在右窗格中，确保 TCP/IP 已启用。 如果 TCP/IP 处于禁用状态，请右键单击“TCP/IP”，然后单击“启用”。
    3. 确保 TCP/IP 的协议顺序编号小于命名管道（或较低版本上的 VIA）协议。 通常，应该将共享内存保留为顺序 1，TCP/IP 保留为顺序 2。 仅当客户端和 SQL Server 在同一台计算机上运行时，才使用共享内存。 将按顺序尝试所有已启用的协议，直到其中一个成功为止，只不过当连接不属于同一台计算机时，将跳过共享内存。
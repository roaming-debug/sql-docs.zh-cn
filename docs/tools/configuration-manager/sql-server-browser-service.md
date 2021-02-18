---
title: SQL Server Browser Service
description: 了解如何使用 SQL Server Browser，这是一种服务，可侦听对 SQL Server 资源的请求，并提供有关已安装的 SQL Server 实例的信息。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
f1_keywords:
- sql13.swb.browseservers.local.f1
- sql13.swb.browseservers.network.f1
helpviewer_keywords:
- services [SQL Server], security
- SQL Browser service (See SQL Server Browser Service)
- Browser Service
- SQL Server Browser service
ms.assetid: 3cc00d3a-487c-4cd9-a155-655f02485fa0
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a4c58894434c503f92c8bcf4df4f74cf9fa2c688
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345189"
---
# <a name="sql-server-browser-service"></a>SQL Server Browser 服务
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]浏览器程序以 Windows 服务的形式运行。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器侦听对 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 资源的传入请求，并提供计算机上安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的相关信息。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器可用于执行下列操作：  
  
-   浏览可用服务器列表  
  
-   连接到正确的服务器实例  
  
-   连接到专用管理员连接 (DAC) 端点  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] Browser 服务 (sqlbrowser) 为 [!INCLUDE[ssAS](../../includes/ssas-md.md)]和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的每个实例提供实例名称和版本号。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器随 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]一起安装。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器可以在安装过程中进行配置，也可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器进行配置。 默认情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务会自动启动：  
  
-   升级安装时。  
  
-   在群集上安装时。  
  
-   安装包括所有 SQL Server Express 实例的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的命名实例时。  
  
-   安装 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]的命名实例时。  
  
## <a name="background"></a>背景  
 在 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]之前，一台计算机上只能安装一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 侦听 1433 端口上的传入请求，该端口由官方的 Internet 号码分配机构 (IANA) 分配给 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 只有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的一个实例可以使用端口，因此，在 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 引入了对多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的支持时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 解析协议 (SSRP) 是为侦听 UDP 端口 1434 而开发的。 此侦听器服务使用已安装实例的名称以及该实例使用的端口或命名管道响应客户端请求。 为了解决 SSRP 系统的限制， [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 引入了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务来替换 SSRP。  
  
## <a name="how-sql-server-browser-works"></a>SQL Server Browser 工作原理  
 启动一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例后，如果为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]启用了 TCP/IP 协议，服务器将被分配一个 TCP/IP 端口。 如果启用了 Named Pipes 协议， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将侦听特定的命名管道。 该特定实例将使用此端口（或“管道”）与客户端应用程序交换数据。 在安装过程中，TCP 1433 端口和管道 `\sql\query` 将分配给默认实例，但服务器管理员可以随后使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器进行更改。 由于只有一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例可以使用端口或管道，因此，会将不同的端口号和管道名称分配给命名实例，包括 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]。 默认情况下，命名实例和 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 在启用时便配置为使用动态端口，也就是说，当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动时就分配了可用端口。 如果需要，可以为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例分配特定端口。 连接时，客户端可以指定特定端口，但是如果端口是动态分配的，端口号可能会在重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时被更改，因此正确的端口号对于客户端来说是不确定的。  
  
 在启动后， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器将启动并使用 UDP 1434 端口。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器将读取注册表，识别计算机上的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，并注明它们使用的端口和命名管道。 当一台服务器具有两个或多个网卡时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器会为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]返回其遇到的第一个已启用的端口。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器支持 ipv6 和 ipv4。  
  
 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 客户端请求 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 资源时，客户端网络库将使用 1434 端口向服务器发送一条 UDP 消息。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器将用请求的实例的 TCP/IP 端口或命名管道做出响应。 然后，客户端应用程序中的网络库将使用所需实例的端口或命名管道向服务器发送请求来完成连接。  
  
 有关启动和停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务的信息，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书。  
  
## <a name="using-sql-server-browser"></a>使用 SQL Server Browser  
 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务不运行时，如果您提供了正确的端口号或命名管道，仍可以连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 例如，如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的默认实例在 1433 端口上运行，则可以使用 TCP/IP 连接到此默认实例。  
  
 但是，如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务未运行，则以下连接无效：  
  
-   在未完全指定所有参数（例如 TCP/IP 端口或命名管道）的情况下，组件尝试连接到命名实例。  
  
-   生成或传递其他组件随后要用来进行重新连接的服务器/实例信息的组件。  
  
-   未提供端口号或管道就连接到命名实例。  
  
-   在未使用 TCP/IP 1433 端口的情况下，将 DAC 连接到命名实例或默认实例。  
  
-   OLAP 重定向程序服务。  
  
-   枚举 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、企业管理器或查询分析器中的服务器。  
  
 如果在客户端服务器方案中使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （例如，应用程序通过网络访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ），那么，若要停止或禁用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务，必须为每个实例分配一个特定端口号，并编写客户端应用程序代码以便始终使用该端口号。 此方法存在如下问题：  
  
-   必须更新和维护客户端应用程序代码才能确保它连接到正确的端口。  
  
-   如果服务器上的其他服务或应用程序可以使用您为每个实例选择的端口，则会导致 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例不可用。  
  
## <a name="clustering"></a>群集  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器不是群集资源，不支持从一个群集节点到其他群集节点的故障转移。 因此，在使用群集的情况下，应安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器，并为群集的每个节点启用该浏览器。 在群集中， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器侦听 IP_ANY。  
  
> [!NOTE]  
>  侦听 IP_ANY 时，如果启用侦听特定的 IP，用户必须为每个 IP 配置相同的 TCP 端口，因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器将返回它遇到的第一个 IP/端口对。  
  
## <a name="installing-uninstalling-and-running-from-the-command-line"></a>通过命令行进行安装、卸载和运行  
 默认情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器程序安装在 C:\Program Files (x86)\Microsoft SQL Server\90\Shared\sqlbrowser.exe 中。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器服务在删除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的最后一个实例后被卸载。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以使用 **-c** 开关，通过命令提示符启动浏览器来排除故障：  
  
```  
<drive>\<path>\sqlbrowser.exe -c  
```  
  
## <a name="security"></a>安全性  
  
### <a name="account-privileges"></a>帐户权限  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 解析协议 (SSRP) 侦听 UDP 端口，并接受未经身份验证的请求。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器应该在低特权用户的安全上下文中运行，以将受到恶意攻击的几率降到最低。 通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器更改登录帐户。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器的最小用户权限如下：  
  
-   拒绝通过网络访问该计算机  
  
-   拒绝本地登录  
  
-   拒绝以批处理作业登录  
  
-   拒绝通过“终端服务”登录  
  
-   作为服务登录  
  
-   读取和写入与网络通信（端口和管道）相关的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 注册表项  
  
### <a name="default-account"></a>默认帐户  
 安装程序将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器配置为使用安装期间为服务选定的帐户。 其他可能的帐户包括：  
  
-   所有“域\本地”帐户  
  
-   **“本地服务”** 帐户  
  
-   “本地系统”帐户（不推荐使用，因为其具有不必要的权限）  
  
### <a name="hiding-sql-server"></a>隐藏 SQL Server  
 隐藏的实例是仅支持共享内存连接的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，设置 `HideInstance` 标记来指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器不应对此服务器实例的相关信息做出响应。  
  
### <a name="using-a-firewall"></a>使用防火墙  
 若要与有防火墙保护的服务器上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务进行通信，除了打开 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用的 TCP 端口（如 1433）之外，还要打开 UDP 端口 1434。 有关使用防火墙的信息，请参见“如何：为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Access 配置防火墙”（位于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书中）。  
  
## <a name="see-also"></a>另请参阅  
 [网络协议和网络库](../../sql-server/install/network-protocols-and-network-libraries.md)  
  
  

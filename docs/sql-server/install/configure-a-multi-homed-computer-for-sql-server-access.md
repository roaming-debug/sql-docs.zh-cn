---
title: 配置多宿主计算机以允许访问
description: 了解如何配置 SQL Server 和 Windows 防火墙，以便为多宿主环境中的 SQL Server 实例提供网络连接。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- ports [SQL Server], multi-homed computer
- multi-homed computer [SQL Server] configuring ports
- firewall systems [Database Engine], multi-homed computer
ms.assetid: ba369e5b-7d1f-4544-b7f1-9b098a1e75bc
author: cawrites
ms.author: chadam
ms.openlocfilehash: 336822b6ad2ff420c987e42c033e2efae20c2536
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96127538"
---
# <a name="configure-a-multi-homed-computer-for-sql-server-access"></a>将多宿主计算机配置为允许 SQL Server 访问
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  当服务器必须提供与两个或更多个网络或网络子网的连接时，典型的方案是使用多宿主计算机。 此计算机通常位于外围网络（也称为 DMZ、外围安全区域或屏蔽子网）中。 本文介绍如何在多宿主环境中配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和高级安全 Windows 防火墙，以便为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例提供多个网络连接。  
  
> [!NOTE]  
>  多宿主计算机有多个网络适配器或者已配置为一个网络适配器使用多个 IP 地址。 双宿主计算机有两个网络适配器或者已配置为一个网络适配器使用两个 IP 地址。  
  
 在继续阅读本文之前，用户应当熟悉[配置 Windows 防火墙以允许 SQL Server 访问](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)一文中提供的信息。 本文包含有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件如何与防火墙一起使用的基本信息。  
  
 **本示例假设：**  
  
-   计算机中安装了两个网络适配器。 可以有一个或多个网络适配器是无线的。 可以通过使用一个网络适配器的 IP 地址，使用环回 IP 地址 (127.0.0.1) 作为第二个网络适配器来模拟有两个网络适配器。  
  
-   为简单起见，本示例使用 IPv4 地址。 使用 IPv6 地址可执行相同的过程。  
  
    > [!NOTE]  
    >  IPv4 地址是一串四个数字（称为八位字节）。 每个数字小于 255，由句点分隔，例如 127.0.0.1。 IPv6 地址是一串八个十六进制数字，由冒号分隔，如 fe80:4898:23:3:49a6:f5c1:2452:b994。  
  
-   防火墙规则可能允许通过特定端口（如端口 1433）进行访问， 也可能允许访问 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 程序 (sqlservr.exe)。 哪个方法都不会比另一个方法更好。 因为外围网络中的服务器比 Intranet 上的服务器更容易受到攻击，本文假设用户希望进行更精确的控制并单独选择打开的端口。 出于上述原因，本文假设用户将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置为侦听固定端口。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所用端口的详细信息，请参阅 [配置 Windows 防火墙以允许 SQL Server 访问](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)。  
  
-   本示例使用 TCP 端口 1433 配置对 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的访问。 可以使用相同的常规步骤来配置不同的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件使用的其他端口。  
  
 **本示例中的常规步骤如下：**  
  
-   确定计算机的 IP 地址。  
  
-   将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置为侦听特定的 TCP 端口。  
  
-   配置高级安全 Windows 防火墙。  
  
## <a name="optional-procedures"></a>可选过程  
 如果您已经知道计算机可用的 IP 地址且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]使用该地址，则可以跳过这些过程。  
  
#### <a name="to-determine-the-ip-addresses-available-on-the-computer"></a>确定计算机上可用的 IP 地址  
  
1.  在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机上，依次单击“开始”和“运行”，键入 **cmd**，然后[!INCLUDE[clickOK](../../includes/clickok-md.md)]。  
  
2.  在“命令提示符”窗口中，键入 **ipconfig,** ，然后按 Enter 列出此计算机上可用的 IP 地址。  
  
    > [!NOTE]  
    >  **ipconfig** 命令有时会列出许多可能的连接，包括已断开的连接。 **ipconfig** 命令可同时列出 IPv4 和 IPv6 地址。  
  
3.  请注意正在使用的 IPv4 地址和 IPv6 地址。 列表中的其他信息（例如临时地址、子网掩码和默认网关）是配置 TCP/IP 网络的重要信息。 但是在本示例中未用到这些信息。  
  
#### <a name="to-determine-the-ip-addresses-and-ports-used-by-ssnoversion"></a>确定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1.  单击 **“开始”** ，依次指向 **“所有程序”** 、 [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]和 **“配置工具”** ，然后单击 **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器”** 。  
  
2.  在“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器”的控制台窗格中，依次展开“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 网络配置”、“\<instance name> > 的协议”，然后双击“TCP/IP”   。  
  
3.  在“TCP/IP 属性”对话框的“IP 地址”选项卡上，将显示若干个 IP 地址，格式为：**IP1**、**IP2**...，一直到 **IPAll**。 这些 IP 地址中有一个是环回适配器的 IP 地址 (127.0.0.1)。 其他 IP 地址是计算机上配置的各个 IP 地址。  
  
4.  对于任意 IP 地址，如果 **“TCP 动态端口”** 对话框中包含 **0**，则指示 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 正在侦听动态端口。 本示例使用固定端口，而不是使用在重新启动时会发生更改的动态端口。 因此，如果 **“TCP 动态端口”** 对话框中包含 **0**，则删除 0。  
  
5.  请注意为要配置的每个 IP 地址列出的 TCP 端口。 对于本示例，假设两个 IP 地址都侦听默认端口 1433。  
  
6.  如果不希望 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用某些可用端口，则请在 **“协议”** 选项卡上将 **“全部侦听”** 值更改为 **“否”**；在 **“IP 地址”** 选项卡上，将不想使用的 IP 地址的 **“活动”** 值更改为 **“否”** 。  
  
## <a name="configuring-windows-firewall-with-advanced-security"></a>配置高级安全 Windows 防火墙  
 知道计算机所使用的 IP 地址和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所使用的端口后，就可以创建防火墙规则，然后为特定的 IP 地址配置这些规则。  
  
#### <a name="to-create-a-firewall-rule"></a>创建防火墙规则  
  
1.  在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机上，以管理员身份登录。  
  
2.  依次单击“开始” 、“运行” ，键入 **wf.msc**，然后单击“确定” 。  
  
3.  在“用户帐户控制”对话框中，单击“继续”使用管理员凭据打开高级安全 Windows 防火墙管理单元 。  
  
4.  在 **“概述”** 页上，确认已启用 Windows 防火墙。  
  
5.  在左窗格中，单击 **“入站规则”** 。  
  
6.  右键单击“入站规则”，然后单击“新建规则”以打开“新建入站规则向导”  。  
  
7.  可以为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 程序创建规则。 但是，由于本示例使用固定端口，所以请选择 **“端口”** ，然后单击 **“下一步”** 。  
  
8.  在 **“协议和端口”** 页上，选择 **TCP**。  
  
9. 选择 **“指定的本地端口”** 。 键入端口号（由逗号分隔），然后单击 **“下一步”** 。 在本示例中，你将配置默认端口；因此请输入 **1433**。  
  
10. 在 **“操作”** 页上，查看各选项。 在本示例中，不使用防火墙强制进行安全连接。 因此，请单击 **“允许连接”** ，然后单击 **“下一步”** 。  
  
    > [!NOTE]  
    >  您的环境可能要求使用安全连接。 如果选择其中一个安全连接选项，则可能必须配置证书和 **“强行加密”** 选项。 有关安全连接的详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)和[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
11. 在 **“配置文件”** 页上，为该规则选择一个或多个配置文件。 如果您不熟悉防火墙配置文件，请单击防火墙程序中的 **“了解有关配置文件的详细信息”** 链接。  
  
    -   如果计算机是服务器而且仅当连接到域时才可用，则请选择 **“域”** ，然后单击 **“下一步”** 。  
  
    -   如果计算机为移动计算机（例如便携式计算机），则它在连接到不同网络时很可能使用多个配置文件。 对于移动计算机，可以为不同的配置文件配置不同的访问功能。 例如，可以在计算机使用域配置文件时允许访问，而在计算机使用公共配置文件时不允许访问。  
  
12. 在 **“名称”** 页上，提供规则的名称和说明，然后单击 **“完成”** 。  
  
13. 重复此过程，为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将使用的每个 IP 地址创建另一个规则。  
  
 创建完一个或多个规则后，执行下列步骤以将计算机上的每个 IP 地址配置为使用规则。  
  
#### <a name="to-configure-the-firewall-rule-for-a-specific-ip-addresses"></a>为特定的 IP 地址配置防火墙规则  
  
1.  在“高级安全 Windows 防火墙”的“入站规则”页上，右键单击刚创建的规则，然后单击“属性”  。  
  
2.  在 **“规则属性”** 对话框中，选择 **“范围”** 选项卡。  
  
3.  在 **“本地 IP 地址”** 区域中，选择 **“下列 IP 地址”** ，然后单击 **“添加”** 。  
  
4.  在 **“IP 地址”** 对话框中，选择 **“此 IP 地址或子网”**，然后键入要配置的 IP 地址之一。  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
6.  在 **“远程 IP 地址”** 区域中，选择 **“下列 IP 地址”** ，然后单击 **“添加”** 。  
  
7.  使用 **“IP 地址”** 对话框为计算机上选定的 IP 地址配置连接。 可以启用源自特定 IP 地址、某些范围的 IP 地址、整个子网或源自特定计算机的连接。 若要正确配置此选项，您需要十分了解网络。 有关网络的信息，请与网络管理员联系。  
  
8.  若要关闭 **“IP 地址”** 对话框，请单击 **“确定”** ；然后单击 **“确定”** 关闭 **“规则属性”** 对话框。  
  
9. 若要配置多宿主计算机上的其他 IP 地址，请使用另一 IP 地址和另一规则重复此过程。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Browser 服务（数据库引擎和 SSAS）](../../database-engine/configure-windows/sql-server-browser-service-database-engine-and-ssas.md)   
 [通过代理服务器连接到 SQL Server（SQL Server 配置管理器）](../../database-engine/configure-windows/connect-to-sql-server-through-a-proxy-server-sql-server-configuration-manager.md)  
  
  

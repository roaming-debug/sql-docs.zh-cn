---
title: 为数据库引擎访问配置 Windows 防火墙 | Microsoft Docs
description: 了解如何配置 Windows 防火墙，以便客户端计算机可以通过防火墙访问 SQL Server 数据库引擎的实例。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- connections [SQL Server], firewall systems
- firewall systems, [Database Engine]
- security [SQL Server], firewalls
ms.assetid: 0093b43c-c6b5-4574-9b30-3a0e91e1a1f9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 03f4a469465339664e56aae784c3b734f85ba9b4
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765692"
---
# <a name="configure-a-windows-firewall-for-database-engine-access"></a>为数据库引擎访问配置 Windows 防火墙
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]


  本主题说明如何使用 SQL Server 配置管理器在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中为数据库引擎访问配置 Windows 防火墙。 防火墙系统有助于阻止对计算机资源进行未经授权的访问。 若要通过防火墙访问 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例，必须在运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机上配置防火墙以允许访问。  
  
 有关默认 Windows 防火墙设置的详细信息以及有关影响 [!INCLUDE[ssDE](../../includes/ssde-md.md)]、Analysis Services、Reporting Services 和 Integration Services 的 TCP 端口的说明，请参阅 [配置 Windows 防火墙以允许 SQL Server 访问](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)。 有很多可用的防火墙系统。 有关特定于您系统的信息，请参阅防火墙文档。  
  
 为允许访问而执行的主要步骤如下：  
  
1.  将 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 配置为使用特定的 TCP/IP 端口。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的默认实例使用端口 1433，但可以更改。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 所使用的端口在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志中列出。 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 实例、[!INCLUDE[ssEW](../../includes/ssew-md.md)] 实例以及[!INCLUDE[ssDE](../../includes/ssde-md.md)]的命名实例使用动态端口。 若要配置这些实例以使用特定端口，请参阅[配置服务器以侦听特定 TCP 端口（SQL Server 配置管理器）](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md)。  
  
2.  将防火墙配置为允许授权的用户或计算机访问此端口。  
  
> [!NOTE]  
>  使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务，用户可以连接到不在侦听端口 1433 的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，因而无需知道端口号。 若要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser，必须打开 UDP 端口 1434。 若要提升最安全的环境，请停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务，并将客户端配置为使用端口号进行连接。  
  
> [!NOTE]  
>  默认情况下， [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 将启用 Windows 防火墙，这会关闭端口 1433，从而防止 Internet 计算机连接到您计算机上的默认 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 重新打开端口 1433 之后，才可以使用 TCP/IP 连接到默认实例。 以下过程提供了配置 Windows 防火墙的基本步骤。 有关详细信息，请参阅 Windows 文档。  
  
 除了将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置为侦听固定端口并打开此端口之外，您还可以将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可执行文件 (Sqlservr.exe) 作为已阻止程序的例外列出。 如果要继续使用动态端口，则使用此方法。 通过这种方式只能访问一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要为数据库引擎访问配置 Windows 防火墙，请使用：**  
  
     [SQL Server 配置管理器](#SSMSProcedure)  
  
## <a name="before-you-begin"></a>开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
 打开防火墙的端口可能会使服务器受到恶意攻击。 请确保在打开端口之前了解防火墙系统。 有关详细信息，请参阅 [Security Considerations for a SQL Server Installation](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> 使用 SQL Server 配置管理器  
 适用于 Windows Vista、Windows 7 和 Windows Server 2008  
  
 以下过程通过使用具有高级安全 Microsoft 管理控制台 (MMC) 管理单元的 Windows 防火墙来配置该 Windows 防火墙。 高级安全 Windows 防火墙仅配置当前配置文件。 有关高级安全 Windows 防火墙的详细信息，请参阅 [配置 Windows 防火墙以允许 SQL Server 访问](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)  
  
#### <a name="to-open-a-port-in-the-windows-firewall-for-tcp-access"></a>打开 Windows 防火墙的端口以进行 TCP 访问  
  
1.  在 **“开始”** 菜单上，单击 **“运行”** ，键入 **WF.msc**，然后单击 **“确定”** 。  
  
2.  在“高级安全 Windows 防火墙”的左窗格中，右键单击“入站规则”，并在操作窗格中单击“新建规则”。    
  
3.  在 **“规则类型”** 对话框中，选择 **“端口”** ，然后单击 **“下一步”** 。  
  
4.  在 **“协议和端口”** 对话框中，选择 **TCP**。 选择“特定本地端口”，然后键入 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例的端口号，例如默认实例的端口号 **1433**。 单击“下一步”。  
  
5.  在 **“操作”** 对话框中，选择 **“允许连接”** ，然后单击 **“下一步”** 。  
  
6.  在 **“配置文件”** 对话框中，选择在您想要连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]时描述计算机连接环境的任何配置文件，然后单击 **“下一步”** 。  
  
7.  在 **“名称”** 对话框中，输入此规则的名称和说明，再单击 **“完成”** 。  
  
#### <a name="to-open-access-to-sql-server-when-using-dynamic-ports"></a>在使用动态端口时打开对 SQL Server 的访问  
  
1.  在 **“开始”** 菜单上，单击 **“运行”** ，键入 **WF.msc**，然后单击 **“确定”** 。  
  
2.  在“高级安全 Windows 防火墙”的左窗格中，右键单击“入站规则”，并在操作窗格中单击“新建规则”。    
  
3.  在 **“规则类型”** 对话框中，选择 **“程序”** ，然后单击 **“下一步”** 。  
  
4.  在 **“程序”** 对话框中，选择 **“此程序路径”** 。 单击 **“浏览”** ，导航到要通过防火墙访问的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，再单击 **“打开”** 。 默认情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 位于 **C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\Sqlservr.exe**。 单击“下一步”。  
  
5.  在 **“操作”** 对话框中，选择 **“允许连接”** ，然后单击 **“下一步”** 。  
  
6.  在 **“配置文件”** 对话框中，选择在您想要连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]时描述计算机连接环境的任何配置文件，然后单击 **“下一步”** 。  
  
7.  在 **“名称”** 对话框中，输入此规则的名称和说明，再单击 **“完成”** 。  
  
## <a name="see-also"></a>另请参阅  
 [如何：配置防火墙设置（Azure SQL 数据库）](/azure/azure-sql/database/firewall-configure)  
  

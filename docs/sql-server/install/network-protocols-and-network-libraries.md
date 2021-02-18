---
title: 网络协议和网络库 | Microsoft Docs
description: 可以配置服务器来监视多个网络协议。 可以使用 SQL Server 配置管理器来更改该配置。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- protocols [SQL Server]
- configuration options [SQL Server], protocols
- network libraries [SQL Server]
- protocols [SQL Server], about network protocols
- pipes [SQL Server]
- network protocols [SQL Server]
- default SQL Server configurations
- library [SQL Server]
- network protocols [SQL Server], about network protocols
- configuration options [SQL Server], libraries
ms.assetid: 8cd437f6-9af1-44ce-9cb0-4d10c83da9ce
author: cawrites
ms.author: chadam
ms.openlocfilehash: 81bf039f0a63aaad7a2214c48bb5948618e7ce7e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336154"
---
# <a name="network-protocols-and-network-libraries"></a>网络协议和网络库
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  服务器可以同时监听或监视多个网络协议。 但必须对每个协议都进行配置。 如果没有配置某个协议，则服务器将无法监听该协议。 安装完成后，可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器更改这些协议配置。  
  
## <a name="default-sql-server-network-configuration"></a>默认 SQL Server 网络配置  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的默认实例针对 TCP/IP 端口 1433 进行配置，并命名为管道 \\\\.\pipe\sql\query。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 命名实例配置为采用 TCP 动态端口，其端口号由操作系统分配。  
  
 如果无法使用动态端口地址（例如，当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接必须通过服务器配置为要通过特定端口地址的防火墙时）。 选择一个未分配的端口号。 Internet 号码分配机构负责管理端口号的分配，并在 [https://www.iana.org](https://go.microsoft.com/fwlink/?LinkId=48844) 上列出这些端口号。  
  
 为了增强安全性，当安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时不会完全启用网络连接。 在安装完成后，若要启用、禁用和配置网络协议，请使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 网络配置区域。  
  
## <a name="server-message-block-protocol"></a>服务器消息块协议  
 周边网络中的服务器应该禁用所有不必要的协议，其中包括服务器消息块 (SMB)。 Web 服务器和域名系统 (DNS) 服务器不需要 SMB。 为了应对用户枚举的威胁，应禁用此协议。  
  
> [!WARNING]
>  禁用服务器消息块将阻止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Windows 群集服务访问远程文件共享。 如果您执行或计划执行以下操作之一，请不要禁用 SMB：  
> 
>  -   使用 Windows 群集节点和文件共享多数仲裁模式  
> -   在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的过程中将 SMB 文件共享指定为数据目录  
> -   在 SMB 文件共享上创建数据库文件  
  
#### <a name="to-disable-smb"></a>禁用 SMB  
  
1.  在“开始”菜单中，指向“设置”，然后单击“网络和拨号连接”。  
  
     右键单击面向 Internet 的连接，然后单击“属性”。  
  
2.  选中 **“Microsoft Networks 客户端”** 复选框，然后单击 **“卸载”** 。  
  
3.  执行卸载步骤。  
  
4.  选择 **“Microsoft Networks 文件和打印机共享”** ，然后单击 **“卸载”** 。  
  
5.  执行卸载步骤。  
  
#### <a name="to-disable-smb-on-servers-accessible-from-the-internet"></a>在可通过 Internet 访问的服务器上禁用 SMB  
  
-   在本地连接属性中，使用“传播控制协议/Internet 协议 (TCP/IP) 属性”对话框删除“Microsoft 网络的文件和打印共享”和“Microsoft 网络客户端”。  
  
## <a name="endpoints"></a>终结点  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接引入了一个新概念；在服务器端用 [!INCLUDE[tsql](../../includes/tsql-md.md)]*终结点*。 可以对 [!INCLUDE[tsql](../../includes/tsql-md.md)] 端点授予、撤消和拒绝权限。 默认情况下，所有用户都具备访问端点的权限，除非 sysadmin 组的成员或端点所有者拒绝或撤消了此权限。 GRANT、REVOKE 和 DENY ENDPOINT 语法使用终结点 ID，管理员必须从终结点的目录视图中获取此 ID。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将为所有支持的网络协议以及专用管理员连接创建 [!INCLUDE[tsql](../../includes/tsql-md.md)] 端点。  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] 安装程序所创建的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 端点如下：  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] 本地计算机  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] 命名管道  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] 默认 TCP  
  
 有关终结点的详细信息，请参阅[将数据库引擎配置为侦听多个 TCP 端口](../../database-engine/configure-windows/configure-the-database-engine-to-listen-on-multiple-tcp-ports.md)和[终结点目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)。  
  
 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 网络配置的详细信息，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书中的下列文章：  
  
-   [服务器网络配置](../../database-engine/configure-windows/server-network-configuration.md)  
  
## <a name="see-also"></a>另请参阅  
 [外围应用配置器](../../relational-databases/security/surface-area-configuration.md)   
 [安装 SQL Server 的安全注意事项](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)   
 [计划 SQL Server 安装](../../sql-server/install/planning-a-sql-server-installation.md)  
  
  

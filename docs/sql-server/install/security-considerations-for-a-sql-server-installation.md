---
title: 安全注意事项
description: 本文讨论在安装 SQL Server 之前和之后应考虑的一些最佳安全做法。
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- firewall systems [SQL Server]
- server message blocks [SQL Server]
- disabling protocols
- security [SQL Server], installations
- protocols [SQL Server], disabling
- NetBIOS [SQL Server]
- services [SQL Server], security
- SMB [SQL Server]
- isolating services [SQL Server]
- Setup [SQL Server], security
- low SQL Server installation privileges
- NTFS [SQL Server]
- physical security [SQL Server]
- file system security [SQL Server]
- installing SQL Server, security
ms.assetid: cf96155f-30a8-48b7-8d6b-24ce90dafdc7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5f8a540cf1f740ad2f9cb712db4bdd51f6c1a623
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336112"
---
# <a name="security-considerations-for-a-sql-server-installation"></a>安装 SQL Server 的安全注意事项
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

 安全对于每个产品和每家企业都很重要。 遵循简单的最佳做法，可以避免很多安全漏洞。 本文讨论安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 前和安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 后应考虑的一些最佳安全做法。 这些功能的参考文章中包括了特定功能的安全指南。  
  
## <a name="before-installing-ssnoversion"></a>安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 设置服务器环境时，请遵循以下最佳做法：  
  
-   [增强物理安全性](#physical_security)  
  
-   [使用防火墙](#firewalls)  
  
-   [隔离服务](#isolated_services)  
  
-   [配置安全的文件系统](#sa_with_least_privileges)  
  
-   [禁用 NetBIOS 和服务器消息块](#disabled_protocols)  
  
-   [在域控制器上安装 SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md#Install_DC)  
  
###  <a name="enhance-physical-security"></a><a name="physical_security"></a> Enhance Physical Security  
 物理和逻辑隔离是构成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全的基础。 若要增强 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装的物理安全性，请执行以下任务：  
  
-   将服务器置于专门的房间，未经授权的人员不得入内。  
  
-   将数据库的宿主计算机置于受物理保护的场所，最好是上锁的机房，房中配备水灾检测和火灾检测监视系统或灭火系统。  
  
-   将数据库安装在公司 Intranet 的安全区域中，并且不得将 SQL Server 直接连接到 Internet。  
  
-   定期备份所有数据，并将备份存储在远离工作现场的安全位置。  
  
###  <a name="use-firewalls"></a><a name="firewalls"></a> Use Firewalls  
 防火墙对于协助确保 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装的安全十分重要。 如果遵循以下准则，防火墙将最为有效：  
  
-   在服务器和 Internet 之间放置防火墙。 启用防火墙。 如果防火墙处于关闭状态，请将其开启。 如果防火墙处于开启状态，请不要将其关闭。  
  
-   将网络分成若干安全区域，区域之间用防火墙分隔。 阻止所有流量，然后有选择性地仅接受所需内容。  
  
-   在多层环境中，使用多个防火墙创建屏蔽子网。  
  
-   如果在 Windows 域内部安装服务器，请将内部防火墙配置为允许使用 Windows 身份验证。  
  
-   如果应用程序使用分布式事务处理，可能必须要将防火墙配置为允许 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 分布式事务处理协调器 (MS DTC) 在不同的 MS DTC 实例之间进行通信。 还需要将防火墙配置为允许在 MS DTC 和资源管理器（如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]）之间进行通信。  
  
 有关默认 Windows 防火墙设置的详细信息，以及有关影响 [!INCLUDE[ssDE](../../includes/ssde-md.md)]、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]、 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]和 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]的 TCP 端口的说明，请参阅 [配置 Windows 防火墙以允许 SQL Server 访问](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)。  
  
###  <a name="isolate-services"></a><a name="isolated_services"></a> Isolate Services  
 隔离服务可降低一项遭到入侵的服务可能被用来危害其他服务的风险。 要隔离服务，请考虑以下准则：  
  
-   在不同的 Windows 帐户下运行各自的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务。 对每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务，尽可能使用不同的低权限 Windows 或本地用户帐户。 有关详细信息，请参阅 [配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)预览版本升级问题的解答。  
  
###  <a name="configure-a-secure-file-system"></a><a name="sa_with_least_privileges"></a> Configure a Secure File System  
 使用正确的文件系统可提高安全性。 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装，应执行以下几项任务：  
  
-   使用 NTFS 文件系统 (NTFS)。 NTFS 是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装的首选文件系统，因为它比 FAT 文件系统更加稳定和更容易恢复。 NTFS 还启用安全选项，例如文件和目录访问控制列表 (ACL) 和加密文件系统 (EFS) 文件加密。 在安装期间，如果检测到 NTFS， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将对注册表项和文件设置相应的 ACL。 不应对这些权限做任何更改。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本可能不支持在具有 FAT 文件系统的计算机上进行安装。  
  
    > [!NOTE]  
    >  如果使用 EFS，则将在运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的帐户的标识下加密数据库文件。 只有该帐户才能解密文件。 如果必须更改运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的帐户，则应先在旧帐户下解密文件，然后在新帐户下将文件重新加密。  
  
-   对关键数据文件使用独立磁盘冗余阵列 (RAID)。  
  
###  <a name="disable-netbios-and-server-message-block"></a><a name="disabled_protocols"></a> Disable NetBIOS and Server Message Block  
 外围网络中的服务器应禁用所有不必要的协议，包括 NetBIOS 和服务器消息块 (SMB)。  
  
 NetBIOS 使用以下端口：  
  
-   UDP/137（NetBIOS 名称服务）  
  
-   UDP/138（NetBIOS 数据报服务）  
  
-   TCP/139（NetBIOS 会话服务）  
  
 SMB 使用以下端口：  
  
-   TCP/139  
  
-   TCP/445  
  
 Web 服务器和域名系统 (DNS) 服务器不需要 NetBIOS 或 SMB。 在这些服务器上，禁用这两个协议可以减轻由用户枚举带来的威胁。  
  
###  <a name="installing-ssnoversion-on-a-domain-controller"></a><a name="Install_DC"></a> 在域控制器上安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 出于安全方面的考虑，我们建议您不要将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装在域控制器上。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序不会阻止在作为域控制器的计算机上进行安装，但存在以下限制：  
  
-   在域控制器上，无法在本地服务帐户下运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务。  
  
-   将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装到计算机上之后，无法将此计算机从域成员更改为域控制器。 必须先卸载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，然后才能将主机计算机更改为域控制器。  
  
-   将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装到计算机上之后，无法将此计算机从域控制器更改为域成员。 必须先卸载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，然后才能将主机计算机更改为域成员。  
  
-   在群集节点用作域控制器的情况下，不支持[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序不能在只读域控制器上创建安全组或设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户。 在这种情况下，安装将失败。  
  
## <a name="during-or-after-installation-of-ssnoversion"></a>在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 安装完成后，若要增强所安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 软件的安全性，请遵循以下有关帐户和身份验证模式的最佳做法：  
  
 **服务帐户**  
  
-   使用尽可能低的权限运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务。  
  
-   将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务与低特权 Windows 本地用户帐户或域用户帐户相关联。  
  
-   有关详细信息，请参阅 [配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)预览版本升级问题的解答。  
  
 **身份验证模式**  
  
-   连接 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]时要求 Windows 身份验证。  
  
-   使用 Kerberos 身份验证。 有关详细信息，请参阅 [为 Kerberos 连接注册服务主体名称](../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md)。  
  
 **强密码**  
  
-   始终为 **sa** 帐户分配强密码。  
  
-   始终启用密码策略检查以检查密码强度和有效期。  
  
-   始终对所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名使用强密码。  
  
> [!IMPORTANT]  
>  在安装 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 过程中，为 BUILTIN\Users 组添加一个登录名。 这可以使计算机的所有通过身份验证的用户作为 public 角色成员访问 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 实例。 可以安全地删除 BUILTIN\Users 登录名，以限制 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 对具有单独登录名或为使用此登录名的其他 Windows 组成员的计算机用户的访问。  
  
## <a name="see-also"></a>另请参阅  
 [安装 SQL Server 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
 [网络协议和网络库](../../sql-server/install/network-protocols-and-network-libraries.md)   
 [为 Kerberos 连接注册服务主体名称](../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md)  
  
  

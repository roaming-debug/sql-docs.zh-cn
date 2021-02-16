---
title: 设置数据库镜像（Windows 身份验证）
description: 了解如何使用 SQL Server Management Studio 通过 Windows 身份验证建立数据库镜像会话。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], sessions
ms.assetid: 7cb418d6-dce1-4a0d-830e-9c5ccfe3bd72
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: f09f731b2d8838c5cdcd4ea9861666c9d89cacf0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342353"
---
# <a name="establish-database-mirroring-session---windows-authentication"></a>建立数据库镜像会话 - Windows 身份验证
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 改为使用 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]。  
  
 若要建立数据库镜像会话并修改数据库的数据库镜像属性，请使用 **“数据库属性”** 对话框中的 **“镜像”** 页。在使用 **“镜像”** 页配置数据库镜像之前，请确保已满足下列要求：  
  
-   主体服务器实例和镜像服务器实例必须运行同一版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（Standard 或 Enterprise）。 此外，极力建议这些服务器实例在可以处理相同工作负荷的类似系统上运行。  
  
    > [!NOTE]  
    >  [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的各版本中均未提供见证服务器实例。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
-   镜像数据库必须存在并且为当前数据库。  
  
     如果创建镜像数据库，则需要还原镜像服务器实例上的主体数据库的最近备份（使用 WITH NORECOVERY）。 它还需要在完整备份之后执行一个或多个日志备份并将其依次还原到镜像数据库（使用 WITH NORECOVERY）。 有关详细信息，请参阅 [为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)的各版本中均未提供见证服务器实例。  
  
-   如果服务器实例使用不同的域用户帐户运行，则每个实例还需要在其他实例的 **master** 数据库中具有登录名。 如果登录名不存在，则必须在配置镜像之前创建登录名。 有关详细信息，请参阅 [允许使用 Windows 身份验证对数据库镜像终结点进行网络访问 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)。  
  
### <a name="to-configure-database-mirroring"></a>配置数据库镜像  
  
1.  连接到主体服务器实例之后，在对象资源管理器中，单击服务器名称以展开服务器树。  
  
2.  展开 **“数据库”** ，再选择要镜像的数据库。  
  
3.  右键单击数据库，选择 **“任务”** ，再单击 **“镜像”** 。 这样便可打开 **“数据库属性”** 对话框的 **“镜像”** 页。  
  
4.  若要开始配置镜像，请单击 **“配置安全性”** 按钮以启动配置数据库镜像安全向导。  
  
    > [!NOTE]  
    >  在数据库镜像会话期间，仅可使用此向导添加或更改见证服务器实例。  
  
5.  配置数据库镜像安全向导将在每个服务器实例上自动创建数据库镜像端点（如果不存在任何端点），并在与服务器实例角色（ **“主体”** 、 **“镜像服务器”** 或 **“见证服务器”** ）相对应的字段中输入服务器网络地址。  
  
    > [!IMPORTANT]  
    >  创建端点时，配置数据库镜像安全向导始终使用 Windows 身份验证。 在将此向导与基于证书的身份验证配合使用之前，必须已在每个服务器实例上始终将镜像端点配置为使用证书。 此外，此向导的 **“服务帐户”** 对话框中的所有字段必须保持为空。 有关创建数据库镜像终结点以使用证书的信息，请参阅[ CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md)。  
  
6.  根据需要，可以选择更改运行模式。 某些运行模式的可用性取决于是否为见证服务器指定了 TCP 地址。 选项如下：  
  
    |选项|是否需要见证服务器？|说明|  
    |------------|--------------|-----------------|  
    |**高性能(异步)**|空(如果存在，尚未使用但会话需要仲裁)|为获得最佳性能，镜像数据库始终在某种程度上滞后于主体数据库，永远无法完全同步。 但是，数据库之间的异步间隔通常很小。 丢失伙伴会产生以下影响：<br /><br /> 如果镜像服务器实例变为不可用，则主体服务器继续可用。<br /><br /> 如果主体服务器实例变为不可用，则镜像服务器实例会停止；但是，如果会话没有见证服务器（按建议）或见证服务器连接到镜像服务器，则镜像服务器可以作为备用；数据库所有者可以强制让镜像服务器实例来提供服务（可能造成数据丢失）。<br /><br /> <br /><br /> 有关详细信息，请参阅 [数据库镜像会话期间的角色切换 (SQL Server)](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)的各版本中均未提供见证服务器实例。|  
    |**不带自动故障转移功能的高安全(同步)**|否|保证将所有提交的事务都写入镜像服务器的磁盘上。<br /><br /> 只要伙伴相互连接并且数据库已同步，便可进行手动故障转移。<br /><br /> 丢失伙伴会产生以下影响：<br /><br /> 如果镜像服务器实例变为不可用，则主体服务器继续可用。<br /><br /> 如果主体服务器实例变为不可用，则镜像服务器实例会停止但仍可以作为备用；数据库所有者可以强制让镜像服务器实例来提供服务（可能造成数据丢失）。<br /><br /> <br /><br /> 有关详细信息，请参阅 [数据库镜像会话期间的角色切换 (SQL Server)](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)的各版本中均未提供见证服务器实例。|  
    |**带自动故障转移功能的高安全(同步)**|是（必需）|保证将所有提交的事务都写入镜像服务器的磁盘上。<br /><br /> 通过包含见证服务器实例以支持自动故障转移，来实现最高可用性。 注意，只有首先指定了见证服务器地址，才可以选择 **“带自动故障转移功能的高安全(同步)”** 选项。<br /><br /> 只要伙伴相互连接并且数据库已同步，便可进行手动故障转移。<br /><br /> 如果存在见证服务器，丢失伙伴连接会有以下影响：<br /><br /> 如果主体服务器实例变为不可用，则会发生自动故障转移。 镜像服务器实例将充当主体服务器，并且将其数据库用作主体数据库。<br /><br /> 如果镜像服务器实例变为不可用，则主体服务器继续可用。<br /><br /> <br /><br /> 有关详细信息，请参阅 [数据库镜像会话期间的角色切换 (SQL Server)](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)的各版本中均未提供见证服务器实例。<br /><br /> **&#42;&#42; 重要提示 &#42;&#42;** 如果见证服务器断开连接，则伙伴必须彼此连接，数据库才可用。 有关详细信息，请参阅[仲裁：见证服务器如何影响数据库可用性（数据库镜像）](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md)。|  
  
7.  满足下列所有条件后，单击 **“开始镜像”** 以开始镜像：  
  
    -   您当前已连接到主体服务器实例。  
  
    -   安全性已经正确配置。  
  
    -   已指定主体服务器实例和镜像服务器实例的完全限定的 TCP 地址（在 **“服务器网络地址”** 部分）。  
  
    -   如果运行模式设置为 **“带自动故障转移功能的高安全(同步)”** ，则还需指定见证服务器实例的完全限定的 TCP 地址。  
  
8.  在镜像开始后，您可以更改运行模式，并可以通过单击 **“确定”** 来保存更改。 注意，仅当先指定了见证服务器地址时，才能利用自动故障转移切换到高安全模式。  
  
    > [!NOTE]  
    >  若要删除见证服务器，请从 **“见证服务器”** 字段中删除它的服务器网络地址。 如果从具有自动故障转移功能的高安全性模式切换到高性能模式，则将自动清除“见证服务器”  字段。  
  
## <a name="see-also"></a>另请参阅  
 [数据库镜像会话期间的角色切换 (SQL Server)](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)   
 [为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)   
 [数据库属性（“镜像”页）](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [暂停或恢复数据库镜像会话 (SQL Server)](../../database-engine/database-mirroring/pause-or-resume-a-database-mirroring-session-sql-server.md)   
 [将镜像数据库设置为使用 Trustworthy 属性 (Transact-SQL)](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)   
 [删除数据库镜像 (SQL Server)](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)   
 [角色切换后登录名和作业的管理 (SQL Server)](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)   
 [设置数据库镜像 (SQL Server)](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [当数据库在其他服务器实例上可用时管理元数据 (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [添加或替换数据库镜像见证服务器 (SQL Server Management Studio)](../../database-engine/database-mirroring/add-or-replace-a-database-mirroring-witness-sql-server-management-studio.md)  
  
  

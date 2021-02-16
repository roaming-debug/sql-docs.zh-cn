---
title: 选择身份验证模式 | Microsoft Docs
description: 在安装时为 SQL Server 数据库引擎选择“Windows 身份验证模式”或“混合模式身份验证”。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.ins.instwizard.authenticationmode.f1
- sql13.swb.passwordexpired.f1
helpviewer_keywords:
- sa account
- authentication modes
- trusted connection
- SQL Server Installation Wizard, Authentication Mode page
- choose authentication mode
- authentication [SQL Server], choosing a mode
- Windows authentication [SQL Server]
- mixed mode authentication
- mixed authentication mode
- SQL authentication mode
- Password Expired dialog box
ms.assetid: ff7a6a48-3d38-4209-aa0f-7d6c0a8c64ef
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ebcf99ad025477a562f2a050eb1c2c3f3f63e12f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344404"
---
# <a name="choose-an-authentication-mode"></a>选择身份验证模式
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  在安装过程中，必须为 [!INCLUDE[ssDE](../../includes/ssde-md.md)]选择身份验证模式。 下面是两种可能的模式：Windows 身份验证模式和混合模式。 Windows 身份验证模式会启用 Windows 身份验证并禁用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。 混合模式会同时启用 Windows 身份验证和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。 Windows 身份验证始终可用，并且无法禁用。  
  
## <a name="configuring-the-authentication-mode"></a>配置身份验证模式  
 如果在安装过程中选择混合模式身份验证，则必须为名为 sa 的内置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统管理员帐户提供一个强密码并确认该密码。 sa 帐户通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证进行连接。  
  
 如果在安装过程中选择 Windows 身份验证，则安装程序会为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证创建 sa 帐户，但会禁用该帐户。 如果稍后更改为混合模式身份验证并要使用 sa 帐户，则必须启用该帐户。 您可以将任何 Windows 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户配置为系统管理员。 由于 sa 帐户广为人知且经常成为恶意用户的攻击目标，因此除非应用程序需要使用 sa 帐户，否则请勿启用该帐户。 切勿为 sa 帐户设置空密码或弱密码。 若要从 Windows 身份验证模式更改为混合模式身份验证并使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证，请参阅 [更改服务器身份验证模式](../../database-engine/configure-windows/change-server-authentication-mode.md)。  
  
## <a name="connecting-through-windows-authentication"></a>通过 Windows 身份验证进行连接  
 当用户通过 Windows 用户帐户连接时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用操作系统中的 Windows 主体标记验证帐户名和密码。 也就是说，用户身份由 Windows 进行确认。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不要求提供密码，也不执行身份验证。 Windows 身份验证是默认身份验证模式，并且比 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证更为安全。 Windows 身份验证使用 Kerberos 安全协议，提供有关强密码复杂性验证的密码策略强制，还提供帐户锁定支持，并且支持密码过期。 通过 Windows 身份验证创建的连接有时也称为可信连接，这是因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 信任由 Windows 提供的凭据。  
  
 通过使用 Windows 身份验证，可以在域级别创建 Windows 组，并且可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中为整个组创建登录名。 在域级别管理访问可以简化帐户管理。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
## <a name="connecting-through-sql-server-authentication"></a>通过 SQL Server 身份验证进行连接  
 当使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证时，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中创建的登录名并不基于 Windows 用户帐户。 用户名和密码均通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 创建并存储在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中。 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证进行连接的用户每次连接时都必须提供其凭据（登录名和密码）。 当使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证时，必须为所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户设置强密码。 有关强密码的指南，请参阅 [Strong Passwords](../../relational-databases/security/strong-passwords.md)。  
  
 可供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名选择使用的密码策略有三种。  
  
-   用户在下次登录时必须更改密码  
  
     要求用户在下次连接时更改密码。 更改密码的功能由 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]提供。 如果使用该选项，则第三方软件开发人员应提供此功能。  
  
-   强制密码过期  
  
     对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名强制实施计算机的密码最长使用期限策略。  
  
-   强制实施密码策略  
  
     对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名强制实施计算机的 Windows 密码策略。 这包括密码长度和密码复杂性。 此功能需要通过 `NetValidatePasswordPolicy` API 实现，该 API 只在 [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)] 和更高版本中提供。  
  
#### <a name="to-determine-the-password-policies-of-the-local-computer"></a>确定本地计算机的密码策略  
  
1.  在 **“开始”** 菜单上，单击 **“运行”** 。  
  
2.  在“运行”对话框中，键入 **secpol.msc**，然后单击“确定”。  
  
3.  在 **“本地安全设置”** 应用程序中，依次展开 **“安全设置”** 、 **“帐户策略”** ，然后单击 **“密码策略”** 。  

     密码策略将如结果窗格中所示。  
  
### <a name="disadvantages-of-sql-server-authentication"></a>SQL Server 身份验证的缺点  
  
-   如果用户是拥有 Windows 登录名和密码的 Windows 域用户，则还必须提供另一个 ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) 登录名和密码才能连接。 记住多个登录名和密码对于许多用户而言都较为困难。 每次连接到数据库时都必须提供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 凭据也十分烦人。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证无法使用 Kerberos 安全协议。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名不能使用 Windows 提供的其他密码策略。  
  
-   必须在连接时通过网络传递已加密的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证登录密码。 一些自动连接的应用程序将密码存储在客户端。 这可能产生其他攻击点。  
  
### <a name="advantages-of-sql-server-authentication"></a>SQL Server 身份验证的优点  
  
-   允许 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持那些需要进行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的旧版应用程序和由第三方提供的应用程序。  
  
-   允许 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持具有混合操作系统的环境，在这种环境中并不是所有用户均由 Windows 域进行验证。  
  
-   可让用户从未知或不受信任的域进行连接。 例如，既定客户使用指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名进行连接以接收其订单状态的应用程序。  
  
-   允许 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持基于 Web 的应用程序，在这些应用程序中用户可创建自己的标识。  
  
-   允许软件开发人员通过使用基于已知的预设 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的复杂权限层次结构来分发应用程序。  
  
    > [!NOTE]  
    >  使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证不会限制安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机上的本地管理员权限。  
  
## <a name="see-also"></a>另请参阅  
 [安装 SQL Server 的安全注意事项](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
  
  

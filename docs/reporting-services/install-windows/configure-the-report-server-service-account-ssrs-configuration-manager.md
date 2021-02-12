---
title: 配置报表服务器服务帐户（配置管理器）| Microsoft Docs
description: Reporting Services 是作为单个服务实现的，其中包含报表服务器 Web 服务、Web 门户以及用于计划的报告处理和订阅传递的后台处理应用程序。
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.custom: seo-lt-2019, seo-mmd-2019
ms.date: 06/09/2020
ms.openlocfilehash: 530478c30885e603d73a5bd35fe3a759b607b836
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014898"
---
# <a name="configure-the-report-server-service-account-report-server-configuration-manager"></a>配置报表服务器服务帐户（报表服务器配置管理器）

  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 作为单个服务实现，其中包含报表服务器 Web 服务、 [!INCLUDE[ssRSWebPortal-Non-Markdown](../../includes/ssrswebportal-non-markdown-md.md)]以及用于计划的报告处理和订阅传递的后台处理应用程序。 本主题说明最初如何配置服务帐户以及如何使用 Reporting Services 配置工具修改帐户或密码。  
  
## <a name="initial-configuration"></a>初始配置

 报表服务器服务帐户是在安装过程中定义的。 可以在域用户帐户或内置帐户（如 **虚拟服务帐户**）下运行服务。 没有默认帐户；你在安装向导的“服务帐户”页中指定的任何帐户都将成为报表服务器服务的初始帐户  。  
  
> [!IMPORTANT]  
> 尽管报表服务器 Web 服务和 [!INCLUDE[ssRSWebPortal-Non-Markdown](../../includes/ssrswebportal-non-markdown-md.md)] 是单独的 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 应用程序，但是它们都在同一个报表服务器进程标识中的单个服务体系结构下运行。

> [!NOTE]  
> 不支持将内置 Windows 服务帐户（Local Service 或 Network Service）用作作为域控制器的计算机上的报表服务器服务帐户。
  
## <a name="changing-the-service-account"></a>更改服务帐户

 若要查看和重新配置服务帐户信息，请始终使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器。 服务标识信息在内部存储在多个位置上。 使用该工具可确保在更改帐户或密码的同时相应地更新所有引用。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器将执行以下附加步骤以确保报表服务器仍然可用：  
  
- 自动将新帐户添加到本地计算机上创建的报表服务器组中。 此组是在用于保护 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 文件的访问控制列表 (ACL) 中指定的。  
  
- 自动更新用于托管报表服务器数据库的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例的登录权限。 新帐户已添加到 RSExecRole  。  
  
     旧帐户的数据库登录名不会被自动删除。 请务必删除不再使用的帐户。 有关详细信息，请参阅[管理报表服务器数据库（SSRS 本机模式）](../../reporting-services/report-server/administer-a-report-server-database-ssrs-native-mode.md)。  
  
     只有在首先将报表服务器数据库连接配置为使用新服务帐户的情况下，才需要将数据库权限授予该服务帐户。 如果将报表服务器数据库连接配置为使用域用户帐户或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库登录名，连接信息将不受服务帐户更新的影响。  
  
- 自动更新加密密钥，以包括新帐户的配置文件信息。  
  
    > [!NOTE]  
    > 如果报表服务器是扩展部署的一部分，则只有正在更新的报表服务器会受到影响。 该部署中的其他报表服务器的加密密钥不会受到服务帐户更改的影响。  

## <a name="to-configure-the-report-server-service-account"></a>配置报表服务器服务帐户  
  
1. 启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器并连接到报表服务器。  
  
2. 在“服务帐户”页上，选择描述您要使用的帐户类型的选项。  
  
3. 如果选择了一个 Windows 用户帐户，请指定新的帐户名和密码。 根据 Windows 用户帐户命名规则，帐户不能超过 20 个字符， 且不能包含特殊字符 " / \ [ ] : ; | = , + * ? < > '。  
  
     如果报表服务器部署在支持 Kerberos 身份验证的网络中，则必须使用指定的域用户帐户注册报表服务器服务主体名称 (SPN)。 有关详细信息，请参阅[为报表服务器注册服务主体名称 (SPN)](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md)。  
  
4. 单击“应用”。  
  
5. 当系统提示您备份对称密钥时，请键入对称密钥备份的文件名和位置，并键入用于锁定和解锁该文件的密码，然后单击 **“确定”**。  
  
6. 如果报表服务器使用该服务帐户连接到报表服务器数据库，则连接信息更新为使用新的帐户或密码。 更新连接信息要求连接到数据库。 如果出现 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的“数据库连接”对话框，请输入拥有连接到数据库的权限的凭据，然后单击“确定”。  
  
7. 当系统提示您还原对称密钥时，请键入在步骤 5 中指定的密码，并单击 **“确定”** 。  
  
8. 查看“结果”窗格中的状态消息以验证所有任务是否均已成功完成。  
  
## <a name="choosing-an-account"></a>选择帐户

 为了实现最佳的结果，请指定一个拥有网络连接权限、可以访问网络域控制器和公司 SMTP 服务器或网关的帐户。 下表汇总了各个帐户，并为使用这些帐户提供了建议。  
  
|帐户|说明|  
|-------------|-----------------|  
|域用户帐户|如果您有一个拥有报表服务器操作所需的最小权限的 Windows 域用户帐户，则应使用此帐户。<br /><br /> 之所以建议使用域帐户，是因为这种帐户可以将报表服务器服务与其他应用程序隔离开。 使用共享帐户（如 Network Service）运行多个应用程序会增加恶意用户控制报表服务器的风险，因为在这种情况下，任何一个应用程序的安全漏洞会很容易扩散到使用同一帐户运行的所有其他应用程序。<br /><br /> 如果使用域用户帐户，并且组织实施了密码过期策略，则必须定期更改密码。 您可能还需要使用此用户帐户注册服务。 有关详细信息，请参阅[为报表服务器注册服务主体名称 (SPN)](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md)。<br /><br /> 避免使用本地 Windows 用户帐户。 本地帐户通常没有足够的权限访问其他计算机上的资源。 有关如何使用本地帐户限制报表服务器功能的详细信息，请参阅本主题中的 [使用本地帐户的注意事项](#localaccounts) 。| 
| **组托管服务帐户 (gMSA)** | Windows Server 2008 R2 和 Windows 7 中引入了独立托管服务帐户。 它们是提供自动密码管理和简化的 SPN 管理（包括将管理委派给其他管理员）的托管域帐户。 组托管服务帐户在域中提供相同的功能，但在多个服务器上扩展该功能。 |
|**虚拟服务帐户**|**虚拟服务帐户** 表示 Windows 服务。 它是一个拥有网络登录权限的内置最低特权帐户。 如果没有可用的域用户帐户，或者要避免因密码过期策略而可能导致的任何服务中断，建议使用此帐户。|  
|**Network Service**|Network Service 是一个拥有网络登录权限的内置最低特权帐户。 <br /><br /> 如果您选择 **Network Service**，请尝试将使用同一帐户运行的其他服务的数量降到最低。 对于使用同一帐户运行的多个应用程序，如果一个应用程序出现安全漏洞，则所有其他应用程序的安全都会受到影响。|  
|**Local Service**|**Local Service** 是一个与经过身份验证的本地 Windows 用户帐户类似的内置帐户。 以 **“Local Service”** 帐户运行的服务将以一个没有凭据的 Null 会话形式访问网络资源。 此帐户不适合于 Intranet 部署方案。因为在此部署方案下，报表服务器必须连接至远程报表服务器数据库或网络域控制器，以在打开报表或处理订阅之前对用户进行身份验证。|  
|**Local System**|**Local System** 是一个高特权帐户，运行报表服务器时不需要此帐户。 请勿将此帐户用于报表服务器的安装。 此时应选择域帐户或 **Network Service** 。|  
  
## <a name="considerations-for-using-local-accounts"></a><a name="localaccounts"></a> 使用本地帐户的注意事项

 使用本地帐户的主要注意事项是报表服务器是否需要访问远程数据库服务器、邮件服务器和域控制器。 如果将报表服务器配置为作为本地 Windows 用户帐户、Local Service 或 Local System 运行，则必须考虑如何设置其他配置设置以及订阅创建和传递方面的注意事项：  
  
- 如果使用本地帐户运行服务，则以后在配置远程报表服务器数据库的连接时，选项会受到限制。 具体来说，如果使用的是远程报表服务器数据库，则必须将连接配置为使用有权登录到远程 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的域用户帐户或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库用户。  
  
- 在本地帐户下运行服务将对订阅的创建具有新的要求。 报表服务器存储创建订阅的用户的信息。 如果用户在使用域帐户登录时创建订阅，则在处理订阅时，报表服务器服务会尝试连接到域控制器以对用户进行身份验证。 如果此服务使用本地帐户运行，则在报表服务器尝试将身份验证请求发送到远程域控制器时，此请求会失败。 若要消除此限制，可以使用基于窗体的自定义身份验证扩展插件或让所有用户使用本地用户帐户连接到报表服务器。  
  
- 在本地帐户下运行服务将对订阅的传递具有新的要求。 某些传递扩展插件在订阅定义中包含有用户帐户信息。 如果要将报表发送到基于域用户帐户的电子邮件地址，并且使用本地帐户运行报表服务器服务，则此服务将无法访问远程域控制器以解析目标电子邮件帐户。  
  
- 不支持将内置 Windows 服务帐户（Local Service 或 Network Service）用作作为域控制器的计算机上的报表服务器服务帐户。  
  
本部分中的下列指南和链接可以帮助您确定部署的最佳方法。  
  
- [配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)。  
  
- [服务和服务帐户安全计划指南](https://www.zubairalexander.com/blog/services-and-service-accounts-security-planning-guide/)。  
  
## <a name="updating-an-expired-password"></a>更新过期密码

 如果报表服务器服务在域帐户下运行，并且密码还未在报表服务器配置管理器中更新就已过期，则指定新密码之前，该服务无法启动。  
  
 如果 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的服务帐户密码已过期，则尝试连接到报表服务器时，将出现 **rsReportServerDatabaseUnavailable** 错误。 重置密码可以解决此错误。  
  
## <a name="troubleshooting-service-identity-update-errors"></a>服务标识更新错误故障排除

 更改服务标识将启动一系列事件，其中包括重新启动服务、更新受密码保护的加密密钥、更新 URL 预留以及更新报表服务器数据库连接信息（如果使用该服务帐户来连接报表服务器数据库）。 您可以通过查看页面底部“结果”窗格中的通知来监视这些事件的状态。 如果在此过程中出现错误，可以尝试使用以下方法进行解决：  
  
- 如果无法还原对称密钥，可以尝试使用“加密密钥”页中的“还原”手动还原它。 如果这不起作用，可以考虑删除加密的内容。 必须重新创建数据源连接信息和订阅，但其余内容仍然可用。 有关详细信息，请参阅 [Back Up and Restore Reporting Services Encryption Keys](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)。  
  
- 如果服务未启动，请使用管理工具中的“服务”控制台应用程序手动重新启动它。  
  
- 更新服务帐户时可能会发生 URL 预留错误。 每个 URL 预留都包含一个安全描述符，其中包含授权该服务帐户接受该 URL 上的请求的自由访问控制列表 (DACL)。 更新帐户时，必须重新创建该 URL，以便用新帐户信息更新 DACL。 如果无法重新创建 URL 预留，并且你知道该帐户是有效的，请尝试重新启动计算机。 如果错误仍然存在，请尝试使用不同的帐户。  
  
## <a name="next-steps"></a>后续步骤

 [配置报表服务器 URL（报表服务器配置管理器）](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md) [报表服务器配置管理器（本机模式）](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   

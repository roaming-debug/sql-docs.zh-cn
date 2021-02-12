---
title: 配置无人参与的执行帐户 (Configuration Manager) | Microsoft Docs
description: Reporting Services 提供一个特殊帐户，用于进行无人参与的报表处理和通过网络发送连接请求。
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
helpviewer_keywords:
- no credentials option [Reporting Services]
- security [Reporting Services], unattended report processing
- unattended report processing [Reporting Services]
- credentials [Reporting Services]
- external data sources [Reporting Services]
- accounts [Reporting Services]
- reports [Reporting Services], processing
ms.assetid: 4e50733e-bd8c-4bf6-8379-98b1531bb9ca
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 549a1659bcbcf954a4039a152c5b372a758d0c74
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014827"
---
# <a name="configure-the-unattended-execution-account-report-server-configuration-manager"></a>配置无人参与的执行帐户（报表服务器配置管理器）
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 提供一个特殊帐户，用于进行无人参与的报表处理和通过网络发送连接请求。 可以通过下列方式使用该帐户：  
  
-   通过网络为使用数据库身份验证的报表发送连接请求，或连接到不需要或不使用身份验证的外部报表数据源。 有关详细信息，请参阅[为报表数据源指定凭据和连接信息](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)

-   检索在报表中使用的外部图像文件。 如果您要使用图像文件并且无法通过匿名访问来访问该文件，则可以配置无人参与的报表处理帐户并授予该帐户访问该文件的权限。  
  
 无人参与的报表处理是指任何由事件（计划驱动事件或数据刷新事件）而不是用户请求触发的报表执行过程。 报表服务器使用无人参与的报表处理帐户来登录承载外部数据源的计算机。 由于报表服务器服务帐户从不用于连接到其他计算机，因此该帐户是必需的。  
  
> [!IMPORTANT]  
>  配置该帐户的过程为可选操作。 但是，如果不配置该帐户，用于连接到某些数据源的选项会受到限制，并且可能无法从远程计算机检索图像文件。 如果配置了该帐户，则必须对其进行不断更新。 具体来说，如果允许密码过期或在 Active Directory 中更改了帐户信息，则在下次处理报表时将遇到以下错误：“登录失败 (rsLogonFailed) 登录失败: 未知的用户名或密码不正确。” 即使您从不检索外部图像也不向外部计算机发送连接请求，正确维护无人参与的报表处理帐户也是必要的。 如果配置了该帐户但后来发现不需要使用它，则可以将其删除以避免日常的帐户维护任务。  
  
## <a name="how-to-configure-the-account"></a>如何配置帐户  
 必须使用域用户帐户。 若要发挥该帐户应有的作用，它应不同于用于运行报表服务器服务的帐户。 请确保使用具有如下特征的帐户：对为报表服务器提供数据源和资源的那些计算机只拥有最小权限（具有网络连接权限的只读访问权限就已足够）和有限访问权限。  
  
 若要指定帐户，可使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具或 **rsconfig** 实用工具。 配置无人参与的执行帐户的最简便方法是运行 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具，然后在“执行帐户”页中指定凭据。  
  
1.  启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具，然后连接到要配置的报表服务器实例。 有关说明，请参阅[报表服务器配置管理器（本机模式）](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)。  
  
2.  在“执行帐户”页上，选择 **“指定执行帐户”** 。  
  
3.  键入帐户和密码，再次键入密码，然后单击 **“应用”** 。  
  
### <a name="using-rsconfig-utility"></a>使用 RSCONFIG 实用工具  
 设置帐户的另一种方法是使用 **rsconfig** 实用工具。 若要指定帐户，请使用 **rsconfig** 的 **-e** 参数。 为 **rsconfig** 指定 **-e** 参数可强制该实用工具将帐户信息写入到配置文件中。 您无需指定 RSreportserver.config 的路径。请按照以下步骤来配置该帐户。  
  
1.  创建或选择一个有权访问为报表服务器提供数据或服务的计算机和服务器的域帐户。 您应使用权限受到限制（如只读权限）的帐户。  
  
2.  打开命令提示符窗口：在 **“开始”** 菜单上，单击 **“运行”** ，键入 **cmd**，再单击 **“确定”** 。  
  
3.  键入以下命令，为本地报表服务器实例配置该帐户：  
  
     **rsconfig -e -u\<domain/username> -p\<password>**  
  
 **rsconfig -e** 支持其他参数。 要详细了解语法并查看命令示例，请参阅 [rsconfig 实用工具 &#40;SSRS&#41;](../../reporting-services/tools/rsconfig-utility-ssrs.md).
 
### <a name="how-account-information-is-stored"></a>帐户信息的存储方式  
 设置帐户后，将在本地或远程报表服务器实例上的 RSreportserver.config 文件中以加密值的形式指定以下设置：  
  
```  
<UnattendedExecutionAccount>  
     <UserName></UserName>  
     <Password></Password>  
     <Domain></Domain>  
</UnattendedExecutionAccount>  
```  
  
 设置值后，将无法通过解密以纯文本的形式查看这些值。 如果输错了值或忘记了指定的值，则必须使用 Reporting Services 配置工具或运行 **rsconfig -e** 重新设置。  
  
## <a name="how-to-use-the-unattended-report-processing-account"></a>无人参与的报表处理帐户的使用方法  
 若要检索图像文件，报表服务器将自动使用该帐户，您不需要执行任何具体操作。 若要使用此帐户连接到为报表提供数据的外部数据源，则必须在报表数据源或共享数据源的“数据源属性”页中指定 **“凭据类型”** 选项：  
  
-   在 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 或 SharePoint 网站中，选择“不需要凭据”选项  。  

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。
  
 无人参与的报表处理帐户主要用于连接到外部服务器，而不是用作数据库服务器的登录名。 如果要使用此帐户凭据登录到数据库，则必须在连接字符串中指定凭据。 如果数据库服务器支持 Windows 集成安全性，并且用于无人参与报表处理的帐户拥有数据库读取权限，则可以指定 **Integrated Security=SSPI** 。 否则，必须在连接字符串中输入用户名和密码，该字符串对拥有数据源连接属性编辑权限的任何用户均显示为明文形式。  
  
 虽然系统不会阻止您使用无人参与的报表处理帐户在建立连接后检索数据，但是不建议这样做。 该帐户应该用于非常具体的功能。 如果使用该帐户检索数据，则有违本意。  
  
## <a name="how-to-maintain-the-unattended-report-processing-account"></a>无人参与的报表处理帐户的维护方法  
 定义此帐户后，必须确保不断更新此帐户和密码。 可以使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具来更新用于存储此帐户相关信息的配置设置。  
  
1.  启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具，然后连接到要配置的报表服务器实例。  
  
2.  在“执行帐户”页上，确认已选中 **“指定执行帐户”** 。  
  
3.  键入新帐户或密码，再次键入密码，然后单击 **“应用”** 。  
  
## <a name="how-to-delete-the-unattended-report-processing-account"></a>无人参与的报表处理帐户的删除方法  
 如果您不需要使用该帐户，则可以将其删除以避免日常的帐户维护任务。  
  
1.  启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具，然后连接到要配置的报表服务器实例。  
  
2.  在“执行帐户”页上，清除 **“指定执行帐户”** 。  
  
3.  单击“应用”  。  
  
 将从 RSReportServer.config 文件中删除此帐户的信息。  
  
## <a name="see-also"></a>另请参阅  
 [报表服务器配置管理器（SSRS 本机模式）](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  
  
  

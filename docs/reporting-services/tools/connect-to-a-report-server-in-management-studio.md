---
title: 在 Management Studio 中连接到报表服务器 | Microsoft Docs
description: 了解如何在 SQL Server Management Studio 中使用对象资源管理器连接到 SQL Server 系列中的任何服务器，并以图形方式浏览其内容。
ms.date: 05/07/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
f1_keywords:
- sql13.swb.connecttors.connectionproperties.f1
- sql13.swb.connecttors.login.f1
- sql13.swb.connection.login.reportserver.f1
helpviewer_keywords:
- report servers [Reporting Services], connections
- connections [Reporting Services], report server
- registering report servers
- report servers [Reporting Services], registering
- Connect to Server dialog box, Reporting Services
ms.assetid: c875ff87-ee7d-443a-a702-bdb4b6c27c6e
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 735c7805d1deba9861a6e51249acf9928d97db7c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076768"
---
# <a name="connect-to-a-report-server-in-management-studio"></a>在 Management Studio 中连接到报表服务器

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 提供了对象资源管理器，可用于连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系列中的任何服务器，并以图形方式浏览其内容。 对于 Reporting Services，可以使用对象资源管理器执行以下任务：

- 启用报表服务器功能。

- 设置服务器默认值并配置角色定义。

- 管理正在运行的作业。

- 管理作业计划。

 您可以连接到本机模式的报表服务器，也可以连接到在 SharePoint 集成模式下运行的报表服务器。 连接语法和可执行的操作类型取决于报表服务器的服务器模式和你的权限。 如果无法连接到报表服务器，或者在执行特定任务时遇到问题，你可能没有足够的权限，或者错误地指定了报表服务器的名称。 有关权限和连接语法的详细信息，请参阅本文结尾处的表格。

 不能使用对象资源管理器来查看或管理报表服务器内容。 如果报表服务器在本机模式下运行，则通过 Web 门户执行内容管理；如果报表服务器在 SharePoint 集成模式下运行，则通过 SharePoint 站点执行内容管理。

 通过对象资源管理器，可以与同一工作区中的多个服务器实例建立连接，条件是这些服务器是在同一个服务器组中注册的。 在连接到 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]中的报表服务器实例之前，必须注册该服务器。 如果已注册报表服务器，则可跳过此步骤。 本文结尾介绍了有关注册报表服务器的说明。

### <a name="to-connect-to-a-native-mode-report-server"></a>连接到本机模式的报表服务器

1. 如果对象资源管理器尚未打开报表服务器，请从“视图”菜单中选择该服务器。

2. 选择“连接”查看服务器类型列表，然后选择“Reporting Services”。

3. 在 **“连接到服务器”** 对话框中，输入报表服务器实例的名称。 报表服务器实例的名称取决于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。 默认情况下，本地报表服务器实例的实例名称就是计算机的名称。 如果将报表服务器作为命名实例进行安装，请使用此语法来指定服务器：\<servername>[\\<instancename\>]。

4. 选择“身份验证类型”。 如果使用的是 Windows 身份验证，则使用凭据进行连接。 如果选择“基本身份验证”或“窗体身份验证”，请键入帐户和密码。  
  
5. 选择“连接”。 此时，相应的报表服务器将显示在对象资源管理器中。  

6. 右键单击服务器节点以设置系统属性和服务器默认值。 有关详细信息，请参阅[设置报表服务器属性 (Management Studio)](../../reporting-services/tools/set-report-server-properties-management-studio.md)。

### <a name="to-connect-to-a-sharepoint-integrated-mode-report-server"></a>连接到 SharePoint 集成模式的报表服务器  

1. 如果对象资源管理器尚未打开报表服务器，请从“视图”菜单中选择该服务器。

2. 选择“连接”查看服务器类型列表，然后选择“Reporting Services”。

3. 在 **“连接到服务器”** 对话框中，输入指向 SharePoint 站点的 URL。 下例说明了相应的语法：`https://<web server>/sites/<site>`。

4. 选择“身份验证类型”。 如果使用的是 Windows 身份验证，则必须使用凭据进行连接。 如果选择“基本身份验证”或“窗体身份验证”，请键入帐户和密码。

5. 选择“连接”。 此时，相应的报表服务器将显示在对象资源管理器中。

6. 右键单击服务器节点以设置系统属性和服务器默认值。 有关详细信息，请参阅[设置报表服务器属性 (Management Studio)](../../reporting-services/tools/set-report-server-properties-management-studio.md)。

### <a name="to-register-a-report-server"></a>注册报表服务器

1. 如果无法连接到报表服务器，则可能是你没有访问报表服务器的权限，也可能是未注册服务器。 若要注册服务器，请选择“视图”菜单 > “已注册的服务器”。

2. 选择“Reporting Services”图标。

3. 右键单击“Reporting Services” > “新建” > “服务器注册”。 此时，将显示 **“新建服务器注册”** 对话框。

4. 在 **“服务器名称”** 中，输入一个值。 根据服务器模式指定值：

    - 对于本机模式的报表服务器，请键入报表服务器实例的名称。 报表服务器实例的名称取决于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。 默认情况下，本地报表服务器实例的实例名称就是计算机的名称。 如果将报表服务器作为命名实例进行安装，请使用此语法来指定服务器：\<servername>[\\<instancename\>]。

    - 对于在 SharePoint 集成模式下运行的报表服务器，要连接到的服务器是连接报表服务器的 SharePoint 站点。 连接到 SharePoint 站点，以便可以查看权限级别。 这些权限控制对报表服务器内容和操作的访问。 可以指定网站集中的任何站点。 下例说明了相应的语法：`https://mysharepointsite`。

5. 对于“身份验证”，请选择报表服务器已经使用的身份验证模式。

   - 如果使用的是默认安全性，请选择“Windows 身份验证”。
   - 如果已安装和部署了自定义的安全扩展插件，请选择 **“窗体身份验证”** 。
   - 如果已将报表服务器配置为使用基本身份验证，请选择 **“基本身份验证”** 。
   - 如果将报表服务器配置为 SharePoint 集成模式，请选择 **“Windows 身份验证”** 。

6. 选择“测试”，验证连接是否可用。

7. 出现提示时，选择“确定”，然后选择“保存”。

## <a name="connection-syntax-and-permissions"></a>连接语法和权限

 下表概述了执行特定任务所需的连接语法、步骤和权限。

 在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] “连接到服务器” **对话框中将** 指定为服务器类型时，可以指定报表服务器名称或 Web 服务端点。

|连接到|   任务   |   权限   |
|----------|-----------|-----------------|  
|本机模式的报表服务器，作为默认实例或命名实例进行连接：<br /><br /> \<server name>\<_instance><br /><br /> 与报表服务器的连接是通过报表服务器 WMI 提供程序建立的。|查看和设置服务器属性与默认值。<br /><br /> 查看和取消作业。<br /><br /> 创建和管理共享计划。<br /><br /> 创建、修改或删除角色定义。|分配给“系统管理员”角色。|  
|本机模式的报表服务器，作为默认实例或命名实例，通过终结点连接到报表服务器 Web 服务：<br /><br /> `https://<servername>/reportserver`<br /><br /> 指定指向报表服务器的 URL 提供了另一种连接到报表服务器的方法。|查看和设置服务器属性与默认值。<br /><br /> 查看和取消作业。<br /><br /> 创建和管理共享计划。<br /><br /> 创建、修改或删除角色定义。|分配给“系统管理员”角色。|  
|SharePoint 集成模式的报表服务器，通过 SharePoint 站点进行连接：<br /><br /> `https://<webserver>/<SharePointSite>`|查看和设置服务器属性与默认值。<br /><br /> 查看和取消作业。<br /><br /> 创建和管理为所连接的站点定义的共享计划。<br /><br /> 查看为所连接的站点定义的权限级别。|对所连接的 SharePoint 站点拥有“完全控制”级权限。|
|SharePoint 集成模式的报表服务器，通过报表服务器实例的名称进行连接：<br /><br /> \<server name>\<_instance>|查看和设置服务器属性与默认值。<br /><br /> 查看和取消作业。|对与报表服务器集成的 SharePoint 站点拥有“完全控制”级权限。<br /><br /> 请注意，当连接到报表服务器而非 SharePoint 站点时，可以执行的任务数将会减少。 这是因为报表服务器只能返回在报表服务器数据库，而不是 SharePoint 配置数据库和内容数据库中存储或管理的应用程序数据。|

## <a name="see-also"></a>另请参阅

 [配置报表服务器数据库连接（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)  
 [SQL Server Management Studio 中的 Reporting Services (SSRS)](../../reporting-services/tools/reporting-services-in-sql-server-management-studio-ssrs.md)

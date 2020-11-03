---
description: Power BI 报表服务器集成（配置管理器）
title: Power BI 报表服务器集成（配置管理器）| Microsoft Docs
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.date: 09/17/2017
ms.openlocfilehash: 47964ebf5702542452227589e1426948825cc216
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92678872"
---
# <a name="power-bi-report-server-integration-configuration-manager"></a>Power BI 报表服务器集成（配置管理器）

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器中的“Power BI 集成”页用于向所需的 Azure Active Directory (AD) 托管租户注册报表服务器，以允许报表服务器用户将支持的报表项固定到 [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] 仪表板。 有关可以固定的支持项目列表，请参阅 [将 Reporting Services 项目固定到 Power BI 仪表板](../../reporting-services/pin-reporting-services-items-to-power-bi-dashboards.md)。

## <a name="requirements-for-power-bi-integration"></a><a name="bkmk_requirements"></a> Power BI 集成的要求

除了具有活动的 Internet 连接，以便你可以浏览到 [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] 服务以外，还需要满足以下 [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)]集成的要求。

- **Azure Active Directory：** 你的组织必须使用 Azure Active Directory，以便为 Azure 服务和 Web 应用程序提供目录和身份管理。 有关详细信息，请参阅[什么是 Azure Active Directory？](/azure/active-directory/fundamentals/active-directory-whatis)

- **托管租户：** 你想要将报表项固定到其上的 [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] 仪表板必须是 Azure AD 托管租户的一部分。  托管租户在组织第一次订阅 Azure 服务（如 Microsoft 365 和 Microsoft Intune）时自动创建。   目前不支持病毒性租户。  有关详细信息，请参阅 [“什么是 Azure AD 目录？”](/previous-versions/azure/azure-services/jj573650(v=azure.100)#BKMK_WhatIsAnAzureADTenant)中的“什么是 Azure AD 租户”和“如何获取 Azure AD 目录”部分。

- 执行 [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] 集成的用户需要是 Azure AD 租户的成员、 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 系统管理员和 ReportServer 目录数据库的系统管理员。

- 执行 [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] 集成的用户需要使用用于安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的帐户，或者是 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]服务在其中运行的帐户来启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器

- 你想要从中固定的报表必须使用存储的凭据。 这不是 [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] 集成本身的要求，而是固定项刷新处理的要求。  固定报表项操作会创建 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 订阅来管理 [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)]中磁贴的刷新计划。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 订阅需要存储的凭据。 如果报表不使用存储的凭据，用户仍可以固定报表项，但当与之关联的订阅尝试刷新数据到 [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)]时，你将看到与“我的订阅”  页上类似的以下错误消息。

    PowerBI 传递错误：仪表板：IT 支出分析示例，直观图：Chart2，错误：无法完成当前操作。 用户数据源凭据不符合运行此报表或共享数据集的要求。 请输入用户数据源凭据。

有关如何存储凭据的详细信息，请参阅[在 Reporting Services 数据源中存储凭据中](../../reporting-services/report-data/store-credentials-in-a-reporting-services-data-source.md)的“为特定于报表的数据源配置存储凭据”部分。

有关详细信息，管理员可以查看  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 日志文件。  他们将看到类似于以下内容的消息： 查看和监视 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 日志文件最好的方式是对文件使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Power Query。  有关详细信息和简短视频，请参阅 [Report Server Service Trace Log](../../reporting-services/report-server/report-server-service-trace-log.md)。

- subscription!WindowsService_1!1458!09/24/2015-00:09:27:: e 错误：PowerBI 传递错误：仪表板：IT 支出分析示例，直观图：Chart2，错误：无法完成当前操作。 用户数据源凭据不符合运行此报表或共享数据集的要求。 用户数据源凭据未存储在报表服务器数据库中，或用户数据源配置为不需要凭据但未指定无人参与的执行帐户。

- notification!WindowsService_1!1458!09/24/2015-00:09:27:: e 错误：处理订阅 fcdb8581-d763-4b3b-ba3e-8572360df4f9 时发生错误：PowerBI 传递错误：仪表板：IT 支出分析示例，直观图：Chart2，错误：无法完成当前操作。 用户数据源凭据不符合运行此报表或共享数据集的要求。 用户数据源凭据未存储在报表服务器数据库中，或用户数据源配置为不需要凭据但未指定无人参与的执行帐户。

## <a name="to-integrate-and-register-the-report-server"></a><a name="bkmk_steps2integrate"></a> 集成并注册报表服务器

完成 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器中的以下步骤。 有关详细信息，请参阅[报表服务器配置管理器](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)。

1. 选择 [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] 集成页。

2. 单击“注册 Power BI”。

    >[!Note]
    > 确保端口 443 未被阻止。

3. 在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 登录对话框中，键入用于登录 [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)]的凭据。

4. 注册完成后，“Power BI 注册详细信息”部分将记下 Azure 租户 ID 和重定向 URL。  URL 用作 [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] 仪表板登录和通信过程的一部分，以回传给已注册的报表服务器。

5. 选择“结果”窗口中的“复制”按钮，以将注册详细信息复制到 Windows 剪贴板，这样你便可以将它们保存起来供以后参考。

## <a name="unregister-with-power-bi"></a><a name="bkmk_unregister"></a> 注销 Power BI

**注销：** 从 Azure Active Directory 注销报表服务器，会导致下面的结果：

- “我的设置”链接在 Web 门户菜单栏中不再可见。

- 已固定的报表项仍固定在仪表板中，但将不再更新仪表板上的磁贴。

- 已更新磁贴的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 订阅将仍存在于报表服务器上，但当它们在其配置的计划上运行时，它们将显示与下列消息类似的错误消息。

    **无法加载此订阅的传递扩展插件**

从配置管理器的“Power BI”页中，选择“注销 Power BI”按钮。

##  <a name="update-registration"></a><a name="bkmk_updateregistration"></a> 更新注册

如果你的报表服务器的配置已更改，请使用“更新注册”  。 例如，如果你想要添加或删除用户用于浏览 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)]的 URL。

- 请在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器中，选择“Web 门户 URL”

     选择“高级”。

- 选择“添加”为 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 添加新 HTTP 标识 ，然后选择“确定”。

     [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] 图标将发生更改以指示已更改服务器配置。  ![ssrs_powebi_icon_warning](../../reporting-services/install-windows/media/ssrs-powebi-icon-warning.png "ssrs_powebi_icon_warning")

- 在“Power BI 集成”页上，单击“更新注册”。

     系统将提示你登录到 Azure AD。 刷新该页面后，你将看到在“重定向 URL” 中列出新 URL。

##  <a name="summary-of-the-power-bi-integration-and-pin-process"></a><a name="bkmk_integration_process"></a> Power BI 集成和固定处理摘要

本部分总结了使用 [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] 集成报表服务器以及将报表项固定到仪表板时所涉及的基本步骤和技术。

 **集成：**

1. 在配置管理器中，选择“在 Power BI 中注册”按钮时，系统将提示登录到 Azure Active Directory。

2. 已在托管租户中注册 [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] 客户端应用。

3. Power BI 客户端应用程序在 Azure Active Directory 中的托管租户中创建。

4. 注册包括用户从报表服务器登录时使用的重定向 URL。  应用 ID 和 URL 将保存到报表服务器数据库中。 重定向 URL 将在对 Azure 的身份验证调用期间使用，以便该调用可以返回到报表服务器。 例如，当用户登录或将项固定到仪表板时。

5. 应用 ID 和 URL 显示在配置管理器中。

 ![ssrs_pbiflow_integration](../../reporting-services/install-windows/media/ssrs-pbiflow-integration.png "ssrs_pbiflow_integration")

 **当用户将报表项固定到仪表板时：**

1. 用户可以预览 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 中的报表，并且当用户首次单击从 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 中固定报表项时，

2. 他们将被重定向到 Azure AD 登录页。 用户也可以从“我的设置”[!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 页登录。 当用户登录到 Azure 托管租户时，便在 Azure 帐户和 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 权限之间建立了一种关系。  有关详细信息，请参阅 [我的 Power BI 集成（网站门户）设置](../my-settings-for-power-bi-integration-web-portal.md)相集成。

3. 用户安全令牌返回到报表服务器。

4. 用户安全令牌保存到 ReportServer 数据库。

5. 从 [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] 服务检索到用户有权访问的组和仪表板列表。  用户选择目标组和仪表板并配置他们希望在 [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)] 磁贴上刷新数据的频率。

6. 此报表项已被固定到仪表板。

7. 创建 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 订阅以管理仪表板磁贴上报表项的计划刷新。 订阅使用用户登录时创建的安全令牌。

     令牌适于 90 天，此后用户需要再次登录以创建新的用户令牌。 令牌过期后，已固定的磁贴仍将显示在仪表板上，但不再刷新数据。  用于已固定项的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 订阅将出错，直到创建新的用户令牌。 请参阅[我的 Power BI 集成设置（Web 门户）](../my-settings-for-power-bi-integration-web-portal.md)。 以了解详细信息。

用户第二次固定某一项时，跳过步骤 1-4，改为从 ReportServer 数据库和流中检索应用 ID 和 URL，然后继续执行步骤 5。

![图中显示了用户将报表项固定到仪表板时发生的情况。](../../reporting-services/install-windows/media/ssrs-pin-to-powerbi-flow.png)

 **当触发订阅以刷新仪表板磁贴时：**

1. 触发 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 订阅时，将呈现报表。

2. 从 ReportServer 数据库检索用户令牌。

3. 报表项状态和数据随令牌发送给 [!INCLUDE[sspowerbi](../../includes/sspowerbi-md.md)]服务。

4. 将令牌发送到 Azure AD 进行验证。 如果令牌有效，则报表项数据将发送到仪表板磁贴，并且将更新磁贴的日期属性。

5. 如果令牌无效，则返回错误并通过报表服务器进行记录。  无状态或其他信息发送到仪表板。

![图中显示了触发订阅以刷新仪表板磁贴时发生的情况。](../../reporting-services/install-windows/media/ssrs-subscription-to-powerbi-flow.png)

   <iframe width="560" height="315" src="https://www.youtube.com/embed/QhPQObqmMPc" frameborder="0" allowfullscreen></iframe>

## <a name="considerations-and-limitations"></a>注意事项和限制

* 不支持病毒性租户和政府租户。

## <a name="next-steps"></a>后续步骤

[适用于 Power BI 集成的“我的设置”（Web 门户）](../my-settings-for-power-bi-integration-web-portal.md)  
[将 Reporting Services 项目固定到 Power BI 仪表板](../../reporting-services/pin-reporting-services-items-to-power-bi-dashboards.md)
[Power BI 中的仪表板](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
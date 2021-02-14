---
title: 配置 Web 门户 | Microsoft Docs
description: 了解如何配置 Web 门户应用程序，用于查看报表、管理报表服务器内容以及向用户授予本机模式报表服务器的访问权限。
ms.date: 05/10/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- the web portal [Reporting Services], configuring
ms.assetid: e918986c-af15-48f6-8178-256aed829c6a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: ebbce2708647e15b68ec497cfadd0af2c7c5b154
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074688"
---
# <a name="configure-the-web-portal"></a>配置 Web 门户

Web 门户是一种用于查看报表、管理报表服务器内容以及向用户授予本机模式报表服务器访问权限的 Web 前端应用程序。 如果在安装程序中选择“以默认的本机模式配置安装”选项，则可以将 Web 门户与报表服务器 Web 服务一起安装在同一个报表服务器实例中，并可以选择地进行配置。 还可以在安装完成后配置 Web 门户。 本主题提供有关以下 Web 门户配置方案的信息：

## <a name="prerequisites"></a>先决条件

若要使用 Web 门户，必须满足下列前提条件：

- 必须拥有最小配置的报表服务器。 有关对报表服务器进行最小配置的详细信息，请参阅[配置报表服务器](../../reporting-services/report-server/configure-a-report-server-reporting-services-native-mode.md)。

- 报表服务器必须在本机模式下运行。 在报表服务器配置为 SharePoint 集成模式的情况下无法使用 Web 门户。 在 SQL Server 2012 中，不能将报表服务器从一个模式切换到另一个模式。 如果要更改您环境使用的报表服务器类型，必须安装所需的报表服务器模式，然后将报表项复制或移到新报表服务器。 此过程通常称为“迁移”。 迁移所需的步骤取决于您要迁移到的模式和迁移前所在的版本。 有关详细信息，请参阅 [Upgrade and Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)。

- 还必须装有启用了脚本功能的 Internet Explorer 11 或更高版本。 有关详细信息，请参阅 [Reporting Services 和 Power View 的浏览器支持](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)。

## <a name="configure-the-web-portal-to-use-the-default-url"></a>将 Web 门户配置为使用默认 URL

Web 门户是用户在 Web 浏览器中访问的 Web 应用程序。 因此必须至少定义用于在浏览器窗口中打开该应用程序的 URL。 该 URL 由主机名、端口和虚拟目录组成。 此 URL 的默认值包括为报表服务器 Web 服务 URL 定义的主机名和端口值，再加上 **reports** 虚拟目录名。 如果有命名实例，则虚拟目录为 **reports_instance**，其中 **instance** 是 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 实例的名称。

默认情况下，Web 门户 URL 由唯一的虚拟目录名和为在同一实例中运行的报表服务器 Web 服务定义的端口和主机名组成。 在大多数情况下，主机名是报表服务器计算机的网络名称，但也可以是解析该计算机的 IP 地址或主机标头。 若要将 Web 门户配置为使用默认 URL，请使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具中的“Web 门户 URL”页面。

> [!TIP]
> 如果尝试访问远程计算机上的 Web 门户，但在浏览器中收到连接错误消息，这通常是由于防火墙设置造成的。 有关详细信息，请参阅 [将防火墙配置为允许报表服务器访问](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md)。

#### <a name="to-configure-the-default-the-web-portal-url-and-virtual-directory"></a>配置默认 Web 门户 URL 和虚拟目录

1. 启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具并连接到报表服务器实例。

2. 在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具中，选择“Web 门户 URL”以打开配置 URL 的页面。

3. 为 Web 门户输入唯一的虚拟目录名称。

4. 单击“应用”。

5. 如果使用的是 [!INCLUDE[wiprlhlong](../../includes/wiprlhlong-md.md)] 或 Windows Server 2008，则可能还需要执行其他步骤才能使用 Web 门户。 有关详细信息，请参阅 [为本地管理配置本机模式报表服务器 (SSRS)](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md)。

## <a name="configure-the-web-portal-to-use-a-specific-report-server-url"></a>将 Web 门户配置为使用特定的报表服务器 URL

默认情况下，Web 门户连接到在同一 Report Server 服务中运行的报表服务器 Web 服务。 Web 门户使用报表服务器 Web 服务 URL 进行连接。 如果为报表服务器 Web 服务定义了多个 URL，则 Web 门户将使用所定义的最后一个 URL。 但是，对于某些部署，你可能希望 Web 门户始终通过静态 URL 连接到 Web 服务。 这样做的一种情况是：您对特定端口或 IP 地址配置了数据包筛选，并且希望所有到报表服务器的连接都通过您所定义的筛选规则。

在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具中配置 URL 时，Web 门户将自动检测并使用在同一服务器实例中运行的报表服务器的任何新的或更新的 URL。 如果部署要求对所有报表服务器请求使用单个静态 URL，您可以在 RSReportServer.config 文件中指定该 URL。

#### <a name="to-configure-a-static-report-server-url"></a>配置静态报表服务器 URL

1. 在文本编辑器中打开 **RSReportServer.config** 文件。 默认情况下，该文件位于 \Program Files\Microsoft SQL Server\MSRS12.\<*instancename*>\Reporting Services\ReportServer。  

2. 查找 **ReportServerURL**。

3. 将其替换为相应报表服务器实例的 URL。

4. 保存更改并关闭该文件。

有关配置文件的详细信息，请参阅[修改 Reporting Services 配置文件 (RSreportserver.config)](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md) 和 [RsReportServer.config 配置文件](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)。

## <a name="customize-styles-or-application-title"></a>自定义样式或应用程序标题

可以创建自定义品牌包来更改用于 Web 门户的颜色。 有关详细信息，请参阅[设置 Web 门户的品牌](../branding-the-web-portal.md)

#### <a name="to-modify-application-title"></a>修改应用程序标题

1. 使用分配有报表服务器 **“系统管理员”** 权限的帐户登录。

2. 打开 Internet Explorer。

3. 输入 Web 门户 URL。 默认情况下，该 URL 为 https://\<**your-server-name**>/reports，但如果已将 Reporting Services 作为命名实例安装，则默认 URL 将为 https://\<**your-server-name**>/reports\<**_instancename**>。

4. 选择“站点设置”。 

5. 在 **“常规”** 选项卡上的 **“名称”** 中，将 **SQL Server Reporting Services** 替换为其他名称。

6. 选择“应用”。

## <a name="next-steps"></a>后续步骤

[Web 门户](../../reporting-services/web-portal-ssrs-native-mode.md)  
[Reporting Services 的浏览器支持](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)
[配置 URL](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)   
[验证 Reporting Services 安装](../../reporting-services/install-windows/verify-a-reporting-services-installation.md)   
[打开或关闭 Reporting Services 功能](../../reporting-services/report-server/turn-reporting-services-features-on-or-off.md)   
[管理 Reporting Services 本机模式报表服务器](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)   
[RsReportServer.config 配置文件](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[为本地管理配置本机模式报表服务器](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md)

 更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)

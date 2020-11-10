---
title: 报表服务器的 Web 门户（本机模式）| Microsoft Docs
ms.date: 01/31/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
description: Reporting Services 报表服务器的 Web 门户是一种基于 Web 的体验，支持查看报表、移动报表和 KPI 以及在报表服务器实例中的各元素间导航。
ms.topic: conceptual
ms.assetid: 7349e626-6ed5-4d21-b05f-cf042ad9ad70
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 301d77a972da39f4e3e046e5e983206e9a1ba5c9
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243749"
---
# <a name="the-web-portal-of-a-report-server-ssrs-native-mode"></a>报表服务器的 Web 门户（SSRS 本机模式）

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)]

Reporting Services 报表服务器的 Web 门户基于 Web 体验。 在门户中，可以查看报表、移动报表、KPI，并浏览报表服务器实例中的元素。 还可以使用 Web 门户管理单个报表服务器实例。

![显示 SQL Server Reporting Services 门户的屏幕截图。](../reporting-services/media/ssrsportal.png)

## <a name="what-is-the-web-portal"></a>什么是 Web 门户

可以使用 Web 门户执行以下任务：

- 查看、搜索、打印和订阅报表。
- 创建、保护和维护文件夹层次结构，以便组织服务器上的项。
- 配置基于角色的安全性，确定对项和操作的访问权限。 有关详细信息，请参阅[角色定义 - 预定义角色](security/role-definitions-predefined-roles.md)。
- 配置报表执行属性、报表历史记录和报表参数。
- 创建共享计划和共享数据源，以提高计划和数据源连接的可管理性。
- 创建可以将报表展开为大型收件人列表的数据驱动订阅。
- 创建链接报表，以便按不同方式重用现有报表和重新确定其用途。
- 下载常用工具，如报表生成器和移动报表发布服务器。
- [创建 KPI](../reporting-services/working-with-kpis-in-reporting-services.md)。
- 发送反馈或提出功能请求。

使用 Web 门户可以浏览报表服务器文件夹或搜索特定报表。 可以查看报表、报表的常规属性以及在报表历史记录中捕获的报表以前的副本。 根据权限，可能还可以订阅报表，以便将其传递到电子邮件收件箱或文件系统中的共享文件夹。

> [!NOTE]
> 有关支持的浏览器和版本的信息，请参阅 [Reporting Services 浏览器支持计划](../reporting-services/browser-support-for-reporting-services-and-power-view.md)。

Web 门户仅适用于在本机模式下运行的报表服务器。 配置为 SharePoint 集成模式的报表服务器不支持报表管理器。

仅在特定的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本中才提供某些 Web 门户功能。 有关详细信息，请参阅 [SQL Server 各个版本支持的 Reporting Services 功能](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md)。

如果是全新安装，则只有本地管理员有足够的权限来处理内容和设置。 若要对其他用户授予权限，本地管理员必须创建角色分配，以便提供对报表服务器的访问权限。 用户随后可以访问的应用程序页和任务将取决于该用户的角色分配。 有关详细信息，请参阅[授予用户报表服务器的访问权限](./security/grant-user-access-to-a-report-server.md)

> [!NOTE]
> 如果浏览至服务器正在其上运行的本地计算机上的 Web 门户，你可能会看到一条消息指示你不能查看此文件夹。 这是由于通用访问控制 (UAC) 以及你未以管理员身份运行浏览器的原因造成的。你不能以管理员身份运行 Microsoft Edge。你将需要使用 Internet Explorer。 你可以远程浏览至服务器，或以管理员身份启动 Internet Explorer 并浏览至 Web 门户。 如果想要远程使用 Web 门户，则需要给为你的帐户授予文件夹的内容管理者权限。  

## <a name="start-and-use-the-web-portal"></a>启动和使用 Web 门户

Web 门户是一种 Web 应用程序，可通过在浏览器窗口的地址栏中键入 [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)] URL 将其打开。 启动 [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)] 时，基于在报表服务器中拥有的权限，你所看到的页面、链接和选项会有所不同。 若要执行某项任务，必须为自己分配包括该任务的角色。  如果为某用户分配了具有完整权限的角色，则该用户可以访问用来管理报表服务器的所有应用程序菜单和页。 如果为某用户分配的角色具有查看和运行报表的权限，则该用户只能看到支持这些活动的菜单和页。 对于不同的报表服务器，甚至对于存储在单个报表服务器上的不同报表和文件夹，每个用户可以具有不同的角色分配。

有关角色的详细信息，请参阅 [授予对本机模式报表服务器的权限](../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)。

### <a name="start-the-web-portal"></a>启动 Web 门户

若要从浏览器启动 Web 门户，请执行以下操作：

1. 打开 Web 浏览器。 有关支持的 Web 浏览器的列表，请参阅 [Reporting Services 浏览器支持计划](../reporting-services/browser-support-for-reporting-services-and-power-view.md)。

2. 在 Web 浏览器的地址栏中，键入 Web 门户 URL。

    默认情况下，该 URL 为 https://[ComputerName]/reports。

    报表服务器可能已配置为使用特定的端口。 例如， https://[ComputerName]:80/reports 或 https://[ComputerName]:8080/reports。

## <a name="grouping-by-categories"></a>按类别分组

Web 门户将项按不同的类别分组。 可用类别如下。

- KPI
- 移动报表
- 分页报表
- Power BI 桌面报表
- Excel 工作薄
- 数据集
- “数据源”
- 资源

通过选择右上方的“视图”可以控制显示的内容。 如果选择“显示隐藏项”，这些项将以较浅的颜色显示出来。

![“视图”下拉列表的屏幕截图，其中已选择“显示隐藏项”选项。](../reporting-services/media/ssrswebportal-view.png)

![屏幕截图：不可用的“分页报表”选项。](../reporting-services/media/ssrswebportal-hidden.png)

### <a name="power-bi-desktop-reports-and-excel-workbooks"></a>Power BI 桌面报表和 Excel 工作薄

可以上传、组织和管理 Power BI 桌面报表和 Excel 工作薄的权限。 它们会在 Web 门户中被分组到一起。

![显示“Power BI Desktop 报表”部分和“Excel 工作簿”部分的屏幕截图。](../reporting-services/media/ssrswebportal-view-pbi-and-excel.png)

与其他资源文件类似，文件将存储在 Reporting Services 内。 选择其中一项会将它们下载到本地桌面。 可以通过将更改重新上传到报表服务器来保存已做更改。

## <a name="search-for-items"></a>搜索项目

输入搜索术语，并查看可以访问的所有内容。 结果分为 KPI、报表、数据集和其他项。 然后可以对结果进行交互并将它们添加到你的收藏夹。

![SQL Server Reporting Services 门户的屏幕截图，其中已调出搜索文本框。](../reporting-services/media/ssrswebportal-search.png)

## <a name="web-portal-tasks"></a>Web 门户任务

[设置 Web 门户的品牌](../reporting-services/branding-the-web-portal.md)

[处理 KPI](../reporting-services/working-with-kpis-in-reporting-services.md)

[使用共享数据集](../reporting-services/work-with-shared-datasets-web-portal.md)

## <a name="see-also"></a>另请参阅

[使用 SQL Server 移动报表发布服务器创建移动报表](../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md)  
[配置 URL（报表服务器配置管理器）](../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)  
[Reporting Services 工具](../reporting-services/tools/reporting-services-tools.md)  
[Reporting Services 浏览器支持计划](../reporting-services/browser-support-for-reporting-services-and-power-view.md)  
[SQL Server 各个版本支持的 Reporting Services 功能](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
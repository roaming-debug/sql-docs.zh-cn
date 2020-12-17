---
title: 什么是 SQL Server Reporting Services | Microsoft Docs
description: 了解用于本地移动和分页 Reporting Services 报表的工具和服务。
ms.date: 05/06/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- reports [Reporting Services]
- SSRS
- reports [Reporting Services], about reports
- Reporting Services
- SQL Server Reporting Services
ms.assetid: b8d18d3d-9db0-43e7-8286-7b46cc3a37ed
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b6c33eaeda2a7600039b80c49e1ba3c0fa9e36b5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439362"
---
# <a name="what-is-sql-server-reporting-services-ssrs"></a>什么是 SQL Server Reporting Services (SSRS)？

[!INCLUDE [ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../includes/ssrs-appliesto-not-pbirs.md)]

要查找 Power BI 报表服务器？ 请参阅[什么是 Power BI 报表服务器？](https://docs.microsoft.com/power-bi/report-server/get-started)。

SQL Server Reporting Services (SSRS) 提供了一系列本地工具和服务，用于创建、部署和管理移动和分页报表。

![所有 SQL Server Reporting Services](../reporting-services/media/ss-reporting-services-all-together.png "所有 SQL Server Reporting Services")

## <a name="create-deploy-and-manage-mobile-and-paginated-reports"></a>创建、部署和管理移动和分页报表

SSRS 解决方案灵活地将正确信息提供给正确用户。 用户可以通过 Web 浏览器、移动设备或电子邮件使用报表。

SQL Server Reporting Services 提供产品的更新套件：

* 提供最新的“传统”分页报表，使用户可通过更新工具和用于创建报表的新功能创建新式报表  。
* 具有响应式布局的 **新移动报表** ，可适应不同设备和保存方式。
* **新式 Web 门户** ，可在任何新式浏览器中查看。 在新门户中，可以组织和显示移动和分页 Reporting Services 报表及 KPI。 还可在门户中存储 Excel 工作薄。

请继续阅读有关每项的详细信息。

### <a name="whats-new-in-reporting-services"></a>Reporting Services 中的新增功能

这些源让你可以随时了解最新的 SQL Server Reporting Services 新增功能。

* [Reporting Services 中的新增功能](../reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md)
* [SQL Server Reporting Services 团队博客](https://blogs.msdn.microsoft.com/sqlrsteamblog/)
* [Guy in a Cube YouTube 通道](https://www.youtube.com/channel/UCFp1vaKzpfvoGai0vE5VJ0w)

## <a name="paginated-reports"></a>分页报表

![图中显示了桌面设备屏幕和平板电脑设备上的分页报表。](../reporting-services/media/ssrs-paginated-reports.png)

Reporting Services 与“传统”分页报表相关联，非常适合用于为打印而优化的固定布局文档，如 PDF 和 Word 文件。

现在仍存在核心 BI 工作负荷，因此我们对其进行了改进。 现在，可使用 [SQL Server Data Tools (SSDT)](../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md) 中的报表生成器或报表设计器创建具有更新后的新功能的新式报表。

* 已更新所有默认样式和调色板，因此默认情况下，使用新的极简主义新式样式创建报表。
* 已更新“参数”窗格，因此可随意排列参数。
* 可以导出到 PowerPoint 等新格式。 PowerPoint 中的 Reporting Services 可视化效果是实时且可编辑的，不仅仅是屏幕截图。
* 可创建混合的 Power BI/Reporting Services 体验：可将报表中的视觉对象固定到 Power BI 仪表板，而不是在 Power BI 中重新创建本地 Reporting Services 报表。 然后可以在 Power BI 仪表板上同一个位置监视所有内容。

## <a name="mobile-reports"></a>移动报表

![图中显示了桌面设备屏幕和平板电脑设备上的移动报表。](../reporting-services/media/ssrs-mobile-reports.png)

移动计算已经改变了工作所用的设备，这意味着现在的用户具有不同的报告需求。 引入平板电脑和手机时，固定布局报表体验不佳。 针对宽屏电脑设计的内容无法在小屏幕手机上获得最佳体验，小屏幕手机不仅尺寸更小而且采用纵向或横向布局。

这些差异很大的屏幕外形规格所需的是可适应不同屏幕大小和方向的响应式布局。 为此，我们添加了新的报表类型：移动报表，此报表基于我们在约一年前获得并集成到产品中的 Datazen 技术。 使用 [SQL Server Migration Assistant for Datazen](https://www.microsoft.com/download/details.aspx?id=53128)可以将现有 Datazen 报表迁移到 Reporting Services。

在新的 [移动报表发布服务器](../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md) 应用中创建这些移动报表。 然后在本机适用于 Windows 10、iOS、Android 和 HTML5 的[用于移动设备的 Power BI 应用](https://powerbi.microsoft.com/documentation/powerbi-power-bi-apps-for-mobile-devices/)中，可访问 Power BI、云或 SSRS 中的数据。

创建可视化效果时，移动报表发布服务器将自动生成示例数据。 此功能允许你查看数据的可视化效果，以及适用于各可视化效果的数据类型。

## <a name="web-portal"></a>Web 门户

![图中显示了 Web 门户笔记本电脑。](../reporting-services/media/ssrs-web-portal.png)

对于本机模式 Reporting Services 的最终用户，前门是可在大多数浏览器中查看的新式 Web 门户。 可以在新门户中访问所有 Reporting Services 移动报表、分页报表及 KPI。 KPI 可结合浏览器查看关键业务指标，而无需打开报表。

新的 Web 门户是完全重写的报表管理器。 现在，它是单页、基于标准的 HTML5 应用，新式浏览器针已针对此应用进行了优化，这些浏览器包括：Microsoft Edge、Internet Explorer 10 和 11、Chrome、Firefox、Safari 以及所有主流浏览器。

Web 门户内容按类型进行整理：

* 分页报表
* 移动报表 
* KPI
* Excel 工作薄
* 共享数据集
* 共享数据源

可在此处安全存储和管理它们，并采用传统的文件夹层次结构。 标记收藏夹报表以便快速访问。 那些具有适当权限的用户能够管理 SSRS 内容。

在新的 Web 门户中，仍可计划报表处理、按需访问报表并订阅已发布的报表。

详细了解 [Web 门户](../reporting-services/web-portal-ssrs-native-mode.md)。

::: moniker range="=sql-server-2016"

## <a name="reporting-services-in-sharepoint-integrated-mode"></a>SharePoint 集成模式下的 Reporting Services

在 SharePoint 集成模式下将报表发布到 Reporting Services。 可以计划报表处理，按需访问报表，订阅已发布的报表并将报表导出到其他应用程序，例如 Microsoft Excel。 可以在发布到 SharePoint 站点的报表上创建数据警报，并在报表数据更改时接收电子邮件。  

了解有关 [SharePoint 集成模式下的 Reporting Services 报表服务器](../reporting-services/report-server-sharepoint/reporting-services-report-server-sharepoint-mode.md)的详细信息。

::: moniker-end

## <a name="ssrsnoversion-programming-features"></a>[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 编程功能

充分利用 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 编程功能，以便扩展和自定义报告功能。 使用 SSRS API 在自定义应用程序中集成或扩展数据和报表处理。

更多 [Reporting Services 开发人员文档](../reporting-services/reporting-services-developer-documentation.md).

## <a name="next-steps"></a>后续步骤

* [安装 Reporting Services](../reporting-services/install-windows/install-reporting-services.md)
* [下载 SQL Server Data Tools (SSDT)](https://go.microsoft.com/fwlink/?LinkID=616714)
* [安装报表生成器](../reporting-services/install-windows/install-report-builder.md)

* 更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)

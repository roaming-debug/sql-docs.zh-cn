---
title: SharePoint 站点上的报表查看器 Web 部件 - SSRS | Microsoft Docs
description: 使用报表查看器自定义 Web 部件可以查看、导航、打印和导出 SharePoint 站点中的 SQL Server Reporting Services 报表。
ms.date: 02/11/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1fcd4e099c245ba26d967fd775def2dc86a50d69
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016811"
---
# <a name="report-viewer-web-part-on-a-sharepoint-site---reporting-services"></a>SharePoint 站点上的报表查看器 Web 部件 - Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2016-and-later](../../includes/ssrs-appliesto-sharepoint-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

报表查看器 Web 部件是自定义 Web 部件。 可以使用该 Web 部件在 SharePoint 站点中的报表服务器上查看、导航、打印和导出报表。 报表查看器 Web 部件与由 Microsoft SQL Server Reporting Services 报表服务器处理的报表定义 (.rdl) 文件关联。 

最新的报表查看器 Web 部件还可用于部署到 Power BI 报表服务器的分页报表。 Web 部件不适用于 Power BI 报表。

## <a name="why-the-report-viewer-web-part-is-re-introduced"></a>重新引入报表查看器 Web 部件的原因

报表查看器 Web 部件随附在用于 SharePoint 产品的 Reporting Services 外接程序中。 在 SharePoint 集成模式下，Web 部件特定于报表服务器。 SharePoint 集成模式在 SQL Server 2016 后已弃用。

从 SQL Server 2017 开始，Reporting Services 只有一种安装模式：本机模式。 可使用页面查看器 Web 部件通过 rs:Embed=true URL 参数嵌入所有报表类型。 将报表嵌入到 SharePoint 页是一种由客户请求的集成案例，更新的报表查看器 Web 部件可为分页报表启用此方案。

虽然页面查看器 Web 部件足以将分页报表嵌入到 SharePoint 页，但更新的报表查看器 Web 部件提供了额外的功能。

* 显示/隐藏特定的工具栏按钮
* 替代报表参数值
* 将筛选器 Web 部件连接到报表参数

## <a name="download-the-report-viewer-web-part-solution-package"></a>下载报表查看器 Web 部件解决方案包

可从 Microsoft 下载中心下载报表查看器 Web 部件。

[下载报表查看器 Web 部件解决方案包](https://www.microsoft.com/download/details.aspx?id=55949)

## <a name="considerations-and-limitations"></a>注意事项和限制

列出的项特定于更新的报表查看器 Web 部件。

* Web 部件只能用于经典 SharePoint 页。
* 仅支持在报表查看器 Web 部件中嵌入分页 (RDL) 报表。 如果希望嵌入 Power BI 报表或移动报表，则可以使用 rs:Embed=true URL 参数。

## <a name="next-steps"></a>后续步骤

若要开始使用更新的报表查看器 Web 部件，请参阅[在 SharePoint 站点上部署报表查看器 Web 部件](deploy-report-viewer-web-part.md)。

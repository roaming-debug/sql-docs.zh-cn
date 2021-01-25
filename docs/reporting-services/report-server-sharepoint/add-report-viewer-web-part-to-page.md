---
title: 将 SQL Server Reporting Services 报表查看器 Web 部件添加到 SharePoint 页 | Microsoft Docs
description: 通过将报表查看器 Web 部件添加到 SharePoint 页，从 SQL Server Reporting Services 或 Power BI 报表服务器显示报表。
ms.date: 11/26/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b0fa25f3951ab33001f84f7f44bd4617659a81ae
ms.sourcegitcommit: 7791bd2ba339edc5cd2078a6537c8f6bfe72a19b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/19/2021
ms.locfileid: "98564424"
---
# <a name="add-sql-server-reporting-services-report-viewer-web-part-to-a-sharepoint-page"></a>将 SQL Server Reporting Services 报表查看器 Web 部件添加到 SharePoint 页

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

通过将报表查看器 Web 部件添加到 SharePoint 页，从 SQL Server Reporting Services 或 Power BI 报表服务器显示报表。

![SharePoint 页上的报表查看器 Web 部件](media/sharepoint-report-viewer-web-part-on-page.png)

## <a name="prerequisites"></a>先决条件

* 为了成功加载报表，需要为 Kerberos 约束委派配置 Windows Token Service (C2WTS) 声明。 有关如何配置 C2WTS 的详细信息，请参阅 [Windows Token Service (C2WTS) 和 Reporting Services 声明](../install-windows/claims-to-windows-token-service-c2wts-and-reporting-services.md)。

* 必须将报表查看器 Web 部件部署到 SharePoint 场。 有关如何部署报表查看器 Web 部件解决方案项目的信息，请参阅[在 SharePoint 站点上部署报表查看器 Web 部件](deploy-report-viewer-web-part.md)。

* 若要向网页中添加 Web 部件，则必须拥有站点级别的“添加和自定义页面”权限。 如果使用默认安全设置，此权限将授予拥有“完全控制”级权限的“所有者”  组的成员。

## <a name="add-web-part"></a>添加 Web 部件

1. 在 SharePoint 网站上，选择左上角的“齿轮”图标，并选择“添加页”。

    ![从齿轮图标向 sharepoint 站点添加页。](media/sharepoint-add-a-page.png)

2. 命名页，并选择“创建”。

3. 在页设计器中，选择功能区中的“插入”选项卡。 然后选择“部件”部分中的“Web 部件”。

    ![从 office 功能区插入 Web 部件。](media/sharepoint-insert-web-part.png)

4. 在“类别”下，选择“SQL Server Reporting Services (本机模式)”。 在“部件”下，选择“报表查看器”。 然后选择“添加”。

    ![添加报表查看器 Web 部件。](media/sharepoint-report-viewer-web-part.png)

    最初可能遇到错误。 出现错误的原因是，默认报表服务器 URL 设置为 https://localhost 且可能在该位置不可用。

## <a name="configure-the-report-viewer-web-part"></a>配置报表查看器 Web 部件

若要配置指向特定报表的 Web 部件，请执行以下步骤。

1. 在编辑 SharePoint 页时，选择 Web 部件右上方的向下键，并选择“编辑 Web 部件”。

    ![编辑 Web 部件下拉列表中的网页。](media/sharepoint-edit-web-part.png)

2. 为承载报表的报表服务器输入“报表服务器 URL”。 URL 看起来应类似于 https://myrsserver/reportserver。

3. 输入想要在 Web 部件中显示的报表的路径和名称。 这看起来应类似于 /AdventureWorks Sample Reports/Company Sales。 在此示例中，报表“公司销售额”位于名为 AdventureWorks 示例报表的文件夹中。

4. 如果报表需要参数，则在提供报表服务器 URL 和报表名称后，选择“参数”部分中的“负载参数”。

5. 选择“确定”将所做更改保存到 Web 部件配置。

6. 在 Office 功能区中选择“保存”，将所做更改保存到 SharePoint 页。

![SharePoint 页上的报表查看器 Web 部件](media/sharepoint-report-viewer-web-part-on-page.png)

## <a name="considerations-and-limitations"></a>注意事项和限制

* 报表查看器 Web 部件不能用于 SharePoint 中的新式页面。
* Power BI 报表不能与报表查看器 Web 部件一起使用。
* 如果未看到报表查看器 Web 部件，请确保[已部署报表查看器 Web 部件](deploy-report-viewer-web-part.md)，以便将其添加到页。

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)

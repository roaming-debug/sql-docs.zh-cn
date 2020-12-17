---
title: 支持的 SharePoint 和 Reporting Services 服务器组合 | Microsoft Docs
description: 在 SharePoint 模式下安装的 SQL Server Reporting Services 报表服务器需要在 SharePoint 服务器上安装的 SharePoint 版本和用于 SharePoint 产品的 SQL Server Reporting Services 加载项 (rsSharePoint.msi)。
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
helpviewer_keywords:
- SharePoint mode
- add-in for sharepoint
- rsSharePoint
ms.assetid: dc6a3372-db26-43f0-b7aa-f725acc635c2
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: bd58cde55b2fe31f1253e5a6d2016332b3be23c3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474508"
---
# <a name="supported-combinations-of-sharepoint-and-reporting-services-server"></a>支持的 SharePoint 和 Reporting Services 服务器组合

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE [ssrs-appliesto-not-2017](../../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

在 SharePoint 模式下安装的 SQL Server Reporting Services 报表服务器需要在 SharePoint 服务器上安装的 SharePoint 版本和用于 SharePoint 产品的 SQL Server Reporting Services 加载项 (rsSharePoint.msi)。 本主题概述支持的组合。

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。

## <a name="supported-combinations-of-sharepoint-and-reporting-services-components"></a>支持的 SharePoint 和 Reporting Services 组件组合

 下表汇总了报表服务器、SharePoint 产品的 Reporting Services 外接程序和 SharePoint 产品之间支持的组合。 不支持下表中未列出的组合

### <a name="supported-combinations"></a>支持的组合

|Number|报表服务器|外接程序|SharePoint 版本|
|-|-------------------|-------------|------------------------|
|1|SQL Server 2016|SQL Server 2016|SharePoint 2016|
|2|SQL Server 2016|SQL Server 2016|SharePoint 2013|
|3|SQL Server 2014|SQL Server 2014|SharePoint 2013|
|4|SQL Server 2014|SQL Server 2014|SharePoint 2010|
|5|SQL Server 2012 SP4|SQL Server 2014 和 SQL Server 2012 SP4|SharePoint 2013|
|6|SQL Server 2012 SP3|SQL Server 2014 和 SQL Server 2012 SP3|SharePoint 2013|
|7|SQL Server 2012 SP2|SQL Server 2014 和 SQL Server 2012 SP2|SharePoint 2013|
|8|SQL Server 2012 SP1|SQL Server 2014 和 SQL Server 2012 SP1|SharePoint 2013|
|9|SQL Server 2012 和 SQL Server 2012 SP1*|SQL Server 2014|SharePoint 2010|
|10|SQL Server 2012|SQL Server 2012|SharePoint 2010|
|11|SQL Server 2008 R2|SQL Server 2014|SharePoint 2010|
|12|SQL Server 2008 R2|SQL Server 2012 和 SQL Server 2012 SP1 或更高版本|SharePoint 2010|
|13|SQL Server 2008 R2|SQL Server 2008 R2|SharePoint 2010|
|14|SQL Server 2008 R2|SQL Server 2008 SP2|SharePoint 2007|
|15|SQL Server 2008 SP2|SQL Server 2008 R2|SharePoint 2010|
|16|SQL Server 2008 SP2|SQL Server 2008 和 SQL Server 2008 SP2|SharePoint 2007|

 *例外：不支持 Power View 集成。

 有关外接程序下载页的链接，请参阅 [在何处查找用于 SharePoint 产品的 Reporting Services 外接程序](../../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md)。  

 其他注意事项：

- 请确保升级，以升级场中所有的 SharePoint 服务器。 这包括应用和 Web 前端服务器。

- 支持 SharePoint 2016（包括 Power View 集成）需要 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表服务器以及 SQL Server 2016 或更高版本的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 外接程序版本。

- 支持 SharePoint 2013 （包括 Power View 集成）需要 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表服务器以及 SQL Server 2012 SP1 或更高版本的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 外接程序版本。

- SQL Server 2012 中引入了 Power View。 因此，Power View 与 SharePoint 2010 的集成需要该加载项的 SQL Server 2012 或更高版本。

- SQL Server 2012（或更高版本）报表服务器不支持 SQL Server 2008 R2 外接程序。 SharePoint 2010 必备组件安装程序会自动安装 SQL Server 2008 R2 外接程序。 必须在安装外接程序的更新版本前卸载它。 不支持外接程序的就地升级。

- **升级：** 装有 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 外接程序的 SharePoint 2010 无法就地升级到 SharePoint 2013。 SharePoint 2013 需要 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 加载项和报表服务器的 SQL Server 2012 SP1 或更高版本。 有关升级的详细信息，请参阅 [Upgrade and Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)。

## <a name="next-steps"></a>后续步骤

 [在何处查找用于 SharePoint 产品的 Reporting Services 外接程序](../../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md)   
 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-components-of-sql-server-2017.md)   
 [安装 SQL Server 2016 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)

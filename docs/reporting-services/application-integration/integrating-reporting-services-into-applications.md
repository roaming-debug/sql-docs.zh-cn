---
title: 集成到应用程序中
description: Reporting Services 是一个开放的可扩展报表平台，旨在向开发人员提供一整套用于开发解决方案的 API。
ms.custom: seo-lt-2019
ms.date: 05/14/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.topic: reference
author: maggiesMSFT
ms.author: maggies
monikerRange: = sql-server-2016
ms.openlocfilehash: 32f366bcce7ce8fd40890cd2a383a41518b78758
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466668"
---
# <a name="integrating-reporting-services-into-applications"></a>将 Reporting Services 集成到应用程序中

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-2017](../../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../../includes/ssrs-appliesto-not-pbirs.md)]

  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 是一个开放和可扩展的报表平台，设计用于向开发人员提供一整套用于开发解决方案的 API。

> [!NOTE]
> 从 SQL Server 2017 Reporting Services 开始，REST API 访问可用于开发解决方案。 已弃用 SOAP API 访问。 有关详细信息，请参阅[使用 Reporting Services 的 REST API 进行开发](../developer/rest-api.md)。
  
 有三个选项可用于将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 集成到自定义应用程序中：报表服务器 Web 服务（也称作 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SOAP API）、用于 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 的报表查看器控件和 URL 访问。 每个选项都提供一个不同的方法来将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 集成到应用程序中。
  
## <a name="report-server-web-service"></a>报表服务器 Web 服务

 报表服务器 Web 服务是针对 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 进行开发的主接口。 无论您是开发代码以管理报表目录，还是开发代码以便用支持的格式呈现报表，该 Web 服务都公开所有必需的方法以便将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 集成到您的应用程序中。 Web 门户就是此类应用程序的一个示例，Web 门户随 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 一起提供，它使用该 Web 服务管理报表服务器数据库。  
  
## <a name="report-viewer-controls-for-visual-studio"></a>用于 Visual Studio 的报表查看器控件

 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 随附的报表查看器控件用于将报表查看集成到应用程序中。 存在两个此类控件：一个用于基于 Windows 窗体的应用程序，另一个用于 Web 窗体应用程序。 每个控件都提供查看已部署到报表服务器的报表的功能以及呈现在尚未安装报表服务器的环境中存在的报表的功能。  
  
## <a name="url-access"></a>URL 访问  
 URL 访问是用于将报表查看集成到应用程序中的另一个选项（如果无法选择报表查看器控件）。 此外，URL 访问还用于通过电子邮件将指向报表的链接发送给用户。  
  
## <a name="in-this-section"></a>在本节中

 [使用 SOAP 集成 Reporting Services](../../reporting-services/application-integration/integrating-reporting-services-using-soap.md)  
 说明如何使用报表服务器 Web 服务将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表导航和管理集成到您的现有业务应用程序中。  
  
 [使用报表查看器控件集成 Reporting Services](../../reporting-services/application-integration/integrating-reporting-services-using-reportviewer-controls.md)  
 说明如何使用报表查看器控件将报表查看集成到现有应用程序中。  
  
 [使用 URL 访问集成 Reporting Services](../../reporting-services/application-integration/integrating-reporting-services-using-url-access.md)  
 说明如何使用 URL 访问将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表导航集成到您的现有业务应用程序中。  
  
## <a name="next-steps"></a>后续步骤

要决定是使用 URL 访问还是 SOAP API，请参阅[在 Reporting Services 中的 URL 访问和 SOAP 之间选择](choosing-between-url-access-and-soap.md)。

有关 SQL Server 2017 Reporting Services REST API 的信息，请参阅[使用 Reporting Services 的 REST API 进行开发](../developer/rest-api.md)。

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)

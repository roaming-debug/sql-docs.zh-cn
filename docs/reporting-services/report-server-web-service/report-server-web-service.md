---
title: 报表服务器 Web 服务 | Microsoft Docs
description: Reporting Services 通过报表服务器 Web 服务提供报表服务器的功能，该服务是用于报表执行和管理的 SOAP 服务终结点。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- SSIS, Web service
- Web service [Reporting Services]
- Reporting Services, extending
- SQL Server Reporting Services, Web service
- Reporting Services, Web service
- XML Web service [Reporting Services]
- Report Server Web service
ms.assetid: 16c21dec-6b46-4497-9a0c-1b0f2b6ab8fc
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 55160717791f4d14a7f008725710f725069cd6d4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100022074"
---
# <a name="report-server-web-service"></a>报表服务器 Web 服务
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 通过报表服务器 Web 服务提供对报表服务器的全部功能的访问权限。 报表服务器 Web 服务是具有 SOAP API 的 XML Web 服务。 它使用 HTTP 上的 SOAP (SOAP over HTTP)，并且充当客户端程序与报表服务器之间的通信接口。 该 Web 服务提供两个端点（一个用于报表执行，一个用于报表管理）以及公开报表服务器的功能和使您能够为报表生命周期的任何部分创建自定义工具的方法。  
  
 可以通过三个主要方法基于 Web 服务开发 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 应用程序。 可以：  
  
-   使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 和 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] SDK 开发应用程序。 有关使用 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 生成 Web 服务应用程序的详细信息，请参阅[使用 Web 服务和 .NET Framework 生成应用程序](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)。  
  
-   使用 **rs** 实用工具 (RS.exe)（ [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 脚本环境）开发应用程序。 使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 和 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 脚本，可以运行任何报表服务器 Web 服务操作。 有关 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中脚本编写的详细信息，请参阅[使用 rs.exe 实用工具和 Web 服务编写脚本](../../reporting-services/tools/script-with-the-rs-exe-utility-and-the-web-service.md)。  
  
-   使用任何支持 SOAP 的开发工具集开发应用程序。 有关详细信息，请参阅 [SOAP 在 Reporting Services 中的作用](../../reporting-services/report-server-web-service/the-role-of-soap-in-reporting-services.md)。  
  
## <a name="programming-diagram"></a>编程关系图  
 ![报表服务器 Web 服务部署选项](../../reporting-services/report-server-web-service/media/reportserviceswebserviceprog-01.gif "报表服务器 Web 服务部署选项")  
Reporting Services 可用 Web 服务开发选项  
  
## <a name="in-this-section"></a>本节内容  
 [报表服务器 Web 服务方法](../../reporting-services/report-server-web-service/methods/report-server-web-service-methods.md)  
 介绍每个报表服务器 Web 服务的功能和方法。  
  
 [SOAP 在 Reporting Services 中的作用](../../reporting-services/report-server-web-service/the-role-of-soap-in-reporting-services.md)  
 概述 SOAP 以及如何在报表服务器 Web 服务中使用 SOAP。  
  
 [访问 SOAP API](../../reporting-services/report-server-web-service/accessing-the-soap-api.md)  
 介绍 Web 服务描述语言 (WSDL) 并提供用于访问 Reporting Services WSDL 文件的 URL。  
  
 [使用 Web 服务和.NET Framework 构建应用程序](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)  
 包含与开发调用 Reporting Services SOAP API 的应用程序和 Web 服务有关的信息。  
  
 [使用 rs.exe 实用工具和 Web 服务编写脚本](../../reporting-services/tools/script-with-the-rs-exe-utility-and-the-web-service.md)  
 概要介绍 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 脚本编写环境。  
  
 [技术参考 (SSRS)](../../reporting-services/technical-reference-ssrs.md)  
 包含特定于报表服务器 Web 服务方法以及相应复杂类型的参考材料。  
  
## <a name="user-requirements-for-web-service-development"></a>针对 Web 服务开发的用户要求  
 若要使用报表服务器 Web 服务开发应用程序，您需要：  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] Internet Explorer 5.5 或更高版本安装在具有与报表服务器的 Internet 连接或能够访问报表服务器的计算机上。  
  
-   在计算机上安装 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] SDK（若要使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)] 开发和部署 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 应用程序的话）。  
  
-   深入理解 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 功能。  
  
-   扎实理解 SOAP 和 [!INCLUDE[vstecwebservices](../../includes/vstecwebservices-md.md)]。  
  
-   具有使用与 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 兼容的语言（如 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[csprcs](../../includes/csprcs-md.md)] 或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]）进行开发的经验（如果计划将 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 用作开发平台的话）。  
  
## <a name="see-also"></a>另请参阅  
 [报表服务器 Web 服务](../../reporting-services/report-server-web-service/report-server-web-service.md)  
  
  

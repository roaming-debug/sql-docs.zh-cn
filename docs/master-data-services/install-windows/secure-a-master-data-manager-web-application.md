---
title: 保护主数据管理器 Web 应用程序
description: 在 SQL Server 中，可以通过 HTTPS 保护主数据管理器 web 应用程序。 必须是管理员，并且必须在 web 服务器上安装 MDS。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: e360ba3a-e96b-4f85-b588-ed1f767fa973
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 5e941ce4f06961f19e0d5f4a0dc96aff31039647
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344748"
---
# <a name="secure-a-master-data-manager-web-application"></a>保护主数据管理器 Web 应用程序

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  您可以使用 HTTPS 保护 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序。  
  
> [!NOTE]  
>  [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序可以使用 HTTP 或 HTTPS，但不是同时使用这两者。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须是安装了 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] 的 Web 服务器上的管理员。  
  
-   MDS 必须安装在 Web 服务器上，并且 Web 应用程序必须存在。 有关详细信息，请参阅 [安装 Master Data Services](../../master-data-services/install-windows/install-master-data-services.md) 和 [创建主数据管理器 Web 应用程序 (Master Data Services)](../../master-data-services/install-windows/create-a-master-data-manager-web-application-master-data-services.md)。  

- 不应启用[适用于 Windows 身份验证的 IIS 扩展保护](/iis/configuration/system.webserver/security/authentication/windowsauthentication/extendedprotection/)。 

- 将 web 服务器配置为侦听所有可用的 IP 地址。 不要将 Web 服务器配置为侦听特定的 IP 地址。 

### <a name="to-secure-the-master-data-manager-web-application-with-https"></a>使用 HTTPS 保护主数据管理器 Web 应用程序  
  
1.  在您确认已使用 HTTP 正确配置了 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序后，在 IIS 中创建一个证书。 有关详细信息，请参阅 [在 IIS 7 中配置服务器证书](https://technet.microsoft.com/library/cc732230\(WS.10\).aspx)。  
  
2.  在 **“连接”** 窗格中的 **“站点”** 的下面，单击承载 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序的站点。  
  
3.  在“操作”窗格中，单击“绑定”。  
  
4.  单击 **添加**。  
  
5.  从列表中选择 **https**。  
  
6.  选择 TLS/SSL 证书。  
  
7.  单击“确定”。   
  
8.  可选。 若要删除 HTTP 以便用户只能使用 HTTPS 访问站点，请单击含有 **http** 的行。 单击 **“删除”** ，然后在确认对话框上单击 **“是”**。  
  
    > [!IMPORTANT]  
    >  您必须在删除 HTTP 后更改 basicHttp 和 wsHttpBinding 配置。  
  
9. 若要关闭 **“站点绑定”** 对话框，请单击 **“关闭”**。  
  
10. 现在，从驱动器：\Program Files\Microsoft SQL Server\130\Master Data Services\WebApplication 打开 web.config 文件。  
  
11. 找到字符串 `<security mode="Message">` ，然后将其更改为 `<security mode="Transport">`。  

12. 将 `<serviceMetadata httpGetEnable="true" httpsGetEnabled="false">` 更改为 `<serviceMetadata httpGetEnable="false" httpsGetEnabled="true">`，以防止在 Silverlight 客户端中可能出现的问题。

13. 保存并关闭该文件。 如果您遇到错误，可能是因为您已启用了 UAC。 用户现在应该能够使用 HTTPS 访问该站点了。  

  
## <a name="see-also"></a>另请参阅  
 [创建主数据管理器 Web 应用程序 &#40;Master Data Services&#41;](../../master-data-services/install-windows/create-a-master-data-manager-web-application-master-data-services.md)  
  
  

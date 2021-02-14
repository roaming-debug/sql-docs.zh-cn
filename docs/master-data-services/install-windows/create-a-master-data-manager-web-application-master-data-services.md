---
title: 创建主数据管理器 web 应用程序
description: 主数据管理器 web 应用程序提供了一个界面，使用户能够使用主数据，并使管理员能够配置和管理 MDS。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 241d46d7-8008-47f6-bebd-0dfff1cc856a
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: df266fd09cfdd586d5f2fc438791104bf7097938
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344824"
---
# <a name="create-a-master-data-manager-web-application-master-data-services"></a>创建主数据管理器 web 应用程序 (Master Data Services) 

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序提供了一个接口，便于用户使用主数据，并供管理员用来配置和管理 MDS。  
  
 网站必须始终包含 Web 应用程序。 要创建 Web 应用程序，您必须：  
  
-   使用默认网站，然后创建 Web 应用程序；  
  
-   使用现有网站，然后创建 Web 应用程序；或者  
  
-   创建新网站，这将自动创建 Web 应用程序。  
  
 在您创建 Web 应用程序之后，将其与 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库相关联。  
  
## <a name="prerequisites"></a>先决条件  
  
-   有关承载 Web 应用程序的计算机的要求信息，请参阅 [Web 应用程序要求 (Master Data Services)](../../master-data-services/install-windows/web-application-requirements-master-data-services.md)。  
  
## <a name="to-create-a-master-data-manager-web-application-in-a-new-website"></a>在新网站中创建主数据管理器 Web 应用程序  
 当您创建新网站时，根 Web 应用程序为 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序。 也将该 Web 应用程序添加到新的应用程序池中。  
  
> [!NOTE]  
>  如果遵循此过程，则不能指定 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序的虚拟路径和别名。 如果要指定 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]的虚拟路径和别名，必须在尚未配置为 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序的现有网站中创建 Web 应用程序。  
  
 此外， [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] 支持仅使用 HTTP 绑定创建站点。 若要添加 HTTPS 绑定，请在 [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] 中创建新站点和应用程序，然后参阅 [保护主数据管理器 Web 应用程序](../../master-data-services/install-windows/secure-a-master-data-manager-web-application.md) 以获取详细信息。  
  
#### <a name="to-create-a-master-data-manager-web-application-in-a-new-website"></a>在新网站中创建主数据管理器 Web 应用程序  
  
1.  打开 [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)]。  
  
2.  在左窗格中单击 **“Web 配置”** 。  
  
3.  在 **“Web 配置”** 页上的网站列表中，选择 **“新建网站”**。  
  
4.  在 **“创建网站”** 对话框中，指定新网站的信息。 有关对话框中的用户界面 (UI) 选项的详细信息，请参阅 [创建网站对话框（Master Data Services 配置管理器）](../../master-data-services/create-website-dialog-box-master-data-services-configuration-manager.md)。  
  
5.  单击“确定”。   
  
## <a name="to-create-a-master-data-manager-web-application-in-an-existing-website"></a>在现有网站中创建主数据管理器 Web 应用程序  
 在现有网站中创建 Web 应用程序时，您可以选择 Web 应用程序的虚拟路径和别名。 将该 Web 应用程序添加到新的应用程序池中。  
  
#### <a name="to-create-a-master-data-manager-web-application-in-an-existing-website"></a>在现有网站中创建主数据管理器 Web 应用程序  
  
1.  打开 [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)]。  
  
2.  在左窗格中单击 **“Web 配置”** 。  
  
3.  在 **“Web 配置”** 页上，从 **“网站”** 列表中选择要创建 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序的网站。  
  
4.  单击 **“创建应用程序”**。  
  
5.  在 **“创建 Web 应用程序”** 对话框中，指定新 Web 应用程序的信息。 有关对话框中的用户界面 (UI) 选项的详细信息，请参阅 [创建 Web 应用程序对话框（Master Data Services 配置管理器）](../../master-data-services/create-web-application-dialog-box-master-data-services-configuration-manager.md)。  
  
6.  单击“确定”。   
  
## <a name="next-steps"></a>后续步骤  
  
-   将 Web 应用程序与 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库关联。 有关详细信息，请参阅 [将 Master Data Services 数据库与 Web 应用程序关联](../../master-data-services/install-windows/associate-a-master-data-services-database-and-web-application.md)。  
  
-   （可选） [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] 如果想要使用传输层安全性 (TLS) （以前称为安全套接字层 (SSL) ）对内容进行加密，请将承载 web 应用程序的网站配置为使用 HTTPS 绑定。 必须使用 Internet Information Services (IIS) 工具（如 IIS 管理器）来为 web 服务器配置服务器证书，以及为站点配置 HTTPS 绑定和 TLS 设置。 有关详细信息，请参阅 [Secure a Master Data Manager Web Application](../../master-data-services/install-windows/secure-a-master-data-manager-web-application.md)。  
  
## <a name="see-also"></a>另请参阅  
 [安装 Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)  
  
  

---
title: 创建 Master Data Services 数据库
description: 需要新数据库来支持主数据管理器 web 应用程序和 Master Data Services web 服务时，请创建 Master Data Services 数据库。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 8373bb35-f0f9-4c3c-a53c-dfaa2ce567ac
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 464bfd4714e7cc7680b2a2800d1c2ae1711feeb1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272478"
---
# <a name="create-a-master-data-services-database"></a>创建 Master Data Services 数据库

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在需要新数据库来支持 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] Web 应用程序和 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 服务时，创建 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库。  
  
## <a name="prerequisites"></a>先决条件  
  
-   有关承载该数据库的计算机的要求信息，请参阅[数据库要求 (Master Data Services)](../../master-data-services/install-windows/database-requirements-master-data-services.md)。  
  
### <a name="to-create-a-master-data-services-database"></a>创建 Master Data Services 数据库  
  
1.  打开 [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)]。  
  
2.  在左窗格中单击 **“数据库配置”** 。  
  
3.  在 **“数据库配置”** 页上，单击 **“创建数据库”**。  
  
4.  完成 **“创建数据库”** 向导来创建和配置数据库。 有关向导中的用户界面 (UI) 选项的信息，请参阅[创建数据库向导（Master Data Services 配置管理器）](../../master-data-services/create-database-wizard-master-data-services-configuration-manager.md)。  
  
## <a name="next-steps"></a>后续步骤  
  
-   为数据库和 Web 应用程序配置系统设置。 有关详细信息，请参阅[系统设置 (Master Data Services)](../../master-data-services/system-settings-master-data-services.md)。  
  
-   创建 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序以与此数据库关联。 有关详细信息，请参阅[创建主数据管理器 Web 应用程序 (Master Data Services)](../../master-data-services/install-windows/create-a-master-data-manager-web-application-master-data-services.md)。  
  
-   配置维护计划以备份数据库和事务日志。 有关详细信息，请参阅[数据库要求 (Master Data Services)](../../master-data-services/install-windows/database-requirements-master-data-services.md)。  
  
## <a name="see-also"></a>另请参阅  
 [安装 Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)  
  
  

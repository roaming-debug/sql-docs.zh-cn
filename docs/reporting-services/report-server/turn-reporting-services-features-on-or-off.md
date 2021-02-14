---
title: 打开或关闭 Reporting Services 功能 | Microsoft Docs
description: 了解如何关闭本机模式中的单个功能 - Reporting Services。 有不同的方法可用来配置功能。
ms.date: 06/10/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, configuration
- security [Reporting Services], strategies
ms.assetid: b69db02a-43a7-4fdc-ad9b-438d817a7f83
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: c6b42238a081adbd40f9756fb3d2fb8871e1d7cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023332"
---
# <a name="turn-reporting-services-features-on-or-off"></a>打开或关闭 Reporting Services 功能
  您可以关闭不用作锁定策略一部分的报表服务器功能，以减小生产报表服务器的攻击面。 在大多数情况下，需要同时运行各种 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 功能才能使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]中提供的所有功能。 但是根据所用的部署模型，您可以禁用不需要的功能。 例如，如果所有报表处理均已配置为预定操作，则可以只启用后台处理。 同样，如果只需要交互式的按需报表，可以只运行报表服务器 Web 服务。  
  
 本文中的过程展示了如何禁用本机模式 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 功能。 可以不同的方式配置这些功能，如直接编辑 `RsReportServer.config` 文件或使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中基于策略的管理的“Reporting Services 的外围应用配置”方面。 使用以下链接可以找到说明如何打开或关闭相应功能的步骤：  
  
-   [报表服务器 Web 服务](#RSWebSvc)  
  
-   [预定的事件和处理](#Sched)  
  
-   [Web 门户](#WebPortal)  
  
-   [适用于报表数据源的 Windows 集成安全性](#WinIntSec)  
  
##  <a name="report-server-web-service"></a><a name="RSWebSvc"></a> 报表服务器 Web 服务  
  
### <a name="to-turn-on-or-off-the-report-server-web-service-by-editing-configuration"></a>通过编辑配置来启用或禁用报表服务器 Web 服务的具体步骤  
  
1.  在文本编辑器中打开 `RsReportServer.config` 文件。 有关详细信息，请参阅[修改 Reporting Services 配置文件 (RSreportserver.config)](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)。  
  
2.  若要启用报表服务器 Web 服务，请将 IsWebServiceEnabled 设置为 true：  
  
    ```  
    <IsWebServiceEnabled>true</IsWebServiceEnabled>  
    ```  
  
3.  若要禁用报表服务器 Web 服务，请将 IsWebServiceEnabled 设置为 false：  
  
    ```  
    <IsWebServiceEnabled>false</IsWebServiceEnabled>  
    ```  
  
4.  保存所做的更改，然后关闭该文件。  
  
#### <a name="to-turn-on-or-off-the-report-server-web-service-by-using-sql-server-management-studio"></a>使用 SQL Server Management Studio 启用或禁用报表服务器 Web 服务的具体步骤  
  
1.  打开 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ，然后连接到要配置的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 实例。  
  
2.  在“对象资源管理器”中右键单击 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 节点，指向“策略”，然后单击“方面”。  
  
3.  在 **“方面”** 列表中，选择 **Reporting Services 的外围应用配置器**。  
  
4.  在 **“方面属性”** 下：  
  
    -   若要打开报表服务器 Web 服务，请将 **WebServiceAndHTTPAccessEnabled** 设置为 **True**。  
  
    -   若要关闭报表服务器 Web 服务，请将 **WebServiceAndHTTPAccessEnabled** 设置为 **False**。  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="scheduled-events-and-delivery"></a><a name="Sched"></a> 预定的事件和传递  
  
#### <a name="to-turn-on-or-off-scheduled-events-and-delivery-by-editing-configuration"></a>通过编辑配置打开或关闭预定的事件和传递  
  
1.  在文本编辑器中打开 RsReportServer.config 文件。 有关详细信息，请参阅[修改 Reporting Services 配置文件 (RSreportserver.config)](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)。  
  
2.  若要打开预定的报表处理和传递，请将 **IsSchedulingService**、 **IsNotificationService** 和 **IsEventService** 设置为 **true**：  
  
    ```  
    <IsSchedulingService>true</IsSchedulingService>  
    <IsNotificationService>true</IsNotificationService>  
    <IsEventService>true</IsEventService>  
    ```  
  
3.  若要关闭预定的报表处理和传递，请将 **IsSchedulingService**、 **IsNotificationService** 和 **IsEventService** 设置为 **false**：  
  
    ```  
    <IsSchedulingService>false</IsSchedulingService>  
    <IsNotificationService>false</IsNotificationService>  
    <IsEventService>false</IsEventService>  
    ```  
  
4.  保存所做的更改，然后关闭该文件。  
  
> [!NOTE]  
>  不能完全关闭后台处理，因为它提供执行服务器操作所需的数据库维护功能。  
  
##  <a name="web-portal"></a><a name="WebPortal"></a>Web 门户
  
从 SQL Server 2016 Reporting Services 累积更新 2 开始，将始终启用 Web 门户。
  
##  <a name="windows-integrated-security"></a><a name="WinIntSec"></a> Windows 集成安全性  
  
### <a name="to-turn-on-or-off-windows-integrated-security-by-using-sql-server-management-studio"></a>使用 SQL Server Management Studio 启用或禁用 Windows 集成安全性的具体步骤  
  
1.  打开 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ，然后连接到要配置的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 实例。  
  
2.  在对象资源管理器中右键单击 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 节点，然后单击“属性”。  
  
3.  在“服务器属性”对话框中，选择“选择页”下的“安全性”。  
  
    -   若要启用 Windows 集成安全性，请选中“为报表数据源启用 Windows 集成安全性”选项。  
  
    -   若要禁用 Windows 集成安全性，请取消选中“为报表数据源启用 Windows 集成安全性”选项。  
  
4.  选择“确定”。  
  
## <a name="see-also"></a>另请参阅  
[报表服务器配置管理器（本机模式）](../install-windows/reporting-services-configuration-manager-native-mode.md)

 更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
  

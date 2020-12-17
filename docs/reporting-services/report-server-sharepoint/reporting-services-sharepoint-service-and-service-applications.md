---
title: Reporting Services SharePoint 服务和服务应用程序 | Microsoft Docs
description: 在 SQL Server Reporting Services SharePoint 模式中创建服务应用程序将使该服务可用并生成服务应用程序数据库。
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 8a6513ab2798152e49afc09ae7e67b001b4ea4bd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461478"
---
# <a name="reporting-services-sharepoint-service-and-service-applications"></a>Reporting Services SharePoint 服务和服务应用程序

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

  Reporting Services SharePoint 模式的体系结构以 SharePoint 服务体系结构为基础，并利用 SharePoint 服务和一对多服务应用程序。 创建服务应用程序将使该服务可用并生成服务应用程序数据库。 您可以创建多个 Reporting Services 服务应用程序，但一个服务应用程序已足以用于大多数部署方案。  

> [!NOTE]
> 自 SQL Server 2016 之后，不再提供 Reporting Services 与 SharePoint 的集成这一功能。
  
## <a name="creating-a-reporting-services-service-application"></a>创建 Reporting Services 服务应用程序

 可以使用 SharePoint 管理中心或 PowerShell 脚本创建 Reporting Services 服务应用程序。 有关如何使用 SharePoint 管理中心的详细信息，请参阅[安装 SharePoint 2010 的 Reporting Services SharePoint 模式](../install-windows/install-the-first-report-server-in-sharepoint-mode.md)中的“创建 Reporting Services 服务应用程序”一节。 有关创建服务应用程序的示例 PowerShell 脚本，请参阅本主题中后面的 PowerShell 部分。  
  
## <a name="modify-the-associations-of-the-service-application-with-a-proxy-group"></a>修改服务应用程序与代理服务器组的关联

 创建服务应用程序的“新建”页包含 **“Web 应用程序关联”** 部分。 此部分允许您在创建服务应用程序时对其进行关联。 使用以下步骤可更改关联和将客户配置分配给服务应用程序。 还可以使用相同的常规过程将代理添加到默认组，而不是更改服务应用程序与自定义组的关联。  
  
1.  在 SharePoint 管理中心内，在“应用程序管理”中单击 **“配置服务应用程序关联”** 。  
  
2.  在“服务应用程序关联”页中，将视图更改为 **“服务应用程序”** 。  
  
3.  找到并单击新 Reporting Services 服务应用程序的名称。 也可以单击应用程序代理组名称 **默认值** 以将代理添加到默认组，而不是完成以下步骤。  
  
4.  在 **“编辑以下连接组”** 选择框中，选择 **自定义**。  
  
5.  选中代理对应的框，然后单击 **“确定”** 。  
  
## <a name="edit-service-application-properties"></a>编辑服务应用程序属性

 您可以重新打开服务应用程序的属性页以修改属性。  
  
1.  在 SharePoint 管理中心内，在“应用程序管理”组中，单击 **“管理服务应用程序”** 。  
  
2.  通过单击类型列以选择整行，选择服务应用程序。 如果您单击应用程序的名称，将打开服务的“管理”选项页，而不是打开服务应用程序的属性。  
  
3.  在“服务应用程序”功能区中，单击 **“属性”** 。  
  
## <a name="create-a-reporting-services-service-application-using-powershell"></a>使用 PowerShell 创建 Reporting Services 服务应用程序

 您可以使用 PowerShell 创建服务应用程序和代理。 下面的示例假定您知道要配置服务应用程序使用的应用程序池。  
  
1.  将应用程序池名称的应用程序池对象添加到要传递到“新建”操作的变量。  
  
    ```  
    $appPoolName = get-spserviceapplicationpool "<application pool name>"  
    ```  
  
2.  使用您提供的名称和应用程序池名称创建服务应用程序。  
  
    ```  
    New-SPRSServiceApplication -Name 'MyServiceApplication' -ApplicationPool $appPoolName -DatabaseName 'MyServiceApplicationDatabase' -DatabaseServer '<Server Name>'  
    ```  
  
3.  获取新的服务应用程序对象，并将此对象传送到新代理 cmdlet 的管道中。  
  
    ```  
    Get-SPRSServiceApplication -name MyServiceApplication | New-SPRSServiceApplicationProxy "MyServiceApplicationProxy"  
    ```  
  
## <a name="related-tasks"></a>相关任务
  
|任务|链接|  
|----------|----------|  
|管理您的服务应用程序的设置。|[管理 Reporting Services SharePoint 服务应用程序](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)|  
|备份和还原服务应用程序及相关的组件，如加密密钥和代理。|[备份和还原 Reporting Services SharePoint 服务应用程序](../../reporting-services/report-server-sharepoint/backup-and-restore-reporting-services-sharepoint-service-applications.md)|  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
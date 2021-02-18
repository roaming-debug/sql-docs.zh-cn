---
title: 卸载 Reporting Services | Microsoft Docs
description: 本文介绍如何卸载 Reporting Services，这不会删除已创建的内容或已修改的配置。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 5c764a00-d4bc-465d-b32e-e4efce052ce4
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 650f22c78e0913d8b92c431de65963b802232141
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336054"
---
# <a name="uninstall-reporting-services"></a>卸载 Reporting Services
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  卸载 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 不会删除您已创建的内容或已修改的配置。 但是，如果存在在卸载完成后您需要的内容，建议您首先生成内容的副本，再开始卸载过程。  
  
## <a name="uninstall-sharepoint-mode"></a>卸载 SharePoint 模式  
 卸载 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式时，删除以下内容：  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务和服务代理。  
  
-   用于 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装的文件。  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序不会删除。 如果您不再需要服务应用程序，可以通过使用 Windows PowerShell 或 SharePoint 管理中心删除它们。  
  
 不删除报表项和相关的元数据。 此信息包含在与 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序有关的内容和配置数据库中。 在 SharePoint 模式下，不删除这些数据库，您可以手动将它们迁移到 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的另一安装中。 如果您不再需要此信息，请删除这些数据库。 有关详细信息，请参阅 [Upgrade and Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)。  
  
 以下是不删除的三个 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据库的示例名称。  
  
-   **报表服务器数据库：** ReportingService_7f616e2d253040e8ab5653b3c09a065e  
  
-   **报表服务器临时数据库：** ReportingService_7f616e2d253040e8ab5653b3c09a065eTempDB  
  
-   **报表服务器警报数据库：** ReportingService_7f616e2d253040e8ab5653b3c09a065e_Alerting  
  
### <a name="uninstall-the-add-in-for-sharepoint-products"></a>卸载用于 SharePoint 产品的外接程序。  
 从计算机卸载外接程序时，可以选择仅卸载这些文件或同时从场中删除 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 功能。 有关卸载用于 SharePoint 产品的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 外接程序的信息，请参阅 [安装或卸载用于 SharePoint 的 Reporting Services 外接程序](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md)。  
  
## <a name="uninstall-native-mode"></a>卸载本机模式  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 在您卸载  本机模式时，在安装后创建  或修改的所有内容都将在原地保留。 例如，数据库文件、日志文件、 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置文件以及一些内容项（如报表和数据源文件）。  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 是实例功能，因此不在 Windows“控制面板”的“程序和功能”中列出。 卸载 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 本机模式：  
  
1.  在 Windows“控制面板”中，单击 **“程序和功能”** 。  
  
2.  在“程序和功能”中，选择“Microsoft SQL Server 2016” 。  
  
3.  在卸载向导中，选择包含 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 实例功能 **RS** 的实例。  
  
     ![rs_nativemode_uninstall_selectinstance](../../sql-server/install/media/rs-nativemode-uninstall-selectinstance.gif "rs_nativemode_uninstall_selectinstance")  
  
4.  选择该实例后，选择 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 功能。  
  
     ![rs_nativemode_uninstall_selectfeatures](../../sql-server/install/media/rs-nativemode-uninstall-selectfeatures.gif "rs_nativemode_uninstall_selectfeatures")  
  
5.  完成向导。  
  
## <a name="see-also"></a>另请参阅  
 [卸载现有 SQL Server 实例（安装程序）](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md)   
 [安装或卸载 Power Pivot for SharePoint 外接程序 (SharePoint 2013)](/analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013)   
 [安装或卸载用于 SharePoint 的 Reporting Services 外接程序](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md)  
  

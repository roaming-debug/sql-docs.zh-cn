---
title: SQL Server Data Tools 中的部署和版本支持 (SSDT) | Microsoft Docs
description: 了解可在 SQL Server Data Tools 中创作和发布报表的支持版本，以及如何设置项目配置和部署属性。
ms.date: 03/15/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
ms.assetid: 36f5686d-7e40-4f31-be81-bd197ca33a02
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: c820c7a9e0cefaee2a22a401213754d0a3274152
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596175"
---
# <a name="deployment-and-version-support-in-sql-server-data-tools-ssdt"></a>SQL Server Data Tools 中的部署和版本支持 (SSDT)
  [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 支持以下方案：  
  
-   打开报表定义 (*.rdl) 和报表服务器项目 (\*.rptproj)。  
  
-   生成报表定义。  
  
-   在报表设计器中预览报表。  
  
-   将报表部署到报表服务器。  
  
##  <a name="configuration-and-deployment-properties"></a><a name="bkmk_ConfigurationandDeploymentProperties"></a> 配置和部署属性  
 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 支持项目配置。 项目配置由一组属性组成，当作为预览或部署报表的一个步骤生成项目时，这些属性指定位置和行为。 若要了解有关项目配置的更多信息，请参阅 Visual Studio 文档。  
  
 使用项目配置可以控制报表定义升级到与目标报表服务器兼容的架构版本。 项目配置控制的属性包括目标报表服务器、生成进程临时存储报表定义以便预览和部署的文件夹以及错误级别。  
  
 首先会生成报表，然后将其在报表设计器中以预览方式呈现，或者部署到报表服务器。  
  
 可以在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 的“项目属性”对话框中设置配置属性。  
  
 生成和部署属性包括：  
  
-   OutputPath 是一个生成属性，它标识用于存储在报表生成验证、部署和预览过程中使用的报表定义的文件夹路径。  
  
-   ErrorLevel 是一个生成属性，它标识报告为错误的生成问题的严重性。 严重级别小于或等于 ErrorLevel 的值的问题将报告为错误；否则，将问题报告为警告。 有关详细信息，请参阅[使用报表设计器设计报表 (SSRS)](../../reporting-services/tools/design-reporting-services-paginated-reports-with-report-designer-ssrs.md) 中的“报表验证和错误级别”一节。  
  
-   TargetServerVersion 是一个部署属性，它标识安装在目标报表服务器（在 TargetServerURL 属性中指定）上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的预期版本。  
  
 当你在“项目属性”对话框中指定早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 时，报表不会自动恢复到早期版本。 同样，报表服务器项目可以包含来自两个不同版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的报表。 当部署报表服务器项目时，项目中的所有报表都将转换为在 TargetServerVersion 中指定的版本。  
  
 您可以将多个项目配置添加到项目中；每个配置用于不同方案，例如，部署到不同版本的报表服务器。 有关详细信息，请参阅[设置部署属性 (Reporting Services)](../../reporting-services/tools/set-deployment-properties-reporting-services.md) 和[“项目属性页”对话框](../../reporting-services/tools/project-property-pages-dialog-box.md)。  
  
##  <a name="supported-versions"></a><a name="bkmk_SupportedVersions"></a> 支持的版本  
  
> [!NOTE]  
>  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]，是 32 位报表服务器项目开发环境，根据设计，它不能在基于 [!INCLUDE[vcpritanium](../../includes/vcpritanium-md.md)] 的计算机上运行，也不能在基于 [!INCLUDE[vcpritanium](../../includes/vcpritanium-md.md)] 的服务器上安装。 但是，基于 x64 的计算机可提供对 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 的支持。  
  
 下表说明了可在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中创作和发布报表的支持版本。  
  
> [!NOTE]  
>  架构自 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]以来未更改。  
  
|项目或文件类型|版本|创作报表|发布报表|说明|  
|--------------------------|-------------|--------------------|---------------------|-----------|  
|报表服务器项目<br /><br /> 或<br /><br /> 报表服务器向导项目|[!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)]|2016 RDL 架构|[!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)]||  
|报表服务器项目<br /><br /> 或<br /><br /> 报表服务器向导项目|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|2014 RDL 架构|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]||  
|报表服务器项目<br /><br /> 或<br /><br /> 报表服务器向导项目|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|2012 RDL 架构|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]||  
|报表服务器项目<br /><br /> 或<br /><br /> 报表服务器向导项目|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|2008 R2 RDL 架构|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]||  
|报表服务器项目<br /><br /> 或<br /><br /> 报表服务器向导项目|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|2008 RDL 架构|仅 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表服务器|在本地将 2003 RDL 和 2005 RDL 升级到 2008 RDL 架构。|  
  
 有关在以前版本的报表定义架构中打开报表的详细信息，请参阅 [升级报表](../../reporting-services/install-windows/upgrade-reports.md)。 有关特定报表定义架构的详细信息，请参阅 [Report Definition Language Specification](/openspecs/sql_server_protocols/ms-rdl/53287204-7cd0-4bc9-a5cd-d42a5925dca1)（报表定义语言规范）。  
  
## <a name="see-also"></a>另请参阅  
 [发布数据源和报表](../../reporting-services/reports/publishing-data-sources-and-reports.md)  
  

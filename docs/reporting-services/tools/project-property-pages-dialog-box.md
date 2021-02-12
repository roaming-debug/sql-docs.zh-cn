---
title: “项目属性页”对话框 | Microsoft Docs
description: 了解”项目属性页“对话框中的选项，使用该对话框可以配置报表服务器项目的部署属性。
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: reference
f1_keywords:
- sql13.rpt.rptdesigner.projectpropertypages.general.f1
helpviewer_keywords:
- Project Property Pages dialog box
ms.assetid: 209d9e22-37fc-418f-8739-83adcf447d3f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d9f271797e0a6750f858fb0da5996a25de1746db
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077998"
---
# <a name="project-property-pages-dialog-box"></a>“项目属性页”对话框

  使用项目属性页可以配置报表服务器项目的部署属性。 若要打开此对话框，请在“项目”菜单中，单击\<Report Project Name>“属性”。  
  
 定义配置属性后，可以从位于工具栏上的“解决方案配置”下拉列表中选择配置。  

![ssrs_project_properties](../../reporting-services/reports/media/ssrs-project-properties.png)
  
## <a name="options"></a>选项  
 **配置**  
 选择要编辑的配置。 最初，可使用以下配置：Debug、DebugLocal 和 Release  。 首先显示活动配置，例如 **Active(Debug)** 。  
  
 若要同时查看多个配置的属性，请选择 **“所有配置”** 或 **“多个配置”** 。  
  
 若要创建其他配置，请单击工具栏上的 **“配置管理器”** 。  
  
 **配置管理器**  
 管理整个解决方案的配置或添加其他配置。 有关详细信息，请参阅 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 文档。  
  
 **OutputPath**  
 键入或粘贴用于存储在报表生成验证、部署和预览过程中使用的报表定义的路径。 此路径必须与您用于项目的路径不同，并且它是一个相对路径（即项目路径下的一个子文件夹）。  
  
> [!NOTE]  
>  可以使用多个配置，以便根据您执行的任务在路径间切换。  
  
 **ErrorLevel**  
 键入报告为错误的生成问题的严重性。 严重级别小于或等于 **ErrorLevel** 的值的问题将报告为错误；否则，将问题报告为警告。 任何错误都将导致生成任务失败。 有效的严重级别为 0 到 4（包括这两者）。 默认值为 2。  
  
 **StartItem**  
 选择在项目发布到报表服务器之后将显示在 Web 浏览器中的报表，或选择在本地运行项目时将显示在预览窗口中的报表。 对于生成项目但并不部署项目的配置，以及使用 **Debug** 命令 (**F5**)，启动项是必需的。 对于部署项目的配置，启动项也是必需的。  
  
 **OverwriteDataSources**  
 选择 **True** 可在发布报表时使用项目中的数据源覆盖服务器上的数据源。 选择 **False** 可保留服务器上的现有数据源。  
  
 **TargetServerVersion**  
 选择 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的适当版本，或者选择“检测版本”以自动确定安装在由 **TargetServer URL** 属性标识的服务器上的版本。 默认值为 SQL Server 2017。  
  
 **TargetDataSourceFolder**  
 要在其中存储已发布共享数据源的文件夹的名称。 如果您没有指定文件夹，那么数据源将发布到与报表所在文件夹相同的文件夹。 如果报表服务器上没有该文件夹，则报表设计器将在发布报表时创建该文件夹。  
  
 发布到在本机模式下运行的报表服务器时，指定文件夹层次结构的完整路径（从根文件夹开始）。 例如，Folder1/Folder2/Folder3。  
  
 发布到在 SharePoint 集成模式下运行的报表服务器时，请使用 SharePoint 库的 URL。 例如，`http:\\<servername>\<site>\Documents\MyFolder`。  
  
 **TargetReportFolder**  
 要在其中存储已发布报表的文件夹的名称。 默认情况下，此名称即为报表项目的名称。 如果报表服务器上没有该文件夹，则报表设计器将在发布报表时创建该文件夹。  
  
 发布到在本机模式下运行的报表服务器时，指定文件夹层次结构的完整路径（从根文件夹开始）。 如果文件夹位于另一个文件夹内，则包括从根目录开始的到该文件夹的路径，如 Folder1/Folder2/Folder3。  
  
 发布到在 SharePoint 集成模式下运行的报表服务器时，请使用 SharePoint 库的 URL。 例如，`http:\\<servername>\\<site>\Documents\MyFolder`。  
  
 **TargetServerURL**  
 目标报表服务器的 URL。 在发布报表之前，必须将此属性设置为有效的报表服务器 URL。  
  
 发布到在本机模式下运行的报表服务器时，请使用此报表服务器的虚拟目录 URL。 例如，`http:\\<server>\reportserver`。 这是报表服务器的虚拟目录，而不是报表管理器的虚拟目录。 默认情况下，报表服务器安装在名为“reportserver”的虚拟目录中。  
  
 发布到在 SharePoint 集成模式下运行的报表服务器时，请使用 SharePoint 顶级站点或子站点的 URL。 如果不指定站点，将使用默认顶级站点。 例如： 
+ `http:\\<servername>`, 
+ `http:\\<servername\<site>` 
+ `http:\\<servername>\<site>\<subsite>`.  

## <a name="next-steps"></a>后续步骤

[发布报表](/previous-versions/sql/sql-server-2016/ms159615(v=sql.130))   
[将报表发布到 SharePoint 库](../../reporting-services/reports/publish-a-report-to-a-sharepoint-library.md)   
[设置部署属性 (Reporting Services)](../../reporting-services/tools/set-deployment-properties-reporting-services.md)   
[报表设计器的 F1 帮助](../../reporting-services/tools/report-designer-f1-help.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
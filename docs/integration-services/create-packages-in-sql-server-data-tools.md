---
description: 在 SQL Server Data Tools 中创建包
title: 在 SQL Server Data Tools 中创建包 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- SSIS packages, creating
- Integration Services packages, creating
- packages [Integration Services], creating
- SQL Server Integration Services packages, creating
ms.assetid: bb3c085b-1458-49fa-8348-6a76b6e97ea6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 72529ac177ddd069ca7e8412da375db45fe958e2
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127382"
---
# <a name="create-packages-in-sql-server-data-tools"></a>在 SQL Server Data Tools 中创建包

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  在 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]中，可以使用以下方法之一来创建新包：  
  
-   使用 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 包含的包模板。  
  
-   使用自定义模板  
  
     若要使用自定义包作为创建新包的模板，只需将它们复制到 DataTransformationItems 文件夹中即可。 默认情况下，此文件夹位于 C:\Program Files\Microsoft Visual Studio 10.0\Common7\IDE\PrivateAssemblies\ProjectItems\DataTransformationProject。  
  
-   复制现有包。  
  
     如果现有包中包括了您希望重用的功能，则可以通过复制并粘贴其他包中的对象，在新包中更快地生成控制流和数据流。 有关在 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目中使用复制和粘贴的详细信息，请参阅 [重用包对象](../integration-services/reuse-of-package-objects.md)。  
  
     如果通过复制现有包或使用自定义包作为模板来创建新包，则现有包的名称和 GUID 也会被复制。 应当更新新包的名称和 GUID，以便将它与原始包区分开来。 例如，如果包有相同的 GUID，则难以识别日志数据属于哪个包。 可以通过使用 **中的“属性”窗口，重新生成** ID **属性中的 GUID 以及更新** Name [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]属性的值。 有关详细信息，请参阅 [设置包属性](../integration-services/set-package-properties.md) 和 [dtutil 实用工具](../integration-services/dtutil-utility.md)。  
  
-   使用已指定为模板的自定义包。  
  
-   运行 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 导入和导出向导  
  
     [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 导入和导出向导创建一个用于简单导入或导出的完整包。 此向导可以配置连接、源和目标，以及添加允许您立即运行导入或导出所需的任何数据转换。 您还可以保存包以便以后再次运行该包，或者在 [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]中完善和增强该包。 但是，如果保存该包，则必须先将该包添加到现有的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目中，然后才能更改该包或者在 [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]中运行该包。  
  
 使用 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] 设计器在 [!INCLUDE[ssIS](../includes/ssis-md.md)] 中创建的包被保存到文件系统。 若要将包保存到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 或保存到包存储区，则需要保存包的副本。 有关详细信息，请参阅 [保存一个包副本](./save-packages.md)。  

 有关演示如何使用默认的包模板创建基本包的视频，请参阅 [创建基本包（SQL Server 视频）](/previous-versions/sql/sql-server-2008/cc952921(v=sql.100))。  

## <a name="get-sql-server-data-tools"></a>获取 SQL Server Data Tools
若要安装 SQL Server Data Tools (SSDT)，请参阅 [下载 SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)。

## <a name="create-a-package-in-sql-server-data-tools-using-the-package-template"></a>在 SQL Server Data Tools 中使用包模板创建包  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]中，打开要在其中创建包的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，右键单击“SSIS 包”文件夹，然后单击“新建 SSIS 包”。  
  
3.  还可以向包中添加控制流、数据流任务和事件处理程序。 有关详细信息，请参阅[控制流](../integration-services/control-flow/control-flow.md)、[数据流](../integration-services/data-flow/data-flow.md)和[Integration Services (SSIS) 事件处理程序](../integration-services/integration-services-ssis-event-handlers.md)。  
  
4.  在 **“文件”** 菜单上，单击 **“保存选定项”** ，以保存新建的包。  
  
    > [!NOTE]  
    >  可以保存空包。  
  
## <a name="choose-the-target-version-of-a-project-and-its-packages"></a>选择项目的目标版本及其包  
  
1.  在解决方案资源管理器中，右键单击 Integration Services 项目并选择“属性”  以打开该项目的属性页。  
  
2.  在“配置属性”  的“常规” 选项卡上，选择“TargetServerVersion”  属性，然后选择 SQL Server 2016、SQL Server 2014 或 SQL Server 2012。  
  
     ![“项目属性”对话框中的 TargetServerVersion 属性](../integration-services/media/targetserverversion2.png "“项目属性”对话框中的 TargetServerVersion 属性")  
  
 你可以创建、维护和运行面向 SQL Server 2016、SQL Server 2014 或 SQL Server 2012 的包。  
  

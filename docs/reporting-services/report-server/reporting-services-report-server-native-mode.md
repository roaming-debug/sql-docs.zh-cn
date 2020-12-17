---
title: Reporting Services 报表服务器（本机模式）| Microsoft Docs
description: 了解为本机模式配置的报表服务器，包括管理内容、管理资源以及从报表引用图像资源。
ms.date: 04/21/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- administering Reporting Services
- administering [Reporting Services]
- Reporting Services, administration
ms.assetid: fa0d84e2-4c21-432c-aa7c-23517da75253
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 651abd2ca0261deae83d77a15b88840499dce957
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482234"
---
# <a name="reporting-services-report-server-native-mode"></a>Reporting Services 报表服务器（本机模式）
  配置为本机模式的报表服务器将作为应用程序服务器运行，并专门通过 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]组件提供所有处理和管理功能。  
  
 可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 或 Web 门户来管理 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表。 使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器可在本机模式下管理报表服务器。  
  
 如果将报表服务器配置为 SharePoint 模式，则必须使用 SharePoint 站点上的内容管理页来管理报表、共享数据源和其他报表服务器项。  
  
 本文介绍了以下信息：  
  
-   [本机模式摘要](#bkmk_sum)  
  
-   [管理内容](#bkmk_managecontent)  
  
-   [保护和管理资源](#bkmk_manageresources)  
  
-   [引用报表中的图像资源](#bkmk_referenceimage)  
  
##  <a name="summary-of-native-mode"></a><a name="bkmk_sum"></a> 本机模式摘要  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 本机模式安装由几个需要管理和维护的服务器端功能组成。 这些服务器功能包括：  
  
-   报表服务器 Web 服务（在报表服务器服务中运行）。  
  
-   后台处理应用程序，用于处理计划操作和报表传递。  
  
-   报表服务器数据库。  
  
 若要完全管理 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装，必须具有以下权限：  
  
-   报表服务器计算机上的本地 Administrator 组中的成员身份。 如果安装包括在远程计算机上运行的服务器功能，则在要通过远程连接来管理这些服务器时，必须对这些计算机具有管理员权限。  
  
-   对承载数据库的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的数据库管理员权限。  

::: moniker range="=sql-server-2016"

-   如果要在域控制器上安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ，则您必须是域管理员。  

::: moniker-end

##  <a name="managing-content"></a><a name="bkmk_managecontent"></a> 管理内容  
 在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]中，内容管理是指对报表、模型、文件夹、资源和共享数据源的管理。 通过属性和安全设置，所有这些项可以彼此独立地进行管理。 可以将任何一项移动到报表服务器文件夹命名空间中的不同位置。 为了有效管理项，您需要了解道内容管理员所执行的任务。  
  
> [!NOTE]  
>  内容管理不同于报表服务器管理。 有关如何管理报表服务器运行环境的详细信息，请参阅[配置和管理报表服务器（Reporting Services SharePoint 模式）](../../reporting-services/report-server-sharepoint/configuration-and-administration-of-a-report-server.md)。  
  
 内容管理包括以下任务：  
  
-   通过应用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]提供的基于角色的安全设置，确保报表服务器站点和项的安全。  
  
-   通过添加、修改和删除文件夹构建报表服务器文件夹的层次结构。  
  
-   设置应用到报表服务器所管理的项的默认值和属性。 例如，您可以设置确定报表历史记录存储策略的基线最大值。  
  
-   创建共享数据源项，这些数据源项可用来代替报表特定数据源连接。 发布者或内容管理员可以选择与最初为报表定义的数据源不同的其他数据源；例如用对生产数据库的引用代替对测试数据库的引用。  
  
-   创建可代替报表特定计划和订阅特定计划的共享计划，可以更方便地长时间维护计划信息。  
  
-   创建数据驱动订阅，通过从数据存储中检索数据来生成收件人列表。  
  
-   通过制定报表处理计划，并指定哪些报表按需运行，哪些报表应从缓存加载，从而均衡服务器上的报表处理需求。  
  
 执行管理任务的权限是通过以下两个预定义的角色提供的：“系统管理员”和“内容管理员”。 若要有效地管理报表服务器内容，要求您同时分配有这两个角色。 有关这些预定义角色的详细信息，请参阅[角色和权限 (Reporting Services)](../../reporting-services/security/roles-and-permissions-reporting-services.md)。  
  
 用于管理报表服务器内容的工具包括 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 或 Web 门户。 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 允许您设置默认值和启用功能。 Web 门户用于向用户授予对报表服务器项和操作的访问权限，用于查看和使用报表以及其他内容类型，并用于查看和使用所有共享项和报表分发功能。  
  
##  <a name="securing-and-managing-a-resource"></a><a name="bkmk_manageresources"></a> 保护和管理资源  
 资源是指存储在报表服务器上但不由报表服务器处理的托管项。 通常，资源为报表用户提供外部内容。 例如描述报表中所使用业务规则的 .jpg 文件或 HTML 文件中的图像。 JPG 或 HTML 文件存储在报表服务器上，但报表服务器会将文件直接传递到浏览器，而不会首先对其进行处理。  
  
 若要向报表服务器中添加资源，请上载或发布文件：  
  
|Operation|文件类型|  
|---------------|---------------|  
|上载|除报表定义 (.rdl) 文件和报表模型 (.smdl) 文件之外的所有文件都将作为资源上载。<br /><br /> 若要上传资源，如果报表服务器在本机模式下运行，必须使用 Web 门户；如果报表服务器在 SharePoint 集成模式下运行，必须使用 SharePoint 网站上的应用程序页。 有关详细信息，请参阅[在报表服务器中上传文件或报表](../../reporting-services/reports/upload-a-file-or-report-report-manager.md)，或[将文档上传到 SharePoint 库（SharePoint 模式下的 Reporting Services）](../../reporting-services/report-server-sharepoint/upload-documents-to-a-sharepoint-library-reporting-services-in-sharepoint-mode.md)。|  
|发布|除 .rdl、.smdl 和 .rds 数据源文件之外，项目中的所有文件都将作为资源上载。 若要发布资源，请将现有项添加到报表设计器的一个项目中，然后将该项目发布到报表服务器。|  
  
 所有资源最初都是文件系统中的文件，只是随后上载到报表服务器上而已。 你可以上传的文件类型没有限制，但文件大小不得超过 1GB。 不过，具有等效 MIME 类型的文件类型比其他类型更适于作为资源发布到报表服务器。 例如，在用户单击基于 HTML 和 JPG 文件的资源时，这些资源将在浏览器窗口中打开，以网页形式呈现 HTML 文件，并以图像形式呈现 JPG 文件，这样，用户就可以看到。 相反，对于不具有等效 MIME 类型的资源（如桌面应用程序文件），则不能在浏览器窗口中呈现。  
  
 报表用户是否可以查看资源取决于浏览器的查看功能。 由于报表服务器不对资源进行处理，因此浏览器必须提供用于呈现特定 MIME 类型的查看功能。 如果浏览器无法呈现资源的内容，则查看资源的用户只能看到资源的常规属性。  
  
 在报表服务器文件夹层次结构中，资源与报表、共享数据源、共享计划和文件夹都以命名项的形式显示在一起。 您可以搜索、查看、保护资源和设置资源属性，就像对报表服务器上存储的任何其他项一样。 若要查看或管理资源，您的角色分配中必须拥有查看资源或管理资源的任务。  
  
##  <a name="referencing-an-image-resource-from-a-report"></a><a name="bkmk_referenceimage"></a> 引用报表中的图像资源  
 资源可以包含报表中引用的图像。 如果报表要求包括使用外部图像，则可以考虑将图像存储为资源的以下好处：  
  
-   在报表服务器数据库中集中存储。 如果将报表服务器数据库及其内容移到其他计算机，则外部图像会与报表保存在一起。 无需跟踪不同计算机的磁盘上存储的图像文件。  
  
-   通过角色分配而不是文件系统安全来进行保护。 可以将用于查看报表的权限同样应用于资源。 相反，如果将图像存储在磁盘上，则必须确保匿名用户帐户或无人参与的执行帐户拥有访问该文件的权限。  
  
 若要在报表中使用某个图像资源，请将该图像文件添加到项目，并与报表一起发布。 发布图像之后，可以更新报表中的图像引用，使之指向报表服务器上的相应资源，然后只需重新发布该报表即可保存所做的更改。 随后，即可通过重新发布资源来独立更新报表的图像。 报表将使用报表服务器上可用的最新版本的图像。  
  
## <a name="see-also"></a>另请参阅  
 [配置和管理报表服务器（SSRS 本机模式）](../../reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode.md)   
 [排除 Reporting Services 安装故障](../../reporting-services/install-windows/troubleshoot-a-reporting-services-installation.md)  
  
  

---
title: 管理报表部件 | Microsoft Docs
description: 通过发布、权限和安全性的灵活选项整理多个用户和报表的报表部件。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 41947b4c-8ecf-4e4f-b30e-66e1d6692b74
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1ac27d7f3f83a65f6a9e7b06da511bd9b9baf751
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062632"
---
# <a name="managing-report-parts"></a>管理报表部件
  报表部件可在分页报表中由多个用户重用，并且可以在多个报表中重用。 用户可以搜索服务器上的报表部件并将其添加到报表中。  用户还可以接收服务器上的报表部件更新通知，以及重新发布报表部件的新版本。 这些报表创作操作可能受 Reporting Services 安全权限的影响和控制。  本主题介绍报表部件在位于服务器上后的属性和行为。  
  
## <a name="managing-report-parts"></a>管理报表部件  
 若要管理报表部件，在本机模式下，可以将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 门户用于报表服务器；在 SharePoint 集成模式下，可以将应用程序页用于报表服务器。  
  
### <a name="server-side-interaction-and-search"></a>服务器端交互和搜索  
 报表部件可在本机模式或 SharePoint 集成模式下发布到报表服务器上。 用户可以使用报表创作应用程序（例如 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 报表生成器）中的报表部件库功能来查找报表部件以及将报表部件添加到其报表中。 当用户搜索报表部件时，搜索将查找报表服务器目录，而与安装服务器时采用的模式无关。  
  
 在从报表生成器之类的报表创作应用程序将报表部件发布到 SharePoint 集成模式下的报表服务器时，报表服务器目录也将更新，从库中搜索将精确反映新的或更新的报表部件。  
  
#### <a name="directly-uploading-report-parts-to-a-sharepoint-folder"></a>将报表部件直接上载到 SharePoint 文件夹  
 如果某一报表部件直接上载到 SharePoint 文档文件夹（而非从报表创作应用程序发布），将不会更新报表服务器目录。 从报表部件库搜索将找不到上载的报表部件。 若要帮助保持您的 SharePoint 文件夹和报表服务器目录同步，您可以激活 SharePoint 服务器上的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 文件同步功能。 有关详细信息，请参阅 [在 SharePoint 管理中心中激活报表服务器文件同步功能](../../reporting-services/report-server-sharepoint/activate-the-report-server-file-sync-feature-in-sharepoint-ca.md)。  
  
 通过调用某些 Reporting Services 管理 API（例如 GetProperties 和 SetProperties），还可以同步文件。  
  
### <a name="organizing-and-moving-report-parts"></a>组织和移动报表部件  
 您应该事先考虑和计划您的团队将如何使用和组织报表部件、共享数据集和共享数据源。 尽管您可以以后移动它们，但这样可能造成问题。  
  
#### <a name="native-mode-report-server"></a>本机模式下的报表服务器  
 如果您要将处于本机模式下的报表服务器上的报表部件移到同一服务器上的任何其他文件夹中，这不会影响报表创作应用程序搜索或下载报表部件的更新的能力。 这是因为服务器依赖唯一 ComponentID。 但是，如果将报表部件移到用户对其没有权限的文件夹，则用户的搜索将找不到该报表部件，并且在存在对该报表部件的更新时将不会通知他们的报表。  
  
#### <a name="report-server-in-sharepoint-integrated-mode"></a>SharePoint 集成模式下的报表服务器  
 将报表部件移到不同的文档库或文件夹与将它们直接上载到 SharePoint 服务器具有相同的结果：报表服务器目录将不同步。 为避免此情况，请激活 SharePoint 服务器上的报表服务器文件同步功能。  
  
 但子文件夹是一个例外。 搜索操作将始终搜索子文件夹，因此，如果您手动将某一报表部件移到一个子文件夹，则在从报表库中进行搜索时将会找到该报表部件。 从报表部件库中进行搜索时仍会找到它。  
  
### <a name="report-server-catalog-properties"></a>报表服务器目录属性  
 下表描述了现有报表服务器目录字段如何与报表部件以及添加到报表部件目录的新字段相关。 这些字段在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 门户和 SharePoint 库以及报表创作应用程序（例如报表生成器）中公开。  
  
 (*) 表明它是在此版本中新推出的。  
  
|属性|描述|报表部件<br /><br /> 库搜索条件|  
|--------------|-----------------|---------------------------------------------|  
|名称|这是用户可在报表部件库中搜索的条件之一。|是|  
|说明|您可能希望以更方便用户在库中查找的方式组织报表部件名称。 例如，对于涉及销售相关的数据和展示的所有报表部件，您可以搜索以“Sales>>”开头的说明。|是|  
|CreatedBy|将报表部件添加到了报表服务器数据库的用户的 ID。 确切格式取决于身份验证方法。 例如，某些身份验证方法将导致 CreatedBy 和 ModifiedBy 字段中显示完整的“域\用户名”。|是|  
|CreationDate|最初创建报表部件的日期。<br /><br /> 这是用户可在报表部件库中搜索的条件之一。|是|  
|ModifiedBy|ModifiedBy 是上次修改报表部件的用户的 ID。|是|  
|ModifiedDate|在服务器上最后修改报表部件的日期。<br /><br /> 此字段用作确定何时存在对报表部件的服务器端更新的逻辑的一部分。 有关详细信息，请参阅本表后面对 ComponentID 的说明。|是|  
|SubType (*)|SubType 是指示要搜索的报表部件类型的字符串，如“Tablix”或“Chart”。|是|  
|ComponentID (*)|ComponentID 是报表部件的唯一标识符。 这是添加到目录中的新字段，并且在服务器端以及报表创作应用程序（例如报表生成器）中均可见。<br /><br /> 客户端应用程序在检查服务器上是否存在报表部件的更新时使用此字段。 客户端应用程序搜索服务器上在当前客户端报表中的 ComponentID。 当存在匹配的 ComponentID 时，然后将 ModifiedDate 与该报表项的客户端 SyncDate 进行比较。|否|  
  
## <a name="controlling-access-to-report-parts"></a>控制对报表部件的访问  
 以下各表描述默认角色分配以及它们是如何使您能够执行不同操作的。 根据所使用的报表服务器的类型，角色分配名称也会有所不同。  
  
### <a name="server-in-native-mode"></a>本机模式下的服务器  
  
|操作|角色|  
|-------------|-----------|  
|添加、删除、编辑项属性，管理安全性，以及下载报表部件|内容管理员<br /><br /> 我的报表|  
|添加、删除和下载报表部件|发布者|  
|搜索和重用|浏览者<br /><br /> 报表生成器|  
  
### <a name="server-in-sharepoint-integrated-mode"></a>SharePoint 集成模式下的服务器  
  
|操作|角色|  
|-------------|----------|  
|添加、删除、编辑项属性，管理安全性，以及下载报表部件|完全控制|  
|添加、删除、编辑项属性，以及下载报表部件|设计<br /><br /> 参与|  
|搜索和重用|读取<br /><br /> 仅查看|  
  
### <a name="security-considerations"></a>安全注意事项  
  
-   在报表中重用报表部件定义时，会将其完整复制到报表定义中，同时复制的还有标识 ComponentID。 如果报表部件在服务器上被更新，用户可以选择将更新后的报表部件下载到其报表中。 更新也是报表部件的完整副本，并替换已位于其报表中的报表部件的现有版本。  
  
    > [!IMPORTANT]  
    >  在上述各步骤中，应确保报表部件在来自可信位置和用户的报表中重用。  
  
-   报表部件使用与现有“资源”项类型相同的权限策略。 在一个文件夹内，从安全性继承角度来说，传统的资源项与报表部件之间没有差异。 报表部件将继承与同一文件夹中的图像相同的权限策略。 在需要这一区别时，可为所需的报表部件配置项级别安全性。 否则，应将报表部件放置于配置了正确权限的单独文件夹中。  
  
## <a name="see-also"></a>另请参阅  
 [报表生成器中的报表部件和数据集](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md)   
 [报表服务器内容管理（SSRS 本机模式）](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md)   
 [报表设计器中的报表部件 (SSRS)](../../reporting-services/report-design/report-parts-in-report-designer-ssrs.md)  
  
  

---
title: 概述
description: 了解 Master Data Services 的关键数据组织和管理功能。 Master Data Services 使你能够管理数据的主集。
ms.custom: ''
ms.date: 02/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
keywords:
- what is master data
helpviewer_keywords:
- Master Data Services, overview
- Master Data Services
ms.assetid: 8a4c28b1-6061-4850-80b6-132438b8c156
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 1ddbda633eb54060ea63b8bdd5f17ca3142c4ec2
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833818"
---
# <a name="master-data-services-overview-mds"></a>Master Data Services 概述 (MDS)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

本主题介绍 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]的关键数据组织和管理功能。 
  
 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 帮助管理组织的主数据集。 可以将数据整理到模型中，创建更新数据的规则，并控制由谁更新数据。 通过使用 Excel 可以和组织中的其他用户共享主数据集。 
  
 >  有关 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 体系结构的说明，请在 simple-talk.com 上参阅文章 [Master Data Services - 基础知识](https://www.simple-talk.com/sql/database-delivery/master-data-services-basics) 。 有关中的新功能的信息 [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)] ，请参阅 [MASTER DATA SERVICES &#40;MDS 中的新增功能&#41;](../master-data-services/what-s-new-in-master-data-services-mds.md)  
   **有关如何安装 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]、设置数据库和网站以及部署示例模型的说明，请参阅** [Master Data Services 安装和配置](../master-data-services/master-data-services-installation-and-configuration.md)。  
  
 在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，模型是你的主数据结构中的最高级别的容器。 创建一个模型来管理类似数据的组，例如管理联机产品数据。 模型包含一个或多个实体，而实体包含属于数据记录的成员。 实体类似于表。  
  
 例如，你的在线产品模型可能包含多个实体（如 Product、Color 和 Style）。 Color 实体可以含有红色、银色和黑色成员。  
  
 ![Color 实体](../master-data-services/media/mds-productmodel-colorentity-composite.png "Color 实体")  
  
 模型还包含实体中定义的属性。 属性包含可帮助描述实体成员的值。 存在自由格式的属性和基于域的属性。  基于域的属性包含实体成员填充的值，并且可用作其他实体的属性值。  
  
 例如，Product 实体可能具有针对 Cost 和 Weight 的自由格式的属性。 并且，对于颜色 ![编号 1](../master-data-services/media/mds-number1.png "编号 1") ，有一个基于域的属性，其中包含 color 实体成员填充的值。 此颜色主列表将用作 Product 实体 ![2](../master-data-services/media/mds-number2.png "编号 2")的属性值。  
  
 ![用于颜色的基于域的属性](../master-data-services/media/mds-productentity-color-domainattribute.png "用于颜色的基于域的属性")  
  
 派生的层次结构来自模型中各个实体之间的关系。 这些都是基于域的属性关系。 例如，在产品模型中，可以有一个颜色派生层次结构 ![编号 1](../master-data-services/media/mds-number1.png "编号 1") ，该层次结构来自第 ![2](../master-data-services/media/mds-number2.png "编号 2") 级和第 3 product 第 ![3](../master-data-services/media/mds-number3.png "编号 3") 层实体之间的关系。  
  
 ![颜色派生层次结构](../master-data-services/media/mds-derivedhierarchy.png "颜色派生层次结构")  
  
 在定义了数据的基本结构后，可以开始使用导入功能来添加数据记录（成员）。 将数据加载到临时表、使用业务规则验证数据并将数据加载到 MDS 表。  业务规则还可用于设置属性值。  
  
 下表概述了关键 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 任务。 除非另有说明，否则，所有以下过程都要求您是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
> [!NOTE]  
>  您最好在测试环境中完成以下任务并且使用在按照 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]时提供的示例数据。 有关详细信息，请参阅[部署模型 (Master Data Services)](../master-data-services/deploying-models-master-data-services.md)。  
  
|操作|详细信息|相关主题|  
|------------|-------------|--------------------|  
|创建模型|在您创建一个模型时，该模型将被视作 VERSION_1。|[模型 (Master Data Services)](../master-data-services/models-master-data-services.md)<br /><br /> [创建模型 (Master Data Services)](../master-data-services/create-a-model-master-data-services.md)|  
|创建实体|可以创建所需的任何数目的实体以便包含您的成员。|[实体 (Master Data Services)](../master-data-services/entities-master-data-services.md)<br /><br /> [创建实体 (Master Data Services)](../master-data-services/create-an-entity-master-data-services.md)|  
|创建要用作基于域的属性的实体|若要创建基于域的属性，请首先创建实体以便填充属性值列表。|[基于域的属性 (Master Data Services)](../master-data-services/domain-based-attributes-master-data-services.md)<br /><br /> [创建基于域的属性 (Master Data Services)](../master-data-services/create-a-domain-based-attribute-master-data-services.md)|  
|为实体创建属性|创建用于描述成员的属性。 Name 和 Code 属性自动包括在每个实体中，并且不能被删除。 您可能要创建其他自由格式的属性以便包含文本、日期、数字或文件。|[属性 (Master Data Services)](../master-data-services/attributes-master-data-services.md)<br /><br /> [创建文本属性 (Master Data Services)](../master-data-services/create-a-text-attribute-master-data-services.md)<br /><br /> [创建数字属性 (Master Data Services)](../master-data-services/create-a-numeric-attribute-master-data-services.md)<br /><br /> [创建日期属性 (Master Data Services)](../master-data-services/create-a-date-attribute-master-data-services.md)<br /><br /> [创建链接属性 (Master Data Services)](../master-data-services/create-a-link-attribute-master-data-services.md)<br /><br /> [创建文件属性 (Master Data Services)](../master-data-services/create-a-file-attribute-master-data-services.md)|  
|创建属性组|如果对于某一实体您具有超过四个或五个属性，则最好创建属性组。 这些组是在 **“资源管理器”** 中网格之上显示的选项卡，并且它们有助于通过在单独的选项卡上将某些属性组合在一起，简化导航行为。|[属性组 (Master Data Services)](../master-data-services/attribute-groups-master-data-services.md)<br /><br /> [创建属性组 (Master Data Services)](../master-data-services/create-an-attribute-group-master-data-services.md)|  
|为您的支持实体导入成员|通过使用临时过程为你的支持实体导入数据。 对于 Product 模型，这可能意味着导入颜色或大小。 也可以手动创建成员。<br /><br /> <br /><br /> 注意：如果用户至少对实体的叶模型对象具有“更新” [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]**权限并且有权访问“资源管理器”****功能区域，就可以在** 中创建成员。|[概述：导入表中数据 (Master Data Services)](../master-data-services/overview-importing-data-from-tables-master-data-services.md)<br /><br /> [创建叶成员 &#40;Master Data Services&#41;](../master-data-services/create-a-leaf-member-master-data-services.md)|  
|创建和应用业务规则以确保数据质量|创建和发布业务规则以便确保您的数据的精确性。 可以使用业务规则执行以下操作：<br /><br /> 设置默认属性值。<br /><br /> 更改属性值。<br /><br /> 在数据未通过业务规则验证时发送电子邮件通知。|[业务规则 (Master Data Services)](../master-data-services/business-rules-master-data-services.md)<br /><br /> [创建和发布业务规则 (Master Data Services)](../master-data-services/create-and-publish-a-business-rule-master-data-services.md)<br /><br /> [针对业务规则验证特定成员 (Master Data Services)](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)<br /><br /> [配置电子邮件通知 (Master Data Services)](../master-data-services/configure-email-notifications-master-data-services.md)<br /><br /> [配置业务规则以发送通知 (Master Data Services)](../master-data-services/configure-business-rules-to-send-notifications-master-data-services.md)|  
|为您的主实体导入成员并应用业务规则|通过使用临时过程为您的主实体导入成员。 完成后，对版本进行验证，将业务规则应用于该模型版本中的所有成员。<br /><br /> 然后，您可以纠正任何业务规则验证问题。|[验证 (Master Data Services)](../master-data-services/validation-master-data-services.md)<br /><br /> [针对业务规则验证版本 (Master Data Services)](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)<br /><br /> [验证存储过程 (Master Data Services)](../master-data-services/validation-stored-procedure-master-data-services.md)|  
|创建派生层次结构|派生的层次结构可以随着你的业务需求的变化进行更新，并且确保所有成员都处于适当的级别。|[派生层次结构 (Master Data Services)](../master-data-services/derived-hierarchies-master-data-services.md)<br /><br /> [创建派生层次结构 (Master Data Services)](../master-data-services/create-a-derived-hierarchy-master-data-services.md)|  
|如果需要，创建显式层次结构|如果您想要创建不是基于级别并且包括来自单个实体的成员的层次结构，则可以创建显式层次结构。|[显式层次结构 (Master Data Services)](../master-data-services/explicit-hierarchies-master-data-services.md)<br /><br /> [创建显式层次结构 (Master Data Services)](../master-data-services/create-an-explicit-hierarchy-master-data-services.md)|  
|如果想要，创建集合|如果您想要查看不同的成员分组以便进行报告或分析并且不需要完整层次结构，则可以创建集合。<br /><br /> <br /><br /> 注意：如果用户至少对集合模型对象具有“更新” [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]**权限并且有权访问“资源管理器”****功能区域，就可以在** 中创建成员。|[集合 (Master Data Services)](../master-data-services/collections-master-data-services.md)<br /><br /> [创建集合 (Master Data Services)](../master-data-services/create-a-collection-master-data-services.md)|  
|创建用户定义元数据|为了描述您的模型对象，请将用户定义元数据添加到您的模型中。 这些元数据可以包括对象的所有者或者数据源。||  
|锁定模型版本和分配版本标志|锁定模型版本可以防止对成员进行的更改，但管理员除外。 在根据业务规则已成功对版本的数据进行了验证后，您可以提交该版本，这阻止所有用户对成员的更改。<br /><br /> 创建一个版本标志并将其分配给模型。 标志帮助用户和订阅系统确定要使用模型的哪个版本。|[版本 (Master Data Services)](../master-data-services/versions-master-data-services.md)<br /><br /> [锁定版本 (Master Data Services)](../master-data-services/lock-a-version-master-data-services.md)<br /><br /> [创建版本标志 (Master Data Services)](../master-data-services/create-a-version-flag-master-data-services.md)|  
|创建订阅视图|为使您的订阅系统可以使用您的主数据，创建订阅视图，这在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库中创建标准视图。|[概述：导出数据 (Master Data Services)](../master-data-services/overview-exporting-data-master-data-services.md)<br /><br /> [创建订阅视图以导出数据 (Master Data Services)](../master-data-services/create-a-subscription-view-to-export-data-master-data-services.md)|  
|配置用户和组权限|您不能将测试环境中的用户和组权限复制到生产环境。 但是，可以使用您的测试环境确定最终要在生产中使用的安全性。|[安全性 (Master Data Services)](../master-data-services/security-master-data-services.md)<br /><br /> [添加组 (Master Data Services)](../master-data-services/add-a-group-master-data-services.md)<br /><br /> [添加用户 (Master Data Services)](../master-data-services/add-a-user-master-data-services.md)|  
  
 在就绪后，您可以将模型（具有或不具有有关数据）部署到您的生产环境中。 有关详细信息，请参阅[部署模型 (Master Data Services)](../master-data-services/deploying-models-master-data-services.md)。  
  
  


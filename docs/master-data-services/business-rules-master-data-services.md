---
title: 业务规则
description: 了解 Master Data Services 中的业务规则，这些规则可更新数据、发送电子邮件或启动业务流程或工作流。
ms.custom: ''
ms.date: 03/18/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], about business rules
- business rules [Master Data Services]
ms.assetid: a9f9e41a-2461-4845-b947-58b3a205543f
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4869875867f7b1cad99780b1ec82664c9d569201
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341482"
---
# <a name="business-rules-master-data-services"></a>业务规则 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，业务规则是您用于确保主数据的质量和准确性的规则。 可以使用业务规则自动更新数据、发送邮件或启用业务流程或工作流。  
  
 若要查看业务规则示例，请参阅[业务规则示例 &#40;Master Data Services&#41;](../master-data-services/business-rule-examples-master-data-services.md)。  
  
## <a name="create-and-publish-business-rules"></a>创建和发布业务规则  
 业务规则是您在 **ssMDSmdm** 中创建的 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]语句。 如果属性值符合指定条件，会执行操作，否则执行 Else 操作。 可能的操作包括设置默认值或更改值。 这些操作可与发送电子邮件通知这一操作结合使用。  
  
 业务规则可以基于特定属性值（例如，如果 Color=Blue，则执行操作），或在属性值发生更改时（例如，如果 Color 属性的值发生更改，则执行操作）应用。 有关跟踪非特定更改的详细信息，请参阅[更改跟踪 &#40;Master Data Services&#41;](../master-data-services/change-tracking-master-data-services.md)。  
  
 若要使用业务规则，您必须首先创建和发布规则，然后将已发布的规则应用于数据。 您可以通过验证某一版本，将规则应用于该版本的数据的子级或全部数据。 在所有属性都通过业务规则验证前，不能提交版本。  
  
 如果用户尝试添加未通过业务规则验证的某一属性值，仍可以保存该值。 您可以查看并纠正在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中显示的验证问题。  
  
 在您创建模型部署包时，如果想要包括业务规则，则必须包括来自该包中版本的数据。  
  
 如果您创建使用 **OR** 运算符的业务规则，应为可以独立进行计算的每个条件语句创建单独的规则。 然后，您可以根据需要排除规则，提供更高的灵活性以及更便于排除故障。  
  
## <a name="how-business-rules-are-applied"></a>如何应用业务规则  
 你可以通过上移和下移业务规则来设置要运行的规则的优先级顺序。 但是，在考虑优先级前，基于规则执行的操作类型应用业务规则。 该顺序如下所示：  
  
1.  **默认值**  
  
2.  **更改值**  
  
3.  **验证**  
  
4.  **外部操作**  
  
5.  **用户定义的操作脚本**  
  
 在这些组内，操作按从最低到最高的优先级顺序应用。 因此，例如，四个单独的规则可能有 **“默认值”** 操作。 根据 Web 用户界面中指定的优先级顺序，首先执行 **“默认值”** 操作。  
  
 其他有关应用规则的重要说明：  
  
-   如果某一业务规则被排除或者未以 **“活动”** 状态发布，该规则仍可用，但在应用业务规则时不包括它。  
  
-   业务规则适用于所有叶成员或所有合并成员的属性值，但不能同时适用于这两者。  
  
-   业务规则可以应用于模型的所有 **“打开”** 或 **“已锁定”** 版本。  
  
-   应用业务规则时对数据所做的更改不记录为事务。  
  
-   一个业务规则不能包含多个 **“启动工作流”** 操作。  
  
## <a name="system-settings"></a>系统设置  
 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] 中有两个会影响业务规则的设置。 可以在 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] 中或直接在“系统设置”表中调整这些设置。 有关详细信息，请参阅[系统设置 (Master Data Services)](../master-data-services/system-settings-master-data-services.md)。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务说明|主题|  
|----------------------|-----------|  
|创建和发布新的业务规则。|[创建和发布业务规则 (Master Data Services)](../master-data-services/create-and-publish-a-business-rule-master-data-services.md)|  
|向业务规则添加多个条件。|[向业务规则添加多个条件 &#40;Master Data Services&#41;](../master-data-services/add-multiple-conditions-to-a-business-rule-master-data-services.md)|  
|创建业务规则来要求属性具有值。|[要求属性值 &#40;Master Data Services&#41;](../master-data-services/require-attribute-values-master-data-services.md)|  
|创建业务规则以便基于对属性值的更改执行操作。|[基于属性值更改启动操作 (Master Data Services)](../master-data-services/initiate-actions-based-on-attribute-value-changes-master-data-services.md)|  
|创建业务规则，将用户定义脚本用作条件|[业务规则扩展 &#40;Master Data Services&#41;](../master-data-services/business-rules-extension-master-data-services.md)|  
|创建业务规则，将用户定义脚本用作操作|[业务规则扩展 &#40;Master Data Services&#41;](../master-data-services/business-rules-extension-master-data-services.md)|  
|更改现有业务规则的名称。|[更改业务规则名称 &#40;Master Data Services&#41;](../master-data-services/change-a-business-rule-name-master-data-services.md)|  
|将 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 配置为在应用业务规则时发送通知。|[配置业务规则以发送通知 (Master Data Services)](../master-data-services/configure-business-rules-to-send-notifications-master-data-services.md)|  
|将业务规则应用于特定成员。|[针对业务规则验证特定成员 (Master Data Services)](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)|  
|排除业务规则以便不使用该规则。|[排除业务规则 (Master Data Services)](../master-data-services/exclude-a-business-rule-master-data-services.md)|  
|删除现有业务规则。|[删除业务规则 (Master Data Services)](../master-data-services/delete-a-business-rule-master-data-services.md)|  
  
## <a name="related-content"></a>相关内容  
  
-   [Master Data Services 概述 (MDS)](../master-data-services/master-data-services-overview-mds.md)  
  
-   [版本 (Master Data Services)](../master-data-services/versions-master-data-services.md)  
  
-   [验证 (Master Data Services)](../master-data-services/validation-master-data-services.md)  
  
-   [更改跟踪 &#40;Master Data Services&#41;](../master-data-services/change-tracking-master-data-services.md)  
  
  

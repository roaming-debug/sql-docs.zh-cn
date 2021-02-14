---
description: 分类的 Web 服务操作 (Master Data Services)
title: 分类的 Web 服务操作
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: reference
ms.assetid: e3f346b5-7e26-481d-9821-1846e2e91289
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 65656aa902b5500d1dcc57dde914b3a0b2265008
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350136"
---
# <a name="categorized-web-service-operations-master-data-services"></a>分类的 Web 服务操作 (Master Data Services)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

   Web 服务包含一组完整的操作，可让您通过编写代码以控制  通过其用户界面所执行的所有功能。 Web 服务操作由 <xref:Microsoft.MasterDataServices.IService> 接口定义，并在 <xref:Microsoft.MasterDataServices.ServiceClient> 类中作为方法实现。 本主题将 Web 服务操作分组为概念性类别，以帮助您了解如何使用 Web 服务 API。  
  
## <a name="model-operations"></a>模型操作  
 这些操作用于创建、更新和删除模型，以及对所有模型内容（如实体、层次结构和版本）执行操作。 有关详细信息，请参阅[模型 (Master Data Services)](../../master-data-services/models-master-data-services.md)。  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataUpdate%2A>
  
## <a name="entity-operations"></a>实体操作  
 这些操作用于创建、更新和删除单个实体的成员。 有关详细信息，请参阅[实体 &#40;Master Data Services&#41;](../../master-data-services/entities-master-data-services.md) 和[成员 &#40;Master Data Services&#41;](../../master-data-services/members-master-data-services.md)。  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberKeyLookup%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersCopy%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersMerge%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersUpdate%2A>
  
## <a name="member-operations"></a>成员操作  
 这些操作用于获取、更新和删除成员。 这组操作的成员可能包含多个实体中的成员。 有关详细信息，请参阅[成员 (Master Data Services)](../../master-data-services/members-master-data-services.md)。  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersBulkDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersBulkMerge%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersBulkUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersGet%2A>
  
## <a name="attribute-and-hierarchy-operations"></a>属性和层次结构操作  
 这些操作用于获取属性和层次结构信息。 还可通过使用模型操作来修改属性和层次结构，如 <xref:Microsoft.MasterDataServices.ServiceClient.MetadataUpdate%2A>。 有关详细信息，请参阅[属性 &#40;Master Data Services&#41;](../../master-data-services/attributes-master-data-services.md) 和[层次结构 &#40;Master Data Services&#41;](../../master-data-services/hierarchies-master-data-services.md)。  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberAttributesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.HierarchyMembersGet%2A>
  
## <a name="business-rule-operations"></a>业务规则操作  
 这些操作用于创建、更新、删除和发布业务规则。 有关详细信息，请参阅[业务规则 (Master Data Services)](../../master-data-services/business-rules-master-data-services.md)。  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesPaletteGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesPublish%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesUpdate%2A>
  
## <a name="annotation-operations"></a>批注操作  
 这些操作用于创建、更新和删除批注。 有关详细信息，请参阅[批注 &#40;Master Data Services&#41;](../../master-data-services/annotations-master-data-services.md)。  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.AnnotationsDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.AnnotationsUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberAnnotationsCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberAnnotationsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionAnnotationsCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionAnnotationsGet%2A>
  
## <a name="transaction-operations"></a>事务操作  
 这些操作用于获取和撤消事务。 有关详细信息，请参阅[事务 (Master Data Services)](../../master-data-services/transactions-master-data-services.md)。  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionsReverse%2A>
  
## <a name="version-and-validation-operations"></a>版本和验证操作  
 这些操作用来复制和验证版本。 有关详细信息，请参阅[版本 &#40;Master Data Services&#41;](../../master-data-services/versions-master-data-services.md) 和[验证 &#40;Master Data Services&#41;](../../master-data-services/validation-master-data-services.md)。  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.VersionCopy%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ValidationGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ValidationProcess%2A>
  
## <a name="data-quality-operations"></a>数据质量操作  
 这些操作用于执行数据质量任务并检查其结果。  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityCleansingOperationCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityMatchingOperationCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityInstalledState%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityKnowledgeBasesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityOperationStart%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityOperationResultsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityOperationStatus%2A>
  
## <a name="data-import-operations"></a>数据导入操作  
 这些操作用于将数据导入到 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库。 有关详细信息，请参阅 [概述：从表导入数据 &#40;Master Data Services&#41;](../../master-data-services/overview-importing-data-from-tables-master-data-services.md)。  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingClear%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingLoad%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingProcess%2A>
  
 以下操作用于通过使用 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 中所包含的临时过程导入数据。 这些操作应仅用于支持现有数据库。 对于新的开发工作，请使用前面列出的操作。  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingClear%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingNameCheck%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingProcess%2A>
  
## <a name="data-export-operations"></a>数据导出操作  
 这些操作可用于通过使用订阅视图导出数据。 有关详细信息，请参阅 [概述：导出数据 (Master Data Services)](../../master-data-services/overview-exporting-data-master-data-services.md)。  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewListGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewUpdate%2A>
  
## <a name="security-operations"></a>安全运营  
 这些操作用于修改控制对 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库进行访问的安全设置。 有关详细信息，请参阅[安全性 (Master Data Services)](../../master-data-services/security-master-data-services.md)  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesUpdate%2A>
  
## <a name="system-operations"></a>系统操作  
 这些操作用于获取和更新系统设置和用户首选项。  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.ServiceCheck%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ServiceVersionGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemDomainListGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemPropertiesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemSettingsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemSettingsUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.UserPreferencesDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.UserPreferencesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.UserPreferencesUpdate%2A>
  
  

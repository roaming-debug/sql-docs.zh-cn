---
description: 生成模型（用于 Excel 的 MDS 外接程序）
title: 生成模型
ms.custom: microsoft-excel-add-in
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 8ae26ec3-c5d5-4c4f-a810-2951a7454439
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ffdf789438f2699bfa9d18c74fa4f748bfe3996d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351512"
---
# <a name="building-a-model-mds-add-in-for-excel"></a>生成模型（用于 Excel 的 MDS 外接程序）

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]中，管理员可以执行在 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序中提供的一部分管理功能。  
  
 管理员可以在 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)] 中执行的模型生成任务是：  
  
-   创建实体。 有关实体的详细信息，请参阅 [实体 (Master Data Services)](../../master-data-services/entities-master-data-services.md)。  
  
-   创建所有类型的属性，包括基于域的属性。 有关属性的详细信息，请参阅 [属性 (Master Data Services)](../../master-data-services/attributes-master-data-services.md) 和 [基于域的属性 (Master Data Services)](../../master-data-services/domain-based-attributes-master-data-services.md)。  
  
 作为管理员，您必须通过使用 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序或 Web 服务来创建模型。 然后，您可以使用 [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)] 在模型中创建实体和属性。 有关模型对象的详细信息，请参阅 [模型 (Master Data Services)](../../master-data-services/models-master-data-services.md)。  
  
## <a name="related-tasks"></a>Related Tasks  
 大多数管理任务仍必须在 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序中或通过使用 Web 服务完成。 下表显示管理员可用于在 MDS 中完成任务的工具。  
  
|任务说明|工具|主题|  
|----------------------|----------|-----------|  
|创建模型。|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序或 Web 服务|[创建模型 (Master Data Services)](../../master-data-services/create-a-model-master-data-services.md)|  
|创建一个实体。|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序、Web 服务或 [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]|[创建实体（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/create-an-entity-mds-add-in-for-excel.md)|  
|创建基于域的属性。|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序、Web 服务或 [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]|[创建基于域的属性（用于 Excel 的 MDS 外接程序）](../../master-data-services/microsoft-excel-add-in/create-a-domain-based-attribute-mds-add-in-for-excel.md)|  
|创建属性组。|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序或 Web 服务|[创建属性组 (Master Data Services)](../../master-data-services/create-an-attribute-group-master-data-services.md)|  
|创建业务规则。|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序或 Web 服务|[创建和发布业务规则 (Master Data Services)](../../master-data-services/create-and-publish-a-business-rule-master-data-services.md)|  
|创建订阅视图。|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序或 Web 服务|[创建订阅视图以导出数据 (Master Data Services)](../../master-data-services/create-a-subscription-view-to-export-data-master-data-services.md)|  
|创建层次结构。|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序或 Web 服务|[创建派生层次结构 (Master Data Services)](../../master-data-services/create-a-derived-hierarchy-master-data-services.md)<br /><br /> [创建显式层次结构 (Master Data Services)](../../master-data-services/create-an-explicit-hierarchy-master-data-services.md)|  
|创建集合。|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序或 Web 服务|[创建集合 (Master Data Services)](../../master-data-services/create-a-collection-master-data-services.md)|  
|创建数据版本。|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序或 Web 服务|[锁定版本 (Master Data Services)](../../master-data-services/lock-a-version-master-data-services.md)|  
|部署模型。|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序、Web 服务或 MDSModelDeploy 工具。|[使用 MDSModelDeploy 创建模型部署包](../../master-data-services/create-a-model-deployment-package-by-using-mdsmodeldeploy.md)|  
  
## <a name="related-content"></a>相关内容  
  
-   [模型 (Master Data Services)](../../master-data-services/models-master-data-services.md)  
  
-   [实体 (Master Data Services)](../../master-data-services/entities-master-data-services.md)  
  
-   [属性 (Master Data Services)](../../master-data-services/attributes-master-data-services.md)  
  
-   [基于域的属性 (Master Data Services)](../../master-data-services/domain-based-attributes-master-data-services.md)  
  
-   [属性组 (Master Data Services)](../../master-data-services/attribute-groups-master-data-services.md)  
  
-   [业务规则 (Master Data Services)](../../master-data-services/business-rules-master-data-services.md)  
  
-   [概述：导出数据 (Master Data Services)](../../master-data-services/overview-exporting-data-master-data-services.md)  
  
-   [层次结构 (Master Data Services)](../../master-data-services/hierarchies-master-data-services.md)  
  
-   [集合 (Master Data Services)](../../master-data-services/collections-master-data-services.md)  
  
-   [版本 (Master Data Services)](../../master-data-services/versions-master-data-services.md)  
  
-   [安全性 (Master Data Services)](../../master-data-services/security-master-data-services.md)  
  
-   [部署模型 (Master Data Services)](../../master-data-services/deploying-models-master-data-services.md)  
  
  

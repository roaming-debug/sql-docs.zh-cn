---
description: 概述：导出数据 (Master Data Services)
title: Exporting Data
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- exporting data [Master Data Services]
- subscription views [Master Data Services]
- subscription views [Master Data Services], about subscription views
ms.assetid: 8b74409a-ea70-45f8-84c7-da6905e4901a
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: b2092963964354417775698242e12498e7387a1c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340252"
---
# <a name="overview-exporting-data-master-data-services"></a>概述：导出数据 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  本文介绍了订阅视图格式的类型，以及如何确定何时需要根据对模型对象的更改而编辑视图。  
  
 创建订阅视图可将 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据导出到订阅系统，例如 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。 使用订阅系统可查看 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库中的数据。  有关如何创建订阅视图的信息，请参阅 [创建订阅视图以导出数据 (Master Data Services)](../master-data-services/create-a-subscription-view-to-export-data-master-data-services.md)  
  
 有关视图的详细信息，请参阅[视图](../relational-databases/views/views.md)。  
  
## <a name="subscription-view-formats"></a>订阅视图格式  
 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 中创建某一视图时，从  提供的一组标准视图格式中进行选择。 可以使用这些格式创建显示以下内容的视图：  
  
-   所有叶成员及其属性。  
  
-   所有合并成员及其属性。  
  
-   所有集合及其属性。  
  
-   显式添加到集合的成员。  
  
-   派生层次结构中的成员，采用父子或级别格式。  
  
-   某一实体的所有显式层次结构中的成员，采用父子或级别格式。  
  
## <a name="subscription-views-can-become-out-of-date"></a>订阅视图可能会过期  
 在您创建针对某一实体或层次结构的订阅视图后，对关联的模型对象的更改不能自动反映在视图中。 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 您可能需要在中重新生成一个订阅视图，以便反映对模型对象的更改。 在对象模型更改时， **“导出”** 页上的 **“已更改”** 列将更新为 **True** 。 **True** 指示您应该编辑订阅视图并且保存它，这将重新生成该视图。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务说明|主题|  
|----------------------|-----------|  
|创建主数据的订阅视图。|[创建订阅视图以导出数据 (Master Data Services)](../master-data-services/create-a-subscription-view-to-export-data-master-data-services.md)|  
|删除现有订阅视图。|[删除订阅视图 &#40;Master Data Services&#41; ](../master-data-services/delete-a-subscription-view-master-data-services.md)|  
  
## <a name="related-content"></a>相关内容  
  
-   [订阅视图格式 (Master Data Services)](../master-data-services/subscription-view-formats-master-data-services.md)  
  
-   [视图](../relational-databases/views/views.md)  
  
  

---
description: 关系临时表 (Master Data Services)
title: 关系临时表
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- relationships staging table [Master Data Services]
- database [Master Data Services], relationships table
ms.assetid: e19b6002-67bd-4e7d-9f19-ecb455522b1a
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 199b351fb0aea4036b906aee326c64fac128d425
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353698"
---
# <a name="relationship-staging-table-master-data-services"></a>关系临时表 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  使用 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库中的关系临时表 (stg.name_Relationship) 基于成员之间的相互关系更改显式层次结构中的成员位置。  
  
##  <a name="table-columns"></a><a name="TableColumns"></a> 表列  
 下表说明 Relationship 临时表中每个字段的用途。  
  
|列名|描述|“值”|  
|-----------------|-----------------|-----------|  
|**ID**|自动分配的标识符。|不要在此字段中输入值。 如果尚未处理此批次，则此字段为空。|  
|**RelationshipType**|必须<br /><br /> 正在设置的关系的类型。|可能的值包括：<br /><br /> **1**：父级<br /><br /> **2**：同级（同一级）|  
|**ImportStatus_ID**|必须<br /><br /> 导入过程的状态。|可能的值包括：<br /><br /> **0**- 指定此值表示记录已经准备好临时存储。<br /><br /> **1**- 自动分配的值，表示记录的暂存过程已成功。<br /><br /> **2**- 自动分配的值，表示记录的临时过程已失败。|  
|**Batch_ID**|仅对 Web 服务为必需的<br /><br /> 自动分配的标识符，该标识符将记录分组以便临时存储。<br /><br /> 如果尚未处理此批次，则此字段为空。|将为此批次中的所有成员分配此标识符，此标识符显示在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 用户界面中的 **ID** 列。|  
|**BatchTag**|必需，但是 Web 服务除外<br /><br /> 批次的唯一名称，最多包含 50 个字符。||  
|**HierarchyName**|必须<br /><br /> 显式层次结构名称。 每个合并成员只能属于一个层次结构。||  
|**ParentCode**|必须<br /><br /> 对于父-子关系，是将为子叶成员或合并成员的父级的合并成员代码。<br /><br /> 对于同级关系，为以下同级之一的代码。||  
|**ChildCode**|必须<br /><br /> 对于父-子关系，是将为子级的合并成员或叶成员的代码。<br /><br /> 对于同级关系，为以下同级之一的代码。||  
|**排序顺序**|可选<br /><br /> 一个整数，表示该成员相对于父级下其他成员的成员顺序。 每个子成员应具有唯一标识符。||  
|**ErrorCode**|显示错误代码。 有关 **ImportStatus_ID** 为 **2** 的所有记录，请参阅 [临时过程错误 (Master Data Services)](../master-data-services/staging-process-errors-master-data-services.md)。||  
  
## <a name="see-also"></a>另请参阅  
 [概述：从表中导入数据 &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md)   
 [查看在暂存 &#40;Master Data Services 中发生的错误&#41;](../master-data-services/view-errors-that-occur-during-staging-master-data-services.md)   
 [临时过程错误 (Master Data Services)](../master-data-services/staging-process-errors-master-data-services.md)  
  
  

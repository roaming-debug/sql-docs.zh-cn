---
description: 订阅视图格式 (Master Data Services)
title: 订阅视图格式
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: ff1e2566-ac8f-467d-a6d9-12c3f13879b9
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: b94496efb521fce2099a59d7e0e99b90438de361
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336250"
---
# <a name="subscription-view-formats-master-data-services"></a>订阅视图格式 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  基于您选择的实体或派生层次结构，为订阅视图提供以下格式。  
  
## <a name="subscription-view-formats"></a>订阅视图格式  
  
|“属性”|描述|  
|----------|-----------------|  
|**叶成员**|包含叶成员及其关联的属性值。|  
|**叶成员历史记录**|包含叶成员历史数据及关联的属性值。 视图格式是缓慢更改维度类型 4 样式。|  
|**叶成员 SCD 类型 2**|包含叶成员历史数据和当前数据及关联的属性值。 视图格式是缓慢更改维度类型 2 样式。|  
|**合并成员**|包含合并成员及其关联的属性值。|  
|**合并成员历史记录**|包含合并成员历史数据及其关联的属性值。 视图格式是缓慢更改维度类型 4 样式。|  
|**合并成员 SCD 类型 2**|包含合并成员历史数据和当前数据及关联的属性值。 视图格式是缓慢更改维度类型 2 样式。|  
|**集合成员身份**|包含集合列表及其关联的属性值。|  
|**集合**|包含集合列表、每个集合中的成员以及权重值和排序顺序。|  
|**集合成员历史记录**|包含集合成员历史数据及关联的属性值。 视图格式是缓慢更改维度类型 4 样式。|  
|**集合成员 SCD 类型 2**|包含集合成员历史数据和当前数据及关联的属性值。 视图格式是缓慢更改维度类型 2 样式。|  
|**显式父子**|包含实体的显式层次结构，以父子格式显示。|  
|**显式级别**|包含实体的显式层次结构，以级别格式显示。|  
|**派生的父子（派生层次结构视图）**|包含派生层次结构，以父子格式显示。|  
|**派生级别（派生层次结构视图）**|包含派生层次结构，以级别格式显示。|  
  
## <a name="see-also"></a>另请参阅  
 [概述：将数据导出 &#40;Master Data Services&#41;](../master-data-services/overview-exporting-data-master-data-services.md)   
 [创建订阅视图以导出数据 (Master Data Services)](../master-data-services/create-a-subscription-view-to-export-data-master-data-services.md)  
  
  

---
description: 轴集合 (ADO MD)
title: 轴集合 (ADO MD) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Axes
- Cellset::Axes
helpviewer_keywords:
- Axes collection [ADO MD]
ms.assetid: 072fb21a-ec0f-4b02-9022-1cef3ad4bfff
author: rothja
ms.author: jroth
ms.openlocfilehash: 757d833da689c3d5097540571074292ec1bd2d69
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166183"
---
# <a name="axes-collection-ado-md"></a>轴集合 (ADO MD)
包含定义单元集的 [轴](./axis-object-ado-md.md) 对象。  
  
## <a name="remarks"></a>备注  
 [单元格集](./cellset-object-ado-md.md)对象包含一个 **轴** 集合。 打开 **单元集** 后，此集合将至少包含一个 **轴**。 有关如何使用 **轴** 对象的更详细说明，请参阅 [axis](./axis-object-ado-md.md)对象。  
  
> [!NOTE]
>  **单元集** 的筛选轴不包含在 **轴** 集合中。 有关详细信息，请参阅 [FilterAxis](./filteraxis-property-ado-md.md) 属性。  
  
 **轴** 是标准 ADO 集合。 通过集合的属性和方法，你可以执行以下操作：  
  
-   获取集合中具有 [Count](../ado-api/count-property-ado.md) 属性的对象的数目。  
  
-   返回集合中具有默认 [项](../ado-api/item-property-ado.md) 属性的对象。  
  
-   用 [Refresh](../ado-api/refresh-method-ado.md) 方法更新集合中的对象。  
  
 本部分包含以下主题。  
  
-   [属性、方法和事件](./axes-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VB) 单元集示例 ](./cellset-example-vb.md)   
 [轴对象 (ADO MD)](./axis-object-ado-md.md)
---
description: 轴对象 (ADO MD)
title: Axis 对象 (ADO MD) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Axis
helpviewer_keywords:
- Axis object [ADO MD]
ms.assetid: 5f498c9a-b1e7-4e6e-9ae6-71eadaf9aada
author: rothja
ms.author: jroth
ms.openlocfilehash: 64cb464f4843476dda737609596da148e304c2fc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169987"
---
# <a name="axis-object-ado-md"></a>轴对象 (ADO MD)
表示单元集的位置或筛选轴，其中包含一个或多个维度的选定成员。  
  
## <a name="remarks"></a>备注  
 Axis 对象可以包含 **Axis** [集合，](./axes-collection-ado-md.md)或由 [单元集](./cellset-object-ado-md.md)的 [FilterAxis](./filteraxis-property-ado-md.md)属性返回。  
  
 使用 **Axis** 对象的集合和属性，可以执行以下操作：  
  
-   标识具有 [Name](./name-property-ado-md.md)属性的 **轴**。  
  
-   使用 [position](./positions-collection-ado-md.md)集合循环访问 **轴** 上的每个位置。  
  
-   通过 [DimensionCount](./dimensioncount-property-ado-md.md)属性获取 **轴** 上的维度数。  
  
-   获取具有标准 ADO [属性](../ado-api/properties-collection-ado.md)集合的 **轴** 特定于提供程序的属性。  
  
 本部分包含以下主题。  
  
-   [属性、方法和事件](./axis-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VBScript) 的轴示例 ](./axis-example-vbscript.md)   
 [轴集合 (ADO MD) ](./axes-collection-ado-md.md)   
 [位置集合 (ADO MD) ](./positions-collection-ado-md.md)   
 [属性集合 (ADO)](../ado-api/properties-collection-ado.md)
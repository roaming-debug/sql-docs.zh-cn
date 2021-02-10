---
description: 单元集对象 (ADO MD)
title: " (ADO MD) 的单元集对象 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Cellset
helpviewer_keywords:
- Cellset object [ADO MD]
ms.assetid: 5e2452c0-cac0-49b2-8099-836c35794d50
author: rothja
ms.author: jroth
ms.openlocfilehash: dc64756d2b2ac679e185c4dff8888352a7865f83
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100055722"
---
# <a name="cellset-object-ado-md"></a>单元集对象 (ADO MD)
表示多维查询的结果。 它是从多维数据集或其他单元集选择的单元的集合。  
  
## <a name="remarks"></a>备注  
 使用直接的、类似数组的访问检索 **单元集** 内的数据。 您可以向下钻取到特定的成员，以获取有关该成员的数据。 例如，下面的代码返回名为的单元集的第一个轴上第一个位置的第一个成员的标题 `cst` ：  
  
```  
cst.Axes(0).Positions(0).Members(0).Caption  
```  
  
 单元集内没有当前单元格的概念。 相反， [Item](./item-property-ado-md-cellset.md) 属性从单元集中检索特定的 [单元](./cell-object-ado-md.md) 对象。 **Item** 属性的参数确定要检索的单元格。 可以指定单元格的唯一序号值。 还可以通过使用单元格集的每个轴上的位置号来检索单元格。 有关检索单元格的详细信息，请参阅 [Item](./item-property-ado-md-cellset.md) 属性。  
  
 使用 **单元集** 对象的集合、方法和属性，可以执行以下操作：  
  
-   通过设置其 [ActiveConnection](./activeconnection-property-ado-md.md)属性，将打开的连接与 **单元集** 对象关联起来。  
  
-   使用 [Open](./open-method-ado-md.md) 方法执行和检索多维查询的结果。  
  
-   从具有 [Item](./item-property-ado-md-cellset.md)属性的 **单元** 格集中检索 **单元**。  
  
-   返回用 [轴](./axes-collection-ado-md.md)集合定义 **单元集** 的 [轴](./axis-object-ado-md.md)对象。  
  
-   使用 [FilterAxis](./filteraxis-property-ado-md.md)属性检索有关用于筛选 **单元** 集中的数据的维度的信息。  
  
-   返回或指定用于定义具有 [源](./source-property-ado-md.md)属性的 **单元集** 的查询。  
  
-   返回 **单元集** 的当前状态， (打开、关闭、执行或用 [state](./state-property-ado-md.md) 属性连接) 。  
  
-   使用 [close](./close-method-ado-md.md)方法关闭打开的 **单元集**。  
  
-   检索有关具有标准 ADO [属性](../ado-api/properties-collection-ado.md)集合的 **单元集** 的特定于提供程序的信息。  
  
 本部分包含以下主题。  
  
-   [属性、方法和事件](./cellset-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VB) 单元集示例 ](./cellset-example-vb.md)   
 [轴集合 (ADO MD) ](./axes-collection-ado-md.md)   
 [Cell Object (ADO MD) ](./cell-object-ado-md.md)   
 [ADO) 的连接对象 (](../ado-api/connection-object-ado.md)   
 [属性集合 (ADO)](../ado-api/properties-collection-ado.md)
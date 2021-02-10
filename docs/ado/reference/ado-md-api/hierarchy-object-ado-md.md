---
description: 层次结构对象 (ADO MD)
title: 层次结构对象 (ADO MD) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Hierarchy
helpviewer_keywords:
- Hierarchy object [ADO MD]
ms.assetid: 034af340-ac79-494e-ba5e-2b57da1cb9de
author: rothja
ms.author: jroth
ms.openlocfilehash: 4f84fe2157e9da38ad55a56334436a5d54128823
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051148"
---
# <a name="hierarchy-object-ado-md"></a>层次结构对象 (ADO MD)
表示一个 [维度](./dimension-object-ado-md.md) 成员可以聚合或 "汇总" 的方式。 可以在一个或多个层次结构上聚合维度。  
  
## <a name="remarks"></a>备注  
 使用 **层次结构** 对象的集合和属性，可以执行以下操作：  
  
-   标识具有 [Name](./name-property-ado-md.md)和 [UniqueName](./uniquename-property-ado-md.md)属性的 **层次结构**。  
  
-   返回一个有意义的字符串，该字符串描述具有 [Description](./description-property-ado-md.md)属性的 **层次结构**。  
  
-   返回与 [Level](./level-object-ado-md.md) [集合构成](./levels-collection-ado-md.md)**层次结构** 的级别对象。  
  
-   使用标准 ADO [Properties](../ado-api/properties-collection-ado.md) 集合获取有关 **层次结构** 对象的其他信息。  
  
 **Properties** 集合包含提供程序提供的属性。 下表列出了可用的属性。 实际属性列表可能有所不同，具体取决于提供程序的实现。 有关可用属性的更完整列表，请参阅提供程序的文档。  
  
|名称|说明|  
|----------|-----------------|  
|AllMember|层次结构中最高级别的汇总的成员。|  
|CatalogName|此多维数据集所属的目录的名称。|  
|CubeName|多维数据集的名称。|  
|DefaultMember|此层次结构的默认成员的唯一名称。|  
|说明|层次结构的有意义的说明。|  
|DimensionType|此层次结构所属的维度的类型。|  
|DimensionUniqueName|维度的明确名称。|  
|HierarchyCaption|与层次结构关联的标签或标题。|  
|HierarchyCardinality|层次结构中的成员数。|  
|HierarchyGUID|层次结构的 GUID。|  
|HierarchyName|层次结构的名称。|  
|HierarchyUniqueName|层次结构的明确名称。|  
|SchemaName|此多维数据集所属架构的名称。|  
  
 本部分包含以下主题。  
  
-   [属性、方法和事件](./hierarchy-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [VBScript) 的 CubeDef 示例 (](./cubedef-example-vbscript.md)   
 [维度对象 (ADO MD) ](./dimension-object-ado-md.md)   
 [层次结构集合 (ADO MD) ](./hierarchies-collection-ado-md.md)   
 [级别收集 (ADO MD) ](./levels-collection-ado-md.md)   
 [属性集合 (ADO)](../ado-api/properties-collection-ado.md)
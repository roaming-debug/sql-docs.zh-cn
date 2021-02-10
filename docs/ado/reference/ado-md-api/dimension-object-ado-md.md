---
description: 维度对象 (ADO MD)
title: 维度对象 (ADO MD) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Dimension
helpviewer_keywords:
- Dimension object [ADO MD]
ms.assetid: 66adbbd2-23a3-4c19-a91b-84c31309aa1b
author: rothja
ms.author: jroth
ms.openlocfilehash: 311ea31a50bb9004425ad8db14c1beb309e15eb0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054802"
---
# <a name="dimension-object-ado-md"></a>维度对象 (ADO MD)
表示多维数据集的一个维度，其中包含一个或多个成员的层次结构。  
  
## <a name="remarks"></a>备注  
 使用 **维度** 对象的集合和属性，可以执行以下操作：  
  
-   标识具有 [Name](./name-property-ado-md.md)和 [UniqueName](./uniquename-property-ado-md.md)属性的 **维度**。  
  
-   返回一个有意义的字符串，该字符串描述具有 [Description](./description-property-ado-md.md)属性的 **维度**。  
  
-   返回与 [层次结构](./hierarchies-collection-ado-md.md)集合构成 **维度** 的 [层次结构](./hierarchy-object-ado-md.md)对象。  
  
-   使用标准 ADO [Properties](../ado-api/properties-collection-ado.md) 集合获取有关 **维度** 对象的其他信息。  
  
 **Properties** 集合包含提供程序提供的属性。 下表列出了可用的属性。 实际属性列表可能有所不同，具体取决于提供程序的实现。 有关可用属性的更完整列表，请参阅提供程序的文档。  
  
|名称|说明|  
|----------|-----------------|  
|CatalogName|此多维数据集所属的目录的名称。|  
|CubeName|多维数据集的名称。|  
|DefaultHierarchy|默认层次结构的唯一名称。|  
|说明|多维数据集的有意义的说明。|  
|DimensionCaption|与维度关联的标签或标题。|  
|DimensionCardinality|维度中的成员数。|  
|DimensionGUID|维度的 GUID。|  
|DimensionName|维的名称。|  
|DimensionOrdinal|构成多维数据集的维度组中的维度的序号。|  
|DimensionType|维度类型。|  
|DimensionUniqueName|维度的明确名称。|  
|SchemaName|此多维数据集所属架构的名称。|  
  
 本部分包含以下主题。  
  
-   [属性、方法和事件](./dimension-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [VBScript) 的 CubeDef 示例 (](./cubedef-example-vbscript.md)   
 [CubeDef 对象 (ADO MD) ](./cubedef-object-ado-md.md)   
 [维度集合 (ADO MD) ](./dimensions-collection-ado-md.md)   
 [层次结构集合 (ADO MD) ](./hierarchies-collection-ado-md.md)   
 [属性集合 (ADO)](../ado-api/properties-collection-ado.md)
---
description: CubeDef 对象 (ADO MD)
title: CubeDef)  (ADO MD 对象 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CubeDef
helpviewer_keywords:
- CubeDef object [ADO MD]
ms.assetid: feb2581c-fc41-471c-bb69-29f8a55fda70
author: rothja
ms.author: jroth
ms.openlocfilehash: bae255b0de860a1caee016d5e8867c7fb9b3ffee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174316"
---
# <a name="cubedef-object-ado-md"></a>CubeDef 对象 (ADO MD)
表示多维架构中的一个多维数据集，其中包含一组相关的维度。  
  
## <a name="remarks"></a>备注  
 使用 **CubeDef** 对象的集合和属性，可以执行以下操作：  
  
-   标识具有 [Name](./name-property-ado-md.md)属性的 **CubeDef** 。  
  
-   返回一个字符串，该字符串描述具有 [Description](./description-property-ado-md.md) 属性的多维数据集。  
  
-   返回与 [维度](./dimensions-collection-ado-md.md) 集合构成多维数据集的维度。  
  
-   获取有关具有标准 ADO [属性](../ado-api/properties-collection-ado.md)集合的 **CubeDef** 的其他信息。  
  
 **Properties** 集合包含提供程序提供的属性。 下表列出了可用的属性。 实际属性列表可能有所不同，具体取决于提供程序的实现。 有关可用属性的更完整列表，请参阅提供程序的文档。  
  
|名称|说明|  
|----------|-----------------|  
|CatalogName|此多维数据集所属的目录的名称。|  
|CreatedOn|多维数据集的创建日期和时间。|  
|CubeGUID|多维数据集 GUID。|  
|CubeName|多维数据集的名称。|  
|CubeType|多维数据集的类型。|  
|DataUpdatedBy|执行上次数据更新的人员的用户 ID。|  
|说明|多维数据集的有意义的说明。|  
|LastSchemaUpdate|上次架构更新的日期和时间。|  
|SchemaName|此多维数据集所属架构的名称。|  
|SchemaUpdatedBy|执行最后一个架构更新的人员的用户 ID。|  
  
 本部分包含以下主题。  
  
-   [属性、方法和事件](./cubedef-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [VBScript) 的 CubeDef 示例 (](./cubedef-example-vbscript.md)   
 [目录对象 (ADO MD) ](./catalog-object-ado-md.md)   
 [CubeDefs 集合 (ADO MD) ](./cubedefs-collection-ado-md.md)   
 [维度集合 (ADO MD) ](./dimensions-collection-ado-md.md)   
 [属性集合 (ADO)](../ado-api/properties-collection-ado.md)
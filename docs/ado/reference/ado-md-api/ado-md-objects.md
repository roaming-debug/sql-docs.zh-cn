---
description: ADO MD 对象
title: ADO MD 对象 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- ADO MD, objects
- objects [ADO MD]
ms.assetid: 2a32e873-3282-4520-a7ed-89493f1da80e
author: rothja
ms.author: jroth
ms.openlocfilehash: ae9723fb593a6f2599a3fe13cbfc359fa280febe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172311"
---
# <a name="ado-md-objects"></a>ADO MD 对象

|对象|说明|  
|-|-|  
|[轴](./axis-object-ado-md.md)|表示单元集的位置或筛选轴，其中包含一个或多个维度的选定成员。|  
|[目录](./catalog-object-ado-md.md)|包含多维架构信息 (多维数据提供程序) 特定于多维数据访问接口 (MDP) 的多维数据集和基础维度、层次结构、级别和成员。|  
|[单元](./cell-object-ado-md.md)|表示轴坐标相交处的数据，该数据集包含在单元集中。|  
|[格](./cellset-object-ado-md.md)|表示多维查询的结果。 它是从多维数据集或其他单元集选择的单元的集合。|  
|[CubeDef](./cubedef-object-ado-md.md)|表示多维架构中的一个多维数据集，其中包含一组相关的维度。|  
|[维度](./dimension-object-ado-md.md)|表示多维数据集的一个维度，其中包含一个或多个成员的层次结构。|  
|[层次结构](./hierarchy-object-ado-md.md)|表示一个维度成员可以聚合或 "汇总" 的方式。 可以在一个或多个层次结构上聚合维度。|  
|[级别](./level-object-ado-md.md)|包含一组成员，其中每个成员在层次结构中具有相同的级别。|  
|[成员](./member-object-ado-md.md)|表示多维数据集中某一级别的成员、某一级别的某个成员的子级或沿某个单元集的一个位置的成员。|  
|[位置](./position-object-ado-md.md)|表示一个或多个不同维度的成员的集合，这些成员定义沿轴的点。|  
  
 此外， **目录** 对象还连接到 ado **连接** 对象，该对象包含在标准 ADO 库中：  
  
|对象|说明|  
|------------|-----------------|  
|[连接](../ado-api/connection-object-ado.md)|表示到数据源的连接是打开的。|  
  
 这些对象之间的关系如 [ADO MD 对象模型](./ado-md-object-model.md)中所示。  
  
 许多 ADO MD 对象可以包含在相应的集合中。 例如， [CubeDef](./cubedef-object-ado-md.md)对象可以包含在 **目录** 的 [CubeDefs](./cubedefs-collection-ado-md.md)集合中。 有关详细信息，请参阅 [ADO MD 集合](./ado-md-collections.md)。  
  
## <a name="see-also"></a>另请参阅  
 [ADO MD API 参考](./ado-md-object-model.md)   
 [ADO MD 代码示例](./ado-md-code-examples.md)   
 [ADO MD 集合](./ado-md-collections.md)   
 [ADO MD 枚举常量](./ado-md-enumerated-constants.md)   
 [ADO MD 方法](./ado-md-methods.md)   
 [ADO MD 对象模型](./ado-md-object-model.md)   
 [ADO MD 属性](./ado-md-properties.md)
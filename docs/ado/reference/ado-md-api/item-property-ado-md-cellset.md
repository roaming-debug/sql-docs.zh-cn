---
description: Item 属性（ADO MD 单元集）
title: 项属性 (ADO MD 单元格集) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Item
- Cellset::Item
helpviewer_keywords:
- Item property [ADO MD]
ms.assetid: 0e93d79b-b12e-4e98-889e-c2dfcca20fd0
author: rothja
ms.author: jroth
ms.openlocfilehash: 6d2fe7bb3d13b988b7bc25b6d3f17a5b2ccf945a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164539"
---
# <a name="item-property-ado-md-cellset"></a>Item 属性（ADO MD 单元集）
[使用单元坐标的坐标](./cellset-object-ado-md.md)从单元集中检索单元格。  
  
## <a name="syntax"></a>语法  
  
```  
  
Set  
Cell = Cellset.Item ( Positions)  
```  
  
## <a name="parameters"></a>参数  
 *位置*  
 唯一指定单元格的值的 **VariantArray** 。 *位置* 可以是以下各项之一：  
  
-   位置号的数组  
  
-   成员名称的数组  
  
-   序号位置  
  
## <a name="remarks"></a>备注  
 使用 **Item** 属性可以返回单元 [集](./cellset-object-ado-md.md)对象内的 [Cell](./cell-object-ado-md.md)对象。 如果 **Item** 属性找不到对应于 *位置* 参数的单元，则会出现错误。  
  
 **Item** 属性是 **单元集** 对象的默认属性。 以下语法形式可互换：  
  
```  
  
Cellset.Item ( Positions )Cellset ( Positions )  
```  
  
 *位置* 参数指定要返回的单元格。 可以按序号位置或沿每个轴上的位置来指定单元格。 按位置沿每个轴指定单元格时，可以指定位置的数值或每个位置的成员的名称。  
  
 序号位置是唯一 **标识单元格** 集中某个单元格的数字。 从概念上讲，单元在 **单元集中编号，就如同** 单元 **集** 是一个二维数组，其中 *p* 是 *轴的数目*。 单元按行优先的顺序排列。 下面是用于计算单元序号的公式：  
  
 如果成员名称作为字符串传递给 **Item**，则成员必须按数值轴标识符的递增顺序列出。 在轴中，成员必须按维度嵌套的递增顺序列出，也就是说，最外面的维度成员首先出现，然后是内部维度的成员。 每个维度都应由单独的字符串表示，并且应用逗号分隔成员字符串列表。  
  
> [!NOTE]
>  数据访问接口可能不支持按成员名称检索单元格。 有关详细信息，请参阅提供程序的文档。  
  
## <a name="applies-to"></a>应用于  
 [单元集对象 (ADO MD)](./cellset-object-ado-md.md)  
  
## <a name="see-also"></a>另请参阅  
 [Cell Object (ADO MD) ](./cell-object-ado-md.md)   
 [单元集对象 (ADO MD)](./cellset-object-ado-md.md)
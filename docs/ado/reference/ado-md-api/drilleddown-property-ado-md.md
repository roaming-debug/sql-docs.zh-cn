---
description: DrilledDown 属性 (ADO MD)
title: DrilledDown 属性 (ADO MD) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- DrilledDown
- Member::DrilledDown
helpviewer_keywords:
- DrilledDown property [ADO MD]
ms.assetid: bf39dd36-fc7a-4f6e-86c0-fa71430c0d86
author: rothja
ms.author: jroth
ms.openlocfilehash: 2e25d23cfdbf7f33f024203e25814b16d4ec6da4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169859"
---
# <a name="drilleddown-property-ado-md"></a>DrilledDown 属性 (ADO MD)
指示子元素是否紧靠在轴上的 [成员](./member-object-ado-md.md) 后面。  
  
## <a name="return-values"></a>返回值  
 返回 **布尔** 值并且是只读的。 如果轴上的当前成员没有子成员，则 **DrilledDown** 将返回 **True** 。 如果当前成员在轴上有一个或多个子成员，则 **DrilledDown** 将返回 **False** 。  
  
## <a name="remarks"></a>备注  
 使用 **DrilledDown** 属性可确定紧靠此成员之后，在轴上是否至少有一个此成员的子级。 显示成员时，此信息很有用。  
  
 此属性仅在属于某个[位置](./position-object-ado-md.md)对象的[成员](./member-object-ado-md.md)对象上受支持。 从属于 [Level](./level-object-ado-md.md)对象的 **成员** 对象引用此属性时，将发生错误。  
  
## <a name="applies-to"></a>应用于  
 [成员对象 (ADO MD)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>另请参阅  
 [ParentSameAsPrev 属性 (ADO MD)](./parentsameasprev-property-ado-md.md)
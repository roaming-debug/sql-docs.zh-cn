---
description: DefinedSize 属性
title: DefinedSize 属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::DefinedSize
helpviewer_keywords:
- DefinedSize property [ADO]
ms.assetid: 3ee27314-a305-4fbc-8433-9ee9a909afd6
author: rothja
ms.author: jroth
ms.openlocfilehash: 756fe40bf7916dfa3e56a3e559be2874b5e8cd85
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171307"
---
# <a name="definedsize-property"></a>DefinedSize 属性
指示 [Field](../../../ado/reference/ado-api/field-object.md) 对象的数据容量。  
  
## <a name="return-value"></a>返回值  
 返回一个 **长整型** 值，该值反映字段的定义大小，该字段依赖于 field 对象的数据类型;有关详细信息，请参阅 [类型](../../../ado/reference/ado-api/type-property-ado.md) 。 对于使用固定长度的数据类型的字段，返回值为数据类型的大小（以字节为单位）。 对于使用可变长度数据类型的字段，这是以下项之一：  
  
1.  如果字段具有定义的长度，则以字符为单位 **() 的** 最大 **长度（以字符为单位**， (为 **adVarBinary**），而 **adVarNumeric**) 。 例如， **adVarChar (5)** 的字段的最大长度为5。  
  
2.  对于 AdBinary 和 AdNumeric **) ，数据** 类型的最大长度（以字符) 为单位） (用于和 **(** （如果该字段没有定义的长度）。  
  
3.  ~ 0 (按位，该值不是 0;如果字段和数据类型都没有定义的最大长度，则所有位都将设置为 1) 。  
  
4.  对于没有长度的数据类型，此值设置为 ~ 0 (按位运算，该值不是 0;所有位均设置为 1) 。  
  
## <a name="remarks"></a>备注  
 使用 **DefinedSize** 属性可确定 **Field** 对象的数据容量。  
  
 **DefinedSize** 和 [ActualSize](../../../ado/reference/ado-api/actualsize-property-ado.md)属性不同。 例如，假设某个 **字段** 对象的类型为 **adVarChar** ， **DefinedSize** 属性值为50，其中包含单个字符。 它返回的 **ActualSize** 属性值是单个字符的长度（以字节为单位）。  
  
## <a name="applies-to"></a>应用于  
 [字段对象](../../../ado/reference/ado-api/field-object.md)  
  
## <a name="see-also"></a>另请参阅  
 [ActualSize 和 DefinedSize 属性示例 (VB) ](../../../ado/reference/ado-api/actualsize-and-definedsize-properties-example-vb.md)   
 [ActualSize 和 DefinedSize 属性示例 (VC + +) ](../../../ado/reference/ado-api/actualsize-and-definedsize-properties-example-vc.md)   
 [ActualSize 属性 (ADO)](../../../ado/reference/ado-api/actualsize-property-ado.md)

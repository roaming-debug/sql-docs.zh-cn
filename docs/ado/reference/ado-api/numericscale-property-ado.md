---
description: NumericScale 属性 (ADO)
title: " (ADO) 的 NumericScale 属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::NumericScale
- Field20::NumericScale
helpviewer_keywords:
- NumericScale property [ADO]
ms.assetid: 29a02992-64be-4fcd-be13-445cba205893
author: rothja
ms.author: jroth
ms.openlocfilehash: 3164dcc61165171ff0d1c33956f152bcb65f33b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170741"
---
# <a name="numericscale-property-ado"></a>NumericScale 属性 (ADO)
指示 [参数](./parameter-object.md) 或 [字段](./field-object.md) 对象中数值的小数位数。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 **字节** 值，该值指示将对数值解析成的小数位数。  
  
## <a name="remarks"></a>备注  
 使用 **NumericScale** 属性可确定小数点右边的位数，将用来表示数值 **参数** 或 **字段** 对象的值。  
  
 对于 **参数** 对象， **NumericScale** 属性是可读/写的。  
  
 对于 **字段** 对象， **NumericScale** 通常是只读的。 但是，对于已追加到 [记录](./record-object-ado.md)的 [字段](./fields-collection-ado.md)集合的新 **字段** 对象， **NumericScale** 仅在指定了 **字段** 的 [值](./value-property-ado.md)属性并且数据提供程序已通过调用 [Fields](./fields-collection-ado.md)集合的 [Update](./update-method.md)方法成功添加了新 **字段** 之后，才是可读/写的。  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [字段对象](./field-object.md)  
    :::column-end:::
    :::column:::
        [参数对象](./parameter-object.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅  
 [NumericScale 和 Precision 属性示例 (VB) ](./numericscale-and-precision-properties-example-vb.md)   
 [NumericScale 和 Precision 属性示例 (VC + +) ](./numericscale-and-precision-properties-example-vc.md)   
 [Precision 属性 (ADO)](./precision-property-ado.md)
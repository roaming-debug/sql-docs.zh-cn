---
description: NumericScale 属性 (ADOX)
title: NumericScale 属性 (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Column::PutNumericScale
- _Column::GetNumericScale
- _Column::NumericScale
- _Column::put_NumericScale
- _Column::get_NumericScale
helpviewer_keywords:
- NumericScale property [ADOX]
ms.assetid: 573ee5d1-57c7-4a27-be79-a0e12944ad9b
author: rothja
ms.author: jroth
ms.openlocfilehash: 8dbf66f202a051740e7d3237f7b52a1fda03b77a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053982"
---
# <a name="numericscale-property-adox"></a>NumericScale 属性 (ADOX)
指示列中数值的小数位数。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置并返回一个 **字节** 值，该值表示当 [类型](./type-property-column-adox.md) 属性为 **adNumeric** 或 **adDecimal** 时，列中的数据值的小数位数。 对于所有其他数据类型，将忽略 **NumericScale** 。  
  
## <a name="remarks"></a>备注  
 默认值为零 (0)。  
  
 对于已追加到集合的 [列](./column-object-adox.md)对象， **NumericScale** 为只读。  
  
## <a name="applies-to"></a>应用于  
 [列对象 (ADOX)](./column-object-adox.md)  
  
## <a name="see-also"></a>另请参阅  
 [ADOX 代码示例： NumericScale 和 Precision 属性示例 (VB) ](./adox-code-example-numericscale-and-precision-properties-example-vb.md)   
 [Type 属性（列）(ADOX)](./type-property-column-adox.md)
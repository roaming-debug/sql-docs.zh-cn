---
description: Precision 属性 (ADOX)
title: 精度属性 (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Column::put_Precision
- _Column::PutPrecision
- _Column::GetPrecision
- _Column::get_Precision
- _Column::Precision
helpviewer_keywords:
- Precision property [ADOX]
ms.assetid: 0e0ecbbf-d7de-49d4-a128-5a519ecd54ba
author: rothja
ms.author: jroth
ms.openlocfilehash: f98cc2ee59e7b3e0c873a6bc48c3234148543484
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053893"
---
# <a name="precision-property-adox"></a>Precision 属性 (ADOX)
指示 [列](./column-object-adox.md)中数据值的最大精度。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置并返回一个 **Long** 值，该值为 [类型](./type-property-column-adox.md) 属性为数值类型时列中数据值的最大精度。 对于所有其他数据类型，将忽略 **精度**。  
  
## <a name="remarks"></a>备注  
 默认值为零 (**0**)。  
  
 对于已追加到集合的 [列](./column-object-adox.md) 对象，此属性是只读的。  
  
## <a name="applies-to"></a>应用于  
 [列对象 (ADOX)](./column-object-adox.md)  
  
## <a name="see-also"></a>另请参阅  
 [ADOX 代码示例： NumericScale 和 Precision 属性示例 (VB) ](./adox-code-example-numericscale-and-precision-properties-example-vb.md)   
 [ (ADOX)  (列的类型属性) ](./type-property-column-adox.md)   
 [列对象 (ADOX)](./column-object-adox.md)
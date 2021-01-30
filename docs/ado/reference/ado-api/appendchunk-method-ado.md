---
description: AppendChunk 方法 (ADO)
title: " (ADO) 的 AppendChunk 方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::AppendChunk
- Field20::AppendChunk
helpviewer_keywords:
- AppendChunk method [ADO]
ms.assetid: c648b5a8-d4f1-4d16-836e-3957feb03617
author: rothja
ms.author: jroth
ms.openlocfilehash: b7abcebce9f587260d7147745ab99c2b4cecc78a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167873"
---
# <a name="appendchunk-method-ado"></a>AppendChunk 方法 (ADO)
将数据追加到大文本或二进制数据 [字段](./field-object.md)，或追加到 [参数](./parameter-object.md) 对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
object.AppendChunk Data  
```  
  
#### <a name="parameters"></a>参数  
 *object*  
 一个 **字段** 或 **参数** 对象。  
  
 *数据*  
 一个包含要追加到对象的数据的 **变量** 。  
  
## <a name="remarks"></a>备注  
 对 **字段** 或 **参数** 对象使用 **AppendChunk** 方法，以使用较长的二进制或字符数据填充它。 在系统内存有限的情况下，可以使用 **AppendChunk** 方法在部分中而不是完整地操作长值。  
  
## <a name="field"></a>字段  
 如果 **字段** 对象的 [Attributes](./attributes-property-ado.md)属性中的 **adFldLong** 位设置为 **true**，则可以对该字段使用 **AppendChunk** 方法。  
  
 **Field** 对象上的第一个 **AppendChunk** 调用将数据写入字段，并覆盖任何现有数据。 后续 **AppendChunk** 调用将添加到现有数据。 如果要将数据追加到一个字段，然后设置或读取当前记录中另一个字段的值，则 ADO 会假定您已完成将数据追加到第一个字段。 如果再次对第一个字段调用 **AppendChunk** 方法，则 ADO 会将调用解释为新的 **AppendChunk** 操作，并覆盖现有数据。 访问不是第一个 **记录集** 对象的克隆的其他 [记录集](./recordset-object-ado.md)对象中的字段将不会中断 **AppendChunk** 操作。  
  
 如果对 **字段** 对象调用 **AppendChunk** 时没有当前记录，则会发生错误。  
  
> [!NOTE]
>  **AppendChunk** 方法不 [ (ADO) 对象在记录对象](./record-object-ado.md)的 **字段** 对象上操作。 它不执行任何操作，并将生成运行时错误。  
  
## <a name="parameter"></a>参数  
 如果 **参数** 对象的 **Attributes** 属性中的 **adParamLong** 位设置为 **true**，则可以对该参数使用 **AppendChunk** 方法。  
  
 **参数** 对象上的第一个 **AppendChunk** 调用将数据写入参数，并覆盖任何现有数据。 后续 **AppendChunk** 对 **参数** 对象的调用将添加到现有参数数据。 传递 null 值的 **AppendChunk** 调用会丢弃所有参数数据。  
  
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
 [AppendChunk 和 GetChunk 方法示例 (VB) ](./appendchunk-and-getchunk-methods-example-vb.md)   
 [AppendChunk 和 GetChunk 方法示例 (VC + +) ](./appendchunk-and-getchunk-methods-example-vc.md)   
 [ADO)  (特性属性 ](./attributes-property-ado.md)   
 [GetChunk 方法 (ADO)](./getchunk-method-ado.md)
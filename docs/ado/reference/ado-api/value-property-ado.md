---
description: Value 属性 (ADO)
title: ADO)  (值属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::Value
- _Parameter::Value
helpviewer_keywords:
- Value property [ADO]
ms.assetid: 48919c74-86d4-462e-99b9-8854ceb8d683
author: rothja
ms.author: jroth
ms.openlocfilehash: b809a24429b490961e1d0d009656539db9fe81d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172409"
---
# <a name="value-property-ado"></a>Value 属性 (ADO)

指示赋给 [字段](./field-object.md)、 [参数](./parameter-object.md)或 [属性](./property-object-ado.md) 对象的值。
  
## <a name="settings-and-return-values"></a>设置和返回值

设置或返回一个表示对象的值的 **变量** 值。 默认值取决于 [Type](./type-property-ado.md) 属性。
  
## <a name="remarks"></a>备注

使用 **Value** 属性可以设置或返回 **字段** 对象中的数据，使用 **参数** 对象设置或返回参数值，或者使用 **属性** 对象设置或返回属性设置。 **Value** 属性是可读/写还是只读取决于许多因素。 有关详细信息，请参阅各自的对象主题。

ADO 允许设置和返回带有 **Value** 属性的长二进制数据。
  
> [!NOTE]
> 对于 **参数** 对象，ADO 仅从提供程序读取一次 **值** 属性。 如果命令包含的 **参数** 的 **值** 属性为空，并且您从命令创建 [记录集](./recordset-object-ado.md) ，请确保先关闭 **记录集** ，然后再检索 **值** 属性。 否则，对于某些访问接口， **值** 属性可能为空，并且不包含正确的值。
> 
> 对于附加到 [Record](./record-object-ado.md)对象的 [Fields](./fields-collection-ado.md)集合的新 **字段** 对象，必须先设置 **Value** 属性，然后才能指定任何其他 **字段** 属性。 首先，必须在名为的 **字段** 集合上分配和 [更新](./update-method.md)**值** 属性的特定值。 然后，可以访问其他属性，例如 [类型](./type-property-ado.md) 或 [属性](./attributes-property-ado.md) 。
  
## <a name="applies-to"></a>应用于

:::row:::
    :::column:::
        [字段对象](./field-object.md)  
    :::column-end:::
    :::column:::
        [参数对象](./parameter-object.md)  
    :::column-end:::
    :::column:::
        [属性对象 (ADO)](./property-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅

[值属性示例 (VB) ](./value-property-example-vb.md) 
[值属性示例 (VC + +) ](./value-property-example-vc.md)
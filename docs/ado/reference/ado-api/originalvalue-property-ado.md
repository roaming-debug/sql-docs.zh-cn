---
description: OriginalValue 属性 (ADO)
title: " (ADO) 的 OriginalValue 属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::OriginalValue
helpviewer_keywords:
- OriginalValue property [ADO]
ms.assetid: 6e33c6ec-14d9-4b1d-ba9b-cb99862e7bac
author: rothja
ms.author: jroth
ms.openlocfilehash: e0948b7d5dd6fef29336fbb9af49e2578b2bb4ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041237"
---
# <a name="originalvalue-property-ado"></a>OriginalValue 属性 (ADO)
指示在进行任何更改之前，记录中是否存在 [字段](./field-object.md) 的值。  
  
## <a name="return-value"></a>返回值  
 返回一个表示字段值的 **变量** 值，此值在任何更改之前。  
  
## <a name="remarks"></a>备注  
 使用 **OriginalValue** 属性可以返回当前记录中字段的原始字段值。  
  
 在 *即时更新模式下* (在调用 [update](./update-method.md) 方法) 后，提供程序将更改写入基础数据源， **OriginalValue** 属性返回在任何更改前存在的字段值 (也就是说，自上次 **更新** 方法调用) 之后。 此值与 [CancelUpdate](./cancelupdate-method-ado.md) 方法用来替换 [value](./value-property-ado.md) 属性的值相同。  
  
 在 *批处理更新模式下* (提供程序将在其中缓存多个更改，并仅在调用 [UpdateBatch](./updatebatch-method.md) 方法) 时将这些更改写入基础数据源，而 **OriginalValue** 属性返回在任何更改前存在的字段值 (也就是说，自上次 **UpdateBatch** 方法调用) 。 此值与 [CancelBatch](./cancelbatch-method-ado.md) 方法用来替换 **value** 属性的值相同。 当你将此属性与 [UnderlyingValue](./underlyingvalue-property.md) 属性一起使用时，你可以解决因批更新引起的冲突。  
  
## <a name="record"></a>记录  
 对于 [记录](./record-object-ado.md)对象，在调用 [Update](./update-method.md)之前添加的字段的 **OriginalValue** 属性将为空。  
  
## <a name="applies-to"></a>应用于  
 [字段对象](./field-object.md)  
  
## <a name="see-also"></a>另请参阅  
 [OriginalValue 和 UnderlyingValue 属性示例 (VB) ](./originalvalue-and-underlyingvalue-properties-example-vb.md)   
 [OriginalValue 和 UnderlyingValue 属性示例 (VC + +) ](./originalvalue-and-underlyingvalue-properties-example-vc.md)   
 [UnderlyingValue 属性](./underlyingvalue-property.md)
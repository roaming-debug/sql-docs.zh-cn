---
description: UnderlyingValue 属性
title: UnderlyingValue 属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::GetUnderlyingValue
- Field20::get_UnderlyingValue
- Field20::UnderlyingValue
helpviewer_keywords:
- UnderlyingValue property
ms.assetid: 00a0c8b8-8b63-433f-95b8-020ab05874a0
author: rothja
ms.author: jroth
ms.openlocfilehash: dd9ba64953632ad4681e7650ba8028df45e53da6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166345"
---
# <a name="underlyingvalue-property"></a>UnderlyingValue 属性
指示数据库中 [Field](./field-object.md) 对象的当前值。  
  
## <a name="return-value"></a>返回值  
 返回一个表示 **字段** 值的 **变量** 值。  
  
## <a name="remarks"></a>备注  
 使用 **UnderlyingValue** 属性从数据库返回当前字段值。 **UnderlyingValue** 属性中的字段值是对你的事务可见的值，可能是另一个事务的最新更新导致的。 这可能不同于 [OriginalValue](./originalvalue-property-ado.md) 属性，后者反映最初返回到 [记录集](./recordset-object-ado.md)的值。  
  
 这类似于使用 [Resync](./resync-method.md) 方法，但 **UnderlyingValue** 属性仅返回当前记录中特定字段的值。 此值与 [Resync](./resync-method.md) 方法用来替换 [value](./value-property-ado.md) 属性的值相同。  
  
 当你将此属性与 **OriginalValue** 属性一起使用时，你可以解决因批更新引起的冲突。  
  
## <a name="record"></a>记录  
 对于 [记录](./record-object-ado.md) 对象，在调用 [Update](./update-method.md) 之前添加的字段的此属性将为空。  
  
## <a name="applies-to"></a>应用于  
 [字段对象](./field-object.md)  
  
## <a name="see-also"></a>另请参阅  
 [OriginalValue 和 UnderlyingValue 属性示例 (VB) ](./originalvalue-and-underlyingvalue-properties-example-vb.md)   
 [OriginalValue 和 UnderlyingValue 属性示例 (VC + +) ](./originalvalue-and-underlyingvalue-properties-example-vc.md)   
 [OriginalValue 属性 (ADO) ](./originalvalue-property-ado.md)   
 [重新同步方法](./resync-method.md)
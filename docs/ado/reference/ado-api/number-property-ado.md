---
description: Number 属性 (ADO)
title: ADO)  (数字属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error::Number
- Error::GetNumber
- Error::get_Number
helpviewer_keywords:
- number property [ADO]
ms.assetid: f92323c5-dd11-4a63-a505-d9014a0f067f
author: rothja
ms.author: jroth
ms.openlocfilehash: 82206b827f505994cd151833e0bee4b1ff4ad7de
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167039"
---
# <a name="number-property-ado"></a>Number 属性 (ADO)
指示唯一标识 [错误](./error-object.md) 对象的数字。  
  
## <a name="return-value"></a>返回值  
 返回可能对应于其中一个 [ErrorValueEnum](./errorvalueenum.md)常量的 **Long** 值。  
  
## <a name="remarks"></a>备注  
 使用 **Number** 属性可确定发生的错误。 属性的值是与错误条件对应的唯一编号。  
  
 [Errors](./errors-collection-ado.md)集合以十六进制格式返回 HRESULT (例如，0x80004005) 或 long 值 (例如 2147467259) 。 这些 Hresult 可以由基础组件（例如 OLE DB 甚至 OLE 本身）引发。 有关这些数字的详细信息，请参阅 [OLE DB 程序员参考](/previous-versions/windows/desktop/ms713643(v=vs.85))中 [OLE DB)  (错误](/previous-versions/windows/desktop/ms724533(v=vs.85))*。*  
  
## <a name="applies-to"></a>应用于  
 [错误对象](./error-object.md)  
  
## <a name="see-also"></a>另请参阅  
 [Description、HelpContext、"帮助"、"NativeError"、"Number"、"Source" 和 "SQLState Properties" 示例 (VB) ](./description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Description、HelpContext、"帮助"、"NativeError"、"Number"、"Source" 和 "SQLState Properties" 示例 (VC + +) ](./description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [Description 属性](./description-property.md)   
 [HelpContext，帮助的属性](./helpcontext-helpfile-properties.md)   
 [Source 属性（ADO 错误）](./source-property-ado-error.md)
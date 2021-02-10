---
title: Description 属性 |Microsoft Docs
description: 了解 ADO 中 Error 对象的 description 属性，该属性返回包含错误说明的字符串值。
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error::Description
- Error::GetDescription
- Error::get_Description
helpviewer_keywords:
- Description property
ms.assetid: 4b5d6790-6c29-42aa-bf78-d9cfb8ad7965
author: rothja
ms.author: jroth
ms.openlocfilehash: 7181bffdad12291c08bf4fb5c6a00b4b1b2a6cc5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034307"
---
# <a name="description-property"></a>Description 属性
描述 [错误](../../../ado/reference/ado-api/error-object.md) 对象。  
  
## <a name="return-value"></a>返回值  
 返回一个 **字符串** 值，该值包含对错误的说明。  
  
## <a name="remarks"></a>备注  
 使用 **Description** 属性可获取错误的简短说明。 显示此属性以提醒用户无法或不想处理的错误。 该字符串来自 ADO 或提供程序。  
  
 提供程序负责向 ADO 传递特定的错误文本。 ADO 将 [错误](../../../ado/reference/ado-api/error-object.md) 对象添加到每个提供程序错误或它收到的警告的 **错误** 集合中。 枚举 **errors** 集合以跟踪提供程序传递的错误。  
  
## <a name="applies-to"></a>应用于  
 [错误对象](../../../ado/reference/ado-api/error-object.md)  
  
## <a name="see-also"></a>另请参阅  
 [Description、HelpContext、"帮助"、"NativeError"、"Number"、"Source" 和 "SQLState Properties" 示例 (VB) ](../../../ado/reference/ado-api/description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Description、HelpContext、"帮助"、"NativeError"、"Number"、"Source" 和 "SQLState Properties" 示例 (VC + +) ](../../../ado/reference/ado-api/description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [HelpContext，帮助的属性](../../../ado/reference/ado-api/helpcontext-helpfile-properties.md)   
 [ADO (的数字属性) ](../../../ado/reference/ado-api/number-property-ado.md)   
 [Source 属性（ADO 错误）](../../../ado/reference/ado-api/source-property-ado-error.md)

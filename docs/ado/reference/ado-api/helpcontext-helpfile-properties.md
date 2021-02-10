---
description: HelpContext、HelpFile 属性
title: HelpContext，帮助的属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error::GetHelpContext
- Error::GetHelpFile
- Error::get_HelpFile
- Error::get_HelpContext
- Error::HelpContext
- Error::HelpFile
helpviewer_keywords:
- HelpContext property [ADO]
- HelpFile property [ADO]
ms.assetid: 2b9ef441-993c-44d4-8f87-fac0979dac1d
author: rothja
ms.author: jroth
ms.openlocfilehash: ee683a07b0a581788657af2233944dccf5ab05b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020784"
---
# <a name="helpcontext-helpfile-properties"></a>HelpContext、HelpFile 属性
指示与 [错误](./error-object.md) 对象关联的帮助文件和主题。  
  
## <a name="return-values"></a>返回值  
  
-   **帮助** 对于帮助文件中的主题，返回一个上下文 ID，作为一个 **长** 值。  
  
-   **帮助** 返回一个 **字符串** 值，该值的计算结果为帮助文件的完全解析路径。  
  
## <a name="remarks"></a>备注  
 如果 **在 "帮助文件" 属性** 中指定了帮助文件，则将使用 **HelpContext** 属性来自动显示它所标识的帮助主题。 如果没有可用的相关帮助主题，则 **HelpContext** 属性返回零，而 " **帮助** 值" 属性返回长度为零的字符串 ( "" ) 。  
  
## <a name="applies-to"></a>应用于  
 [错误对象](./error-object.md)  
  
## <a name="see-also"></a>另请参阅  
 [Description、HelpContext、"帮助"、"NativeError"、"Number"、"Source" 和 "SQLState Properties" 示例 (VB) ](./description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Description、HelpContext、"帮助"、"NativeError"、"Number"、"Source" 和 "SQLState Properties" 示例 (VC + +) ](./description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [Description 属性](./description-property.md)   
 [ADO (的数字属性) ](./number-property-ado.md)   
 [Source 属性（ADO 错误）](./source-property-ado-error.md)
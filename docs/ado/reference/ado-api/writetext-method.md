---
description: WriteText 方法
title: WriteText 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_WriteText
- _Stream::WriteText
helpviewer_keywords:
- WriteText method [ADO]
ms.assetid: 7a669048-13f4-4574-a2b1-985e089729d5
author: rothja
ms.author: jroth
ms.openlocfilehash: b4c05ab892ce442db5eefefddf3eb0796af5dec9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166248"
---
# <a name="writetext-method"></a>WriteText 方法
向 [Stream](./stream-object-ado.md) 对象写入指定的文本字符串。  
  
## <a name="syntax"></a>语法  
  
```  
  
Stream.WriteText Data, Options  
```  
  
#### <a name="parameters"></a>参数  
 *数据*  
 一个 **字符串** 值，该值包含要写入的字符中的文本。  
  
 *选项*  
 可选。 一个 [StreamWriteEnum](./streamwriteenum.md) 值，该值指定是否必须在指定字符串的末尾写入行分隔符。  
  
## <a name="remarks"></a>备注  
 将指定的字符串写入 **流** 对象，而不会在每个字符串之间插入空格或字符。  
  
 [当前位置设置为写入](./position-property-ado.md)数据后的字符。 **WriteText** 方法不会截断流中的其余数据。 如果要截断这些字符，请调用 [SetEOS](./seteos-method.md)。  
  
 如果写入超出当前 [EOS](./eos-property.md)位置，则会将 **流** 的 [大小](./size-property-ado-stream.md)增加为包含任何新字符，而 **EOS** 会移动到 **流** 中的新最后一个字节。  
  
> [!NOTE]
>  **WriteText** 方法用于文本流 ([类型](./type-property-ado-stream.md)为 **adTypeText**) 。 对于二进制流 (**类型** 为 **adTypeBinary**) ，请使用 [Write](./write-method.md)。  
  
## <a name="applies-to"></a>应用于  
 [流对象 (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [Write 方法](./write-method.md)
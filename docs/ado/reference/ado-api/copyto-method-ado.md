---
description: CopyTo 方法 (ADO)
title: " (ADO) 的 CopyTo 方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_CopyTo
- _Stream::CopyTo
helpviewer_keywords:
- CopyTo method [ADO]
ms.assetid: b4aa5714-916b-48b8-8b09-cc2708379602
author: rothja
ms.author: jroth
ms.openlocfilehash: 07c5a4bc4db5fc05cb24ee51ac7a9ec989dd1e29
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171397"
---
# <a name="copyto-method-ado"></a>CopyTo 方法 (ADO)
根据 [流](./stream-object-ado.md)中的 [类型](./type-property-ado-stream.md)) 将指定数量的字符或字节 (复制到另一个 **流** 对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
Stream.CopyTo DestStream, NumChars  
```  
  
#### <a name="parameters"></a>参数  
 *DestStream*  
 一个对象变量值，该值包含对打开的 **流** 对象的引用。 当前 **流** 将复制到由 *DestStream* 指定的目标 **流** 中。 目标 **流** 必须已打开。 否则，将发生运行时错误。  
  
> [!NOTE]
>  *DestStream* 参数可能不是 **stream** 对象的代理，因为这需要访问无法远程访问客户端的 **流** 对象上的专用接口。  
  
 *NumChars*  
 可选。 一个 **整数** 值，指定要从源 **流** 中的当前位置复制到目标 **流** 的字节数或字符数。 默认值为-1，指定将所有字符或字节从当前位置复制到 [EOS](./eos-property.md)。  
  
## <a name="remarks"></a>备注  
 此方法从 [位置](./position-property-ado.md) 属性指定的当前位置开始复制指定数量的字符或字节。 如果指定的数字大于在 **eos** 之前的可用字节数，则仅复制当前位置到 **eos** 之间的字符或字节。 如果 *NumChars* 的值为-1 或省略，则会复制从当前位置开始的所有字符或字节。  
  
 如果目标流中存在现有字符或字节，则不会保留副本结束点之外的所有内容，并且不会被截断。 **位置** 成为紧接最后复制的字节之后的字节。 如果要截断这些字节，请调用 [SetEOS](./seteos-method.md)。  
  
 **CopyTo** 应用于将数据复制到与源 **流** 具有相同类型的目标 **流** (它们的 **类型** 属性设置既是 **adTypeText** ，要么都是 **adTypeBinary**) 。 对于文本 **流** 对象，可以更改目标 **流** 的 [字符集](./charset-property-ado.md)属性设置，以将其从一个字符集转换到另一个字符集。 此外，文本 **流** 对象可以成功复制到二进制 **流** 对象，但不能将二进制 **流** 对象复制到文本 **流** 对象中。  
  
## <a name="applies-to"></a>应用于  
 [流对象 (ADO)](./stream-object-ado.md)
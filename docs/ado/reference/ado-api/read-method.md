---
description: Read 方法
title: Read 方法 |Microsoft Docs
ms.prod: sql
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Stream::raw_Read
- _Stream::Read
helpviewer_keywords:
- Read method [ADO]
ms.assetid: 838502de-80f1-4eeb-8838-dd3d9403e567
author: rothja
ms.author: jroth
ms.openlocfilehash: 030627c58eb33d064b50c4e9347a06330f01cf9a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040787"
---
# <a name="read-method"></a>Read 方法
从二进制 [流](./stream-object-ado.md) 对象中读取指定数目的字节。  
  
## <a name="syntax"></a>语法  
  
```  
  
Variant = Stream.Read ( NumBytes)  
```  
  
#### <a name="parameters"></a>参数  
 *NumBytes*  
 可选。 一个 **长整型** 值，指定要从文件中读取的字节数，或 [StreamReadEnum](./streamreadenum.md) 值 **adReadAll**（这是默认值）。  
  
## <a name="return-value"></a>返回值  
 **Read** 方法从 **流** 对象读取指定数目的字节或整个流，并将生成的数据作为 **变量** 返回。  
  
## <a name="remarks"></a>备注  
 如果 *NumBytes* 大于 **流** 中剩余字节数，则仅返回剩余字节数。 不填充数据读取以匹配 *NumBytes* 指定的长度。 如果没有剩余字节可供读取，则返回具有 null 值的变量。 **读取** 不能用于向后读取。  
  
> [!NOTE]
>  *NumBytes* 始终度量字节。 对于文本 **流** 对象 ([类型](./type-property-ado-stream.md) 为 **AdTypeText**) ，请使用 [ReadText](./readtext-method.md)。  
  
## <a name="applies-to"></a>应用于  
 [流对象 (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ReadText 方法](./readtext-method.md)
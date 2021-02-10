---
description: LoadFromFile 方法 (ADO)
title: " (ADO) 的 LoadFromFile 方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_LoadFromFile
helpviewer_keywords:
- LoadFromFile method [ADO]
ms.assetid: b18d8d38-7354-4a94-b637-6ac035faa433
author: rothja
ms.author: jroth
ms.openlocfilehash: b1b3b375158bb8d296c9010caab900db4791fcec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041837"
---
# <a name="loadfromfile-method-ado"></a>LoadFromFile 方法 (ADO)
将现有文件的内容加载到 [流](./stream-object-ado.md)中。  
  
## <a name="syntax"></a>语法  
  
```  
  
Stream.LoadFromFileFileName  
```  
  
#### <a name="parameters"></a>参数  
 *FileName*  
 一个 **字符串** 值，该值包含要加载到 **流** 中的文件的名称。 *文件名* 可以包含 UNC 格式的任何有效路径和名称。 如果指定的文件不存在，则会发生运行时错误。  
  
## <a name="remarks"></a>备注  
 此方法可用于将本地文件的内容加载到 **流** 对象中。 这可用于将本地文件的内容上载到服务器。  
  
 在调用 **LoadFromFile** 之前，**流** 对象必须已打开。 此方法不更改 **流** 对象的绑定;它仍将绑定到最初打开 **流** 时所指定的 URL 或 **记录** 的对象。  
  
 **LoadFromFile** 用从文件读取的数据覆盖 **流** 对象的当前内容。 该文件的内容将覆盖 **流** 中的任何现有字节。 **LoadFromFile** 创建的 [EOS](./eos-property.md)之后的任何先前存在的和剩余的字节将被截断。  
  
 调用 **LoadFromFile** 后，当前位置设置为 **流** 的开始位置 ([位置](./position-property-ado.md) 是 0) 。  
  
 由于可以将2个字节添加到流的开头进行编码，因此流的大小可能与加载它的文件的大小不完全一致。  
  
## <a name="applies-to"></a>应用于  
 [流对象 (ADO)](./stream-object-ado.md)
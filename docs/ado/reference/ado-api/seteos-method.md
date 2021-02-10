---
description: SetEOS 方法
title: SetEOS 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_SetEOS
- _Stream::SetEOS
helpviewer_keywords:
- SetEOS method [ADO]
ms.assetid: 707c18ca-6a56-4970-bbd6-ae1fb86a0b8a
author: rothja
ms.author: jroth
ms.openlocfilehash: a68804b31f36c108096b5f0bb6b3b1d20b4109cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040567"
---
# <a name="seteos-method"></a>SetEOS 方法
设置流的结束位置。  
  
## <a name="syntax"></a>语法  
  
```  
  
Stream.SetEOS  
```  
  
## <a name="remarks"></a>备注  
 **SetEOS** 通过 [使当前位置成为流末尾，来](./position-property-ado.md)更新 [EOS](./eos-property.md)属性的值。 位于当前位置后面的任何字节或字符都将被截断。  
  
 由于 [Write](./write-method.md)、 [WriteText](./writetext-method.md)和 [CopyTo](./copyto-method-ado.md) 不会截断现有 **流** 对象中的任何额外值，因此可以通过使用 **SetEOS** 设置新的流末尾位置来截断这些字节或字符。  
  
> [!CAUTION]
>  如果将 **EOS** 设置为流的实际末尾之前的位置，则会丢失新的 **EOS** 位置之后的所有数据。  
  
## <a name="applies-to"></a>应用于  
 [流对象 (ADO)](./stream-object-ado.md)
---
description: Position 属性 (ADO)
title: " (ADO) 的位置属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::Position
helpviewer_keywords:
- Position property [ADO]
ms.assetid: daa8319a-49aa-4c1c-9af6-0b01e9ab2f9d
author: rothja
ms.author: jroth
ms.openlocfilehash: 75473101028de4ab22d9faad8b5f1269aab33f12
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041047"
---
# <a name="position-property-ado"></a>Position 属性 (ADO)
指示 [流](./stream-object-ado.md) 对象内的当前位置。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 **长整型** 值，该值指定相对于流开头的当前位置的偏移量（以字节为单位）。 默认值为0，表示流中的第一个字节。  
  
## <a name="remarks"></a>备注  
 当前位置可移至流末尾之后的某个点。 如果指定超出流末尾的当前位置，则会相应地增加 **流** 对象的 [大小](./size-property-ado-stream.md)。 以这种方式添加的任何新字节都将为 null。  
  
> [!NOTE]
>  **Position** 始终度量字节。 对于使用多字节字符集的文本流，请将位置乘以字符大小以确定字符数。 例如，对于双字节字符集，第一个字符的位置为0，第二个字符的位置是2，第三个字符的位置是4，依此类推。  
  
> [!NOTE]
>  负值不能用于更改 **流** 中的当前位置。 只有正数才能用于 **位置**。  
  
> [!NOTE]
>  对于只读 **流** 对象，如果 **将 "位置**" 设置为大于 **流****大小** 的值，ADO 将不会返回错误。 这不会更改 **流** 的大小，也不会以任何方式更改 **流** 内容。 但是，应避免这样做，因为它会导致不确切的 **位置** 值。  
  
## <a name="applies-to"></a>应用于  
 [流对象 (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [Charset 属性 (ADO)](./charset-property-ado.md)
---
description: StreamReadEnum
title: StreamReadEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- StreamReadEnum
helpviewer_keywords:
- StreamReadEnum enumeration [ADO]
ms.assetid: cfa1b416-003a-436f-a21b-bd2397e54db3
author: rothja
ms.author: jroth
ms.openlocfilehash: 8fe599d5a737cb1f7d1eef0120c5e0b18d2de31a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056562"
---
# <a name="streamreadenum"></a>StreamReadEnum
指定是应从 [流](./stream-object-ado.md) 对象中读取整个流还是下一行。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adReadAll**|-1|默认。 从当前位置开始，将流中的所有字节读入 [eos](./eos-property.md) 标记。 这是 ([类型](./type-property-ado-stream.md)为 **adTypeBinary**) 的二进制流唯一有效的 **StreamReadEnum** 值。|  
|**adReadLine**|-2|读取 [LineSeparator](./lineseparator-property-ado.md) 属性) 指定的 (流中的下一行。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 这些常量没有 ADO/WFC 等效项。  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [Read 方法](./read-method.md)  
    :::column-end:::
    :::column:::
        [ReadText 方法](./readtext-method.md)  
    :::column-end:::
:::row-end:::
---
description: Type 属性（ADO 流）
title: " (ADO Stream) 类型属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::Type
- _Stream::get_Type
- _Stream::put_Type
helpviewer_keywords:
- Type property [ADO Stream]
ms.assetid: f6a17e8c-7a28-48d0-bded-76b9e0cf7639
author: rothja
ms.author: jroth
ms.openlocfilehash: 61e170cd368771fc75c2c6e552d4465dfe51866c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166377"
---
# <a name="type-property-ado-stream"></a>Type 属性（ADO 流）
指示 [流](./stream-object-ado.md) 中包含的数据的类型 (二进制或文本) 。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 [StreamTypeEnum](./streamtypeenum.md) 值，该值指定 **流** 对象中包含的数据的类型。 默认值为 **adTypeText**。 但是，如果二进制数据最初写入新的空 **流**，则该 **类型** 将更改为 **adTypeBinary**。  
  
## <a name="remarks"></a>备注  
 仅当当前位置在 **流** 的开头时， **Type** 属性才是可读/写的 ([位置](./position-property-ado.md)是 0) ，而在任何其他位置则为只读。  
  
 **Type** 属性确定应该使用哪些方法来读取和写入 **流**。 对于文本 **流**，请使用 [ReadText](./readtext-method.md) 和 [WriteText](./writetext-method.md)。 对于二进制 **流**，请使用 [读取](./read-method.md) 和 [写入](./write-method.md)。  
  
## <a name="applies-to"></a>应用于  
 [流对象 (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [RecordType 属性 (ADO) ](./recordtype-property-ado.md)   
 [Type 属性 (ADO)](./type-property-ado.md)
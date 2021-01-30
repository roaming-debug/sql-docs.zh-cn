---
description: LineSeparator 属性 (ADO)
title: " (ADO) 的 LineSeparator 属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::LineSeparator
helpviewer_keywords:
- LineSeparator property [ADO]
ms.assetid: 0b20fbb8-6b83-48ec-b442-f96c8a4bafbb
author: rothja
ms.author: jroth
ms.openlocfilehash: 629d2948bbbf62987f1352712df02521cf002189
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167177"
---
# <a name="lineseparator-property-ado"></a>LineSeparator 属性 (ADO)
指示要在文本 [流](./stream-object-ado.md) 对象中用作行分隔符的二进制字符。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 [LineSeparatorsEnum](./lineseparatorsenum.md) 值，该值指示 **流** 中使用的行分隔符。 默认值为 **adCRLF**。  
  
## <a name="remarks"></a>备注  
 **LineSeparator** 用于在读取文本 **流** 内容时解释行。 可以通过 [SkipLine](./skipline-method.md) 方法跳过行。  
  
 **LineSeparator** 仅与 ([类型](./type-property-ado-stream.md)为 **adTypeText**) 的文本 **流** 对象一起使用。 如果 **类型** 是 **adTypeBinary**，则忽略此属性。  
  
## <a name="applies-to"></a>应用于  
 [流对象 (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [流对象 (ADO)](./stream-object-ado.md)
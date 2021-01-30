---
description: Charset 属性 (ADO)
title: " (ADO) 的字符集属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::Charset
helpviewer_keywords:
- Charset property [ADO]
ms.assetid: e42507cb-9b46-4ce4-8191-2948eaf14ca2
author: rothja
ms.author: jroth
ms.openlocfilehash: 3fa8b9b5d657208784cd6e9f78e033ebc84f51b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167784"
---
# <a name="charset-property-ado"></a>Charset 属性 (ADO)
指示一个字符集，文本 [流](./stream-object-ado.md) 的内容应在 **流** 对象的内部缓冲区中转换为存储。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 **字符串** 值，该值指定将转换 **流** 内容的字符集。 默认值为 **Unicode**。 允许的值是作为 Internet 字符集名称通过接口传递的典型字符串 (例如，"iso-8859-1"、"Windows-1252" 等) 。 有关系统已知的字符集名称列表，请参阅 Windows 注册表中 HKEY_CLASSES_ROOT\MIME\Database\Charset 的子项。  
  
## <a name="remarks"></a>备注  
 在文本 **流** 对象中，文本数据存储在 **字符集属性指定的字符集** 中。 默认值为 Unicode。 **字符集** 属性用于将数据转换为 **流** 中或 **流** 出的数据。 例如，如果 **流** 包含 ISO-8859-1 数据，并且将数据复制到 BSTR，则 **流** 对象会将数据转换为 Unicode。 反之亦然。  
  
 对于打开的 **流**，[当前位置必须位于](./position-property-ado.md)**流** 的开头 (0) 才能设置 **字符集**。  
  
 **字符集** 仅用于文本 **流** 对象， ([类型](./type-property-ado-stream.md) 为 **adTypeText**) 。 如果 **类型** 是 **adTypeBinary**，则忽略此属性。  
  
 有关代码示例，请参阅 [步骤4：填充详细信息文本框](../../guide/data/step-4-populate-the-details-text-box.md)。  
  
## <a name="applies-to"></a>应用于  
 [流对象 (ADO)](./stream-object-ado.md)
---
description: 字段对象
title: Field 对象 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field
helpviewer_keywords:
- Field object [ADO]
ms.assetid: b10a72fc-3c4b-4186-a70b-993dc9f7a092
author: rothja
ms.author: jroth
ms.openlocfilehash: 6042032018ea75eb7b2b13bb677ad48dca56290b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167375"
---
# <a name="field-object"></a>字段对象
表示数据类型为通用数据类型的列。  
  
## <a name="remarks"></a>备注  
 每个 **字段** 对象都对应于 [记录集中](../../../ado/reference/ado-api/recordset-object-ado.md)的一列。 使用 **Field** 对象的 [Value](../../../ado/reference/ado-api/value-property-ado.md)属性可以设置或返回当前记录的数据。 根据提供程序公开的功能， **字段** 对象的某些集合、方法或属性可能不可用。  
  
 使用 **Field** 对象的集合、方法和属性，可以执行以下操作：  
  
-   返回具有 [name](../../../ado/reference/ado-api/name-property-ado.md) 属性的字段的名称。  
  
-   查看或更改 " **值** " 属性字段中的数据。 **值** 是 **Field** 对象的默认属性。  
  
-   返回具有 [类型](../../../ado/reference/ado-api/type-property-ado.md)、 [精度](../../../ado/reference/ado-api/precision-property-ado.md)和 [NumericScale](../../../ado/reference/ado-api/numericscale-property-ado.md) 属性的字段的基本特征。  
  
-   返回具有 [DefinedSize](../../../ado/reference/ado-api/definedsize-property.md) 属性的字段的声明大小。  
  
-   使用 [ActualSize](../../../ado/reference/ado-api/actualsize-property-ado.md) 属性返回给定字段中数据的实际大小。  
  
-   使用 [特性](../../../ado/reference/ado-api/attributes-property-ado.md) 属性和 [属性](../../../ado/reference/ado-api/properties-collection-ado.md) 集合确定给定字段支持的功能类型。  
  
-   用 [AppendChunk](../../../ado/reference/ado-api/appendchunk-method-ado.md) 和 [GetChunk](../../../ado/reference/ado-api/getchunk-method-ado.md) 方法操作包含长二进制或长字符数据的字段的值。  
  
-   如果提供程序支持批更新，则在批处理更新过程中通过 [OriginalValue](../../../ado/reference/ado-api/originalvalue-property-ado.md) 和 [UnderlyingValue](../../../ado/reference/ado-api/underlyingvalue-property.md) 属性解析字段值中的差异。  
  
 在打开 **字段** 对象的 **记录集** 之前，所有元数据属性 (**名称**、**类型**、 **DefinedSize**、**精度** 和 **NumericScale**) 都可用。 此时设置它们对于动态构造窗体非常有用。  
  
 本部分包含以下主题。  
  
-   [字段对象属性、方法和事件](../../../ado/reference/ado-api/field-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [字段集合 (ADO) ](../../../ado/reference/ado-api/fields-collection-ado.md)   
 [ADO)  (属性集合 ](../../../ado/reference/ado-api/properties-collection-ado.md)   
 [记录集对象 (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)

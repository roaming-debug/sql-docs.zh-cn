---
description: FieldAttributeEnum
title: FieldAttributeEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FieldAttributeEnum
helpviewer_keywords:
- FieldAttributeEnum enumeration [ADO]
ms.assetid: 6e34d886-005a-40dc-bd5c-6adcbf81e5cd
author: rothja
ms.author: jroth
ms.openlocfilehash: aa880e4a9a39e50cef558a77ba6a72fedfbbd8b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034037"
---
# <a name="fieldattributeenum"></a>FieldAttributeEnum
指定 [Field](../../../ado/reference/ado-api/field-object.md) 对象的一个或多个属性。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adFldCacheDeferred**|0x1000|指示提供程序缓存字段值，并且后续读取是从缓存中完成的。|  
|**adFldFixed**|0x10|指示字段包含固定长度数据。|  
|**adFldIsChapter**|0x2000|指示字段包含章节值，该值指定与此父字段相关的特定子记录集。 通常，章节字段用于数据定形或筛选器。|  
|**adFldIsCollection**|0x40000|指示字段指定由记录表示的资源是其他资源（如文件夹）的集合，而不是简单资源（如文本文件）的集合。|  
|**adFldKeyColumn**|0x8000|指示字段指定列主键的全部或部分。|  
|**adFldIsDefaultStream**|0x20000|指示字段包含由记录表示的资源的默认流。 例如，默认流可以是网站上的根文件夹（在指定根 URL 时自动提供）的 HTML 内容。|  
|**adFldIsNullable**|0x20|指示该字段接受 null 值。|  
|**adFldIsRowURL**|0x10000|指示此字段包含从记录表示的数据存储区中对资源进行命名的 URL。|  
|**adFldLong**|0x80|指示该字段是长二进制字段。 还指示你可以使用 [AppendChunk](../../../ado/reference/ado-api/appendchunk-method-ado.md) 和 [GetChunk](../../../ado/reference/ado-api/getchunk-method-ado.md) 方法。|  
|**adFldMayBeNull**|0x40|指示您可以从该字段中读取 null 值。|  
|**adFldMayDefer**|0x2|指示字段已延迟，即，字段值不是从数据源中检索整个记录，而只是在显式访问字段值时进行检索。|  
|**adFldNegativeScale**|0x4000|指示该字段表示来自支持负值刻度值的列的数值。 小数位数由 [NumericScale](../../../ado/reference/ado-api/numericscale-property-ado.md) 属性指定。|  
|**adFldRowID**|0x100|指示字段包含不能写入的持久性行标识符，并且除标识行 (（如记录号、唯一标识符) 等）之外没有有意义的值。|  
|**adFldRowVersion**|0x200|指示字段包含某些用于跟踪更新的时间戳或日期戳。|  
|**adFldUnknownUpdatable**|0x8|指示提供程序无法确定是否可以写入字段。|  
|**adFldUnspecified**|-1 0xFFFFFFFF|指示提供程序未指定字段特性。|  
|**adFldUpdatable**|0x4|指示你可以向字段写入。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums.FieldAttribute.CACHEDEFERRED|  
|AdoEnums. FieldAttribute|  
|AdoEnums. FieldAttribute. ISNULLABLE|  
|AdoEnums. FieldAttribute|  
|AdoEnums.FieldAttribute.MAYBENULL|  
|AdoEnums.FieldAttribute.MAYDEFER|  
|AdoEnums.FieldAttribute.NEGATIVESCALE|  
|AdoEnums. FieldAttribute|  
|AdoEnums. FieldAttribute. ROWVERSION|  
|AdoEnums.FieldAttribute.UNKNOWNUPDATABLE|  
|AdoEnums. FieldAttribute。未指定|  
|AdoEnums. FieldAttribute|  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [Append 方法 (ADO)](../../../ado/reference/ado-api/append-method-ado.md)  
    :::column-end:::
    :::column:::
        [Attributes 属性 (ADO)](../../../ado/reference/ado-api/attributes-property-ado.md)  
    :::column-end:::
:::row-end:::

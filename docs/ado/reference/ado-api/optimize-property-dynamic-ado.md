---
description: Optimize 属性 - 动态 (ADO)
title: 优化 Property-Dynamic (ADO) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Optimize property [ADO]
ms.assetid: a491c4ce-2b04-4c84-be83-3846bde8d16b
author: rothja
ms.author: jroth
ms.openlocfilehash: 839c2cb27fc14546523272c1c5b6d69f233557fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166945"
---
# <a name="optimize-property-dynamic-ado"></a>Optimize 属性 - 动态 (ADO)
指定是否应在 [字段](./field-object.md)上创建索引。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 **布尔** 值，该值指示是否应创建索引。  
  
## <a name="remarks"></a>备注  
 索引可以提高在 [记录集中](./recordset-object-ado.md)查找或排序值的操作的性能。 索引是 ADO 内部的;你不能在应用程序中显式访问或使用该应用程序。  
  
 若要对字段创建索引，请将 **Optimize** 属性设置为 **True**。 若要删除索引，请将此属性设置为 **False**。  
  
 当 [CursorLocation](./cursorlocation-property-ado.md)属性设置为 **AdUseClient** 时， **Optimize** 是追加到 [Field](./field-object.md)对象 [Properties](./properties-collection-ado.md) collection 的动态属性。  
  
## <a name="usage"></a>使用情况  
  
```  
Dim rs As New Recordset  
Dim fld As Field  
rs.CursorLocation = adUseClient      'Enable index creation  
rs.Fields.Append "Field1", adChar, 35, adFldIsNullable  
rs.Open  
Set fld = rs.Fields(0)  
fld.Properties("Optimize") = True    'Create an index  
fld.Properties("Optimize") = False   'Delete an index  
```  
  
## <a name="applies-to"></a>应用于  
 [字段对象](./field-object.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VB 优化属性示例) ](./optimize-property-example-vb.md)   
 [ (VC + + 的优化属性示例) ](./optimize-property-example-vc.md)   
 [筛选器属性](./filter-property.md)   
 [Find 方法 (ADO) ](./find-method-ado.md)   
 [Sort 属性](./sort-property.md)
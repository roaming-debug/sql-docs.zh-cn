---
description: Seek 方法
title: Seek 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset21::Seek
- Recordset21::raw_Seek
helpviewer_keywords:
- Seek method [ADO]
ms.assetid: 129293d2-19d3-4940-bf64-483ee72fb4a1
author: rothja
ms.author: jroth
ms.openlocfilehash: 9819ce55d8898fb55e0cc98fb9dab10f339cfb17
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041697"
---
# <a name="seek-method"></a>Seek 方法
搜索 [记录集](./recordset-object-ado.md) 的索引以快速找到与指定值匹配的行，并将当前行的位置更改为该行。  
  
## <a name="syntax"></a>语法  
  
```  
  
recordset.Seek KeyValues, SeekOption  
```  
  
#### <a name="parameters"></a>参数  
 *架构*  
 **变量** 值的数组。 索引由一个或多个列组成，数组包含一个要与每个对应列进行比较的值。  
  
 *SeekOption*  
 一个 [SeekEnum](./seekenum.md) 值，指定要在索引的列和对应的 *架构* 之间进行的比较的类型。  
  
## <a name="remarks"></a>备注  
 如果基础提供程序支持 **Recordset** 对象上的索引，则将 **Seek** 方法与 [Index](./index-property.md)属性结合使用。 使用 [支持](./supports-method.md)**(adSeek)** 方法来确定基础提供程序是否支持 **查找**，并 **支持 (a)** 方法来确定提供程序是否支持索引。  (例如， [Microsoft Jet 的 OLE DB 提供程序](../../guide/appendixes/microsoft-ole-db-provider-for-microsoft-jet.md) 支持 **查找** 和 **索引**。 )   
  
 如果查找找不到所需的行 **，则不** 会出现错误，并且行位于 **记录集** 的末尾。 执行此方法之前，请将 **index** 属性设置为所需的索引。  
  
 只有服务器端游标才支持此方法。 当 **记录集** 对象的 [CursorLocation](./cursorlocation-property-ado.md) 属性值为 **adUseClient** 时，不支持查找。  
  
 仅当使用 [CommandTypeEnum](./commandtypeenum.md)值 **adCmdTableDirect** 打开 **Recordset** 对象时，才能使用此方法。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [Seek 方法和 Index 属性示例 (VB) ](./seek-method-and-index-property-example-vb.md)   
 ["查找方法和索引" 属性示例 (VC + +) ](./seek-method-and-index-property-example-vc.md)   
 [Find 方法 (ADO) ](./find-method-ado.md)   
 [Index 属性](./index-property.md)
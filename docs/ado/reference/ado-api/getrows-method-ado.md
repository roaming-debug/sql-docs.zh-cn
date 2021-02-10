---
description: GetRows 方法 (ADO)
title: " (ADO) 的 GetRows 方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::GetRows
- Recordset15::raw_GetRows
helpviewer_keywords:
- Getrows method [ADO]
ms.assetid: 14b92860-4171-47d9-a413-dd60dd6a8880
author: rothja
ms.author: jroth
ms.openlocfilehash: 717569716fa6191cfb4fc7258c32240d9ebd2e17
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100021034"
---
# <a name="getrows-method-ado"></a>GetRows 方法 (ADO)
将 [记录集](./recordset-object-ado.md) 对象的多个记录检索到一个数组中。  
  
## <a name="syntax"></a>语法  
  
```  
  
array = recordset.GetRows(Rows, Start, Fields )  
```  
  
## <a name="return-value"></a>返回值  
 返回一个 **变量** ，其值为二维数组。  
  
#### <a name="parameters"></a>参数  
 *行*  
 可选。 一个 [GetRowsOptionEnum](./getrowsoptionenum.md) 值，指示要检索的记录数。 默认值为 **adGetRowsRest**。  
  
 *启动*  
 可选。 一个 **字符串** 值或 **变量** ，其计算结果为应从中开始 **GetRows** 操作的记录的书签。 还可以使用 [BookmarkEnum](./bookmarkenum.md) 值。  
  
 *Fields*  
 可选。 一个表示单个字段名称或序号位置的 **变量** ，或是一个字段名称或序号位置编号的数组。 ADO 仅返回这些字段中的数据。  
  
## <a name="remarks"></a>备注  
 使用 **GetRows** 方法将记录从 **记录集** 复制到二维数组。 第一个下标标识字段，第二个下标标识记录号。 当 **GetRows** 方法返回数据时，*数组* 变量自动标注为正确的大小。  
  
 如果未指定 *Rows* 参数的值，则 **GetRows** 方法会自动检索 **Recordset** 对象中的所有记录。 如果请求的记录超过可用记录数，则 **GetRows** 只返回可用记录的数目。  
  
 如果 **Recordset** 对象支持书签，则可以通过在 *Start* 参数中传递该记录的 "[书签](./bookmark-property-ado.md)" 属性的值来指定在哪个记录中， **GetRows** 方法应开始检索数据。  
  
 如果要限制 **GetRows** 调用返回的字段，可以在 *fields* 参数中传递单个字段名称/数字或字段名称/数字的数组。  
  
 调用 **GetRows** 之后，下一个未读记录将成为当前记录，或者如果没有更多记录，则 [EOF](./bof-eof-properties-ado.md) 属性将设置为 **True** 。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VB) GetRows 方法示例 ](./getrows-method-example-vb.md)   
 [GetRows 方法示例 (VC++)](./getrows-method-example-vc.md)
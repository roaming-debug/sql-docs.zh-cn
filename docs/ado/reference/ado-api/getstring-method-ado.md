---
description: GetString 方法 (ADO)
title: " (ADO) 的 GetString 方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset20::raw_GetString
- Recordset20::GetString
helpviewer_keywords:
- GetString method [ADO]
ms.assetid: 92452940-b2a7-456e-94fc-3780c71da33c
author: rothja
ms.author: jroth
ms.openlocfilehash: b1cfaa8b75f7e8d1dfb5fc0cecf7e5d99634960a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020805"
---
# <a name="getstring-method-ado"></a>GetString 方法 (ADO)
以字符串的形式返回 [记录集](./recordset-object-ado.md) 。  
  
## <a name="syntax"></a>语法  
  
```  
  
Variant = recordset.GetString(StringFormat, NumRows, ColumnDelimiter, RowDelimiter, NullExpr)  
```  
  
## <a name="return-value"></a>返回值  
 以字符串值 **变体** 形式返回 (BSTR) 的 **记录集**。  
  
#### <a name="parameters"></a>参数  
 *StringFormat*  
 一个 [StringFormatEnum](./stringformatenum.md) 值，指定应如何将 **记录集** 转换为字符串。 *RowDelimiter*、 *ColumnDelimiter* 和 *NullExpr* 参数仅用于 **adClipString** 的 *StringFormat* 。  
  
 *NumRows*  
 可选。 要在 **记录集中** 转换的行数。 如果未指定 *NumRows* ，或大于 **记录集中** 的总行数，则将转换 **记录集中** 的所有行。  
  
 *ColumnDelimiter*  
 可选。 如果已指定，则在列之间使用分隔符，否则为制表符。  
  
 *RowDelimiter*  
 可选。 如果已指定，则在行间使用分隔符，否则返回回车符。  
  
 *NullExpr*  
 可选。 如果指定，则使用表达式代替 null 值，否则为空字符串。  
  
## <a name="remarks"></a>备注  
 行数据（但不是架构数据）保存到字符串中。 因此，不能使用此字符串重新打开 **记录集** 。  
  
 此方法等效于 RDO **GetClipString** 方法。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [GetString 方法示例 (VB)](./getstring-method-example-vb.md)
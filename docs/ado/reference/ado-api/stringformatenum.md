---
description: StringFormatEnum
title: StringFormatEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- StringFormatEnum
helpviewer_keywords:
- StringFormatEnum enumeration [ADO]
ms.assetid: 28f7d1ec-092b-4323-a39d-d3f882c6c81a
author: rothja
ms.author: jroth
ms.openlocfilehash: b23a6bc4354f4c67c07bcdc1f4b4d463fff072f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056502"
---
# <a name="stringformatenum"></a>StringFormatEnum
指定以字符串形式检索 [记录集](./recordset-object-ado.md) 时的格式。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adClipString**|2|按 *RowDelimiter*、 *ColumnDelimiter* 和 *NullExpr* 分隔行。 [GetString](./getstring-method-ado.md)方法的这三个参数仅对 **adClipString** 的 *StringFormat* 有效。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums.StringFormat.CLIPSTRING|  
  
## <a name="applies-to"></a>应用于  
 [GetString 方法 (ADO)](./getstring-method-ado.md)
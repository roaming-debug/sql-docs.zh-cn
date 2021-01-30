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
ms.openlocfilehash: 199aaab9215165e06598ca0c1be783b0a123a1c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166396"
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
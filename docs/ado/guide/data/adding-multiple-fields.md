---
description: 添加多个字段和值
title: 添加多个字段 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- AddNew method [ADO]
- ADO, adding data
- editing data [ADO], adding multiple fields
- editing data [ADO], AddNew method
ms.assetid: f3648ef4-9f36-4991-a868-83a617389844
author: rothja
ms.author: jroth
ms.openlocfilehash: 4b72ed3beafd211439890946da9c41f39af8eb90
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028622"
---
# <a name="adding-multiple-fields-and-values"></a>添加多个字段和值
有时，将字段及其相应值传入到 **AddNew** 方法可能更高效，而不是为每个新字段多次设置 **值** 。 如果 *FieldList* 是一个数组，则 *值* 也必须是具有相同成员数的数组;否则，将发生错误。 字段名称的顺序必须与每个数组中的字段值顺序相匹配。 下面的代码将字段数组和值数组传递到 **AddNew** 方法。

```
'BeginAddNew2
    Dim avarFldNames As Variant
    Dim avarFldValues As Variant

    avarFldNames = Array("CompanyName", "Phone")
    avarFldValues = Array("Sample Shipper 2", "(931) 555-6334")

    If objRs1.Supports(adAddNew) Then
        objRs1.AddNew avarFldNames, avarFldValues
    End If

    'Re-establish a Connection and update
    Set objRs1.ActiveConnection = GetNewConnection
    objRs1.UpdateBatch
'EndAddNew2
```
